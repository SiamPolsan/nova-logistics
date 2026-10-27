# Thailand Privacy and Compliance Inputs

**As of 28 October 2026. Design input, not legal advice.** Confirm each point with counsel before the pilot (P-15).

## PDPA roles

- The tenant (the logistics company) is the **data controller** for its riders, drivers and their documents. Nova Logistics is the **data processor** under a signed data processing agreement (DPA).
- The platform team reads tenant data only under a time-limited, audited support grant requested by the tenant.
- Each tenant needs a privacy notice for riders; the driver PWA shows it at first sign-in and records acknowledgement with its version.

## Sensitive and protected data

| Data | Treatment |
| --- | --- |
| Criminal record check | Sensitive personal data under PDPA section 26; collect only where the tenant states a lawful basis; restrict to Owner/Admin/Document reviewer; encrypted; audit every open |
| Health data (medical certificate) | Sensitive under section 26; same treatment |
| Biometric use of face photos | Sensitive if used for identification; Phase 1 does not do face matching |
| Citizen ID, passport, work permit numbers | Encrypted at field level; masked in grids except for permitted roles; keyed hash for duplicate checks |
| ID-card copies | May show religion or blood type on some card versions; advise masking at upload; never use for anything beyond identity |
| Bank account | Encrypted, masked; changes audited and notified to the driver |

## Rights, breach and transfer

- Data subject requests (access, correction, deletion, objection): an export of a driver's data and a deletion workflow that keeps audit and legal-hold records.
- Breach: PDPA requires notifying the PDPC within 72 hours of becoming aware, unless low risk; the runbook must name who decides and who notifies (tenant as controller, platform supports).
- Cross-border transfer: hosting outside Thailand (e.g. GCP Singapore) needs the safeguards the PDPC requires; record the choice with P-02.

## Retention

Proposed (P-15): documents of a driver kept while engaged plus 1 year, then files deleted and metadata minimised; audit kept longer; tenant can set a longer period per document type for contractual reasons; legal hold stops deletion.

## Vehicle and driver documents in Thailand

| Document | Notes for the product |
| --- | --- |
| CMI (พ.ร.บ.) | Compulsory motor insurance under the Motor Vehicle Accident Victims Protection Act; annual; required for every vehicle; blocking by default |
| Annual vehicle tax (ป้ายภาษี) | Renewed yearly with the DLT; usually requires valid CMI first; blocking by default |
| Registration book (เล่มทะเบียน) | No expiry; owner name drives the power-of-attorney rule when owner ≠ driver |
| Voluntary insurance | Classes 1, 2+, 2, 3+, 3; required for 4W by most hirers; optional for 2W |
| Driving licence | Temporary (2 years) then 5-year licences; class must match the vehicle class; commercial classes for some trucks; class rules per vehicle class are tenant settings |
| Foreign workers | Passport and work permit as identity documents with expiry; both blocking by default |
| GPS for trucks | The DLT requires GPS with a licence-card reader for certain vehicle types; relevant to Phase 3 telematics, not Phase 1 |

## Tax and payments (later phases)

Contractor riders are commonly paid with 3% withholding tax on services and employees with 1% or progressive tax; swp-account-go hard-codes these. Any later payment feature keeps rates as dated settings, issues WHT certificates and never hard-codes them.
