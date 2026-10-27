# ADR 0002: Separate Repository Adopting nova-sg-all Conventions

Status: **Accepted** 2026-10-28 (the owner created `SiamPolsan/nova-logistics` and asked that all planning live there, D-12). Whether code is later shared with nova-sg-all is open: P-01.
Date: 2026-10-28
Decision owner: Nova Logistics product owner
Related: [working agreement](../04-ai-development-workflow.md), [architecture](../03-architecture.md)

## Context

nova-sg-all (security-guard SaaS) already has what Phase 1 needs underneath: tenants and memberships, roles, audit, settings inheritance, a MySQL job queue/outbox, private object storage and an OpenAPI contract. Its own plan (`docs/13-transport-service-line.md`, D-142) proposed transport as a third service line inside it. The owner instead created a separate repository for logistics and asked for nova-sg-all's way of working.

## Decision

- Nova Logistics is its own repository, product, release cycle and database.
- It adopts nova-sg-all's conventions: `AGENTS.md`, numbered planning documents, ADRs, decisions log, feature plans, implementation logs, module layout, verification-by-risk.
- Platform patterns (tenancy, membership context, audit, settings inheritance, job queue/outbox, S3 adapter) are ported by reading nova-sg-all and rewriting for this product, with a provenance note in the implementation log. Nothing is imported as a runtime dependency and no file is copied with another product's names, IDs or settings.

## Alternatives

| Alternative | Benefit | Why not now |
| --- | --- | --- |
| Transport service line inside nova-sg-all | Reuse everything, including Phase 2 scheduling and attendance | Couples logistics releases to the guard pilot; different buyers and pricing; the owner chose a separate repository |
| Shared Go platform module used by both | One fix lands in both products | Needs a stable interface both teams agree on; premature before Phase 1 shows which parts really match |
| Start from scratch with no reference | No inherited assumptions | Throws away proven patterns and the agent workflow |

## Consequences

Independent releases and branding. Fixes to ported platform code must be carried across by hand until P-01 is decided. Phase 2 scheduling and attendance will be ported or rebuilt rather than reused directly.

## Acceptance and revisit criteria

Revisit after Phase 1 GA (proposed May 2027): if more than half of the platform core is identical in both repositories, propose a shared module ADR.
