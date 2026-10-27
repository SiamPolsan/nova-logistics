# Feature: <name>

Status: proposed / ready / implementing / built / verified / released
Owner: <person or team>
Module and surfaces: <owning module; office web / driver PWA / platform team>
Source: <MVP-* item, D-*, accepted ADR, issue>
Updated: <YYYY-MM-DD>

## Outcome and scope

Describe the actor, problem and observable result. List included behaviour and explicit exclusions. Separate confirmed requirements from assumptions; link material open `P-*`/`Q-*` items and name who can resolve them.

## Use case and acceptance

| ID | Requirement or observable result | Evidence |
| --- | --- | --- |
| UC-<MOD>-001 | Actor, preconditions, trigger and main flow | <linked MVP-* item> |
| AC-<MOD>-001 | Specific expected result | <test or manual evidence; initially planned> |
| AC-<MOD>-002 | Failure, retried, concurrent or unauthorised action | <test or manual evidence; initially planned> |

Describe alternate flows, state transitions, timezone and BE/CE dates, bulk (import/export) behaviour and corrected records where relevant. Do not mark a criterion passed if its wording differs from the built behaviour.

## Authorization and contract

- Audience and operation: <route, job or UI action; OpenAPI change or none>
- Verified actor scope: <tenant, role, branch, customer, self (driver)>
- Allowed fields: <protected data (citizen ID, bank, criminal record) and how it is masked>
- Revocation and replay: <session, membership, signed URL, re-submitted import>
- Compatibility: <affected clients and rollout order>

## Data and side effects

- Owner and necessity: <tables, records, authoritative vs derived fields, UDA extension>
- Queries and volume: <predicates, pagination, expected rows per tenant, indexes>
- Transaction: <invariant, lock or version rule; composed transaction participants if any>
- Idempotency and async work: <command IDs, jobs, retries, outbox events>
- Cache: <keys (tenant-prefixed), TTL, invalidation; or none>
- Files: <object keys, size limits, retention; or none>
- Audit and privacy: <events, readers, encryption, retention, logging>
- Migration and recovery: <expand/backfill/contract, forward repair, restore>

Write `not applicable` with a reason for unaffected areas. Do not invent volume, legal basis or retention periods to fill the template.

## Implementation and verification

List the smallest implementation steps and owned files. For UI, record components and tokens, loading/empty/error states, viewports (360 px, 1440 px), light and dark.

| Check | Result and evidence | Limitation |
| --- | --- | --- |
| <test, build or device scenario> | planned / passed / failed / not run; <path or record> | <what remains unproven> |

Use synthetic fixtures. Include cross-tenant negatives when access changes and real-MySQL evidence when persistence changes.

## Completion record

- Built: <what changed and why; code and contract links>
- Verified: <claims supported by executed evidence>
- Decisions and remaining gaps: <owner, next action, due point>
- Deployment: <migration, configuration, provider, storage, rollout, smoke checks, or none>
- Recovery: <rollback or forward repair; irreversible effects>
- Canonical documents updated: <links>
- Business rules updated: <BR-* codes added or changed, or none>
- Implementation log: <link>
