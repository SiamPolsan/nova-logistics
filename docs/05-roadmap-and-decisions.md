# Nova Logistics Roadmap and Decisions

Phase 1 proves that a sub-contractor can keep every rider, document and vehicle compliant without spreadsheets, and that a new small company can start in a day. **Status: proposed plan, 28 October 2026.** Dates assume the team in "Planning estimate" and are not a committed launch date.

## Phases

| Phase | Window (proposed) | Scope | Exit evidence |
| --- | --- | --- | --- |
| 0: Discovery | 2–13 Nov 2026 | Answers to the blocking `Q-*`; sample sheets and document scans from TTP and SWP; P-01, P-02, P-07 decided; screens drawn for the first journeys | Owner signs off scope and the first screens |
| 1: Core MVP | 16 Nov 2026 – 26 Mar 2027 | MVP-01 to MVP-24 ([01](01-product-and-mvp.md)) | All Phase 1 `AC-*` verified locally and on UAT |
| 1: Pilot | 29 Mar – 30 Apr 2027 | TTP and SWP migrated and running in parallel with their current systems | AC-P-01 to AC-P-07 |
| 1: GA | May 2027 | Self-serve sign-up open; first external XS/S tenants | 5 paying external tenants |
| 2: Operations | Jun – Sep 2027 | Check-in/out with GPS and selfie, shift schedules, leave and cover (patterns from nova-sg-all), hirer portal (P-16), public API v1 and webhooks, recruiting and training if chosen (Q-10, Q-11), Google Sheets connector | A pilot tenant runs a month of shifts and attendance in the product |
| 3: Logistics | Q4 2027 – 2028 | Fuel logs and fuel cards, maintenance depth, telematics port (Cartrack, DTC), route optimisation, orders for sub-contract and owned-logistics modes, accounting/ERP adapters; OMS and WMS after | Trips and fuel for one owned-fleet tenant |

## Phase 1 sprint plan (two-week sprints)

| Sprint | Dates | Deliverables | MVP |
| --- | --- | --- | --- |
| S0 | 2–13 Nov 2026 | Discovery, decisions P-01/P-02/P-07, screen drawings, repository scaffold, local stack, CI skeleton | — |
| S1 | 16–27 Nov | Tenancy, sign-up, memberships, roles, branch scope, audit, job queue/outbox, S3 and Redis adapters, i18n shell, driver sign-in spike (PWA, LINE) | 01, 04, 22 |
| S2 | 30 Nov – 11 Dec | Settings, Customization (UDA) and field settings, customers and sites, grid component spike and choice | 02, 20, 21, 18 |
| S3 | 14–25 Dec | Driver records, lifecycle, duplicates, profile tabs, self-registration link/QR and review, grid inline and bulk edit, saved views | 03, 24, 18 |
| — | 28 Dec – 1 Jan 2027 | New Year break | — |
| S4 | 4–15 Jan 2027 | Import/export engine with mapping, dry run, upsert, undo; export any view | 19 |
| S5 | 18–29 Jan | Document types, versioned upload, review queue, compliance matrix, optional AI extraction | 05, 06, 08 |
| S6 | 1–12 Feb | Expiry engine, alerts (in-app, email, LINE OA), digest; document browser; ZIP upload and download | 07, 09, 10 |
| S7 | 15–26 Feb | Vehicles, ownership, insurance and tax, assignments, odometer, equipment | 11, 12, 13, 14 |
| S8 | 1–12 Mar | Inspection templates, photo capture in the PWA, defects, service reminders | 15, 16 |
| S9 | 15–26 Mar | Fleet overview and reports, subscriptions, performance and security review, restore rehearsal | 17, 23 |
| S10 | 29 Mar – 9 Apr | TTP and SWP migration, UAT, training | Pilot |
| — | 12–16 Apr | Songkran | — |
| S11 | 19–30 Apr | Parallel run, fixes, go/no-go | Pilot |

## Planning estimate

Assumed team: owner/PM, 2 Go engineers, 2 Next.js engineers, 1 QA, a part-time designer and part-time DevOps, with AI agents doing a large share of implementation under review, as in nova-sg-all. With half that team, Phase 1 takes roughly twice as long; confirm in Q-12.

## Success measures

- Onboarding: median time from sign-up to first compliance view under one working day (XS/S).
- Compliance: share of drivers fully compliant rises and days-expired falls month on month for pilot tenants.
- Bulk work: at least 70% of driver records in pilot tenants created or updated by import or grid paste in month one.
- Commercial: 5 paying external tenants within 60 days of GA.

## Confirmed decisions

