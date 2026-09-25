# CMS Medicare Utilization Data Platform

## Purpose

CMS publishes synthetic Medicare beneficiary and claims datasets so developers and researchers can learn the structure and complexity of Medicare claims data without exposing real beneficiary information. Those source files are useful individually, but repeated beneficiary-level utilization analysis requires schema interpretation, linkage, validation, standardization, reconciliation, and aggregation across datasets.

This project will build a reproducible local data-integration platform that preserves the original CMS synthetic source files, makes them inspectable, standardizes and validates records, integrates beneficiary and claim information, and produces reusable analysis-ready utilization datasets.

The project is a personal engineering/learning project. It is **not** production healthcare experience, real claims adjudication, payer employment, or analysis of real patients.

## Why this project exists

The engineering problem is independent of any job description: heterogeneous healthcare claims data must be landed, understood, mapped, validated, transformed, integrated, curated, and made queryable. Current healthcare reference architectures from AWS describe analogous patterns: preserve raw source data, transform/normalize it, apply data-quality and referential-integrity rules, curate data, catalog metadata/lineage, and expose purpose-built analytical products. CMS itself states that DE-SynPUF is intended to let developers create programs/products and learn the complexity of CMS claims data.

The project's private learning objective is to turn prior academic/basic exposure to the Hadoop ecosystem into genuine, recent hands-on implementation experience. That objective must never be confused with the public product purpose.

## Primary user

A healthcare data analyst working with CMS synthetic Medicare data who wants a reusable beneficiary-level annual utilization dataset without reconstructing source relationships for every analysis.

This is a project persona, not a claim of a real client.

## Initial source scope

Authoritative candidate source: **CMS 2008-2010 Data Entrepreneurs' Synthetic Public Use File (DE-SynPUF)**.

Initial source types:

- Beneficiary Summary
- Inpatient Claims
- Outpatient Claims

Carrier Claims and Prescription Drug Events are deliberately deferred until evidence shows that adding them materially improves the product.

The exact sample and local working subset will be selected during Phase -1 after inspecting file sizes, documentation, and machine constraints.

## Proposed data product

### `beneficiary_year_utilization`

Provisional grain: one record per beneficiary per analysis year.

Candidate measures/dimensions include beneficiary identifier, year, available geography, defensible age grouping, chronic-condition indicators/counts, inpatient claim count, outpatient claim count, inpatient/outpatient reimbursement, and total utilization measures.

**The schema is not final.** No field becomes part of the product until the source documentation and profiling support it.

A secondary `claim_utilization_summary` may be added only if it provides genuine analytical value.

## Fixed technology stack

The project stack is fixed at the technology-family level:

- Apache Hadoop / HDFS
- HDFS CLI (`hdfs dfs`)
- Bash
- Apache Hive / HiveQL
- Apache Spark
- PySpark
- Spark SQL
- Apache Parquet
- Apache Impala
- Python
- Git / GitHub
- Local containerized development using a zero-cost container runtime compatible with Compose workflows

The exact compatible versions/images are **not pre-fabricated**. The agent must research and propose a compatible pinned version matrix before environment creation. Changing a technology family requires owner approval; changing a version for compatibility is an implementation decision that must be documented.

Scala is not in the core stack. Hue and FHIR are future/optional extensions and must not be added to core scope without approval.

## Candidate architecture

```text
CMS synthetic files
        |
        v
Local landing / provenance record
        |
        v
HDFS RAW (immutable source preservation)
        |
        +--> Hive external/raw tables
        |          |
        |          +--> profiling / validation SQL
        |
        v
PySpark standardization + mapping + quality rules
        |
        v
HDFS STANDARDIZED (Parquet)
        |
        v
PySpark/Spark SQL integration + reconciliation
        |
        v
HDFS CURATED (partitioned Parquet)
        |
        +--> Hive metadata
                 |
                 v
              Impala
                 |
                 v
        Analyst SQL exploration
```

This architecture is **proposed but not blindly immutable**. The stack is fixed, but storage layout, partition strategy, table design, data-quality policy, schemas, and implementation sequencing must be evidence-driven. Necessary changes must be proposed with reasoning before implementation.

## Industry-shaped requirements

