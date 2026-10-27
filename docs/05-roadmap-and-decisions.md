# Nova Logistics Roadmap and Decisions

Phase 1 proves that a sub-contractor can keep every rider, document and vehicle compliant without spreadsheets, and that a new small company can start in a day. **Status: proposed plan, 28 October 2026.** Dates assume the team in "Planning estimate" and are not a committed launch date.

## Phases

| Phase | Window (proposed) | Scope | Exit evidence |
| --- | --- | --- | --- |
| 0: Discovery | 2–13 Nov 2026 | Answers to the open `Q-*`; sample sheets and document scans from TTP and SWP; P-01, P-07, P-12, P-18 decided; NIPA accounts and UAT resources requested (D-28); screens drawn for the first journeys | Owner signs off scope and the first screens |
| 1: Core MVP | 16 Nov 2026 – 26 Mar 2027 | MVP-01 to MVP-25 ([01](01-product-and-mvp.md)) | All Phase 1 `AC-*` verified locally and on UAT |
| 1: Pilot | 29 Mar – 30 Apr 2027 | TTP and SWP migrated and running in parallel with their current systems | AC-P-01 to AC-P-07 |
| 1: GA | May 2027 | Self-serve sign-up open; first external XS/S tenants | 5 paying external tenants |
| 2: Operations | Jun – Sep 2027 | Check-in/out with GPS and selfie, shift schedules, leave requests and cover (patterns from nova-sg-all), public API v1 with writes and webhooks extending the Phase 1 pilot read API, Google Sheets connector | A pilot tenant runs a month of shifts and attendance in the product |
| 3: Logistics | Q4 2027 – 2028 | Fuel logs and fuel cards, maintenance depth, telematics port (Cartrack, DTC), route optimisation, orders for sub-contract and owned-logistics modes, accounting/ERP connectors (P-21); OMS and WMS after | Trips and fuel for one owned-fleet tenant |
| Later | Not scheduled | Recruiting on global applicant-tracking practice (D-20), training (D-21), rider pay and leave calculation (D-24, P-20), company-wide stock (D-18) | The owner sets the order |

## Phase 1 sprint plan (two-week sprints)

