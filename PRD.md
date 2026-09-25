# Product Requirements Document (PRD)

## 1. Product definition

**Product:** CMS Medicare Utilization Data Platform  
**Type:** Local, reproducible healthcare data-integration and analytical data-product pipeline  
**Status:** Pre-implementation / Phase -1  
**Core data:** CMS synthetic Medicare public-use files only

## 2. Problem statement

CMS synthetic Medicare data is distributed across beneficiary and claims datasets with different record grains and schemas. Repeated beneficiary-level utilization analysis requires recurring source interpretation, linkage, cleaning, type normalization, validation, reconciliation, and aggregation. This creates duplicated analytical preparation and makes it harder to prove how a result was derived.

The product will create a reproducible integration layer that preserves source data, documents mappings, validates relationships, produces standardized/curated datasets, and exposes an analyst-ready beneficiary-year utilization product.

## 3. Product purpose

Provide a repeatable path from authoritative synthetic CMS source files to a validated analytical dataset while preserving traceability to the original inputs.

## 4. Users

### Primary user - Healthcare Data Analyst
Needs SQL-queryable, analysis-ready beneficiary utilization data without rebuilding raw integration logic.

### Secondary user - Data Engineer / Maintainer
Needs deterministic ingestion/transformation, explicit mappings, data-quality evidence, troubleshooting information, and reproducible local setup.

### Learning owner
Needs to personally understand and execute core HDFS/Hive/Spark/Impala operations. This is an internal project constraint, not the public product purpose.

## 5. Core user outcomes

The analyst can:
- query annual beneficiary utilization;
- inspect claim counts and reimbursement aggregates supported by source data;
- analyze supported geographic/time dimensions;
- identify repeated utilization using defined metrics;
- understand whether claims link to expected beneficiary records;
- reconcile curated values to source-derived aggregates.

The engineer can:
- identify source provenance;
- reproduce ingestion;
- inspect raw files in HDFS;
- inspect Hive metadata;
- run transformations and tests;
- diagnose failed quality/reconciliation gates;
- reproduce curated outputs.

## 6. Primary data product

### `beneficiary_year_utilization`

Provisional grain: beneficiary + year.

Candidate attributes are intentionally provisional until discovery. Every final target field must exist in the source-to-target mapping with a documented derivation and validation rule.

## 7. Functional requirements

**FR-001 Source acquisition/provenance**  
Record authoritative source page, dataset/sample, filename, source type, years, acquisition date, and usage/redistribution considerations.

**FR-002 Raw preservation**  
Source files must remain unchanged after landing/ingestion. Derived outputs may never overwrite raw inputs.

**FR-003 HDFS storage**  
Approved source files must be stored in a clearly defined HDFS raw hierarchy and be operable using standard `hdfs dfs` commands.

**FR-004 Raw discoverability**  
Raw data must be inspectable/queryable through Hive external metadata without transferring ownership of the source data to the table definition.

**FR-005 Data discovery**  
Before stable transformations are designed, profile schemas, row counts, key cardinality, null patterns, candidate duplicates, date/value ranges, and cross-source relationships. Findings must be evidence-backed.

**FR-006 Explicit stable schemas**  
Exploration may infer schemas; production-like pipeline stages must use explicit schemas unless an approved ADR says otherwise.

**FR-007 Source-to-target mapping**  
Every curated field must document source dataset/column, source type, target field/type, transformation, nullability, and relevant quality rule.

**FR-008 Standardization**  
Apply only justified transformations for types, dates, names, null representations, identifiers, claim type, and monetary fields.

**FR-009 Integration**  
Integrate beneficiary and claims data only through relationships supported by CMS documentation and verified against observed data.

**FR-010 Data-quality gates**  
Implement justified schema, completeness, uniqueness, validity, referential-integrity, and reconciliation checks.

**FR-011 Invalid/anomalous data policy**  
No suspicious record may silently disappear. The project must explicitly decide whether each failure class blocks, flags, retains, or quarantines records.

