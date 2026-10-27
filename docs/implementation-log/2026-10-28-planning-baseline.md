# 2026-10-28 Planning baseline

## What changed

- Created the repository's planning baseline: `README.md`, `AGENTS.md` (+ `CLAUDE.md` pointer), documents 01–11, business rules, ADRs 0001–0008, the feature-plan template and planned feature list, a research note on the reference systems, the pull-request template, `.gitignore` and `.gitattributes`.
- Added `platform/compose.yaml` (MinIO, bucket bootstrap, Redis) and `platform/.env.example`.

## Why

The owner asked (28 October 2026) to combine TTP's and SWP's rider and fleet practice into one multi-tenant logistics product, with a competitor and gap analysis, a Phase 1 plan, a roadmap and open questions; then to set the stack (Go modular monolith, Next.js, Redis, S3, MySQL 8.4; local Docker with MinIO and the existing host MySQL 8.4), to adopt nova-sg-all's human/AI way of working as Markdown, and to store everything here and push it to `main` (D-01 to D-13). Later the same day the owner added registration to rider and driver management (D-14, MVP-24, BR-DRV-07 to BR-DRV-09).

## Evidence and sources

- Reference systems read by agents from local checkouts of ttp-project, swp-driver-go, swp-account-go and nova-sg-all ([research note](../research/2026-10-28-reference-systems.md)).
- Competitor facts gathered by a research agent from vendor and third-party pages in October 2026 (links in [02](../02-competitor-gap-analysis.md)); not every price was independently re-opened by the primary agent.
- A shared Claude document with the same analysis was also prepared during the session; this repository is the canonical copy.

## Verification

- `docker compose --env-file platform/.env.example -f platform/compose.yaml config --quiet` passed (exit 0). The services were not started, to avoid clashing with nova-sg-all's MinIO on port 9000; start them with your own `platform/.env`.
- Relative Markdown links checked by script: 89 checked, 0 broken. No runtime code exists, so no tests were run.

## Decisions and remaining work

- Needed before S1: P-01, P-02, P-03, P-05, P-12, P-17, P-18 and answers to Q-02 to Q-06 and Q-12 ([05](../05-roadmap-and-decisions.md)).
- ADRs 0003 (tenancy part), 0004 (usage rules), 0006, 0007, 0008 are proposed and need spikes in S1–S2.
- Credentials committed in the reference repositories should be rotated before migration work starts.

## Deployment

None.
