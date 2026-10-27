# ADR 0003: MySQL 8.4 with Shared-Schema Multi-Tenancy

Status: MySQL 8.4 **Accepted** 2026-10-28 (owner, D-09, D-10). Shared-schema tenancy **Proposed** (P-03).
Date: 2026-10-28
Decision owner: Nova Logistics engineering owner
Related: [architecture](../03-architecture.md#tenant-and-scope-isolation), [privacy](../06-thailand-privacy-and-compliance.md)

## Context

The product is sold to many companies (D-01), most of them small. Every reference system uses MySQL; the developer already runs MySQL 8.4 locally. MySQL 8.4 is the current LTS line, with JSON columns, multi-valued indexes, generated columns and window functions that UDA and grids need.

## Decision

- MySQL 8.4 LTS for every environment. One schema per environment (`nova_logistics`), `utf8mb4` and `utf8mb4_0900_ai_ci`, UTC session time zone.
- Proposed tenancy: one shared schema. Every tenant-owned table has `tenant_id BINARY(16)` and primary/foreign keys of the form `(tenant_id, id)`. IDs are UUIDv7 stored as `BINARY(16)`.
- The tenant comes only from the server-verified session or API credential. Repository methods take the tenant as a required parameter; there is no query path without it.
- Mandatory negative tests for every module: another tenant, a guessed ID, a revoked membership, exports and file keys.
- Separate database users: `nl_migrate` (DDL) and `nl_app` (DML only).
- An enterprise tenant may later get a dedicated database; no code assumes the shared one.

## Alternatives

| Alternative | Benefit | Why not |
| --- | --- | --- |
| Schema or database per tenant | Strong isolation, easy per-tenant restore | Hundreds of small tenants means hundreds of migrations and connection pools |
| PostgreSQL with row-level security | Database-enforced isolation, JSONB | The owner chose MySQL 8.4; the team and every reference system use MySQL |
| `company_code` string as in SWP | Already familiar | A filter, not isolation; not enforced from the token |

## Consequences

Cheap to run many small tenants. Isolation depends on code discipline and tests, so the repository layer and the test suite carry it. Noisy-neighbour risk for big imports is handled by worker lanes and per-tenant job limits.

## Acceptance and revisit criteria

Accept the tenancy model after the S1 spike shows the repository pattern and the negative-test helper. Revisit for a tenant above 50,000 drivers or a contractual requirement for dedicated storage.
