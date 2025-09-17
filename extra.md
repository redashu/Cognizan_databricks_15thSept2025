# Examples of Extra Layers

| Layer | Purpose | Example Use Case |
|-------|---------|-----------------|
| Landing / Raw | Direct dump from source, no schema enforcement. Often short-lived. | Copy raw JSON/CSV from API or Kafka before ingesting with Auto Loader. |
| Bronze | Ingested, lightly structured raw data. | Store events from Kafka with metadata. |
| Silver | Cleansed, validated, enriched. | Deduplication, joins with reference data. |
| Gold | Business-ready marts/aggregates. | Sales KPIs, dashboards, ML features. |
| Platinum (optional) | Highly aggregated, domain- or product-specific. | Executive dashboards with rolled-up metrics. |
| Sandbox / Lab | For analysts or data scientists to experiment. | Feature engineering, hypothesis testing. |

## Why create new layers?

- **Governance & Compliance** – Sometimes raw dumps must be stored separately from curated data (e.g., regulatory needs).

- **Team Separation** – Different teams may own different stages (ingestion vs data quality vs analytics).

- **Complex Pipelines** – Some domains (finance, healthcare, IoT) need more than 3 hops to get from raw to business-ready.

- **Reusability** – Intermediate layers can serve multiple downstream pipelines