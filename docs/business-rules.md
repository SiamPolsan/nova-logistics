# Business Rules

Every business rule lives here with a code, the rule in plain words, an example, its status and its source. Status: **proposed** (not yet confirmed by the owner), **confirmed** (owner agreed; date in the source) or **built** (implemented and verified; link the evidence). A code is never reused. Figures a tenant can change are listed in [Figures](#figures) and live in Settings.

## Drivers (BR-DRV)

| Code | Rule | Example | Status | Source |
| --- | --- | --- | --- | --- |
| BR-DRV-01 | A driver is identified within a tenant by citizen ID, or by passport number for a foreign worker; a second driver with the same identity number is refused and the existing record is offered | Importing a rider whose citizen ID already exists updates that rider instead of creating a copy | proposed | D-04, TTP/SWP duplicate checks |
| BR-DRV-02 | A phone number already used by another active driver in the tenant raises a warning, not a refusal | Two brothers sharing a phone can both be registered after a warning | proposed | SWP recruit check |
| BR-DRV-03 | A driver becomes Active only when every document required for their vehicle classes and primary customer is Valid | A 4W driver without an approved medical certificate stays Onboarding | proposed | TTP doc 02 |
| BR-DRV-04 | Every status change records who, when and a reason; Resigned and Terminated need an end date | Terminating a rider asks for the date and reason | proposed | SWP resignation flow |
| BR-DRV-05 | A blacklisted person can be viewed but not re-activated without an Owner or Admin override with a reason | A rehire attempt shows the blacklist reason | proposed | brief |
| BR-DRV-06 | Citizen ID, passport, work permit and bank account numbers are shown masked except to roles allowed to see them | A viewer sees `1-xxxx-xxxxx-12-3` | proposed | PDPA, [06](06-thailand-privacy-and-compliance.md) |
| BR-DRV-07 | A self-registration is accepted only after the phone is verified by OTP and the privacy notice is accepted; it creates an Applicant tagged with the link's branch and source | A rider who scans Branch A's QR appears as an Applicant of Branch A, source "QR" | proposed | D-14, SWP `/v1/register` |
| BR-DRV-08 | A registration whose citizen ID, passport or phone matches an existing driver or a blacklisted person is kept but flagged for the office; the applicant is not told why | A resigned rider re-registers and the office sees the earlier record | proposed | D-14 |
| BR-DRV-09 | An Applicant not moved to Onboarding within the tenant's period is closed as expired; personal data follows the retention rule | After 90 days an untouched applicant closes | proposed | D-14, P-15 |

## Documents (BR-DOC)

| Code | Rule | Example | Status | Source |
| --- | --- | --- | --- | --- |
| BR-DOC-01 | Uploading a new version of a document type for the same subject keeps the old version; the old one becomes Superseded only when the new one is approved | A renewed CMI stays Pending review while the old CMI remains Valid until approval | proposed | TTP renewals, SWP cancel-on-upload |
| BR-DOC-02 | A document type with expiry needs an expiry date before approval; the reviewer may correct an AI-read date | Gemini reads 31/12/2569; the reviewer approves 2026-12-31 | proposed | TTP extraction |
| BR-DOC-03 | The daily scan sets Expiring when the days left reach the first threshold and Expired the day after the expiry date, in the tenant's timezone | A licence expiring on 1 March is Expired on 2 March 00:00 Bangkok time | proposed | TTP scanner |
| BR-DOC-04 | An alert is sent once per document per threshold; a renewal approved before the next threshold stops further alerts | 60-day and 30-day alerts sent; renewal approved at day 20; no 15-day alert | proposed | P-13 |
| BR-DOC-05 | When a blocking document expires, the driver or vehicle is flagged Not compliant; if the tenant enabled auto-suspend, the status changes to Suspended or Out of service | Expired CMI puts the vehicle Out of service where auto-suspend is on | proposed | P-14, TTP design doc |
| BR-DOC-06 | A driver may upload or renew only their own documents; office roles review | The PWA shows only the signed-in driver's documents | proposed | SWP owner-or-employee rule |
| BR-DOC-07 | A rejection needs a reason the driver can read | "Photo blurred, please retake" | proposed | TTP review queue |

## Vehicles (BR-VEH)

| Code | Rule | Example | Status | Source |
| --- | --- | --- | --- | --- |
| BR-VEH-01 | A vehicle is unique in a tenant by plate and province | Two `1กก-1234 กรุงเทพมหานคร` are refused | proposed | brief |
| BR-VEH-02 | A vehicle has at most one primary assignment at a time; assigning a new primary closes the previous one with an end odometer | Handing a pickup to a new driver ends the old assignment | proposed | brief |
| BR-VEH-03 | When the registered owner is not the assigned driver, a power-of-attorney document is required | A rider using a sibling's motorcycle must upload a power of attorney | proposed | TTP/SWP owner fields |
| BR-VEH-04 | An odometer reading lower than the previous one, or higher by more than the daily limit, is kept but flagged for review | 25,000 then 24,800 km is flagged | proposed | brief |
| BR-VEH-05 | A failed critical inspection item puts the vehicle Out of service and opens a defect; resolving the defect returns it to Active | Brake failure blocks the vehicle until repaired | proposed | TTP checklist rules |
| BR-VEH-06 | A failed non-critical item opens a defect with a due date but leaves the vehicle Active | A cracked mirror is due for repair in 7 days | proposed | TTP checklist rules |

## Imports and bulk edits (BR-IMP)

| Code | Rule | Example | Status | Source |
| --- | --- | --- | --- | --- |
| BR-IMP-01 | Every import runs a dry run first and shows row errors; nothing is written until the user commits | 3 of 500 rows have bad dates; the user fixes and re-uploads | proposed | ADR-0007 |
| BR-IMP-02 | Rows are matched on the natural key; a match updates, no match creates | A sheet with existing citizen IDs updates those riders | proposed | ADR-0007 |
| BR-IMP-03 | A year above 2400 is read as Buddhist era and converted | 15/03/2569 → 2026-03-15 | proposed | TTP sheets |
| BR-IMP-04 | A whole import batch can be undone while no later change touched its rows; otherwise the undo lists the rows it cannot revert | Undo after a wrong mapping restores 500 riders | proposed | ADR-0007 |
| BR-IMP-05 | A grid paste is applied all-or-nothing by default; errors are returned per cell | Pasting 200 sites with 2 unknown codes changes nothing and highlights 2 cells | proposed | ADR-0007 |

## Files (BR-FILE)

| Code | Rule | Example | Status | Source |
| --- | --- | --- | --- | --- |
| BR-FILE-01 | A file is downloaded only through a link valid for at most 5 minutes, issued after an access check, and the opening is audited | A reviewer's preview link stops working after 5 minutes | proposed | ADR-0005 |
| BR-FILE-02 | ZIP uploads match files by `{citizen_id or driver_code or plate}_{doc_type_code}` or folder path; unmatched files wait in a review tray | `1234567890123_cmi.jpg` attaches to that rider's vehicle CMI | proposed | MVP-10 |
| BR-FILE-03 | Photos are stored without EXIF; capture time and GPS are kept as separate fields | A photo's location is visible to the office but not inside the downloaded file | proposed | nova-sg-all practice |

## UDA (BR-UDA)

| Code | Rule | Example | Status | Source |
| --- | --- | --- | --- | --- |
| BR-UDA-01 | Removing a field archives it; values are kept and can be restored | Archiving "Shirt size" hides it everywhere | proposed | D-06, ADR-0006 |
| BR-UDA-02 | Making a field required later marks existing records incomplete instead of blocking them | 300 riders show "incomplete" until edited | proposed | ADR-0006 |
| BR-UDA-03 | Core identity fields cannot be hidden or made optional | Citizen ID/passport and plate stay required | proposed | D-07 |

## Figures

| Figure | Default | Range | Set by | Source |
| --- | --- | --- | --- | --- |
| Alert thresholds (days before expiry) | 60, 30, 15, 7, 3 | 1–180, up to 6 values | Tenant, per document type | P-13 |
| Manager digest time | 07:00 | any hour | Tenant | proposed |
| Applicant expiry | 90 days | 7–365 days | Tenant | BR-DRV-09 |
| Odometer daily jump limit | 2W 400 km, 4W 1,000 km | 50–3,000 km | Tenant, per vehicle class | proposed |
| Download link validity | 5 minutes | 1–5 minutes | Platform | ADR-0005 |
| ZIP upload limits | 2 GB, 5,000 files, 50 MB per file | — | Platform | proposed |
| Import size | 50,000 rows per file | — | Platform | ADR-0007 |
| Filterable UDA fields | 10 per entity | — | Platform, by tier | ADR-0006 |
