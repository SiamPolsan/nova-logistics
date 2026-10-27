# Nova Logistics Product and MVP

**Planning baseline: 28 October 2026 (D-01 to D-08).** Nova Logistics is a multi-tenant SaaS for logistics companies in Thailand, first for sub-contractors that supply riders (2W) and drivers (4W) to retailers. Phase 1 replaces the spreadsheets and LINE groups such companies use to keep riders, their documents and their vehicles in order. Reference companies: TTP (Big C, 2W and 4W) and SWP (ALL Now, 2W) (D-13).

## Who it is for

| Segment | Size (drivers) | How they work today | What they buy |
| --- | --- | --- | --- |
| XS | 1–20 | One spreadsheet, a LINE group, photos in phones | Free or lowest tier; import their sheet in an hour |
| S | 21–150 | Several sheets per customer, Google Drive folders | Expiry alerts, document browser, bulk edit |
| SME | 151–2,000 | In-house tools or sheets plus staff to chase documents (TTP and SWP sit here) | Compliance at scale, roles by branch, UDA, migration, later API |

Phase 1 serves the **sub-contract mode**: a company supplies riders or drivers, often with their own vehicles, to a hirer's stores or DC (D-05). The **owned-logistics mode** (a company running its own fleet and orders) arrives with orders in Phase 3.

## Audiences

| Audience | Surface | Examples |
| --- | --- | --- |
| Office | Web (desktop first, works at 360 px) | Owner, admin, operations, fleet manager, document reviewer, viewer |
| Driver | Driver self-service PWA ([ADR-0008](adr/0008-driver-self-service-pwa.md)) | Rider or driver, employee or contractor |
| Platform team | Web, own workspace | Nova Logistics staff: create tenants, plans, support; reads no tenant data without a grant |

## Phase 1 scope (MVP)

