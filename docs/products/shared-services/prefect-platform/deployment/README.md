# deployment

## Pipeline mechanics

Every flow project has a `pipeline.yml` that references `deployfiles/master-template.yml` in the Prefect repo root. All deployment logic lives in the master template.

### Steps (master-template.yml)

1. Install `uv` package manager and recreate venv
2. **Black check** — hard gate; unformatted code fails the build immediately
3. Check for `tests/` folder
4. If tests exist: login to ACR → pull Docker image → run `python -m unittest` inside container (with CIFS volumes mounted)
5. Install deployment dependencies (`prefect==3.4.18`, `prefect-docker`, `prefect-azure`, etc.)
6. Create `.prefectignore`
7. Run `deployfiles/update_prefect_yaml.py` to generate `prefect.yaml`
8. `prefect deploy` to `http://prefect.geomatic.dk/api`
9. `git clean -xdf`

### Environment rules

| Branch | Environment | Schedule |
|---|---|---|
| `production` | `prod` | Applied as configured (cron or rrule) |
| Any other | `dev` | No schedule |

`base-latest` image tag is blocked in prod — the pipeline fails if detected.

## Work queues

| Queue | When used |
|---|---|
| `default` | All normal flows |
| `prod-limit-N` | Flows with `concurrency_limit` set (prod only) |

## Schedules

Two schedule types via `master-template.yml` parameters:

- `cron` — standard cron expression
- `rrule` — for complex recurrence (e.g. last weekday of month)

`day_or` parameter controls whether the cron day field means day-of-week or day-of-month.

## Key files

| File | Purpose |
|---|---|
| `deployfiles/master-template.yml` | Full shared pipeline definition |
| `deployfiles/update_prefect_yaml.py` | Generates `prefect.yaml` from pipeline parameters |
| `<project>/pipeline.yml` | Per-flow entry point; passes params to master template |
