# Migration from TTP and SWP

**Status: proposed, 28 October 2026** (P-11, Q-02, Q-03, Q-18). TTP and SWP become the first two tenants (EPL possibly a third, Q-03). Migration runs through the same import engine customers use, so it also proves MVP-19 and MVP-10.

## Sources

| Source | System | What moves |
| --- | --- | --- |
| TTP riders (Big C, 2W) | ttp-project: MySQL `ttp_db` (`bigc_rider_documents`, `rider_vehicles`, `rider_insurance_records`, `rider_document_renewals`, `rider_equipment_history`, `bigc_branches`), files in GCS and Google Drive | Riders, branches as customer sites, vehicles, insurance, current documents and files, equipment issued |
| TTP fleet registry | ttp-project: `fleet_drivers`, `fleet_vehicles` | 2W/4W drivers and vehicles not in the rider table |
| TTP 4W | d4w service and database (not in the reviewed repositories, Q-02) | To confirm |
| SWP riders (ALL Now, 2W) | swp-driver-go: MySQL (`driver`, `driver_address`, `driver_document`, `driver_vehicle`, `driver_vehicle_insurance`), files in GCS | Riders (SWP and EPL company codes), addresses, bank info (JSON), documents and files, vehicles, insurance |
| SWP stores | swp-account-go store master (ALN) | Customer sites for ALL Now |
| SWP 4W contracts | swp-driver-go `temp_contracts` | 4W drivers with depot, plate, mileage (to confirm) |

## Mapping highlights

| From | To | Note |
| --- | --- | --- |
| TTP wide row with `*_url` columns | One document per non-empty column, type mapped (`act` → `cmi`, `insurance_policy` → `voluntary_insurance`, `book_bank` → `bank_book`) | Expiry from `*_expiry` columns or the latest approved renewal |
| TTP `work_status` candidate/hired/waiting/suspended/resigned/rejected | Applicant / Active / Onboarding / Suspended / Resigned / (not migrated) | Confirm `waiting` |
| SWP `driver.id` = citizen ID or passport | New driver ID; citizen ID or passport kept as encrypted identity field | Natural key for upsert |
| SWP `status` PENDING/APPROVED/RESIGNED | Onboarding / Active / Resigned | `driver_type` M/R → UDA "rider type" |
| SWP `bank_info` JSON array | Bank account rows | Primary = `is_active` |
| SWP `driver_document.status` request/approve/reject/cancel | Pending review / Valid / Rejected / Superseded | Expiry recomputed by the scan |
| Vehicle types (free text) | Vehicle class via SWP's `vehicleTypeCodeMap` | Unknowns go to the review list |
| Files in GCS/Drive | Copied into the tenant prefix with SHA-256; metadata linked | Drive files exported first |

## Steps

1. **Extract** to anonymised staging files with synthetic samples for development; real extraction only in the migration environment.
2. **Transform** to the import templates (drivers, vehicles, documents metadata) plus a ZIP of files per tenant named by convention.
3. **Dry run** on UAT; fix mappings until zero blocking errors.
4. **Reconcile** counts per entity, per document type and per status against the source; spot-check 30 random riders per tenant with the operations team.
5. **Cutover**: freeze edits in the old system for driver master data, final delta import, switch alerts to Nova Logistics, parallel run for 2 weeks (P-11).
6. **Integration during the pilot** (Q-18): payroll and billing stay in the current systems and read driver and vehicle master data from Nova Logistics by export or API.

## Risks

- Data quality: duplicates by phone, missing expiry dates, Drive links without access. Mitigation: dry runs and a review list.
- Credentials in the reference repositories (found during reading on 28 October 2026) must be rotated before any migration access is granted.
