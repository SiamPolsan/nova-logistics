# ADR 0001: Go Modular Monolith with a Next.js Web App

Status: **Accepted** 2026-10-28 by the owner ("architect based go-lang and next.js with modular monolith", D-09)
Date: 2026-10-28
Decision owner: Nova Logistics product and engineering owners
Related: [architecture](../03-architecture.md), [environment workflow](../09-environment-workflow.md), [ADR-0003](0003-mysql-84-and-shared-schema-tenancy.md), [ADR-0004](0004-redis-as-cache-only.md), [ADR-0005](0005-s3-object-storage.md)

## Context

Phase 1 serves small and medium logistics sub-contractors, starting with TTP (Big C, 2W and 4W) and SWP (ALL Now, 2W). The two reference companies run today on four separate services in two languages (Express/TypeScript for TTP; two Go/Gin services for SWP) that share no tenancy model. nova-sg-all runs a Go modular monolith with API, worker and migrate commands, a Next.js web app and strict module boundaries, and it works for a small team building with AI agents. Team size, traffic peaks and budget for Nova Logistics are not yet known (Q-04, Q-12).

## Decision

- One Go backend, one repository, one release, with three commands: `api` (HTTP), `worker` (jobs, imports, ZIP processing, expiry scans, notifications) and `migrate` (explicit schema migrations). An `operator` command for the platform team may follow.
- Bounded modules under `backend/internal/modules/<module>` with `domain`, `application`, `adapters` and `transport` layers. A module owns its tables and exposes a root-package interface; others call that interface. An architecture test enforces the import rules, as nova-sg-all's `archtest` does.
- HTTP router: `chi` (as nova-sg-all) unless a spike shows a reason otherwise; plain SQL with `database/sql` and the MySQL driver; no ORM.
- One OpenAPI 3.1 contract in `contracts/openapi/` is the source for the web client's generated types (`openapi-typescript`, `openapi-fetch`). A route and its contract change together.
- One Next.js web app in `apps/web` (App Router, TypeScript) serves the office and, as a PWA, the driver self-service screens ([ADR-0008](0008-driver-self-service-pwa.md)).

## Alternatives

| Alternative | Benefit | Why not |
| --- | --- | --- |
| Extend swp-driver-go or ttp-be | Real users and data now | Single-company designs; tenancy would be retrofitted into every table and query; hard-coded customers and rates |
| Microservices | Independent scaling and releases | More contracts, credentials and partial failures before any scale is demonstrated |
| Unstructured monolith | Fastest start | Shared table access makes tenant isolation and later change hard to verify |
| Nuxt/Vue web (prime) | Existing Vue/PrimeVue experience in the team | The owner chose Next.js; it matches nova-sg-all |

## Consequences

Most invariants commit in one transaction; deployment is simple; modules leave an extraction path. API and worker share a database failure boundary. Composed cross-module transactions are deliberate coupling and must be documented. One web codebase means office and driver screens release together.

## Acceptance and revisit criteria

Revisit a module boundary for measured independent scale, a new security boundary or separate team ownership. Before launch, demonstrate tenant isolation tests, migration recovery and a worker crash/reclaim test.