| ID | Decision | Date and source |
| --- | --- | --- |
| D-01 | A multi-tenant logistics SaaS sold to many companies; Thailand first; XS, S and SME companies | 2026-10-28, owner brief |
| D-02 | Phase 1 scope: rider/driver management; documents with expiry alerts; vehicles with insurance, equipment, status and mileage, photos around the vehicle, checklists (e.g. electrical), who drives it, owned vehicles; fleet management | 2026-10-28, owner brief |
| D-03 | Phase 2: check-in/out and operations scheduling (reference nova-sg-all). Phase 3: fuel consumption, route optimisation, orders for sub-contract and owned-logistics modes. OMS, WMS and open API in later phases | 2026-10-28, owner brief |
| D-04 | Work like Excel/Sheets: import and export, bulk upload and bulk edit; a document browser like a GCS bucket; ZIP and bulk file upload | 2026-10-28, owner brief |
| D-05 | Support 2W and 4W; sub-contract mode first, owned-logistics mode later | 2026-10-28, owner brief |
| D-06 | Tenants add, edit and remove their own fields (UDA) with types free text, date, number, checkbox and radio, each required or optional | 2026-10-28, owner brief |
| D-07 | Keep a balanced set of standard fields; the rest is UDA | 2026-10-28, owner brief |
| D-08 | Follow global logistics/TMS practice; accounting and ERP are reached by integration (swp-account-go is a reference, not a design) | 2026-10-28, owner brief |
| D-09 | Go backend as a modular monolith, Next.js web, MySQL 8.4, Redis cache, documents in S3 | 2026-10-28, owner ([ADR-0001](adr/0001-go-modular-monolith-and-nextjs.md), [0003](adr/0003-mysql-84-and-shared-schema-tenancy.md), [0004](adr/0004-redis-as-cache-only.md), [0005](adr/0005-s3-object-storage.md)) |
| D-10 | Local development in Docker with MinIO for S3; MySQL 8.4 is the developer's existing host server, not in Compose | 2026-10-28, owner |
| D-11 | Human and AI agents work the nova-sg-all way, written as Markdown: AGENTS.md, ADRs, decisions log, feature plans, implementation logs | 2026-10-28, owner ([04](04-ai-development-workflow.md)) |
| D-12 | All planning lives in `SiamPolsan/nova-logistics`; the baseline was pushed to `main` at the owner's request | 2026-10-28, owner ([ADR-0002](adr/0002-separate-repository-adopting-sg-all-conventions.md)) |
| D-13 | Reference companies: TTP (Thaitaweeporn; Big C; 2W and 4W; sub-contract) and SWP (Siwaporn Express; ALL Now; 2W; sub-contract) | 2026-10-28, owner brief |
| D-14 | Rider and driver management includes registration: applicant self-registration and its review are Phase 1 (MVP-24); campaigns, lead rotation and kanban stay open in Q-10 | 2026-10-28, owner ("Rider and driver management also registration") |

## Decisions still needed

Working proposals let independent work continue; nothing that depends on them becomes production behaviour until answered.