| Sprint | Dates | Deliverables | MVP |
| --- | --- | --- | --- |
| S0 | 2–13 Nov 2026 | Discovery, decisions P-01/P-07/P-12/P-18, NIPA UAT resources requested, screen drawings, repository scaffold, local stack, CI skeleton | — |
| S1 | 16–27 Nov | Tenancy, sign-up, memberships, roles, branch scope, audit, job queue/outbox, S3 and Redis adapters, i18n shell, driver sign-in spike (PWA, LINE) | 01, 04, 22 |
| S2 | 30 Nov – 11 Dec | Settings, Customization (UDA) and field settings, customers and sites, grid component spike and choice | 02, 20, 21, 18 |
| S3 | 14–25 Dec | Driver records, lifecycle, duplicates, profile tabs, self-registration link/QR and review, grid inline and bulk edit, saved views | 03, 24, 18 |
| — | 28 Dec – 1 Jan 2027 | New Year break | — |
| S4 | 4–15 Jan 2027 | Import/export engine with mapping, dry run, upsert, undo; export any view; export templates in each hirer's format (D-17) | 19 |
| S5 | 18–29 Jan | Document types, versioned upload, review queue, compliance matrix, optional AI extraction | 05, 06, 08 |
| S6 | 1–12 Feb | Expiry engine, alerts (in-app, email, LINE OA), digest; document browser; ZIP upload and download | 07, 09, 10 |
| S7 | 15–26 Feb | Vehicles, ownership, insurance and tax, assignments, odometer, equipment issue/return checklist | 11, 12, 13, 14 |
| S8 | 1–12 Mar | Inspection templates, photo capture in the PWA, defects, service reminders | 15, 16 |
| S9 | 15–26 Mar | Fleet overview and reports, subscriptions, pilot read API with API keys (D-29), performance and security review, restore rehearsal on NIPA | 17, 23, 25 |
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
| D-15 | TTP, SWP and EPL are separate tenants, used internally for the pilot; they are not sales targets | 2026-10-28, owner answer to Q-03 |
| D-16 | Size for 1,000+ driver records per tenant, including inactive and resigned | 2026-10-28, owner answer to Q-04 |
| D-17 | Hirers do not use the product: the tenant imports data into the hirer's own system, so Phase 1 gives tenant-defined export templates in the hirer's format and no hirer portal. Required documents are fully tenant-configurable: a tenant decides what is required and can switch off types it does not use | 2026-10-28, owner answer to Q-05; supersedes the P-16 proposal |
| D-18 | Equipment is issue and return only, recorded as a checklist; company-wide stock is a later phase | 2026-10-28, owner answer to Q-07 |
| D-19 | EV vehicles may be used: keep EV classes and powertrain on the vehicle; no battery or swap features in Phase 1 | 2026-10-28, owner answer to Q-09 ("maybe") |
| D-20 | Recruiting beyond registration (campaigns, lead rotation, kanban, referral) is a later phase and follows global applicant-tracking practice | 2026-10-28, owner answer to Q-10 |
| D-21 | Training is a later phase, not Phase 2 | 2026-10-28, owner answer to Q-11 |
| D-22 | Thai and English first, built on i18n/l10n so more languages can be added | 2026-10-28, owner answer to Q-13 |
| D-23 | The product name is Nova Logistics | 2026-10-28, owner answer to Q-15 |
| D-24 | Rider pay calculation and leave calculation are a later phase; pay must handle rates that vary by customer, site, period, volume and rider (P-20) | 2026-10-28, owner remark on Q-16 |
| D-25 | Accounting integration is deferred and must be configurable per tenant (P-21) | 2026-10-28, owner answer to Q-17 |
| D-26 | Every vehicle ownership type is supported: rider-owned, company-owned and lent, rented from a leasing company, and rent-to-own (instalment deductions arrive with pay, P-20) | 2026-10-28, owner answer to Q-06 ("it can be all") |
| D-27 | Each tenant sets its own inspection schedule per vehicle class and ownership; the product ships defaults the tenant can change | 2026-10-28, owner answer to Q-08 (option 4) |
| D-28 | Hosting is NIPA Cloud in Thailand: k3s for API, worker and web; NIPA managed MySQL 8.4; NIPA S3; Redis on k3s unless NIPA offers it managed. Answers P-02 | 2026-10-28, owner answer to Q-14 |
| D-29 | During the pilot, TTP's and SWP's payroll systems read drivers, vehicles and assignments from a Nova Logistics API, and exports stay available. A read-only pilot API with API keys therefore moves into Phase 1 (MVP-25); the full public API stays in Phase 2 | 2026-10-28, owner answer to Q-18 |

## Decisions still needed

Working proposals let independent work continue; nothing that depends on them becomes production behaviour until answered.

