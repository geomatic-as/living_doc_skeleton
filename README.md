# Living Docs Skeleton

A lightweight, structured workspace for living documentation across multiple products.

The goal is not to produce perfect documentation in one go. The goal is to give each pod a place to capture the basics quickly:

- what the product is
- which repos matter
- how the system roughly hangs together
- what is painful, risky, or unclear
- what needs follow-up

## How To Use This Repo

Each product gets its own folder under [`docs/products/`](docs/products/README.md), with a starter `README.md` and 16 standard subfolders for different kinds of notes (architecture, backend, decisions, incidents, etc.).

Two starter products — `product-a` and `product-b` — are included as templates. Copy one, rename it, and start filling it in.

Cross-cutting concerns that span multiple products live under [`docs/products/shared-services/`](docs/products/shared-services/README.md) (auth, observability, ci-cd, etc.).

When something is no longer active but worth keeping, move it into [`docs/archive/`](docs/archive/README.md) instead of deleting.

## Suggested Contribution Style

- Prefer short, concrete notes over polished prose.
- Distinguish fact from inference where it matters.
- Capture unknowns explicitly instead of guessing.
- Prioritise orientation value over completeness.
- Add the obvious high-value facts first; deepen later.

## Tree

```
docs/
├── products/
│   ├── product-a/        ← starter template (copy this)
│   ├── product-b/        ← starter template (copy this)
│   └── shared-services/  ← cross-product concerns
└── archive/              ← deprecated / completed / legacy
```

See [`docs/README.md`](docs/README.md) for a fuller index.
