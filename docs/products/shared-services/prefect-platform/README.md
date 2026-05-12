# prefect-platform

Geomatic's self-hosted Prefect 3.x workflow orchestration platform. Schedules and executes all Python data pipelines — customer deliveries, ETL ingestion, reports, analytics — in isolated Docker containers.

## Summary

Self-hosted Prefect instance running on GEO-FREJA. Developers write flows in the Prefect repo; Azure Pipelines deploys them; Prefect workers on GEO-FREJA pick them up and run each flow in its own Docker container.

## Why It Matters

Every automated customer delivery and ETL pipeline runs through this platform. If the Prefect server or its workers go offline, all scheduled jobs stop silently. It is the operational backbone for everything in `customers/`, `etl/`, `conzoom_dk/`, and `reports/`.

## Key Repos

- [`Geomatic/Analytics/Prefect`](https://dev.azure.com/Geomatic/Analytics/_git/Prefect) — all workflow code (flows, ETL, customer deliveries, reports, utilities)
- [`Geomatic/Analytics/AnalyticsFoundation`](https://dev.azure.com/Geomatic/Analytics/_git/AnalyticsFoundation) — Docker base images (`geomatic.azurecr.io/prefect-base-image`)

## Main Components

- **Prefect server** — self-hosted, runs on GEO-FREJA in WSL, UI at http://prefect.geomatic.dk
- **Prefect workers** — two NSSM services on GEO-FREJA (dev + prod), polling work pools in WSL
- **Azure build agent** — self-hosted on GEO194, `Analytics` agent pool; runs linting, tests, and deployment for every pipeline.yml
- **Docker base image** — `geomatic.azurecr.io/prefect-base-image` from AnalyticsFoundation repo; versioned by tag
- **Shared deployment template** — `deployfiles/master-template.yml` in the Prefect repo; all per-flow `pipeline.yml` files reference this
- **File storage** — SMB shares mounted as Docker CIFS volumes: dev on geo-sif, prod on geo-heimdal
- **Code storage** — Azure Blob Storage, container `prefectv2`
- **Secrets** — Azure Key Vault via `Geomatic_DS_Tools.SecretHandler`; also mirrored as Prefect blocks

## Consumers

All products in the Prefect repo:

- `customers/` — 20+ customer delivery workflows
- `etl/` — source data ingestion (agriculture, DST, energy labels, etc.)
- `conzoom_dk/` — Conzoom delivery system
- `projects/` — POI distance, Lead Engine
- `reports/` — KPI geocoder, DK_ReportsG6

## Open Questions

- Who owns the Docker base image releases in AnalyticsFoundation? Is there a changelog or versioning policy?
- GEO-FREJA is a single point of failure for both the Prefect server and both workers — is there a failover plan?
- How long does the self-hosted Prefect server retain logs, and does the SQL Server backup via `utilities/prefectlogs/` cover everything we need?
- `prefect-warning` Slack webhook is configured but not in use — intentional?
- Nested virtualisation on GEO-FREJA is managed by ITM8 — who is the contact if WSL/Docker stops working?

## Sources

- Wiki: [Geo-Freja Analytics server / Prefect overview](https://dev.azure.com/Geomatic/Analytics/_wiki/wikis/Analytics.wiki/294)
- Wiki: [DS Databases](https://dev.azure.com/Geomatic/Analytics/_wiki/wikis/Analytics.wiki/755)
- Repo inspected: `Geomatic/Analytics/Prefect` — CLAUDE.md, deployfiles/master-template.yml, utilities/