| ID | Decision | Options | Working proposal | Needed before |
| --- | --- | --- | --- | --- |
| P-01 | Code relationship with nova-sg-all | 1. Port patterns, no shared code. 2. Shared Go platform module. 3. Build inside nova-sg-all | 1 now; review after GA (ADR-0002) | S1 |
| P-02 | Cloud and runtime | 1. GCP (where TTP/SWP run, Cloud Run + Cloud SQL + GCS S3 interop). 2. NIPA k3s + NIPA MySQL/S3 (as nova-sg-all). 3. AWS | Decide on hosting cost and any data-residency demand from Q-14; keep S3-compatible to stay portable | S2 (UAT by S4) |
| P-03 | Tenancy enforcement | 1. Shared schema + app checks + negative tests. 2. Add DB views per tenant role | 1 (ADR-0003) | S1 |
| P-04 | Driver app | 1. Next.js PWA. 2. Flutter app | 1 for Phase 1; decide native before Phase 2 (ADR-0008) | S1 spike |
| P-05 | Driver sign-in | 1. LINE Login. 2. SMS OTP. 3. Both | 3: LINE first, SMS fallback | S1 |
| P-06 | LINE Official Account model | 1. Each tenant connects its own OA. 2. One platform OA. 3. Both | 3: tenant OA when connected, platform OA otherwise | S6 |
| P-07 | Grid library | 1. Glide Data Grid (MIT). 2. AG Grid Community (range paste and fill handle are Enterprise only). 3. AG Grid Enterprise (paid). 4. TanStack Table + own editing | 1, after an S2 spike (ADR-0007) | S2 |
| P-08 | AI extraction of document number and expiry | 1. Gemini (TTP). 2. Cloud Vision OCR (SWP). 3. None in Phase 1 | Optional behind a port; measure cost per page in S5 | S5 |
| P-09 | Pricing in THB | Tiers by active drivers | Free ≤ 10 drivers; Starter flat price ≤ 50; Growth per active driver; Enterprise quote. Validate numbers with Q-16 | GA |
| P-10 | Billing unit | 1. Active driver. 2. Vehicle. 3. Higher of the two | 1, vehicles included | GA |
| P-11 | Migration scope for TTP and SWP | History depth, resigned riders, old document versions | Active + resigned in the last 12 months; current and previous document version; parallel run 2 weeks ([11](11-migration-from-ttp-and-swp.md)) | S9 |
| P-12 | Branch and merge policy | 1. Short-lived branches, agent fast-forwards verified work into `main` and pushes when asked (as nova-sg-all D-21). 2. Pull requests with human review before merge | 1 for docs and small changes, 2 for schema and access changes | S0 |
| P-13 | Default alert thresholds | 1. 60/30/15/7/3 days (TTP today). 2. 90/30/7 (TTP design doc) | 1, per document type, tenant-editable | S6 |
| P-14 | Effect of an expired blocking document | 1. Flag "not compliant" only. 2. Auto-suspend the driver or vehicle | 1 by default; 2 as a tenant option | S6 |
| P-15 | Retention per document type and after a driver leaves | Legal input | Keep 1 year after the engagement ends, then delete files; keep audit records | S6 |
| P-16 | Hirer view (Big C, ALL Now see their sub-contractors' compliance) | 1. Phase 2 portal. 2. Phase 1 export pack only | 2 now, 1 in Phase 2 | S9 |
| P-17 | Key management for field encryption | Cloud KMS of P-02; local development key | Follows P-02 | S1 |
| P-18 | Design system | 1. Own tokens and components drawn first (as nova-sg-all). 2. Off-the-shelf kit (shadcn/ui) themed | 2 with own tokens; draw key screens before coding | S0 |
| P-19 | Malware scanning of uploads | 1. ClamAV in the worker. 2. Cloud provider scanning. 3. None in Phase 1 | 1 | S6 |

## Questions for the owner

| ID | Question | Why it matters |
| --- | --- | --- |
| Q-01 | Which legal entity owns and sells Nova Logistics, and who are the first external pilot customers after TTP and SWP? | Contracts, DPA, pricing tests |
| Q-02 | Where are TTP's 4W system (d4w) code and data, and is it migrated in Phase 1? | Migration scope; 4W fields |
| Q-03 | Are TTP, SWP and EPL separate tenants (SWP data carries `company_code` SWP and EPL)? | Tenancy and migration |
| Q-04 | How many riders, drivers, vehicles and documents does each company have today, and how many join and leave per month? | Sizing, pricing, performance targets |
| Q-05 | Which documents does each hirer (Big C, ALL Now) require, how do they audit, and do they want direct access? | Default required rules; P-16 |
| Q-06 | What share of vehicles is rider-owned, company-owned, rented or financed (rent-to-own)? | Ownership model; equipment deposits |
| Q-07 | Should equipment carry deposit and deduction amounts now, or only issue/return? | Finance link in Phase 1 |
| Q-08 | How often must riders inspect vehicles (daily, weekly, at handover) and who reviews? | Inspection workload and alerts |
| Q-09 | Are EV motorcycles or battery swaps in use, and is battery data needed? | Vehicle model |
| Q-10 | Registration is Phase 1 (D-14). Do campaigns with UTM tracking, lead rotation among recruiters, the recruiting kanban (SWP) and referral rewards (TTP) also belong in Phase 1, Phase 2 or a separate product? | Scope |
| Q-11 | Training and quizzes (both companies) — which phase? | Scope |
| Q-12 | Team and budget: how many engineers, QA and design, and is a May 2027 GA the target? | Plan realism |
| Q-13 | Languages beyond Thai and English (e.g. Burmese for foreign riders)? | i18n |
| Q-14 | Hosting preference and any data-residency requirement from hirers? | P-02 |
| Q-15 | Is "Nova Logistics" the product name? | Branding, domains |
| Q-16 | What would TTP and SWP pay per rider per month, and what price would an XS company accept? | P-09 |
| Q-17 | Which accounting systems do target customers use (Express, PEAK, FlowAccount, SAP Business One)? | Phase 3 adapters |
| Q-18 | During the pilot, should TTP's and SWP's payroll keep running in their current systems and read drivers and vehicles from Nova Logistics by API? | Integration in Phase 1 |

## Next discovery package

1. Anonymised samples: one rider sheet from each company, one ZIP of 20 synthetic or masked document scans, the 2W and 4W checklists in use.
2. Answers to Q-02 to Q-06 and Q-12.
3. Decisions P-01, P-02, P-07, P-12 and P-18.
4. Drawings of the first journeys (onboarding by import, document renewal, vehicle handover) for owner review.
