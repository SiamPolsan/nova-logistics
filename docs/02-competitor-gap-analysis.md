# Nova Logistics: Competitor and Gap Analysis

**As of 28 October 2026.** No competitor combines a person-centred driver record, the Thai document set and the driver's vehicle at SME prices. Western fleet tools model the vehicle, last-mile tools model the task, Thai incumbents sell GPS hardware, and workforce apps have no vehicle.

## How to read this

Facts come from vendor pricing and help pages, or third-party listings where noted, gathered by a research agent in October 2026 (sources below). Y = confirmed in public docs, P = partial, N = not offered or not found, ? = not verified. Prices are USD per vehicle or user per month unless marked; 1 USD ≈ ฿33–36. Re-check a price before quoting it to a customer.

## Market map

| | Task or order centred | Vehicle centred | Person centred |
| --- | --- | --- | --- |
| Hardware or enterprise contract | Bringg, Locus, FarEye, LogiNext, Shipsy, GIZTIX, NOSTRA TMS Plus+, 360TECHX | Samsara, Motive, Geotab, Verizon Connect, Cartrack, DTC, Chevin FleetWave, Mapon | none found |
| Self-serve SaaS | Onfleet, Tookan, Shipday, Fleetbase | Fleetio, Whip Around, AUTOsist, Simply Fleet, Odoo Fleet | Connecteam, Deputy |

Nova Logistics targets self-serve SaaS spanning the vehicle and person columns.

## Feature and price matrix

