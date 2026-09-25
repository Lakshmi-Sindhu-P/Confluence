# User Stories & Acceptance Criteria

## Epic A - Source trust and discovery

### US-A1 - Record authoritative source provenance
**As a data engineer, I want each input tied to an authoritative CMS source so that the pipeline is reproducible and auditable.**

Acceptance criteria:
- CMS source page is recorded.
- Dataset/sample/file name and years are recorded.
- Usage/disclaimer/redistribution considerations are reviewed.
- No unofficial mirror is treated as authoritative when CMS is available.

### US-A2 - Preserve raw inputs
**As a maintainer, I want original files preserved unchanged so that every transformation can be reproduced from source.**

Acceptance criteria:
- raw layer is immutable by convention and implementation;
- derived jobs write elsewhere;
- a rerun does not mutate raw content.

### US-A3 - Profile before transforming
**As a data engineer, I want to measure the real source characteristics before defining cleaning rules so that transformations respond to evidence rather than assumptions.**

Acceptance criteria:
- row counts, schemas, candidate keys, null rates, duplicate candidates, date/value ranges, and linkage coverage are measured where relevant;
- findings cite commands/queries/output;
- no anomaly is documented as fact without evidence.

## Epic B - Hadoop/HDFS operation

### US-B1 - Operate the HDFS namespace manually
**As the project owner, I want to manually create, inspect, upload, download, measure, and remove HDFS objects so that I understand the filesystem rather than only running automation.**

Acceptance criteria:
- owner executes required `hdfs dfs` commands;
- expected output is verified;
- learning ledger is updated conservatively.

### US-B2 - Organize data by lifecycle stage
**As an engineer, I want raw, standardized, and curated data separated so that source preservation and derived-data ownership are clear.**

Acceptance criteria:
- HDFS hierarchy is documented;
- raw is not overwritten;
- paths are deterministic.

## Epic C - Hive metadata and SQL

### US-C1 - Query raw HDFS files through external tables
**As an engineer, I want raw source files represented as Hive external tables so that I can inspect/query them without giving Hive ownership of source files.**

Acceptance criteria:
- external table DDL exists and runs;
- table schema is validated against source documentation/data;
- dropping a controlled test external table does not delete its source data;
- owner can explain external vs managed semantics.

### US-C2 - Use HiveQL for validation and exploration
**As an engineer, I want SQL queries over raw/curated tables so that schema and aggregate checks are easy to reproduce.**

Acceptance criteria:
- joins/aggregations/validation queries exist where justified;
- results are documented;
- no query is added merely as a keyword demonstration.

## Epic D - Standardization and mapping

### US-D1 - Define explicit schemas
**As a maintainer, I want stable source schemas explicitly encoded so that unexpected schema drift does not silently change processing.**

Acceptance criteria:
- stable PySpark ingestion uses explicit schemas;
- discovery-only inference is not mistaken for the contract;
- incompatible changes fail visibly.

### US-D2 - Maintain source-to-target mappings
**As an engineer, I want every curated field mapped back to source so that lineage and transformation logic are understandable.**

Acceptance criteria:
- each target field has mapping entry;
- transformation and rationale are documented;
- mapping status distinguishes proposed vs verified.

### US-D3 - Standardize source-specific records
**As an analyst, I want consistent types and field semantics so that downstream queries do not repeatedly clean the same raw inputs.**

Acceptance criteria:
- only evidence-supported standardizations are implemented;
- standardized outputs are Parquet;
- row-count changes are explained.

## Epic E - Integration and data quality

### US-E1 - Link claims to beneficiaries
**As an analyst, I want beneficiary and claim records integrated using verified keys so that utilization can be analyzed at beneficiary-year grain.**

Acceptance criteria:
- linkage key is supported by CMS docs and profiling;
- join cardinality is measured;
- unmatched records are quantified and handled explicitly;
- no silent loss.

### US-E2 - Detect quality failures
**As a maintainer, I want executable quality rules so that invalid outputs are not published silently.**

Acceptance criteria:
- rules have IDs/severity/expected conditions;
- results are machine-readable or consistently structured;
- critical failures block curated publication.

### US-E3 - Reconcile transformations
**As an engineer, I want stage-level counts and important aggregates reconciled so that I can explain where records/values changed.**

Acceptance criteria:
- pre/post counts captured for cardinality-changing operations;
- aggregate checks are defined where meaningful;
- unexplained differences fail review.

## Epic F - Curated analytical product

### US-F1 - Build beneficiary-year utilization
**As a healthcare data analyst, I want one validated annual beneficiary utilization dataset so that common utilization questions can be answered without rebuilding claims integration.**

Acceptance criteria:
- grain is explicitly tested;
- fields are fully mapped;
- product passes required DQ gates;
- output is partitioned only when justified;
- output is stored as Parquet.

### US-F2 - Query curated data interactively
**As an analyst, I want Impala SQL access to curated data so that I can explore utilization without editing Spark code.**

Acceptance criteria:
- curated table is visible to Impala;
- metadata refresh workflow is understood;
- representative filters, aggregations, and joins run successfully;
- owner can explain Impala's role vs Spark/Hive.

## Epic G - Reproducibility and operation

### US-G1 - Reproduce the local environment
**As a new developer, I want documented zero-cost setup so that I can recreate the platform locally.**

Acceptance criteria:
- compatible versions are pinned;
- startup/verification steps are documented;
- no paid service is required.

### US-G2 - Run the pipeline repeatedly
**As a maintainer, I want deterministic scripts after manual learning so that repeated execution does not require remembering every command.**

Acceptance criteria:
- scripts orchestrate already-understood steps;
- rerun does not duplicate derived data;
- failures are visible.

## Epic H - Learning and truthful evidence

### US-H1 - Understand before automation
**As the project owner, I want explanations and manual checkpoints so that I can personally defend the implementation in an interview.**

Acceptance criteria:
- every major new component receives concept/purpose/command/expected result/verification/failure modes/interview explanation;
- agent stops for owner execution where specified.

### US-H2 - Maintain truthful skills evidence
**As the project owner, I want verified implementation evidence separated from plans so that future resume claims do not exaggerate my experience.**

Acceptance criteria:
- installation alone never counts as skill evidence;
- IMPLEMENTED requires execution;
- VERIFIED requires inspected/tested output;
- professional/production claims are prohibited.
