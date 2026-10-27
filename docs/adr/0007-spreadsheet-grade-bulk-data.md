# ADR 0007: Spreadsheet-Grade Grid and Asynchronous Import/Export Engine

Status: **Proposed** (grid library choice open: P-07)
Date: 2026-10-28
Decision owner: Nova Logistics engineering owner
Related: [product](../01-product-and-mvp.md), [architecture](../03-architecture.md#importexport-engine), D-04

## Context

Target customers run on Excel and Google Sheets today (D-04). TTP imports rider sheets with dozens of Thai header variants and syncs Google Sheets; SWP exports customer-specific Excel templates. nova-sg-all imports CSV only (2,000 rows, 1 MB). Competitors offer CSV templates but not grid editing or bulk document ingestion.

## Decision

- **Grid:** every list screen uses one virtualised data-grid component with sort, filter, column chooser, frozen columns, saved views, inline edit, multi-cell copy and paste from Excel, fill-down, undo and bulk actions. Working proposal: Glide Data Grid (MIT), because AG Grid's range selection, fill handle and range paste are Enterprise features; confirm in an S2 spike (P-07).
- **Bulk edit:** a paste or fill becomes one batch command validated server-side; the response returns per-cell errors; the batch is applied all-or-nothing by default.
- **Import:** xlsx and csv up to 50,000 rows per file (to validate). Steps: upload → detect sheet and header row → map columns (saved per tenant, with Thai aliases) → dry run with row errors → commit as a worker job → result file. Upsert by natural key (citizen ID or passport for drivers, plate + province for vehicles, code for others). Every touched row records the import batch ID, so a batch can be undone while no later change touched those rows.
- **Export:** the current view (filters and columns, including UDA) to xlsx or csv, built by the worker for large sets and delivered as a temporary object. Tenant-defined export templates (column order and headers) replace SWP-style customer templates.
- **Dates:** accept `dd/mm/yyyy` in BE or CE, ISO dates and Excel serials; a BE year is detected by range (> 2400) and converted at the edge.
- Library: excelize for xlsx (already used in SWP).

## Alternatives

| Alternative | Benefit | Why not |
| --- | --- | --- |
| Google Sheets sync as the primary editor | Familiar | Weak validation and access control; Sheets API quotas; keep as a Phase 2 connector |
| Synchronous import in the API | Simpler | Timeouts and memory spikes on big files |
| AG Grid Enterprise | Most complete grid | Licence cost per developer; revisit if Glide falls short |

## Consequences

The worker must be sized for imports; per-tenant job limits prevent one tenant starving others. Undo requires batch IDs on every bulk-written row.

## Acceptance and revisit criteria

Accept after S3–S4 show: 10,000-driver import under 5 minutes, a 200-row paste with mixed errors, undo of a batch, and export of 50,000 rows.
