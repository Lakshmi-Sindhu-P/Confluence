# Fixed Tools & Technologies

## Core rule

Technology families below are fixed for the core project. Exact versions are intentionally **TBD pending compatibility research**; inventing version compatibility would be worse than leaving it open. The agent must propose a pinned version matrix before Phase 0B and obtain owner approval.

## Core stack

| Technology | Fixed? | Project role | What it teaches/proves |
|---|---|---|---|
| Apache Hadoop | Yes | Hadoop ecosystem foundation | Hadoop concepts and local ecosystem integration |
| HDFS | Yes | Raw/standardized/curated distributed filesystem | Namespace, filesystem lifecycle, storage semantics |
| `hdfs dfs` CLI | Yes | Manual filesystem operation | Real command-line HDFS handling |
| Bash | Yes | CLI workflow and later lightweight orchestration | Operational fluency and repeatability |
| Apache Hive | Yes | Metadata/table abstraction over HDFS | External/managed semantics, schemas, partitions, metadata |
| HiveQL | Yes | SQL DDL/DML/validation | Hadoop SQL interaction |
| Apache Spark | Yes | Distributed processing engine | Execution/transformation concepts |
| PySpark | Yes | Main transformation/integration implementation | Explicit schemas, joins, ETL, DQ, writes |
| Spark SQL | Yes | SQL transformations/validation | SQL/DataFrame interoperability |
| Apache Parquet | Yes | Standardized/curated columnar storage | Analytical storage and interoperability |
| Apache Impala | Yes | Interactive SQL consumption | Low-latency analytical querying over Hadoop data |
| Python | Yes | Tests/utilities + PySpark language | Engineering/test support |
| Git/GitHub | Yes | Versioning/portfolio packaging | Reproducibility and engineering history |
| Local container runtime + Compose-compatible workflow | Yes at capability level | Reproducible local services | Environment reproducibility without paid cloud |

## Explicitly not core

- Scala - deferred; only add after core completion if meaningful.
- Hue - optional future convenience; never a substitute for CLI.
- FHIR - industry-context/future extension, not core implementation.
- Kafka, Airflow, Kubernetes, dbt, ML, dashboards - out of scope.

## Cost/licensing posture

Core project must cost $0 to reproduce. Apache Hadoop, Hive, Spark, Impala, and Parquet are Apache open-source projects. Python and Git are open source. CMS DE-SynPUF is public-use synthetic data, not "open-source software"; its CMS disclaimer/user agreement and redistribution terms must be reviewed before deciding what raw/sample data can be committed.

Container runtime selection must be zero-cost for this personal project. If Docker licensing/runtime terms are undesirable, a compatible open-source alternative such as Podman may be evaluated without changing the data-engineering stack.

## Version-selection protocol

Before environment implementation, research:

1. current supported Apache Hadoop releases;
2. Hive compatibility with Hadoop/Java/metastore database;
3. Spark compatibility with Java/Python/Hadoop client libraries;
4. Impala's supported/local Docker or development topology and dependencies;
5. Hive Metastore interoperability required for Impala/Spark/Hive;
6. CPU architecture constraints of the owner's machine;
7. container image provenance and maintenance status.

Produce `docs/VERSION_MATRIX.md` containing:
- component;
- proposed version;
- source/reference;
- compatibility evidence;
- image/build source;
- architecture support;
- known limitations;
- decision status.

Do not use random abandoned Docker images merely because they start quickly.

## Change control

A version change within a fixed technology family may be proposed as an implementation change with evidence. Replacing Hive, Spark, HDFS, or Impala with a different technology requires explicit owner approval because it changes the project's learning contract.
