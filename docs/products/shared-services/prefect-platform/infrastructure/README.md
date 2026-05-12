# infrastructure

## Servers

| Server | Role | Notes |
|---|---|---|
| GEO-FREJA | Prefect server + dev/prod workers | Windows Server 2022; Prefect runs in WSL2 (required for Linux Docker) |
| GEO194 | Azure build agent | Self-hosted; `Analytics` agent pool; runs as `GEODOM\Buildmaster` |
| geo-sif | Dev file storage | SMB share `\\geo-sif\prefect_storage` (IP: 77.66.89.77) |
| geo-heimdal | Prod file storage | SMB share `\\geo-heimdal\prefect_storage` (IP: 77.66.89.85) |
| 77.66.89.66 | Static reference data | SMB share `\\77.66.89.66\geomatic` |

## GEO-FREJA details

WSL2 is required because Windows Server 2022 only supports Windows-based Docker containers, but Prefect flow containers are Linux-based. Nested virtualisation is enabled by ITM8.

- Prefect server runs as a Windows service; exposed via Nginx reverse proxy at `http://prefect.geomatic.dk` (localhost:4200 inside GEO-FREJA)
- Two worker services managed by NSSM, running bash scripts in WSL:
  - `Prefect selfhosted worker agent (DEV)` → work pool `dev`
  - `Prefect selfhosted worker agent (PROD)` → work pool `prod`
- Worker logs: `D:\logs\Worker dev\` and `D:\logs\Worker prod\`
- Prefect Python environment: `/home/prefect-machine/.prefect_env` (in WSL)

## Azure resources

| Resource | Purpose |
|---|---|
| Azure Container Registry (`geomatic.azurecr.io`) | Docker base images |
| Azure Blob Storage (container: `prefectv2`) | Flow code packages pushed during deployment |
| Azure Key Vault | Secrets and connection strings for all flows |
| Azure DevOps variable group `pipeline_lib_dev` | Pipeline credentials (client ID, secret, tenant, taskrunner creds) |

## Docker volumes on GEO-FREJA

CIFS volumes created manually with taskrunner credentials (stored in LastPass):

| Volume | Mounts |
|---|---|
| `prefectdev` | `//77.66.89.77/prefect_storage` |
| `prefectprod` | `//77.66.89.85/prefect_storage` |
| `prefectstaticfolder` | `//77.66.89.66/geomatic` |

These volumes are passed into each flow container at runtime via the dev/prod container blocks in the Prefect UI.
