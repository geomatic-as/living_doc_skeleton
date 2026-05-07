# Docs

Top-level documentation index.

## Where things live

- [`products/`](products/README.md) — per-product documentation. Start here for anything tied to a specific product.
- [`products/shared-services/`](products/shared-services/README.md) — platform concerns shared across products (auth, observability, ci-cd, infrastructure, shared libraries).
- [`archive/`](archive/README.md) — deprecated products, old decisions, legacy infra, completed projects. Things kept for history but no longer iterated on.

## Adding a new product

1. Copy `products/product-a/` (or `product-b/`) to `products/<your-product>/`.
2. Rename and fill in the top of the new `README.md`.
3. Drop notes into the relevant subfolders as you learn things.

See the [products README](products/README.md) for more.