| Vendor | Built around | Doc expiry | Inspection + photos | Custom fields | Bulk import | Thai | Published price |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Fleetio](https://www.fleetio.com/pricing) | Vehicle | Y | Y | Y (by tier) | Y (update by re-import) | N | $4 / $7 / $10 per vehicle, annual (2026) |
| [Whip Around](https://help.whiparound.com/en/articles/6925626-how-whip-around-s-plans-work) | Vehicle, inspection-first | P | Y (AI photo checks) | Y (forms) | ? | N | Free for 1 asset; $5 / $10 per asset |
| [Simply Fleet](https://simplyfleet.app/pricing) | Vehicle | P | Y | P | P | ? | Free up to 5 vehicles; $3 / $5 per vehicle (from 15 Sep 2026) |
| [AUTOsist](https://autosist.com/pricing/) | Vehicle | ? | Y | ? | ? | N | $6–7 per vehicle, $59/month minimum |
| [Chevin FleetWave](https://www.chevinfleet.com/old-pricing) | Vehicle, enterprise | Y | Y | Y | ? | N | £5 / £7 per vehicle (undated) |
| [Samsara](https://airpinpoint.com/compare/samsara-pricing) | Vehicle + telematics | Y (AI reads expiry) | Y | Y | Y | N | Quote; about $39 list, 3-year term (third party) |
| [Motive](https://www.stackscored.com/pricing/fleet-management/motive/) | Vehicle + telematics | Y (AI) | Y | ? | ? | N | Quote; estimated $25–60 (third party) |
| [Geotab](https://www.geotab.com/apac/products/vehicle-tracking-device/) | Vehicle + telematics, resellers | P | Y | ? | Y | ? | Via resellers; estimated $20–40 + hardware |
| [Cartrack](https://www.cartrack.co.th/) | Vehicle + GPS, strong in Thailand | ? | ? | ? | ? | Y | ฿199–399 per vehicle + ฿2,999 install ([2021 dtac bundle](https://www.true.th/blog/en/dtac-business-partners-with-cartrack-to-bring-affordable-world-class-fleet-management-solutions-to-thai-smes/)) |
| [Connecteam](https://help.connecteam.com/en/articles/6365453-documents-how-to-use-expiration-dates) | Person (deskless) | Y (30/7/1-day reminders) | Y (forms) | Y | Y | N | Free up to 10 users; $29–99/month for 30 users |
| [Deputy](https://www.workstream.us/blog/deputy-pricing) | Person (shifts) | ? | N | ? | Y | Y (app) | About $5–9 per user |
| [Onfleet](https://onfleet.com/pricing) | Task (last mile) | N | P (POD) | Y | Y | N | $619 / $1,349 per month; Enterprise from $3,099 |
| [Tookan](https://jungleworks.com/tookan/pricing-tookan) | Task, white-label | N | P | Y | Y | ? | About $59–99/month per ~1,000 tasks |
| [Shipday](https://www.shipday.com/pricing) | Task (small couriers) | N | P | ? | ? | ? | Free for 10 drivers; $29–399/month |
| [Fleetbase](https://www.fleetbase.io/pricing) | Task, open source (AGPL) | ? | P | ? | ? | ? | Cloud $29/month + $5 per driver or vehicle |
| [Odoo Fleet](https://www.odoo.com/pricing) | ERP module | P | P | Y | Y | Y | $31–39 per user |
| [Shipsy](https://thaipost.app.shipsy.ai/) | Task, enterprise APAC | P | P | Y | Y | P (Thailand Post) | Quote |
| [Locus](https://locus.sh/case-studies/siam-makro-cuts-route-planning-time/) | Task, routing | ? | P | Y | Y | P (Makro) | Quote |
| Thai TMS: [GIZTIX](https://e27.co/thai-e-logistics-startup-giztix-nets-us10m-in-series-b-eyeing-ipo-in-2025-20211117/), [NOSTRA TMS Plus+](https://brandinside.asia/?p=239857), [DTC](https://hk.finance.yahoo.com/quote/DTCENT.BK/profile), [360TECHX](https://linkedin.com/company/360techx) | Task + GPS | ? | ? | ? | ? | Y | Quote only |

Regional players McEasy, Kargo Tech (Indonesia) and Abivin (Vietnam) are route- and shipment-centred. Transpo, eTMS, Smart TMS and Logistic.co.th could not be identified as SaaS products.

### Price anchors

| Segment | Typical price | Examples |
| --- | --- | --- |
| Software-only fleet maintenance and inspections | $2–10 per vehicle per month | Fleetio, Whip Around, Simply Fleet |
| Telematics-led fleet | $20–60 per vehicle per month + hardware, multi-year | Samsara, Motive, Geotab |
| Thai GPS bundle | ฿199–399 per vehicle per month + install | Cartrack (2021) |
| Last-mile dispatch | About $30–1,350 per month by task volume | Shipday, Tookan, Onfleet |
| Workforce apps | About $1–5 per user per month | Connecteam, Deputy |

## Best practices to copy

1. Renewal reminders as their own configurable object, with a due-soon window and repeat alerts until resolved (Fleetio).
2. Driver self-renewal from the alert: photo upload, AI reads the expiry, a manager approves (Samsara, Motive, Connecteam).
3. Inspection templates per vehicle type with required defect photos, severity, automatic issue, signature, offline mode (Whip Around, Samsara).
4. Custom fields as a pricing-tier lever that double as accounting tags such as cost centre or client site (Fleetio, Onfleet).
5. Import templates with update-by-re-import keyed on an external ID; export from every list (Fleetio).
6. Unlimited users, charge per asset, a free tier and a no-card trial (Fleetio, Simply Fleet, Shipday).
7. Integrate telematics rather than build it (Fleetio, Simply Fleet); in Thailand that means DLT-certified Cartrack and DTC.
8. API and webhooks first, CSV and scheduled reports for accounting, native ERP connectors later (Fleetio).

## Gap analysis

Columns: what TTP and SWP do today ([research](research/2026-10-28-reference-systems.md)), the best practice found above, and what Phase 1 must close.

| Need | TTP today | SWP today | Best practice | Phase 1 closes it with |
| --- | --- | --- | --- | --- |
| Many companies on one product | None (`bigc_*` tables) | `company_code` filter, not enforced | Tenant isolation, per-tenant settings | `tenant_id` everywhere, negative tests (ADR-0003) |
| Driver record | One wide row with a URL column per document | Citizen ID as primary key; JSON blobs | Person record with typed children | Driver + contacts + bank + documents as rows (MVP-03) |
| Foreign workers | Not modelled | Passport field only | — | Passport and work permit as identity documents |
| Document versions | Renewal table per rider document | Versioned rows, cancel on re-upload | Renewal history, due-soon window | Versioned documents, superseded state (MVP-06) |
| Expiry alerts | Scanner at 60/30/15/7/3 days, LINE | No job; nothing sets "expired" | Configurable thresholds, repeat until resolved | Per-type thresholds in Settings, one alert per threshold (MVP-07) |
| Driver self-renewal | LINE bot intake, review queue | Driver upload, review queue | Upload from the alert, AI reads expiry | PWA deep link from LINE, optional AI (MVP-04, P-08) |
| Document browser and ZIP | Evidence ZIP for disbursement only | None | None found at SME level | Bucket-style browser, ZIP in and out (MVP-09, MVP-10) |
| Vehicle and ownership | One vehicle per rider; fleet registry separate | One active vehicle; owner fields | Vehicle master with assignments | Vehicle + ownership + assignment history (MVP-11, MVP-12) |
| Odometer | Field in registry | Only in a 4W contract register | Meter log with sources | Append-only readings with anomaly flags (MVP-13) |
| Equipment | Stock module | Stock in finance service | Asset issue/return | Issue/return to driver or vehicle (MVP-14) |
| Inspections with photos | Checklists designed, not built | Checklist export only | Templates per type, defect photos | Template builder, standard angles, defects (MVP-15) |
| Spreadsheet work | Thai header aliases, Sheets sync | Customer Excel templates | CSV templates, re-import updates | Grid editing, async import with undo, any-view export (MVP-18, MVP-19) |
| Custom fields | None | None | Tiered custom fields | UDA per entity (MVP-20) |
| Thai and English | Thai only | Thai only | Mostly English only | TH/EN with BE dates (MVP-22) |
| Audit and PDPA | Activity logs | Pub/Sub audit events | Audit trail, access logs | Audit on every change and file open; masked fields |
| Integrations | API keys for an AI agent | Service-to-service API key | API + webhooks, ERP later | Outbox events and OpenAPI now; public API Phase 2 |
| Accounting | Payroll, advances, WHT in-house | Hard-coded rates and WHT | Export to accounting, no own GL | Out of Phase 1; adapters later (D-08) |

## Differentiation hypotheses

1. **Thai rider compliance out of the box:** the Thai document set, BE dates, LINE alerts and foreign-worker identity on day one.
2. **Spreadsheet-native:** paste from Excel, import with undo, ZIP in and out; an XS company is live in a day.
3. **Person and vehicle together:** owner-driver vehicles, assignments and inspections on the same record.
4. **Built for sub-contractors:** customer and site on every driver; compliance by hirer; later a hirer view (P-16).
5. **Priced in THB per active driver**, unlimited office users, free tier for XS.

## Evidence to collect in pilots

- Time to onboard an XS/S tenant from its own sheet; % of ZIP files auto-matched.
- Alerts sent vs documents renewed before expiry; days expired before and after.
- Which UDA fields tenants add (candidates to promote to standard fields).
- Willingness to pay per active driver; which tier gate (UDA count, users, storage) matters.

## Sources

Vendor and third-party pages listed in the matrix, fetched October 2026, plus: [Fleetio renewal reminders](https://help.fleetio.com/en_US/vehicle-renewal-reminders), [Fleetio import guide](https://help.fleetio.com/en_US/vehicle-renewal-reminders-import-guide-2), [Samsara qualifications](https://www.samsara.com/fr/blog/qualifications-management), [Motive documents](https://helpcenter.gomotive.com/hc/en-us/articles/31075948317853-Documents-Overview), [Onfleet driver languages](https://intercom.help/onfleet-4f83d83093ac/en/articles/15254830-driver-settings), [Tookan languages](https://help.jungleworks.com/knowledge-base/languages-supported/), [Locus pricing 2026](https://locus.sh/blogs/locus-pricing-2026/), [Teleport and Shipsy](https://technode.global/prnasia/teleport-enhances-real-time-last-mile-supply-chain-visibility-across-southeast-asia-partnering-with-shipsys-ai-powered-platform/). Limitations: Thai local vendors publish no prices; Cartrack's only public Thai price is from 2021; several third-party estimates are undated.
