# monitoring

## Prefect UI

`http://prefect.geomatic.dk` — view deployments, flow runs, work pool health, logs, and schedules.

## Alerts

| Alert | Mechanism | Condition |
|---|---|---|
| Flow failure | Slack webhook `prefect-slack-fail` | Flow enters failed state |
| Unhealthy work pool | Prefect automation | Work pool goes unhealthy |
| `prefect-warning` | Slack webhook | Configured but currently unused |

## Log persistence

Logs are stored in the self-hosted Prefect server's local database. The `utilities/prefectlogs/` flow additionally reads logs from the Prefect API and writes them to SQL Server for longer-term retention and easier querying.

## Common issues

| Symptom | First check |
|---|---|
| `prefect.geomatic.dk` unreachable | Remote into GEO-FREJA → check `http://localhost:4200`. If up, it is an Nginx issue: open WSL terminal → `service nginx restart` |
| Work pool red dot / flows not starting | GEO-FREJA → Services → find "Prefect selfhosted worker agent (DEV or PROD)" → Restart |
| Prefect server down | GEO-FREJA → Services → "Prefect Server" → Restart |

Full troubleshooting wiki: https://dev.azure.com/Geomatic/Analytics/_wiki/wikis/Analytics.wiki/774
