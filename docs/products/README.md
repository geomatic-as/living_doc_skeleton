# Products

This is where per-product documentation lives. Each product has its own folder with a `README.md` and a standard set of subfolders.

## Starter templates

- [`product-a/`](product-a/README.md) — copy this to start a new product folder.
- [`product-b/`](product-b/README.md) — same shape; use either.

Both are intentionally generic. To onboard a real product, copy a template, rename the folder, and fill in the README.

## Shared concerns

- [`shared-services/`](shared-services/README.md) — anything that spans multiple products: auth, observability, ci-cd, shared infrastructure, shared libraries, internal platform.

## Product vs shared-services — quick rule

- Used by **one product only** → put it in that product's folder.
- Used by **two or more products**, or it *is* the platform → put it in `shared-services/`.
- If unsure, put it under the product that owns it today and link from `shared-services/` later.

## Subfolder layout (per product)

Every product folder has the same 16 subfolders. One-line description of each:

| Folder | What goes here |
|---|---|
| `architecture/` | System shape, component boundaries, data flow |
| `backend/` | Services, APIs, workers, batch jobs |
| `frontend/` | UI apps, design system, client-side concerns |
| `infrastructure/` | Cloud resources, networking, IaC, capacity |
| `database/` | Schemas, migrations, query patterns, data ownership |
| `integrations/` | Third-party systems, partner APIs, contracts |
| `deployment/` | Release process, environments, rollout / rollback |
| `monitoring/` | Dashboards, alerts, SLOs, log queries |
| `testing/` | Test strategy, coverage gaps, fixtures |
| `security/` | Threat model, secrets, auth flows specific to this product |
| `decisions/` | ADRs and decision logs |
| `diagrams/` | Exported images and source files (drawio, mermaid, etc.) |
| `meeting-notes/` | Discussions, syncs, design reviews |
| `roadmap/` | Upcoming work, milestones, dependencies |
| `incidents/` | Postmortems and near-misses for this product |
| `misc/` | Holding pen for things that don't fit yet — move out when a home appears |
