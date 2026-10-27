# How Rider Pay Varies Today (swp-account-go) and How to Handle It

**Read on 28 October 2026** from the local checkout of swp-account-go (`DDL.sql`, `internal/utils/utils.go`, `internal/services/`), at the owner's request on Q-16 ("check the code of account, it varies, how to handle that?"). Pay and leave calculation are a later phase (D-24); this note feeds P-20. Values below are seed or code values, not confirmed contract rates.

## What varies

| Dimension | Evidence | Example |
| --- | --- | --- |
| Pay type per period | `driver_payroll_period_master.payment_type` WEEKLY or MONTHLY | Weekly riders paid per bill; monthly staff paid a base salary minus absence and lateness |
| Base rate per bill | Hard-coded in `internal/utils/utils.go:128-133` | 15 THB to the rider, 17 THB company revenue, the same for every company code |
| Withholding tax | Hard-coded in `utils.go:134-143` | 1% on weekly riders, 3% on monthly staff |
| Store pay structure | `store_structure_master` with a priority (`rider_plus` before `normal`) and a guarantee type (RIDERPLUS, COMPANY, ANL) — `DDL.sql:121-200` | One store uses a Rider Plus structure, another a normal one |
| Tier price by volume | `store_structure_tier_price`, bill-count bands | 10–29 bills 15.25, 30–39 15.50, 40–49 15.75, 50+ 16.00 THB per bill |
| Bonus by volume | `store_structure_bonus` | 30–39 bills +30 THB, 40+ bills +50 THB a day |
| Daily guarantee floor | `store_structure_guarantee`; seed comments | A day below the floor is topped up: ANL 700, COMPANY 600 THB |
| Effective dates per store | `store_structure_period` (store, structure, start, end) | Store 09707: ANL 700 from 1–15 Feb, COMPANY 600 from 16–28 Feb 2026 |
| Per-rider override | `store_structure_driver_period` + `_log` | One rider keeps an old structure at a store |
| Incomes | 11 tables of the same shape (`driver_income_fuel`, `_rental`, `_extra`, `_diligence`, `_insurance`, `_guarantee`, `_contribution_bonus`, …) | Fuel allowance, diligence bonus |
| Deductions | 7 tables (`driver_expense_advance`, `_sim`, `_outfit`, `_rental`, `_insurance`, `_security_deposit`, `_other`) | Advance with 5% default interest plus bank fee, per-rider override |

Weak points: base rates and tax are constants; each new income or deduction type needs a new table and code; there is no period close (recalculation deletes and re-inserts); amounts mix DECIMAL and float64.

## Proposed handling (P-20, option 1)

1. **Pay scheme on the driver**, effective-dated: per unit (bill, drop, trip), per day, monthly salary, or a combination.
2. **Rate cards as data**, versioned, reviewed and published like nova-sg-all's contract terms, each with an effective period. Scope resolves most specific first: rider override → site/store → customer → tenant default, the same inheritance as Settings.
3. **Components** a rate card can hold, each a row, never a table per type:
   - unit price with volume bands (tiers);
   - bonus bands;
   - daily or period guarantee floor (top-up to a minimum);
   - allowances (fuel, extra work, diligence);
   - deductions (advance and its interest, vehicle rental or rent-to-own instalment, insurance, uniform, deposit, SIM);
   - withholding tax and other statutory rates as dated settings, never constants.
4. **Calculation** per period from approved work records (bills or drops from the hirer's file, attendance from Phase 2). Each pay line keeps the rate-card version it used. Money in minor units.
5. **Period life cycle**: open → calculated → approved → closed. A correction after close is an adjustment line in the next period, never a destructive recalculation.
6. **Leave calculation** (entitlement, paid and unpaid days, deductions) arrives with attendance and pay, in the same later phase.
7. **Parity tests**: replay anonymised SWP structures (tiers, bonuses, guarantee floors, store periods, rider overrides) and compare totals with swp-account-go before any tenant switches.

## What Phase 1 already does for it

Customers and sites exist (MVP-02) for rate-card scope; drivers have a stable ID independent of citizen ID; tenant export templates (D-17) can carry work data to an outside payroll tool until the pay module exists.
