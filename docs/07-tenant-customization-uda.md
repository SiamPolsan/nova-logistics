# Tenant Customization: UDA and Field Settings

**Status: proposed design, 28 October 2026** ([ADR-0006](adr/0006-uda-json-extension.md); D-06, D-07). Aligned with nova-sg-all's design (`docs/07-tenant-customization-and-plugins.md`): stable IDs, TH/EN labels, versioned definitions, no per-tenant DDL.

## What a tenant can do

1. **Add a field** to an entity: driver, vehicle, document (per document type), equipment item, inspection, customer site.
2. **Edit** its label (TH/EN), help text, section, order, options, required flag, default, validation, who can see and edit it, and whether it shows in the grid and can be filtered.
3. **Remove** it: the field is archived, hidden everywhere, values kept; restore brings it back. Hard delete only when no record holds a value.
4. **Adjust standard fields** (MVP-21): rename the label, hide an optional field, make an optional field required. Core identity fields (name, citizen ID/passport, plate, vehicle class) cannot be hidden.

## Field types

| Type | Stored as | Validation | Grid filter |
| --- | --- | --- | --- |
| Text | string ≤ 255 | max length, pattern | contains, equals |
| Long text | string ≤ 4,000 | max length | contains (not indexed) |
| Number | decimal string, scale 0–4 | min, max, scale | range |
| Date | ISO date | min, max; BE/CE input | range |
| Yes/no (single checkbox) | boolean | — | is true / false |
| Checkbox list (multi-choice) | array of option IDs | min/max selected | any of |
| Radio (single choice) | option ID | one of active options | any of |
| Dropdown | option ID | one of active options | any of |

Options have stable IDs; renaming an option keeps existing values; archiving an option keeps it on old records and hides it for new input.

## Storage

- `custom_field_definitions` (customization module): `tenant_id, id, entity, key, label_th, label_en, type, options JSON, required, default JSON, help_th, help_en, section, sort_order, rules JSON, visible_roles JSON, editable_roles JSON, filterable, show_in_grid, status, version, created_at, updated_at`.
- Values: `custom_values JSON` on the owning row (e.g. `drivers.custom_values`), keyed by field ID: `{"f_01H…": "XL", "f_01J…": ["opt_a","opt_c"]}`.
- Filterable fields: up to 10 per entity per tenant (to validate) use generated columns or multi-valued indexes added by a migration template keyed by slot, never by runtime DDL.

## Rules

- The owning module validates every write through the Customization interface against the definition version it read; a stale version is refused and retried.
- A field made required later does not block existing records; they show "incomplete" until edited.
- Type changes allowed: text ↔ long text; radio ↔ dropdown; number → text. Others need a new field and a migration of values by import.
- Import templates, export columns, grid columns, API schemas and PWA forms are generated from active definitions; archived fields are excluded from templates but kept in full exports when asked.
- Each tier may limit the number of UDA fields (pricing lever, P-09).

## Definitions are cached

Definitions per tenant and entity are cached in Redis with the version in the key ([ADR-0004](adr/0004-redis-as-cache-only.md)); a change bumps the version.