**FR-012 Curated storage**  
Write approved standardized/curated datasets in Apache Parquet on HDFS. Partition strategy must be justified by data distribution/access patterns.

**FR-013 Curated metadata**  
Expose curated datasets through Hive-compatible metadata.

**FR-014 Interactive SQL**  
Curated data must be queryable using Apache Impala for interactive analytical SQL.

**FR-015 Spark processing**  
Use PySpark for explicit-schema ingestion, transformations, joins, missing-value policy, deduplication when evidence supports it, quality rules, aggregations, and writes.

**FR-016 Spark SQL**  
Use Spark SQL where SQL improves clarity for transformation/validation; do not duplicate every DataFrame transformation simply to demonstrate both APIs.

**FR-017 Reconciliation**  
Track and explain cardinality changes and important aggregate changes across stages. No unexplained record loss.

**FR-018 Idempotency**  
Derived stages should be safely rerunnable without unintended duplication where practical.

**FR-019 Execution evidence**  
Capture enough run information to identify input/output counts, quality status, failures, and relevant stage results without adding heavyweight observability infrastructure.

**FR-020 Manual learning checkpoints**  
Core HDFS/Hive/Impala operations must be manually executed and understood by the owner before being hidden behind scripts.

## 8. Non-functional requirements

**NFR-001 Zero cost:** core reproduction must require no paid service or enterprise license.  
**NFR-002 Open stack:** core data technologies are open-source Apache/Python/Git technologies.  
**NFR-003 Local reproducibility:** must run on a personal machine using a minimal local containerized environment.  
**NFR-004 Traceability:** curated fields and material cardinality changes must be explainable.  
**NFR-005 Testability:** data rules and transformations must have executable verification.  
**NFR-006 Maintainability:** use clear configuration, small modules/scripts, deterministic paths, and documented decisions.  
**NFR-007 Honesty:** documentation must distinguish observed facts, hypotheses, decisions, implementation, and verified implementation.  
**NFR-008 Scope discipline:** no unnecessary platform components merely for portfolio breadth.  
**NFR-009 Resource awareness:** prefer the smallest topology that demonstrates concepts; do not emulate a production cluster for appearance.  
**NFR-010 Explainability:** every major component must have a documented purpose, role, verification method, and tradeoff.

## 9. Constraints

- Synthetic/public-use data only.
- Initial source types limited to Beneficiary Summary, Inpatient Claims, Outpatient Claims.
- Fixed core technology families: Hadoop/HDFS, Bash, Hive/HiveQL, Spark/PySpark/Spark SQL, Parquet, Impala, Python, Git, local container runtime.
- Exact versions must be researched for mutual compatibility before environment implementation.
- Technology-family substitutions require owner approval.
- Necessary implementation-plan changes must be proposed first with evidence/reasoning.

## 10. Non-goals

Production healthcare claims processing; adjudication; PHI; clinical decisions; fraud detection; production HIPAA compliance; production-scale cluster administration; FHIR implementation; cloud deployment; Kafka/Airflow/Kubernetes/dbt/ML/dashboard work.

## 11. Success metrics / acceptance

- Authoritative source provenance documented.
- Selected source files successfully ingested into HDFS.
- Owner demonstrates required HDFS CLI operations.
- Hive raw external tables query approved inputs.
- Explicit schemas and source-to-target mapping exist.
- PySpark standardized/integrated processing succeeds reproducibly.
- Data-quality and reconciliation gates produce auditable results.
- `beneficiary_year_utilization` is produced and its grain/fields are validated.
- Curated Parquet data is registered and queryable through Impala.
- End-to-end rerun succeeds from documented starting point.
- README accurately reflects only implemented capabilities.
- SKILLS_EVIDENCE contains only verified work.
- Owner can verbally explain source-to-query lifecycle and major design decisions.

## 12. Product principle

**The data problem chooses the implementation details. The fixed stack defines the learning/engineering environment, but it does not authorize fabricated requirements or transformations.**
