# Architecture Decision Records

An ADR records one material architecture choice: its context, the decision, alternatives, consequences and when to revisit it. Status is one of **Proposed**, **Accepted** (with date and who accepted it) or **Superseded by ADR-NNNN**. An agent never changes a status on its own; the owner accepts (see [the working agreement](../04-ai-development-workflow.md#how-a-decision-is-made)).

| ADR | Title | Status |
| --- | --- | --- |
| [0001](0001-go-modular-monolith-and-nextjs.md) | Go modular monolith with a Next.js web app | Accepted 2026-10-28 (owner, D-09) |
| [0002](0002-separate-repository-adopting-sg-all-conventions.md) | Separate repository adopting nova-sg-all conventions | Superseded 2026-10-28 by D-30: one platform inside nova-sg-all (its D-158); P-01 answered with option 3 |
| [0003](0003-mysql-84-and-shared-schema-tenancy.md) | MySQL 8.4 with shared-schema multi-tenancy | MySQL 8.4 accepted (D-09); tenancy model proposed |
| [0004](0004-redis-as-cache-only.md) | Redis as a cache, never the source of truth | Accepted 2026-10-28 (Redis, D-09); usage rules proposed |
| [0005](0005-s3-object-storage.md) | S3-compatible object storage, MinIO locally | Accepted 2026-10-28 (D-09, D-10) |
| [0006](0006-uda-json-extension.md) | Tenant-defined fields as a JSON extension owned by each module | Proposed |
| [0007](0007-spreadsheet-grade-bulk-data.md) | Spreadsheet-grade grid and asynchronous import/export engine | Proposed |
| [0008](0008-driver-self-service-pwa.md) | Driver self-service as a Next.js PWA in Phase 1 | Proposed |

New ADRs copy the structure of an existing one: title, status, date, decision owner, related links, then Context, Decision, Alternatives, Consequences, Acceptance and revisit criteria.
