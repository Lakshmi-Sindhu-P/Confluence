# System Requirements & Design (SRD)

## 1. Design status

Pre-implementation. The technology families are fixed; exact versions, schemas, partition keys, quality policies, and physical topology require research/profiling before freezing.

## 2. Fixed logical stack

1. Authoritative CMS DE-SynPUF source files
2. Local landing/provenance
3. Apache Hadoop HDFS raw storage
4. Apache Hive metadata + HiveQL
5. Apache Spark/PySpark processing
6. Spark SQL for selected SQL transformations/validation
7. Apache Parquet standardized/curated storage on HDFS
8. Hive-compatible curated metadata
9. Apache Impala analytical SQL
10. Bash scripts for repeatable operations after manual learning
11. Python for tests/utilities where appropriate
12. Git/GitHub for source control/documentation
13. Zero-cost local containerized runtime

## 3. Candidate physical topology

The implementation agent must research the smallest mutually compatible local topology. Expected logical services include HDFS NameNode/DataNode, Hive Metastore plus required backing metadata database/service, Spark runtime, and Impala services sufficient for local querying. Do not invent a topology before compatibility research.

## 4. Storage zones

Candidate HDFS paths:

- `/cms-utilization/raw/` - immutable source files
- `/cms-utilization/standardized/` - source-specific typed/normalized Parquet
- `/cms-utilization/curated/` - integrated analytical products
- `/cms-utilization/quarantine/` - only if actual quality policy requires isolation
- `/cms-utilization/audit/` - lightweight run/quality artifacts if filesystem-backed audit is chosen

Path names may be refined through an ADR; semantic separation must remain.

## 5. Required HDFS operations

Owner must manually perform and explain at minimum:

- `hdfs dfs -mkdir`
- `hdfs dfs -put`
- `hdfs dfs -ls`
- `hdfs dfs -cat`
- `hdfs dfs -du`
- `hdfs dfs -get`
- `hdfs dfs -rm`

Automation is added later.

## 6. Hive design

Raw layer: expected to use external tables over HDFS raw data so table metadata can be managed separately from immutable source files. This must be confirmed by ADR after environment/data review.

Required Hive/HiveQL work:
- databases;
- external tables;
- a controlled managed-vs-external experiment for learning;
- explicit schemas;
- table inspection;
- joins;
- aggregations;
- transformations where appropriate;
- partitions where justified;
- validation queries.

## 7. Spark/PySpark design

Stable pipeline must demonstrate:
- explicit `StructType`/schema definitions or equivalent explicit schema mechanism;
- source ingestion;
- type casting/date parsing;
- documented missing-value policy;
- duplicate analysis and deduplication only when justified;
- source-to-target mapping;
- joins with cardinality/reconciliation checks;
- derived fields only when defensible;
- data-quality evaluation;
- partition-aware read/write behavior;
- Parquet outputs.

Avoid opaque monolithic scripts. Separate source-specific standardization, integration, quality, and product construction enough to be understandable and testable.

## 8. Spark SQL design

Use temporary/permanent views as appropriate for SQL transformations and validation. Demonstrate deliberate choice between DataFrame API and SQL. Do not create duplicate transformations solely for portfolio coverage.

## 9. Parquet design

Parquet is the fixed curated file format because it is columnar and interoperable with Spark/Hive/Impala. Compression codec and partition layout are implementation details to validate during Phase 0A/0B and profiling.

## 10. Impala design

Impala is the fixed interactive SQL engine for the project. Required hands-on outcomes:
- connect to Impala;
- inspect databases/tables;
- understand/execute metadata refresh operations when needed;
- filter/select;
- aggregate;
- join curated datasets where appropriate;
- run analytical queries over `beneficiary_year_utilization`;
- explain why Impala's role differs from Spark batch processing and Hive metadata/SQL semantics.

Do not claim production Impala tuning experience.

## 11. Data contracts

For each source dataset, document:
- source name and provenance;
- grain/unit of record;
- years;
- expected columns/types;
- candidate/verified identifiers;
- nullability observations;
- linkage fields;
- documented coding conventions;
- schema version/date where available.

Contracts transition from PROVISIONAL to VERIFIED only through authoritative documentation + profiling.

## 12. Source-to-target mapping

Required columns:
- mapping_id
- source_dataset
- source_column
- source_type
- target_dataset
- target_column
- target_type
- transformation_rule
- nullable
- quality_rule_id
- rationale
- evidence/reference
- status

No curated field without a mapping entry.

## 13. Pipeline stages

**Acquire:** obtain approved CMS files from authoritative distribution; record provenance/terms.  
**Land:** preserve unchanged local source archive/file.  
**Ingest:** copy approved files to HDFS raw paths and verify file counts/sizes.  
**Register raw:** create Hive external metadata.  
**Profile:** calculate evidence-backed source characteristics.  
**Standardize:** apply approved source-specific schemas/mappings in PySpark.  
**Validate:** execute structural/semantic checks.  
**Integrate:** link beneficiary/claims using verified relationships.  
**Reconcile:** prove expected cardinality and important aggregates.  
**Curate:** build beneficiary-year product in Parquet.  
**Register curated:** expose metadata.  
**Consume:** execute Impala analytical SQL.  
**Automate:** only after manual operation understanding, create repeatable Bash workflow.

## 14. Data-quality framework

Every implemented rule should have:
- rule ID;
- dataset/stage;
- description;
- severity;
- observed metric/failure count;
- expected condition;
- pass/fail;
- disposition on failure;
- evidence.

Candidate categories (not assumed findings): schema, completeness, uniqueness, validity, referential integrity, reconciliation, product invariants.

Critical failures must prevent publication of untrusted curated output. Noncritical handling must be explicitly decided.

## 15. Testing strategy

- environment/service smoke tests;
- HDFS operation verification;
- Hive DDL/query smoke tests;
- schema tests;
- transformation unit tests where useful;
- quality-rule tests;
- join/reconciliation tests;
- curated product grain/uniqueness tests;
- Impala query smoke tests;
- end-to-end reproducibility test.

A zero exit code alone is never sufficient proof of data correctness.

## 16. Idempotency/restartability

Derived writes should use deterministic destinations and controlled overwrite/partition strategies so reruns do not duplicate data. Raw inputs remain immutable. Exact strategy is proposed after data/profile decisions.

## 17. Failure handling

Fail closed on conditions that invalidate correctness, such as missing required source, incompatible schema, critical key failure, failed required reconciliation, or corrupt output. Anomalies that can safely remain must be flagged or quarantined according to an approved rule.

## 18. Security/data handling

- No secrets in repository.
- No real PHI/PII.
- Use only approved synthetic/public-use files.
- Do not commit large raw CMS datasets unless redistribution terms and repository practicality explicitly support it.
- Prefer download/setup instructions and tiny legally permitted fixtures for tests.

## 19. Version compatibility gate

Before creating the environment, the agent must research official Apache documentation/release notes and credible compatibility evidence for Hadoop, Hive, Spark, Impala, Java, Python, metastore database, and container images. Produce a proposed pinned matrix with rationale and known limitations. Do not silently substitute a technology family. Owner approves the matrix before build.

## 20. Definition of done per phase

Code exists + execution succeeds + output is verified + evidence/docs updated + owner learning checkpoint completed. Planned code or generated files alone do not count.
