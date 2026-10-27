# 2026-10-28 Owner answers to the first questions

## What changed

- Recorded the owner's answers as D-15 to D-25 in [05](../05-roadmap-and-decisions.md); added a status column to the questions table; P-16 answered by D-17; added P-20 (rider pay rate cards) and P-21 (accounting connectors).
- Reworded Q-06, Q-08, Q-14, Q-16 and Q-18, which the owner did not understand ("what?"); Q-01, Q-02, Q-12 and Q-17 skipped.
- Carried the answers through: 01 (internal tenants, sizing, document switch-off, equipment checklist, hirer export templates, TH/EN i18n, later-phase list), 02 (differentiation), 08 (connector model), 10 (equipment, document seeds), 11 (internal tenants, TTP 4W out of scope), business rules BR-DOC-08 and BR-EQP-01 (confirmed), README.
- New research note on how rider pay varies in swp-account-go and the proposed rate-card handling ([note](../research/2026-10-28-rider-pay-variation.md)).

## Evidence

- swp-account-go read for P-20: `DDL.sql` store pay-structure tables and seed values, `internal/utils/utils.go` hard-coded rates and tax.
- Relative links re-checked by script (see the commit).

## Remaining

- Open questions: Q-06, Q-08, Q-14, Q-16, Q-18 (reworded); decisions P-01, P-02, P-07, P-12, P-18 before S1.

## Deployment

None.
