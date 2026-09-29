# Project documentation

This documentation expands the [project README](../README.md). Start there to install and run the application, or read the pages below for the design and experimental record.

| Page | Contents |
|---|---|
| [Project background](project-background.md) | Reply assignment, LUISS course context, objectives, and contribution |
| [Architecture](architecture.md) | Components, contracts, agent runtime, prompts, configuration, and repository layout |
| [Validation](validation.md) | Ingestion, schema, completeness, consistency, anomaly, cross-column, and duplicate checks |
| [Cleaning and reporting](cleaning-and-reporting.md) | Remediation, cleaning requests, generation, application, verification, and reporting |
| [Experiments and results](experiments-and-results.md) | Development iterations and historical cached-run measurements |
| [Limitations and future work](limitations-and-future-work.md) | Failure modes, limits of the approach, and planned improvements |

The [presentation PDF](../ReplyProject.pdf) remains at the repository root. Experimental figures and example artifacts are retained from the original project report; they are not new measurements of the current checkout.

## Editing and Wiki publication

Edit documentation in `docs/` and update images in `images/`. Keep the README focused on the project introduction, architecture, and startup instructions. Check relative links whenever moving a section or image, and distinguish historical findings from current implementation behavior.

The intended publication model is one-way synchronization from `docs/` to the GitHub Wiki when documentation reaches the repository's default branch. This automation is not active yet. Wiki availability, existing pages, and publishing credentials must be checked before enabling it. Once enabled, direct edits to generated Wiki pages may be overwritten; changes should be made here instead.
