# Reference Systems: TTP, SWP and nova-sg-all

**Read on 28 October 2026** by AI agents from local checkouts (ttp-project, swp-driver-go, swp-account-go, nova-sg-all). Source code and business documents only; no production database was queried. File paths are relative to each repository. Credentials found in those repositories are noted by existence only; no value is reproduced here.

## Summary

| Capability | TTP — Thaitaweeporn (Big C, 2W + 4W) | SWP — Siwaporn Express (ALL Now, 2W) | nova-sg-all (security-guard SaaS) |
| --- | --- | --- | --- |
| Stack | Express/TypeScript (`ttp-be`), Next.js 15 (`ttp-fe`), MySQL `ttp_db`, Cloud Run | Go/Gin (`swp-driver-go`), MySQL, Redis, Pub/Sub, Cloud Run; finance in `swp-account-go` | Go modular monolith, MySQL 8.0/8.4, Next.js 16, Flutter, OpenAPI 3.1 (390 operations), k3s at NIPA |
| Tenancy | None; core tables named `bigc_*` | `company_code` (SWP, EPL) as an optional filter; not enforced from the JWT | `tenant_id BINARY(16)` everywhere, composite keys, negative tests |
| Rider record | `bigc_rider_documents`: one wide row with a URL column per document; `work_status` candidate, hired, waiting, suspended, resigned, rejected | `driver` with citizen ID/passport as primary key; bank, witnesses, consents as JSON; status PENDING, APPROVED, RESIGNED; driver type M (main) / R (reserve) | Employees with qualifications and licences |
| Documents | 9 types; `rider_document_renewals` review queue (pending, reviewing, approved, rejected); Gemini expiry extraction; `doc-alert-scanner.ts` levels 60/30/15/7/3 days; LINE-bot intake | `driver_document` (10 types incl. police clearance, vaccine); upload cancels the previous version; review queue; Cloud Vision OCR for ID, licence, registration book; no expiry job | Guard licences and qualifications with valid-until; renewal = new row |
| Vehicles | `rider_vehicles` (one per rider, insurance fields on the row); fleet registry `fleet_drivers`/`fleet_vehicles` with CMI, insurance and tax expiry, maintenance dates, `DRV-2W/4W` codes | `driver_vehicle` + `driver_vehicle_insurance`; one ACTIVE vehicle per driver; one image; no odometer | Planned `fleet` module (`docs/13-transport-service-line.md`, D-142) |
| 4W | Separate d4w service and database (not reviewed) | `temp_contracts`: depot, plate, mileage, fleet card, wage rate; no customer named | Trucks planned as a transport service line |
| Vehicle classes | `2W`/`4W` | `vehicleTypeCodeMap`: 2W, eco car/4W/pickup/box/chill/freeze, EV 2W, EV 3W, EV 4W, 3W/tuk-tuk, 6W, 10W, trailer | Pickup, 4W, 6W, 10W, trailer, tractor (planned) |
| Equipment | `rider_equipment_history` and a full stock module | Stock in `swp-account-go` (withdraw, return, loss) | — |
| Checklists | 20-item 2W and 30-item 4W pre-trip lists designed in `business-structure-docs/02_driver_fleet_database.md`; not built | Rider checklist export only | Versioned checklists for patrol and inspections |
| Import/export | Excel import with Thai header aliases (`rider-import-routes.ts`); Google Sheets and Drive sync; Excel export; evidence ZIP | excelize exports in customer templates (ALN, Last Mile, KPN); plan and attendance uploads | CSV import ≤ 2,000 rows, 1 MB |
| Scheduling and attendance | Shifts, templates, substitutes, check-in with GPS accuracy and selfie | Plans, store shifts, daily assignments, main/reserve, leave, attendance vs customer plan | Roster from contract lines, leave, cover, geofenced check-in, live location |
| Finance | Payroll cycles, advances, auto-deductions, COD deposits, WHT report, disbursement export | Per-bill pay (hard-coded 15 THB rider / 17 THB company), guarantee floors, coins, advances, WHT 1%/3% hard-coded; no double entry, no period close | Records rates; no invoices |
| Other | Safety incidents, tickets, training, recruiting/referral, org chart, API keys for an AI agent (MCP) | Recruit campaigns and kanban, training with quizzes and certificates, FAQ | Incidents, SOS, patrol, visitors, customer reports |

## Lessons carried into Nova Logistics

- Keep: TTP's document catalogue, thresholds, AI extraction, LINE intake and Thai header aliases; SWP's versioned upload, review queue, OCR and vehicle-type codes; nova-sg-all's tenancy, settings inheritance, outbox, audit, S3 adapter and working agreement.
- Avoid: one row per rider with a column per document; citizen ID as primary key; customer-specific tables and templates; hard-coded rates, tax and vendor codes; expiry states nobody computes; Redis or Pub/Sub as a record.

## Security debt observed (existence only)

- swp-driver-go and swp-account-go: `.env`, deployment scripts with inline secrets, GCP service-account key files and 55–65 MB binaries committed; some debug, migration and OCR routes registered without authentication; a document role read from the form body.
- ttp-project: a deployment runbook contains a database password in plain text; OAuth client and Google token files are present in `ttp-be`.
- Recommendation: rotate those credentials and clean the repositories before any data or code is reused for migration.
