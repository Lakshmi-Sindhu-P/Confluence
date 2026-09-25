# Architecture

## Proposed logical architecture

CMS synthetic source -> local landing/provenance -> HDFS raw -> Hive external metadata -> profiling -> PySpark standardization -> Parquet standardized -> Spark integration/reconciliation -> Parquet curated -> Hive metadata -> Impala analytical SQL.

Technology families are fixed. Physical topology, versions, schemas, partitions, and detailed DQ policies are pending evidence/research.

## Design principles
- raw preservation;
- explicit contracts/mappings;
- evidence-driven transformation;
- no silent loss;
- quality gates;
- reusable analytical product;
- SQL consumption;
- local zero-cost reproducibility;
- minimal infrastructure.
