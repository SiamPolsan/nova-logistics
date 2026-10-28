# Nova Logistics Agent Instructions

These rules apply to this repository and to every person or AI agent working in it. They adapt the working practices of nova-sg-all (`AGENTS.md`, `docs/04-ai-development-workflow.md`, ADRs, feature plans and implementation logs). They do not import that product's decisions, credentials, deployment environments or historical exceptions.

## Start with current context

1. Read `README.md`, the requirements in `docs/01-product-and-mvp.md` and the decisions in `docs/05-roadmap-and-decisions.md`. For a rule, read `docs/business-rules.md`.
2. For implementation, read `docs/03-architecture.md`, `docs/04-ai-development-workflow.md`, the ADRs in `docs/adr/` and any instructions inside the affected subtree.
3. Inspect the actual files, `git status`, `git log` and the available checks before editing. Fetch first: other people and agents push to this repository. Preserve changes made by the user or another agent.

Moved (28 October 2026, D-30): new logistics planning and all code live in nova-sg-all (its D-158 and `docs/15-logistics-service-line.md`). Read this repository as the record of the planning baseline, and do not add new plans here.

Current status (28 October 2026): planning documents and the local service definition (`platform/compose.yaml`: MinIO and Redis) only. There is no backend, web app, API contract, migration, CI, deployment or customer data. Do not claim runtime functionality, test coverage, infrastructure, stack versions, deployment or legal compliance exists because a plan describes it. Mark unresolved decisions and do not silently turn them into production commitments.

## Requirements and decisions

- Follow the current user request and applicable higher-priority instructions. Within project documents, confirmed `D-*` decisions and accepted ADRs take precedence over proposals. Surface contradictions with evidence.
- Use the stable IDs: `D-*` confirmed decisions, `P-*` decisions still needed, `Q-*` questions for the owner, `MVP-*` scope items, `AC-*` acceptance criteria, `UC-*` use cases, `BR-*` business rules, `ADR-NNNN` architecture records.
- When a decision changes, update the canonical document in the same change: record why, the date, the source (who said it) and what it supersedes. Do not leave contradictory instructions behind.
- Every business rule belongs in `docs/business-rules.md`. When a change adds or alters a rule, a figure, a state or who may do something, add or correct its row there in the same change. Mark a rule the owner has not confirmed as `proposed`; put a choice for the owner among the open `P-*`/`Q-*` items with numbered options. A rule code is never reused.
- Use `docs/templates/feature-plan.md` for substantive behaviour. Keep small changes proportionate; do not create paperwork that repeats the code.
- TTP (ttp-project), SWP (swp-driver-go, swp-account-go) and nova-sg-all are reference systems. Read them for domain knowledge and patterns. Never copy their credentials, customer data, hard-coded rates, vendor codes or customer-specific templates into this repository.

## Work in small verified changes

- Read the owning module and its conventions first. Plan a complete user journey with normal, empty, permission-denied, failure and (for the driver app) poor-connection outcomes.
- Divide parallel work by files or module ownership. One primary agent owns integration in a session; communicate before editing the same file.
- Keep edits scoped. Do not revert unrelated changes, restructure unrelated code or copy a whole framework to reuse one behaviour.
- Branches: until P-12 is decided, work on a short-lived branch (`claude/<topic>` or `codex/<topic>`) and merge or push to `main` only when the owner asks in the session. The owner asked for the planning baseline to be pushed to `main` on 28 October 2026 (D-12).
- Use existing tools and commands after verifying they exist. Never invent command output or completion evidence.
- Before publishing, deploying, sending messages or changing shared data, establish authorization from the user's request in the session. Routine local edits and checks need no extra ceremony.

## Architecture boundaries

