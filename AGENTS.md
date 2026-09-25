# AGENTS.md - Operating Contract

## Mission

Build the CMS Medicare Utilization Data Platform to completion while maximizing correctness, reproducibility, owner understanding, and truthful evidence. Do not optimize for fastest code generation.

## Required startup sequence

At the start of every session:
1. Read `README.md`.
2. Read `PRD.md`.
3. Read `SRD.md`.
4. Read `TECH_STACK.md`.
5. Read `memory/STATE.md` and `memory/NEXT.md`.
6. Read only the relevant findings/decisions/failures needed for the current task.
7. Inspect repository/runtime state before assuming implementation exists.

## Self-research requirement

Before implementing a phase, independently research the relevant domain/technology using authoritative primary sources whenever possible. The repository's `research/INDUSTRY_RESEARCH.md` is seed research, not unquestionable truth.

Cross-check:
- CMS source documentation and data dictionaries;
- Apache official docs/release notes;
- credible healthcare reference architectures/standards when architecture claims are made;
- local compatibility constraints.

Record meaningful new research in `research/AGENT_RESEARCH_LOG.md` with date, question, source, finding, implication, and confidence. Never copy large copyrighted passages.

## Fixed stack

Core technology families are fixed: Hadoop/HDFS, HDFS CLI, Bash, Hive/HiveQL, Spark/PySpark/Spark SQL, Parquet, Impala, Python, Git/GitHub, local zero-cost containerized runtime.

Exact versions are not fixed until compatibility research. Propose a version matrix before environment setup.

Do not silently replace a fixed technology family. If a necessary implementation change is discovered, first present:
- problem/evidence;
- current plan;
- proposed change;
- alternatives considered;
- impact on scope/learning/reproducibility;
- recommendation.

Then wait for owner confirmation for material architecture/stack changes. Minor code fixes that preserve approved design do not need approval.

## Problem before technology

Every component/feature must map to a real requirement or approved learning constraint. Never fabricate a business requirement merely to exercise a tool.

## No fabricated data findings

Never claim duplicates, null problems, orphan claims, invalid dates, schema drift, unusual reimbursement values, or other anomalies until actual profiling proves them. Mark hypotheses as hypotheses.

## Evidence states

Use these states consistently:
- PLANNED
- IN_PROGRESS
- IMPLEMENTED
- VERIFIED

Generated code is not IMPLEMENTED until executed successfully. Installation is not skill evidence. VERIFIED requires inspected/tested evidence.

## Human learning loop

For every major component, explain:
1. What it is.
2. Why enterprises use it.
3. What problem it solves here.
4. Where it sits in the architecture.
5. What command/code is being executed.
6. Expected behavior.
7. How to verify it.
8. Common failure modes.
9. Connection to Hadoop/Spark/data-engineering concepts.
10. How the owner can explain it in an interview.

For core manual-learning tasks (especially HDFS CLI, Hive operations, Impala operations), stop and ask the owner to execute the task and return output before automating or advancing past the learning gate.

## Work loop

Within an approved phase:
1. Research if needed.
2. Explain proposed unit of work.
3. Implement a small coherent increment.
4. Test/verify it.
5. Explain result/failure.
6. Update project memory/evidence/docs.
7. Decide whether the next step is safe to continue autonomously or requires owner execution/approval.
8. Repeat until phase exit criteria are met.

Do not jump phases simply because later code can be generated.

## Architecture/change control

The fixed stack does not mean every physical detail is fixed. Storage paths, schemas, partition keys, quality dispositions, service topology, and implementation ordering are evidence-driven.

Material changes require a decision record before implementation. Never silently drift architecture.

## Data integrity rules

- Preserve raw data.
- No silent record loss.
- Explain cardinality changes.
- Explicit schemas after discovery.
- Source-to-target mapping for curated fields.
- Data quality is part of the pipeline, not an end-stage decoration.
- Critical failed quality/reconciliation gates prevent trusted publication.

## Scope rules

Do not add Kafka, Airflow, Kubernetes, dbt, ML, dashboards, FHIR implementation, cloud services, or unrelated frameworks without explicit owner approval. Prefer Future Work over scope inflation.

## Repository truth hierarchy

1. Runtime/data evidence
2. Authoritative CMS/Apache documentation
3. Approved PRD/SRD/decisions
4. Implementation
5. README/resume claims

Documentation must conform to reality, never the reverse.

## Required checkpoint report

At learning/approval checkpoints report:

### What we are doing
### Why it matters
### Architecture position
### What changed
### What the owner should execute/review
### Expected result
### Verification
### Common failure modes
### Concept connection
### Interview explanation
### Evidence/memory updates
### Proposed next step

Then stop if owner action/approval is required.

## Phase order

-1 Problem & Data Discovery
0A Requirements & Architecture
0B Environment
1 HDFS Foundation
2 HDFS Operations
3 Hive
4 PySpark Integration
5 Spark SQL
6 Curated Data Product
7 Data Quality
8 Impala / Consumption
9 End-to-End Reproducibility
10 GitHub Engineering Record
11 Interview & Evidence Review

## Current state

Phase -1 NOT STARTED. No implementation claims exist.
