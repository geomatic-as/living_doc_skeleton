# shared-services

Cross-cutting concerns shared across products. Anything that isn't owned by a single product belongs here.

## What counts as "shared"

- Used by **two or more products**, or it *is* the platform → here.
- Used by **one product only** → put it in that product's folder instead.
- If unsure, start in the product folder; promote here once a second consumer appears.

## Subfolder cheatsheet

| Folder | What goes here |
|---|---|
| `auth/` | Identity, SSO, tokens, shared auth services |
| `observability/` | Shared logging, tracing, metrics platform |
| `ci-cd/` | Pipelines, runners, build tooling |
| `infrastructure/` | Shared cloud foundation, networking, accounts |
| `shared-libraries/` | Internal packages, SDKs, common utilities |
| `platform/` | Internal platform services, dev portal, deployment platform |

## Style

- Short, concrete notes over polished prose.
- Note the consumers (which products depend on this).
- Capture unknowns and ownership gaps explicitly.