- Stack (D-09, ADR-0001): one Go backend as a modular monolith with `api`, `worker` and `migrate` commands from one codebase; one Next.js web app; MySQL 8.4; Redis as a cache; S3-compatible object storage for documents and photos (MinIO locally).
- Domain and application code must not depend on HTTP frameworks, provider SDKs or concrete persistence. Composition roots wire adapters to typed ports.
- A module owns its tables. Other modules reach it through its root-package interface, never by writing its tables. Cross-module references are IDs. A documented composed transaction may span modules for a synchronous invariant.
- Keep handlers and screens thin. Centralise authenticated API clients, validation, localisation (TH/EN), configuration, error mapping and safe telemetry.
- Redis is never the source of truth (ADR-0004). Business state, jobs, outbox, idempotency keys and audit live in MySQL. Every cache key starts with the tenant ID and every entry has a TTL.
- Files live in object storage under `t/{tenant_id}/…`, private, reached only through short-lived signed URLs issued after a fresh access check (ADR-0005). The database keeps keys and metadata, never public URLs.
- Tenant-defined fields (UDA): definitions belong to the Customization module; values and invariants stay with the business module, stored in its JSON extension column keyed by stable field IDs (ADR-0006, `docs/07-tenant-customization-uda.md`). No per-tenant DDL.
- Bulk work (imports, exports, ZIP uploads, expiry scans) runs in the worker as jobs with progress, row-level errors and an undo path (ADR-0007).
- A figure a tenant may set (alert thresholds, odometer jump limit, photo rules) lives in Settings with inheritance site → customer → branch → company → default. Never write such a figure as a constant in a business module; copy it onto any record whose meaning it decides.
- A module does not call Notifications directly: it puts what happened on the job queue in the same transaction, and a handler decides whom to tell. A notification names things by ID and holds no ID-card number, bank account or document image.
- Add services, brokers or infrastructure only for a demonstrated requirement. Do not build microservices because the reference systems have several services.

## Tenant and personal-data safety

- Derive identity, tenant membership and scope (branch, customer) on the server. Client-supplied IDs or headers never establish authorization.
- Apply tenant and scope checks to queries, writes, exports, imports, jobs, cache keys, signed URLs and ZIP downloads.
- Citizen ID, passport, bank account and criminal-record data are protected: encrypted at field level where stored, masked by role, kept out of logs, notification text, analytics and URLs. Criminal records are sensitive personal data under the PDPA (`docs/06-thailand-privacy-and-compliance.md`).
- Use synthetic data in tests, fixtures, screenshots and committed examples. Never commit credentials, ID-card images, real rider data, private endpoints or tokens.
- Sensitive reads and every change need attributable audit records. Verify current access before issuing a download.

## Correctness

- Persist authoritative state in MySQL 8.4. Use explicit state machines, optimistic `version` columns, stable command IDs, idempotency and a transactional outbox where required.
- Preserve original observations (odometer readings, inspection results, uploaded files). A correction is a separate record with a reason.
- Version schema changes as explicit migrations run by `migrate`; never mutate the schema on API start-up. The runtime database user cannot change the schema.
- Money in minor units with a currency; instants in UTC with the tenant's timezone for display; Buddhist-era years only at the display and import edges.

## Verification and completion

- For access changes, test another tenant, wrong branch or customer scope, revoked membership, guessed IDs and forbidden fields, including exports, imports and file downloads.
- For import/export, test Thai headers, BE and CE dates, duplicates, partial failure, re-import (upsert) and undo of a batch.
- For files and ZIP, test wrong-tenant keys, expired signed URLs, oversized and malformed archives and unmatched files.
- For the expiry engine, test boundary days, timezone midnight, renewals that supersede, and that each alert is sent once per threshold.
- Run the checks that exist for the change. Documentation-only edits need link, consistency and source checks; do not invent a test suite.
- Finish with what changed, the evidence actually collected, the checks not run, remaining limitations and consequential decisions. Write an implementation-log record for any substantive session (`docs/implementation-log/YYYY-MM-DD-<topic>.md`).
