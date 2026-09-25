# Rough Implementation Plan

This is a controlled plan, not permission to implement every phase without owner checkpoints.

## Phase -1 - Problem & Data Discovery
Deliverables:
- source inventory;
- CMS provenance/terms notes;
- selected sample/subset rationale;
- profiling plan;
- initial data dictionary notes;
- evidence-backed findings only.
Exit gate: owner reviews actual data observations.

## Phase 0A - Requirements & Architecture
Deliverables:
- finalized initial data-product grain;
- data contracts;
- source-to-target mapping framework;
- HDFS zone/path decision;
- external/managed Hive decision;
- DQ disposition policy;
- partitioning hypothesis;
- ADRs.
Exit gate: owner approves architecture decisions.

## Phase 0B - Environment
Deliverables:
- independent compatibility research;
- `docs/VERSION_MATRIX.md` proposal;
- owner approval;
- container/Compose files;
- service health checks;
- resource notes/troubleshooting.
Exit gate: each required service verified individually; no data pipeline yet.

## Phase 1 - HDFS Foundation
Teach/implement HDFS architecture, namespace, directories, ownership/lifecycle model.
Exit gate: owner can explain NameNode/DataNode and project storage zones.

## Phase 2 - HDFS Operations
Owner manually executes required `hdfs dfs` commands on project/sample data. Then add repeatable scripts.
Exit gate: manual evidence recorded.

## Phase 3 - Hive
Create project DB/raw external tables, controlled managed-vs-external experiment, schemas, validation queries, partitions only if justified.
Exit gate: owner can explain metadata vs underlying HDFS data and run core HiveQL.

## Phase 4 - PySpark Integration
Build explicit schemas and source-specific standardization; mapping; joins; evidence-driven null/duplicate handling; partition-aware Parquet writes.
Exit gate: standardized/integrated outputs verified with reconciliation.

## Phase 5 - Spark SQL
Implement selected SQL transformations/validation and explain DataFrame-vs-SQL choice.
Exit gate: queries verified and owner understands execution role.

## Phase 6 - Curated Data Product
Build `beneficiary_year_utilization`; validate grain, mappings, measures, and traceability.
Exit gate: product acceptance criteria pass.

## Phase 7 - Data Quality
Formalize executable DQ framework and stage gates based on actual findings/contracts.
Exit gate: critical failures demonstrably block trusted publication; results recorded.

## Phase 8 - Impala / Consumption
Register/refresh metadata as needed; execute representative analytical queries; explain Impala/Hive/Spark roles.
Exit gate: owner manually queries curated data and can explain the flow.

## Phase 9 - End-to-End Reproducibility
Create orchestration Bash scripts after manual understanding; idempotent/restartable workflow; clean-environment smoke test.
Exit gate: documented end-to-end run succeeds.

## Phase 10 - GitHub Engineering Record
Polish README using only verified facts; architecture diagram; mappings; DQ findings; troubleshooting; limitations; setup.
Exit gate: another developer can reproduce without hidden steps.

## Phase 11 - Interview & Evidence Review
Review skills ledger; create truthful talking points/resume bullets; run architecture/debugging questions.
Exit gate: owner can explain full source-to-query lifecycle without relying on generated prose.

## Change-management rule

If the agent discovers a necessary deviation, it must first provide: evidence/problem, proposed change, alternatives, impact, and recommendation. Material changes wait for owner confirmation. Once approved, update ADR/plan/memory before implementation.