1. Preserve original source data.
2. Maintain provenance and source inventory.
3. Make raw data inspectable without mutating it.
4. Profile before defining transformations.
5. Use explicit stable schemas after discovery.
6. Maintain source-to-target mappings.
7. Standardize types/names only where justified.
8. Validate linkage and referential integrity.
9. Reconcile row counts and important aggregates across stages.
10. Never silently lose records.
11. Produce reusable curated data products.
12. Support SQL consumption without requiring analysts to edit PySpark.
13. Make reruns deterministic/idempotent where practical.
14. Keep the entire core project reproducible locally for $0.
15. Record decisions, findings, failures, and verified skill evidence.

## Build phases

- **Phase -1 - Problem & Data Discovery:** authoritative documentation, provenance, source inventory, licensing/usage review, sample selection, profiling questions, first observations.
- **Phase 0A - Requirements & Architecture:** freeze data contracts, source-to-target mapping approach, storage layers, DQ policy, data-product grain, architecture decisions.
- **Phase 0B - Environment:** research compatibility matrix, pin versions, build the smallest reproducible local stack, verify services individually.
- **Phase 1 - HDFS Foundation:** filesystem concepts, namespace, directories, raw-layer design.
- **Phase 2 - HDFS Operations:** manual CLI ingestion/inspection/retrieval/removal before automation.
- **Phase 3 - Hive:** database, external tables, managed-table experiment, schemas, partitions, joins, aggregations, validation queries.
- **Phase 4 - PySpark Integration:** explicit schemas, transformations, joins, null/duplicate policy, mappings, partition-aware writes.
- **Phase 5 - Spark SQL:** SQL transformations/validation where they improve clarity.
- **Phase 6 - Curated Data Product:** build and reconcile `beneficiary_year_utilization`.
- **Phase 7 - Data Quality:** automated structural, completeness, uniqueness, validity, referential-integrity, and reconciliation checks justified by evidence.
- **Phase 8 - Impala / Consumption:** interactive SQL over curated data, metadata operations, joins/aggregations, comparison with Hive/Spark roles.
- **Phase 9 - End-to-End Reproducibility:** scripts/orchestration after manual understanding; deterministic reruns.
- **Phase 10 - GitHub Engineering Record:** architecture, mappings, DQ findings, troubleshooting, limitations, reproducibility.
- **Phase 11 - Interview & Evidence Review:** explain lifecycle, validate skills evidence, produce truthful resume language.

## Human-learning rule

The implementation agent may work autonomously inside an approved unit of work, but it must not hide core learning operations behind automation before the owner has manually executed and understood them. At meaningful checkpoints it must explain what it built, why, expected behavior, verification, failure modes, concept connections, and interview framing.

## Definition of success

The project is successful when a clean local environment can reproducibly turn approved CMS synthetic inputs into a validated `beneficiary_year_utilization` dataset that is queryable through SQL, with source-to-target traceability, quality evidence, documented decisions, and no unexplained record loss.

The owner must personally be able to explain: **what happens to a source file from acquisition and HDFS ingestion through Hive metadata, Spark transformation, validation, curated storage, and Impala querying.**

## Explicit non-goals

- Production claims adjudication
- Real patient/beneficiary data
- Clinical decision support
- Fraud detection
- HIPAA production certification
- FHIR server implementation
- Kubernetes
- Kafka
- Airflow
- dbt
- ML/AI models
- dashboards/web apps
- paid cloud infrastructure
- pretending a single-node local environment is a production Hadoop cluster

## Repository documents

- `PRD.md` - product requirements and success criteria
- `SRD.md` - system requirements/design
- `USER_STORIES.md` - user and engineering stories with acceptance criteria
- `TECH_STACK.md` - fixed technologies, roles, constraints, and version-validation protocol
- `AGENTS.md` - operating contract for Conductor/implementation agents
- `SKILLS_EVIDENCE.md` - conservative record of what was actually implemented/verified
- `docs/` - architecture, discovery, mappings, data quality, troubleshooting, interview notes
- `memory/` - durable project state separated by facts/findings/decisions/failures/learning/next step
- `research/INDUSTRY_RESEARCH.md` - researched basis for the product and architecture
- `planning/IMPLEMENTATION_PLAN.md` - rough execution plan and gates

## Current state

**Phase -1: NOT STARTED**

No implementation capability should be claimed yet.
