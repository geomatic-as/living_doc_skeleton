# architecture

## System overview

Three stages: local development → Azure Pipelines → Prefect runtime.

```
Developer (local)
  └─ writes flow code in Prefect repo
  └─ runs black formatter, commits, merges to branch

Azure Pipelines (build agent on GEO194)
  └─ triggered by push to any branch
  └─ runs black check (hard gate)
  └─ if tests/ exists: runs unittest inside the Docker container
  └─ generates prefect.yaml deployment file via deployfiles/update_prefect_yaml.py
  └─ deploys to Prefect API at http://prefect.geomatic.dk/api

Prefect runtime (GEO-FREJA)
  └─ Prefect server: stores deployments, schedules, run history
  └─ Workers (dev + prod): poll work pools, spin up one Docker container per run
  └─ Docker container: pulls base image from ACR, mounts file storage, runs flow
```

## Environment split

Branch `production` → `prod` worker + prod file storage (geo-heimdal). All other branches → `dev` worker + dev file storage (geo-sif). Schedules are only registered in prod — dev deployments are deployed but get no cron/rrule.

## Key dependencies

- All flows depend on `Geomatic_DS_Tools` (internal package, Azure Artifacts feed). Python 3.11 required — 3.13 does not have it installed.
- Secrets resolved at runtime via `Geomatic_DS_Tools.SecretHandler` → Azure Key Vault (or Prefect secret blocks).
- File I/O goes through `DATA_FOLDER` env var, which points to the SMB share mounted as a Docker CIFS volume.

## Deployment naming

The deployment name registered in Prefect is auto-generated: `{parent}.{child1}.{child2}.{env}` (e.g. `customers.Codan.DK_Ejendomsdata.prod`).

Overriding `deployname` in `pipeline.yml` changes only the name shown in the Prefect UI (and used by automations or external triggers). It does **not** affect `blobpath` — code is always stored at `{env}/{parent}/{child1}/{child2}` in Azure Blob Storage regardless of what `deployname` is set to. A rename in Prefect will leave the old deployment name orphaned unless it is manually deleted from the UI.

## Related

- [deployment/](../deployment/) — pipeline steps and scheduling details
- [infrastructure/](../infrastructure/) — server and storage specifics