| ID | Decision | Options | Working proposal | Needed before |
| --- | --- | --- | --- | --- |
| P-01 | Code relationship with nova-sg-all | 1. Port patterns, no shared code. 2. Shared Go platform module. 3. Build inside nova-sg-all | 1 now; review after GA (ADR-0002) | S1 |
| P-02 | Cloud and runtime | — | **Answered by D-28:** NIPA Cloud in Thailand | — |
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
| P-16 | Hirer view | — | **Answered by D-17:** no hirer portal; hirers receive exports in their own format | — |
| P-17 | Key management for field encryption | 1. NIPA's key service, if offered. 2. Envelope encryption with a master key kept as a k3s Secret outside Git. 3. An external KMS | Check 1 in S1, else 2 (D-28); local uses a development key | S1 |
| P-18 | Design system | 1. Own tokens and components drawn first (as nova-sg-all). 2. Off-the-shelf kit (shadcn/ui) themed | 2 with own tokens; draw key screens before coding | S0 |
| P-19 | Malware scanning of uploads | 1. ClamAV in the worker. 2. Cloud provider scanning. 3. None in Phase 1 | 1 | S6 |
| P-20 | How rider pay handles varying rates (later phase, D-24) | 1. Rate-card engine: effective-dated, approved rate cards scoped tenant → customer → site → rider, with tiers, bonus bands, daily guarantee floors, allowances and typed deductions as data. 2. Fixed pay types with per-tenant numbers. 3. Export work records to an outside payroll tool | 1 ([research note](research/2026-10-28-rider-pay-variation.md)) | Before the pay phase |
| P-21 | How a tenant connects its accounting system (D-25) | 1. Connectors: built-in adapters (Express, PEAK, FlowAccount, Xero, SAP B1…) the tenant switches on and configures (credentials, account mapping, schedule), plus export templates and webhooks for anything else. 2. Tenant-uploaded plugin code. 3. Export files only | 1; tenant code is not run inside the backend (as nova-sg-all's customization design) | Before the accounting phase |

## Questions for the owner

Answers given by the owner on 28 October 2026 are recorded as `D-*` rows above. Five questions were reworded the same day because the first wording was unclear; their answers followed as D-26 to D-29.

| ID | Question | Why it matters | Status |
| --- | --- | --- | --- |
| Q-01 | Which legal entity owns and sells Nova Logistics, and who are the first external pilot customers after TTP and SWP? | Contracts, DPA, pricing tests | Skipped by the owner; deferred |
| Q-02 | Where are TTP's 4W system (d4w) code and data, and is it migrated in Phase 1? | Migration scope; 4W fields | Skipped; TTP 4W is out of migration scope until answered |
| Q-03 | Are TTP, SWP and EPL separate tenants? | Tenancy and migration | Answered → D-15 |
| Q-04 | How many records does each company have? | Sizing, performance targets | Answered → D-16 |
| Q-05 | Which documents does each hirer require, how do they audit, do they want direct access? | Required rules; hirer view | Answered → D-17 |
| Q-06 | **Reworded.** Who owns the vehicles your riders and drivers use, and roughly what share of each: (1) the rider's own vehicle, (2) a company vehicle lent to the rider, (3) rented from a leasing company, (4) rent-to-own, with instalments deducted from pay? | Which owner documents (power of attorney) and rental contracts to track; whether instalment deductions matter later | Answered → D-26 |
| Q-07 | Equipment: deposits and deductions now, or issue/return only? | Finance link | Answered → D-18 |
| Q-08 | **Reworded.** How often should a rider do the vehicle checklist with photos: (1) every day before work, (2) weekly or monthly, (3) only when joining or when a vehicle changes hands, (4) each company sets its own schedule? Who looks at failed items? Working proposal: 4, default "at joining and monthly" for 2W and "daily" for company 4W | How many inspections the system sends and reviews | Answered → D-27 |
| Q-09 | Are EV motorcycles or battery swaps in use? | Vehicle model | Answered → D-19 |
| Q-10 | Recruiting beyond registration: which phase? | Scope | Answered → D-20 |
| Q-11 | Training: which phase? | Scope | Answered → D-21 |
| Q-12 | Team and budget: how many engineers, QA and design, and is a May 2027 GA the target? | Plan realism | Skipped; plan keeps its assumed team |
| Q-13 | Languages? | i18n | Answered → D-22 |
| Q-14 | **Reworded.** Where should Nova Logistics run: (1) Google Cloud, Singapore region, where TTP and SWP run today, (2) NIPA Cloud in Thailand, where nova-sg-all's UAT runs, (3) AWS, Bangkok region? Does any customer require data kept in Thailand? | Cost, operations playbook, PDPA transfer rules (P-02) | Answered → D-28 |
| Q-15 | Product name? | Branding | Answered → D-23 |
| Q-16 | **Reworded.** What monthly price should a company pay for Nova Logistics itself, for example ฿20, ฿30 or ฿50 per active rider per month, free up to 10 riders? (The owner's note on varying rider pay is recorded as D-24 and P-20.) | Pricing (P-09) | Skipped by the owner; needed before GA |
| Q-17 | Which accounting systems do target customers use? | Accounting connectors | Skipped; integration made tenant-configurable → D-25, P-21 |
| Q-18 | **Reworded.** During the pilot, TTP's and SWP's payroll still needs the rider list. Should it (1) receive a scheduled export from Nova Logistics and import it, (2) read it from Nova Logistics' API, or (3) TTP and SWP stay on their old systems until a pay module exists? Working proposal: 1 | Integration work in Phase 1 | Answered → D-29 (option 2, plus exports) |

## Next discovery package

1. Anonymised samples: one rider sheet from each company, one ZIP of 20 synthetic or masked document scans, the 2W and 4W checklists in use.
2. NIPA accounts and UAT resources for D-28; Q-16 (subscription price) before GA.
3. Decisions P-01, P-07, P-12 and P-18.
4. Drawings of the first journeys (onboarding by import, document renewal, vehicle handover) for owner review.