| ID | Capability | Summary |
| --- | --- | --- |
| MVP-01 | Tenant core | Self-serve sign-up with a 30-day trial, company profile and logo, branches/depots, users, roles (Owner, Admin, Operations, Fleet manager, Document reviewer, Viewer, Driver), scope by branch, audit log |
| MVP-02 | Customers and sites | Hirers (Big C, ALL Now) and their stores or DCs as master data; a driver's primary customer site |
| MVP-03 | Driver records | Standard fields ([data model](10-data-model.md)) plus UDA; lifecycle Applicant → Onboarding → Active ⇄ Suspended → Resigned or Terminated; blacklist flag; duplicate check on citizen ID, passport/work permit and phone; rehire shows history; profile tabs |
| MVP-04 | Driver self-service | Sign in by LINE Login or phone OTP; view profile; upload and renew documents; run inspections with photos; see warnings |
| MVP-05 | Document types | Tenant catalogue seeded with the Thai defaults; per type: subject, expiry, default validity, required for which class or customer, review required, blocking |
| MVP-06 | Documents | Versioned upload (a renewal supersedes and keeps history); review queue pending → approved/rejected with reason; optional AI extraction of number and expiry (P-08) |
| MVP-07 | Expiry engine and alerts | Daily scan; thresholds per type (default 60/30/15/7/3 days, P-13); in-app, email and LINE OA; manager digest; escalation; optional "not compliant" flag when a blocking document expires (P-14) |
| MVP-08 | Compliance | Driver × required document matrix, % complete, by branch and customer, exportable |
| MVP-09 | Document browser | Bucket-style virtual folders by driver, vehicle, company and type; list/grid, preview, search, multi-select ZIP download, drag-and-drop upload |
| MVP-10 | Bulk document upload | ZIP or many files; auto-match by file name or folder path; review tray for unmatched; metadata from an Excel sheet in the same job |
| MVP-11 | Vehicles | Vehicle master, class (2W, EV 2W, 3W, 4W pickup, van, 6W, 10W, trailer), ownership (company, driver-owned, leased/rented, financed), CMI, voluntary insurance and tax |
| MVP-12 | Assignments | Who drove which vehicle, from–to, start/end odometer, primary/pool/temporary |
| MVP-13 | Odometer | Append-only readings from manual, inspection or import, with photo and anomaly flags |
| MVP-14 | Equipment | Catalogue; issue/return to driver or vehicle with serial, condition and deposit amount (recorded, not accounted) |
| MVP-15 | Photos and inspections | Standard photo angles per class; checklist templates per class (TTP's 20-item 2W and 30-item 4W lists as defaults: lights and electrical, tyres, brakes, fluids, safety kit, cargo); critical failure → vehicle out of service and a defect |
| MVP-16 | Service reminders | By date or odometer; light maintenance log with cost and invoice file |
| MVP-17 | Fleet overview | Dashboard and exportable reports: compliance, expiry calendar, assignments, insurance register, equipment register, inspection results |
| MVP-18 | Grid | Spreadsheet-grade grid on every list ([ADR-0007](adr/0007-spreadsheet-grade-bulk-data.md)) |
| MVP-19 | Import/export | xlsx/csv templates incl. UDA columns, saved mappings, Thai/BE dates, dry run, upsert, async with progress, error file, undo batch; export any view |
| MVP-20 | UDA | Tenant fields on driver, vehicle, document (per type), equipment, inspection, customer site ([07](07-tenant-customization-uda.md)) |
| MVP-21 | Field settings | Rename, hide or require standard fields per tenant |
| MVP-22 | Localisation | Thai and English UI; BE or CE date display per user |
| MVP-23 | Subscriptions | Plans and monthly usage count of active drivers (P-09, P-10); invoices issued outside the system |
| MVP-24 | Rider and driver registration | Applicants self-register from a public link, QR code or LINE per tenant and branch, tagged with source or campaign: name, phone (OTP-verified), citizen ID or passport, vehicle class, own vehicle yes/no, area, privacy-notice consent; each lands as an Applicant. The office reviews in the grid (duplicates, blacklist), requests documents through the PWA, moves to Onboarding or rejects with a reason. Bulk registration by import follows the same rules (D-14) |

### Not in Phase 1

Check-in/out, shift schedules and leave (Phase 2); customer portal for hirers (P-16); public API (Phase 2, internal API is designed for it); GPS tracking, fuel, routes, orders, OMS, WMS (Phase 3); payroll, billing and accounting (integrations later, D-08); recruiting campaigns, lead rotation and kanban beyond registration, and training (Q-10, Q-11).

## Key journeys

1. **Registration.** A rider scans the branch's QR code → fills the short form in Thai and verifies the phone by OTP → appears as an Applicant in the office grid → the office checks duplicates, asks for documents through the PWA and moves the rider to Onboarding.
1. **SME onboarding in one day.** Owner signs up → imports the rider sheet (mapping remembered) → uploads a ZIP of document scans named `{citizen_id}_{doc_type}.jpg` → reviews unmatched files → sees compliance % and what expires this month.
2. **Expiry to renewal.** The daily scan finds a CMI (พ.ร.บ.) 30 days from expiry → the rider gets a LINE message with a deep link → uploads a photo in the PWA → AI reads the new expiry (optional) → reviewer approves → the old version is superseded and alerts stop.
3. **Vehicle handover.** Fleet manager assigns a company pickup to a driver → the driver runs the handover inspection with 8 standard photos and the odometer → a failed brake item puts the vehicle out of service and opens a defect.
4. **Bulk fix.** Operations filters 200 riders of one branch in the grid → pastes a new customer site from Excel → the server validates every row → applies or returns per-cell errors → the change can be undone as one batch.

## Pilot acceptance criteria (proposed)

| ID | Criterion |
| --- | --- |
| AC-P-01 | TTP and SWP active riders, vehicles and current documents are migrated with reconciled counts per type ([11](11-migration-from-ttp-and-swp.md)) |
| AC-P-02 | For 30 consecutive days every threshold alert is sent once, on the right day, to the right people |
| AC-P-03 | A new XS/S tenant with no help goes from sign-up to compliance view in under one working day |
| AC-P-04 | Cross-tenant and cross-branch negative tests pass for every endpoint, export, import, job and file URL |
| AC-P-05 | A 500-file ZIP following the naming convention auto-matches at least 95% of files; the rest land in the review tray |
| AC-P-06 | Import of 10,000 drivers completes in under 5 minutes; an export of 50,000 rows completes in under 2 minutes |
| AC-P-07 | A driver completes a 6-photo inspection on a low-end Android phone opened from LINE |
