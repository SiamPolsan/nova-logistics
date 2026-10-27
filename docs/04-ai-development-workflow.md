# Human and AI Agent Working Agreement

**Baseline: 28 October 2026 (D-11).** This is how people and AI agents build Nova Logistics together. It adapts the workflow proven in nova-sg-all (`docs/04-ai-development-workflow.md`, `AGENTS.md`, its ADRs, feature plans and implementation logs) to this product. Recommendations stay proposals until a decision is recorded in [05-roadmap-and-decisions.md](05-roadmap-and-decisions.md).

## Roles

| Role | Who | Owns | Never does alone |
| --- | --- | --- | --- |
| Owner | Product owner (human) | Scope, priorities, answers to `P-*`/`Q-*`, acceptance of ADRs, approval to deploy, publish or contact customers, the weekly real-device review | — |
| Engineer | Human developers | Review of diffs in their area, pairing with agents, production access, incident response | Accept an ADR or change scope without the owner |
| Primary agent | The AI agent running a session (Claude Code, Codex) | Integration of the session's work, keeping docs in step, verification, the implementation-log record, commit and push when asked | Deploy, publish, message customers, change shared data, accept a decision |
| Sub-agent | Agents the primary agent starts for research, implementation or review | Only the files or question it was given | Edit files outside its brief; report unverified work as done |

One primary agent integrates per session. Parallel agents split work by module or file and never edit the same file at once.

## Sources of truth

| Location | Purpose |
| --- | --- |
| `AGENTS.md` | Rules every person and agent follows |
| `README.md` | What the product is, current status, where things are |
| `docs/01-product-and-mvp.md` | Scope with `MVP-*` and acceptance `AC-*` IDs |
| `docs/02-competitor-gap-analysis.md` | Sourced competitor facts and the gap analysis |
| `docs/03-architecture.md` | Modules, runtime, tenancy, files, cache, integration boundary |
| `docs/05-roadmap-and-decisions.md` | Roadmap, sprint plan, `D-*` confirmed decisions, `P-*` decisions needed, `Q-*` questions |
| `docs/business-rules.md` | Every business rule with an example, status and source |
| `docs/adr/NNNN-*.md` | Architecture decisions with status proposed / accepted / superseded |
| `docs/features/<module>/<feature>.md` | One feature: actors, UC/AC, access, data, evidence ([template](templates/feature-plan.md)) |
| `docs/implementation-log/YYYY-MM-DD-<topic>.md` | What a session changed, decided and verified, and what remains |
| `docs/research/` | Dated source summaries with links and limitations |

When prose and code disagree, record the drift: say which is observed behaviour and which is intended policy. A passing unit test or a plan is not proof of production behaviour.

## How a decision is made

1. **Raise it.** Anyone (human or agent) adds a `P-*` (a choice with a working proposal) or `Q-*` (a fact only the owner has) to [05](05-roadmap-and-decisions.md), with numbered options, the working proposal, its impact and when it is needed.
2. **Keep working.** Independent research, reversible spikes and other features continue. Nothing that depends on the answer is built as production behaviour.
3. **Owner answers.** In chat, a review comment or a meeting. The agent quotes the answer and its date.
4. **Record it.** The `P-*`/`Q-*` row is marked answered and points to a new `D-*` row (date, source, what it supersedes). Architectural choices also update the ADR's status.
5. **Carry it through.** Every canonical document, business rule and feature plan that the answer changes is updated in the same change.

## One feature at a time

1. **Understand the outcome.** Actor, business result, owning module, affected surfaces (office web, driver PWA). Read nearby code and tests. Separate confirmed facts from assumptions.
2. **Plan a bounded change.** Copy [the template](templates/feature-plan.md) to `docs/features/<module>/<feature>.md`. Give stable IDs (`UC-DOC-001`, `AC-DOC-001`) linked to `MVP-*`. Define unhappy paths, tenant/scope access and what "done" looks like. For screens, draw them first and show the owner.
3. **Resolve material decisions.** Commercial rules, sensitive-data purposes, destructive transitions and provider choices are settled before they become production behaviour.
4. **Implement within ownership.** Thin handlers and screens; rules in domain/application code; contracts, migrations and settings changed together.
5. **Verify the claim.** Run the checks for the risk (table below). Compare a suspicious failure with the starting state before touching unrelated code. A pre-existing failure stays a recorded limitation.
6. **Review and record.** Read the diff for regressions, privacy leaks and missing failure paths. Update the feature's evidence, business rules and canonical docs. Write the implementation-log record.

## A session, step by step

**Start**

- `git fetch` and `git status`; read `AGENTS.md`, `README.md`, [05](05-roadmap-and-decisions.md) and the feature plan you are working on.
- Restate the goal and the plan in a few lines. If the request conflicts with a `D-*` or an accepted ADR, say so with the evidence before editing.

