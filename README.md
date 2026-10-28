# Nova Logistics

> **Moved to nova-sg-all on 28 October 2026 (D-30).** The owner chose one platform. Logistics is now planned, and will be built, inside nova-sg-all as apps each company switches on, with rider and driver supply and transport as separate service lines: see nova-sg-all's D-158 and its `docs/15-logistics-service-line.md`. This repository stays as the record of the planning baseline below. Its decisions are inputs there; new planning happens in nova-sg-all.

Nova Logistics is a planned multi-tenant SaaS for logistics companies in Thailand. Phase 1 serves sub-contractors that supply riders (2W) and drivers (4W) to retailers: rider and driver registration and records, documents with expiry alerts, vehicles (insurance, equipment, odometer, photos, inspection checklists, who drives what, owned vehicles) and fleet overview — worked like a spreadsheet, with Excel import/export, bulk edit, a bucket-style document browser and ZIP upload, and tenant-defined fields.

**Planning baseline: 28 October 2026.** This repository holds planning documents and the local service definition only. There is no backend, web app, API, migration, CI or deployment yet. Reference companies: TTP (Thaitaweeporn: Big C, 2W and 4W) and SWP (Siwaporn Express: ALL Now, 2W), internal pilot tenants (D-15).

## Direction

- **Stack (D-09):** Go modular monolith (`api`, `worker`, `migrate`), Next.js web with a driver PWA, MySQL 8.4, Redis as a cache, S3-compatible storage for documents and photos — [ADR-0001](docs/adr/0001-go-modular-monolith-and-nextjs.md) to [ADR-0005](docs/adr/0005-s3-object-storage.md).
- **Local (D-10):** Docker for MinIO and Redis; the developer's existing host MySQL 8.4.
- **Hosting (D-28):** NIPA Cloud in Thailand — k3s, managed MySQL 8.4 and S3 — as nova-sg-all uses.
- **Repository (D-12):** its own repository, adopting nova-sg-all's conventions and porting its platform patterns ([ADR-0002](docs/adr/0002-separate-repository-adopting-sg-all-conventions.md)).
- **Phases (D-02, D-03):** 1 core compliance and fleet (to May 2027, proposed); 2 check-in/out, scheduling, public API; 3 fuel, routes, orders, accounting/ERP adapters, then OMS and WMS.

## Planning documents

| Document | Contents |
| --- | --- |
| [AGENTS.md](AGENTS.md) | Rules for every person and AI agent |
| [01 Product and MVP](docs/01-product-and-mvp.md) | Segments, audiences, MVP-01 to MVP-25, journeys, pilot acceptance |
| [02 Competitor and gap analysis](docs/02-competitor-gap-analysis.md) | 24 tools compared, prices, best practices, gaps, differentiation |
| [03 Architecture](docs/03-architecture.md) | Modules, runtime, tenancy, files and ZIP, import/export, cache, integration boundary |
| [04 Human and AI working agreement](docs/04-ai-development-workflow.md) | Roles, decision flow, feature loop, session protocol, status words, verification by risk |
| [05 Roadmap and decisions](docs/05-roadmap-and-decisions.md) | Phases, sprint plan, D-* decisions, P-* decisions needed, Q-* questions |
| [06 Thailand privacy and compliance](docs/06-thailand-privacy-and-compliance.md) | PDPA roles, sensitive data, retention, Thai vehicle and driver documents |
| [07 Tenant customization (UDA)](docs/07-tenant-customization-uda.md) | Field types, storage, rules |
| [08 Open API and integrations](docs/08-open-api-and-integrations.md) | Events, public API, webhooks, accounting/ERP, telematics |
| [09 Environment workflow](docs/09-environment-workflow.md) | Local MySQL 8.4 + Docker MinIO/Redis; configuration contract |
| [10 Data model](docs/10-data-model.md) | Entities, standard fields, document types, lifecycles |
| [11 Migration from TTP and SWP](docs/11-migration-from-ttp-and-swp.md) | Sources, mapping, steps, reconciliation |
| [Business rules](docs/business-rules.md) | BR-* rules and figures |
| [ADRs](docs/adr/README.md) | Architecture decisions and status |
| [Feature plans](docs/features/README.md) | Planned Phase 1 feature breakdown and [template](docs/templates/feature-plan.md) |
| [Research](docs/research/) | Dated source summaries |
| [Implementation log](docs/implementation-log/) | What each session changed and verified |

## Local services

```bash
cp platform/.env.example platform/.env   # then set local-only values
docker compose --env-file platform/.env -f platform/compose.yaml up -d --wait
```

Starts MinIO (S3, private dev and test buckets) and Redis on loopback. MySQL 8.4 stays on the host; see [09](docs/09-environment-workflow.md) for the one-time database setup.

## Current boundaries

Nothing in the plans is built. Do not describe a planned behaviour, stack version, deployment or compliance as existing. Open choices are in [05](docs/05-roadmap-and-decisions.md#decisions-still-needed); questions for the owner in [05](docs/05-roadmap-and-decisions.md#questions-for-the-owner).
