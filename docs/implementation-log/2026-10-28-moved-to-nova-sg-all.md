# 2026-10-28 Logistics moves into nova-sg-all

## What changed

- The owner asked how logistics, transport, better document and vehicle management and a later TMS could be built, with sub-contract (rider and driver supply) and owned logistics as separate service lines, and each company choosing its modules as in Odoo. Asked where that work lives, the owner chose one platform in nova-sg-all over keeping this repository separate and over a shared core module.
- Recorded as D-30 here (answers P-01 with option 3; supersedes ADR-0002 and the separate-repository part of D-12) and as D-158 in nova-sg-all, whose `docs/15-logistics-service-line.md` now holds the plan: apps per company, the two logistics lines, documents, fleet, locations and private persons, orders from a web form, DSD and cross-dock, planning on the built guard schedule, free trucks, route optimization, GPS, Excel import and export, the build order L0 to L6 and the open questions Q-68 to Q-79.
- README, AGENTS.md and ADR-0002 point to nova-sg-all. Nothing else here was changed: the other documents stay as the record of the baseline.

## Evidence

- nova-sg-all read for the move: `AGENTS.md`, `docs/03-architecture.md`, `docs/05-roadmap-and-decisions.md`, `docs/12-cleaning-service-line.md`, `docs/13-transport-service-line.md`, `docs/07-tenant-customization-and-plugins.md`, and its list of backend modules.

## Remaining

- Whether one company owns both products (nova-sg-all Q-79) was assumed, not confirmed.
- Nothing was committed or pushed.

## Deployment

None.
