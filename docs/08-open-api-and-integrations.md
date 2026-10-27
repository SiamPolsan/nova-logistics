# Open API and Integrations (Later Phases)

**Status: planned, 28 October 2026.** Nothing here is built. Phase 1 only makes the product integration-ready (D-03, D-08).

## Ready in Phase 1

- Internal OpenAPI 3.1 contract for every endpoint the web and PWA use.
- Stable IDs (UUIDv7) and natural keys (citizen ID or passport, plate + province, codes) on every record.
- Idempotency keys on create and bulk endpoints.
- Transactional outbox with versioned domain events.

## Event catalogue (initial)

| Event | Payload (IDs only, no personal data) |
| --- | --- |
| `driver.created`, `driver.updated`, `driver.status_changed` | driver ID, status, branch, customer site, version |
| `document.submitted`, `document.approved`, `document.rejected` | document ID, subject type and ID, type code, version |
| `document.expiring`, `document.expired` | document ID, threshold days, expiry date |
| `vehicle.created`, `vehicle.status_changed`, `vehicle.assigned`, `vehicle.unassigned` | vehicle ID, driver ID, odometer |
| `inspection.completed`, `inspection.failed`, `defect.opened`, `defect.resolved` | inspection or defect ID, vehicle ID, severity |
| `equipment.issued`, `equipment.returned` | issue ID, item, holder |

## Public API (Phase 2)

- Separate, versioned contract `/integrations/v1`, reusing the same application use cases.
- Credentials per tenant: API keys for server-to-server, OAuth 2.0 client credentials for partners; scopes per module and action; rate limits per credential.
- Webhooks: tenant registers endpoints per event type; signed payloads (HMAC); retries with backoff; delivery log; replay.
- Bulk endpoints mirror the import engine (dry run, commit, status).
- Machine credentials never inherit a human role; bank-account and criminal-record writes are not exposed (lesson from TTP's agent gateway).

## Accounting and ERP (later phase, D-25)

Nova Logistics never keeps a general ledger. Each tenant chooses and configures its own accounting link (D-25). Proposed model (P-21, option 1): **connectors**, not tenant-uploaded code.

- A connector is a built-in, versioned adapter for one target system, maintained by the product team.
- A tenant switches a connector on and configures it: credentials (kept in the secret store, never shown again), mapping of its chart of accounts, cost centres, tax codes and UDA tags, schedule, and which events post.
- Systems without a connector use tenant-defined export templates (CSV/xlsx), the public API and webhooks.
- Tenant code never runs inside the backend; an isolated extension runtime is a possible later step, as in nova-sg-all's customization design.

First connectors would be chosen from what pilot tenants use (Q-17 was skipped):

| Target | Typical customer | First integration |
| --- | --- | --- |
| Express, PEAK, FlowAccount | Thai XS/S/SME | CSV/xlsx export templates, then API where offered |
| Xero, QuickBooks | International SMEs | API |
| SAP Business One, NetSuite, Dynamics 365 | Larger tenants | API or middleware |

Mapping: contractor riders as vendors, hirers as customers, branches as cost centres, UDA fields as tags (e.g. GL code). Rates, VAT and withholding tax are dated settings, never constants; payment batches carry bank references; periods close instead of being recalculated destructively (lessons from swp-account-go).

## Telematics and fuel (Phase 3)

A port in the fleet module per provider (Cartrack, DTC first in Thailand): positions, ignition, odometer, matched to vehicles; fuel-card statements imported by provider format. Provider keys never enter the repository.

## Messaging

LINE Messaging API (LINE Notify ended on 31 March 2025), email, web push in Phase 1; SMS for OTP through a provider chosen with P-05.
