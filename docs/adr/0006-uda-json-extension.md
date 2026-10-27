# ADR 0006: Tenant-Defined Fields as a JSON Extension Owned by Each Module

Status: **Proposed**
Date: 2026-10-28
Decision owner: Nova Logistics engineering owner
Related: [UDA design](../07-tenant-customization-uda.md), [data model](../10-data-model.md), D-06, D-07

## Context

Each tenant must add, edit and remove its own fields on drivers, vehicles and documents, with types free text, date, number, checkbox and radio, required or optional (D-06), while the product keeps a balanced set of standard fields (D-07). nova-sg-all designed (not built) the same capability: definitions in a Customization module, values kept by the business module, stable IDs, no per-tenant DDL.

## Decision

- A `customization` module owns `custom_field_definitions` (per tenant and entity): stable field ID, key, labels TH/EN, type, options with stable option IDs, required, default, help, section, order, validation rules, visible/editable roles, filterable, status (active/archived), version.
- Each business module stores values in a `custom_values JSON` column on its own row, keyed by field ID. The module validates every write against the current definitions through the Customization interface.
- Filterable fields get a generated column or a multi-valued index created by a migration template, never by tenant action at runtime. Phase 1 limit: 10 filterable UDA fields per entity per tenant (to validate).
- Types: text, long text, number, date, yes/no, checkbox list (multi-choice), radio (single choice), dropdown. Changing type is allowed only between compatible types.
- Remove = archive (values kept, hidden). Hard delete only when no record holds a value. Adding "required" later marks old records incomplete instead of blocking them.
- Import templates, export columns, grid columns and OpenAPI schemas are generated from definitions.

## Alternatives

| Alternative | Benefit | Why not |
| --- | --- | --- |
| EAV table (one row per value) | Easy to index each value | Slow grids and exports; many joins |
| Per-tenant ALTER TABLE | Native columns | Unbounded schema drift; migrations per tenant; risky |
| Fixed spare columns (`custom1..20`) | Simple | Untyped, unlabelled, runs out |

## Consequences

Fast reads of a whole record; flexible per tenant. Filtering on non-indexed JSON fields is slower and is limited to small result sets or background exports.

## Acceptance and revisit criteria

Accept after the S2 spike shows: definition CRUD, validation on write, grid filter on an indexed UDA field over 10,000 drivers under 300 ms, and import/export round trip.
