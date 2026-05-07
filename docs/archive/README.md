# Archive

Things that are no longer active but are worth keeping for historical context. Archive instead of deleting — old context is often the answer to "why is this the way it is?"

## When to archive

- A product is deprecated or sunset.
- A decision has been superseded but the reasoning still informs current choices.
- Infrastructure has been replaced but legacy systems still influence migrations or contracts.
- A project has shipped and its working notes aren't useful day-to-day anymore.

## When *not* to archive

- The thing is still being iterated on, even slowly. Keep it in the live tree.
- The notes are wrong. Fix or delete; don't archive misinformation.

## Subfolders

| Folder | What goes here |
|---|---|
| `deprecated-products/` | Product folders moved out of `docs/products/` when a product is retired |
| `old-decisions/` | Superseded ADRs and decision logs |
| `legacy-infra/` | Notes on infrastructure that's been replaced but still informs migrations |
| `completed-projects/` | Working notes from finished projects |

## Moving things in

When archiving, move the whole folder rather than copying — git history preserves the original location. Add a one-line note at the top of the moved README explaining when and why it was archived.