**During**

- Small scoped changes. Report progress in plain words.
- New questions go into [05](05-roadmap-and-decisions.md) as `P-*`/`Q-*`, not only into chat.
- Never put secrets or real rider data into prompts, fixtures, screenshots, logs or commits. Reference secret names and synthetic people instead.

**End**

- Run the checks; record results honestly, including what was not run.
- Write `docs/implementation-log/YYYY-MM-DD-<topic>.md`: what changed, why, evidence, decisions, remaining work, deployment note (or "none").
- Commit with a message that says what and why. Push or merge only as the branch policy and the owner's request allow.
- Final message: built / verified / not verified / decisions needed, with links.

## Status words

Use these exact words for any claim; never upgrade a claim without evidence.

| Word | Means |
| --- | --- |
| Planned | Described in a document; nothing built |
| Built | Code exists on a branch |
| Verified locally | Checks ran and passed against local services (host MySQL 8.4, Docker MinIO and Redis) |
| Verified on UAT | Checks ran and passed in the shared test environment |
| Released | Deployed to production with health and smoke checks passing |
| Field-validated | Used by a pilot tenant for real work and accepted by the owner |

## Asking an agent for work

Short requests work best when they name the ID and the outcome. Examples:

- "Plan `MVP-07` expiry alerts as a feature plan. Do not code yet; list the `P-*` questions it raises."
- "Implement `AC-DOC-003` and `AC-DOC-004` on a branch. Run the backend checks. Write the implementation log."
- "Review the branch `claude/import-engine` against `docs/03-architecture.md` and the import rules `BR-IMP-*`. Report defects only."
- "Research how Fleetio handles renewal reminders. Add sources to `docs/research/` and update `docs/02-competitor-gap-analysis.md`."
- "Answer to P-07: option 2. Record it and carry it through the docs."

## Verification by risk

| Change | Minimum meaningful evidence |
| --- | --- |
| Documentation or presentation detail | Links, consistency, rendering; no artificial test |
| Domain policy or state transition | Unit/application tests for allowed and forbidden transitions, time boundaries and corrections |
| Database or query | Real MySQL 8.4 migration and integration tests on a disposable database; query plans at representative volume (10,000 drivers, 50,000 documents per tenant) |
| API | OpenAPI contract check, error mapping, explicit audience, consumer compatibility |
| Tenant, access, privacy | Cross-tenant and cross-scope negative tests; masked fields; exports, imports, jobs, cache keys and signed URLs isolated |
| Import/export | Thai headers, BE/CE dates, duplicates, partial failure, re-import as upsert, undo of a batch, 50,000-row file |
| Files and ZIP | Wrong-tenant keys, expired URLs, oversized or malformed archives, zip bombs, unmatched files, MinIO round trip |
| Expiry engine and alerts | Boundary days, timezone midnight, renewal supersedes, one alert per threshold, retry without duplicates |
| UDA | Type validation, required-later, archive keeps values, import/export columns, grid filter on a JSON field |
| Cache | Hit and miss paths, invalidation on write, tenant prefix, Redis down = still correct (slower) |
| Important UI journey | Component tests plus browser evidence at 360 px and 1440 px, light and dark; the driver PWA on a real Android phone |
| Deployment/storage | Rendered configuration, migration order, health, secret handling, backup and restore rehearsal, rollback or forward repair |

## Definition of done

A change is done for its stated scope when its acceptance criteria are met, relevant checks pass, code, contract, migrations and docs agree, and review found no unresolved material defect. Remaining uncertainty is written down with an owner. Every completion record answers: what changed, why, how it was checked, which checks were not run, and what remains outside the claim. A pilot acceptance is separate from code completion.

## Reuse ledger

| Reference | Adopted | Excluded |
| --- | --- | --- |
| nova-sg-all `AGENTS.md`, `docs/04-ai-development-workflow.md`, `docs/templates/feature-plan.md`, `.github/pull_request_template.md`, `platform/compose.yaml` (read 28 October 2026) | Decision traceability, module ownership, status words, verification by risk, feature template, MinIO bootstrap pattern, host-MySQL rule | Guard-specific rules, its D-numbers, NIPA settings, Firebase/Codemagic setup, its Thai HTML rule page format |
| ttp-project (Big C riders) | Document catalogue, alert thresholds, 2W/4W checklists, Thai import header aliases | Code, credentials, `bigc_*` table design, customer data |
| swp-driver-go, swp-account-go (ALL Now riders) | Versioned document upload, review queue, vehicle-type code list, lessons on hard-coded rates and missing period close | Code, credentials, citizen-ID primary key, customer templates, vendor codes |

Copying reference files verbatim would import obsolete assumptions, private identifiers and commands that do not exist here.
