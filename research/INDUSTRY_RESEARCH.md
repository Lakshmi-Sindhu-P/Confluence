# Seed Industry Research

Research date: 2026-09-25

This is seed research for the implementation agent. It is not a substitute for the agent's own current research.

## 1. CMS validates the basic project use case

CMS says Medicare Claims Synthetic Public Use Files were created so interested parties can gain familiarity with Medicare claims data while protecting beneficiary privacy. CMS states the structures are similar to CMS Limited Data Sets and that developers can create programs/products against the synthetic formats. CMS also cautions that synthetic data has limited inferential research value.

Official source:
https://www.cms.gov/data-research/statistics-trends-and-reports/medicare-claims-synthetic-public-use-files

DE-SynPUF page:
https://www.cms.gov/data-research/statistics-trends-and-reports/medicare-claims-synthetic-public-use-files/cms-2008-2010-data-entrepreneurs-synthetic-public-use-file-de-synpuf

Sample 1 download page:
https://www.cms.gov/data-research/statistics-trends-and-reports/medicare-claims-synthetic-public-use-files/cms-2008-2010-data-entrepreneurs-synthetic-public-use-file-de-synpuf/de10-sample-1

Verified high-level facts from CMS:
- five source types: Beneficiary Summary, Inpatient Claims, Outpatient Claims, Carrier Claims, Prescription Drug Events;
- beneficiary summary unit is beneficiary; inpatient/outpatient units are claims;
- CMS provides user documentation/codebook and sample downloads;
- files are synthetic/public-use, not real beneficiary claims.

## 2. Industry workflow shape is real

AWS's healthcare payer reference architecture describes patterns including:
- ingestion from heterogeneous healthcare/payer sources;
- raw source-format landing;
- ETL processing;
- standardization and data-quality rules;
- referential-integrity checks;
- metadata/business terms/lineage;
- curated/purpose-built data stores;
- analytical data products and SQL analysis.

Official reference:
https://docs.aws.amazon.com/reference-architecture-diagrams/latest/healthcare-payor-strategic-focus-areas/healthcare-payor-strategic-focus-areas.html

AWS Healthcare Industry Lens also recommends durable raw storage, schema discovery/cataloging, transformation/normalization, lineage, and SQL analytics as representative healthcare analytics patterns:
https://docs.aws.amazon.com/wellarchitected/latest/healthcare-industry-lens/healthcare-analytics-reference-architecture.html

These references validate the **workflow pattern**, not the exact local Apache implementation. Our project deliberately maps enterprise concepts to a small open-source local stack.

## 3. Why the project is not a fake 'technology checklist'

The product requirement exists before the tools: heterogeneous beneficiary/claims files need preservation, interpretation, mapping, validation, integration, curation, and reusable analytical access. HDFS/Hive/Spark/Impala are the fixed implementation/learning stack used to realize those requirements locally.

## 4. Modern healthcare interoperability context

Modern healthcare architectures commonly include FHIR and other healthcare interchange standards. AWS's interoperability architecture describes ingesting/parsing/standardizing data into HL7 FHIR. This project intentionally does **not** implement FHIR because its purpose is batch claims integration and Hadoop/Spark skill-building; FHIR belongs in Future Work/industry context, not forced scope.

Reference:
https://docs.aws.amazon.com/reference-architecture-diagrams/latest/healthcare-interoperability-stack/healthcare-interoperability-stack.html

## 5. Apache technology research still required

The seed research confirms Apache Spark has active 4.x releases as of 2026, but this does **not** mean the newest Spark version is automatically compatible with the easiest local Hive/Impala stack. The agent must research a mutually compatible matrix before selecting versions.

Apache Spark releases:
https://spark.apache.org/releases/

The agent must independently consult official Hadoop, Hive, Spark, and Impala docs/release notes before Phase 0B.

## 6. Research integrity rules

- Prefer official CMS/Apache documentation for factual implementation claims.
- Use cloud reference architectures only as evidence of industry patterns, not as instructions to add paid cloud services.
- Never infer source-data anomalies from generic healthcare-data expectations.
- Re-check time-sensitive version/compatibility claims at implementation time.
