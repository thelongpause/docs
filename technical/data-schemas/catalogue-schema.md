# Project Sovereign: Catalogue Schema

**Status:** Draft logical schema  
**Authority:** `reference/economy/good-and-service-catalogue.md`  
**Depends on:** Shared measurement and unit-conversion contract

## 1. Purpose and Boundary

This document defines the logical relational schema for canonical product identity and product measurement conventions. It does not define market prices, physical stock, production results, financial values, or operational availability. Those remain owned by their respective systems.

The Catalogue is the authoritative source for product identity and canonical units. Every other domain references a Catalogue entry by `catalogue_id`; no domain creates a parallel product identity.

The schema is logical rather than final migration DDL because the canonical identifier type, simulation-date representation, and unit-registry contract must be established centrally before migrations are generated.

## 2. Entity Relationship Overview

```text
catalogue_entry
  ├── catalogue_alias
  ├── catalogue_classification
  ├── catalogue_designation
  ├── good_definition
  ├── service_definition
  ├── designed_component_definition
  └── armament_definition

measurement_unit
  ├── catalogue_entry.unit_of_measure_id
  ├── good_definition physical property units
  └── service_definition (uses catalogue_entry.unit_of_measure_id)

unit_conversion
  ├── source_unit_id
  └── target_unit_id
```

The class-specific definition tables are optional one-to-one extensions. A Catalogue entry has exactly one primary economic class and must have exactly the corresponding class-specific extension where the class requires one. The primary identity remains in `catalogue_entry`.

## 3. `catalogue_entry`

One row represents one canonical product or service definition.

| Field | Logical type | Required | Rule |
|---|---|---:|---|
| `catalogue_id` | Stable identifier | Yes | Primary key; immutable |
| `canonical_name` | Text | Yes | Non-empty; display text is not identity |
| `description` | Text | Yes | Product definition |
| `physical_form` | Controlled value | Yes | `physical` or `non_physical` |
| `primary_economic_class` | Controlled value | Yes | `Good`, `Service`, `Designed Component`, or `Armament` |
| `category` | Controlled reference/value | Yes | Valid for the primary class |
| `status` | Controlled value | Yes | Catalogue lifecycle state |
| `unit_of_measure_id` | Foreign key | Yes for quantitative entries | The single canonical unit for the Catalogue entry, including quantitative Services |
| `introduced_date` | Simulation date | Conditional | First date the definition is valid |
| `retired_date` | Nullable simulation date | No | Must not precede introduction |
| `parent_entry_id` | Nullable self-reference | No | Parent product/family where applicable |
| `metadata` | Structured optional data | No | Descriptive extensions only; not a replacement for core columns |

### Constraints

- `catalogue_id` is unique and stable.
- `retired_date`, when present, cannot precede `introduced_date`.
- `parent_entry_id` must reference an existing entry and must not create a circular parent chain.
- Every quantitative entry must reference a defined unit.
- A class/physical-form rule must be enforced: Goods, Designed Components, and Armaments are physical; Services are non-physical under the current classification framework.
- The Goods category rule is class-specific. `Staple`, `Luxury`, and `Industrial` categories apply only where the primary economic class is `Good`. Other classes use their own defined category vocabulary.
- A Strategic Resource designation is not a primary economic class.

Cross-row class/extension consistency may be enforced by application validation or a database constraint/trigger, but the invariant must be tested and cannot depend on UI validation.

## 4. `catalogue_alias`

Stores alternative names without duplicating product identity.

| Field | Logical type | Required | Rule |
|---|---|---:|---|
| `catalogue_alias_id` | Stable identifier | Yes | Primary key |
| `catalogue_id` | Foreign key | Yes | References `catalogue_entry` |
| `alias` | Text | Yes | Non-empty |
| `normalised_alias` | Text | Yes | Used for deterministic lookup and duplicate detection |
| `source_or_note` | Text | No | Provenance where useful |

A Catalogue alias never becomes an alternative foreign-key identity. Uniqueness policy for normalised aliases must be decided explicitly: globally unique aliases are simplest, but if collisions are permitted, lookup must return an ambiguity rather than silently select a product.

## 5. `catalogue_classification` and `catalogue_designation`

Represent classifications and designations as controlled relationships, not a free-form list.

### `catalogue_classification`

| Field | Logical type | Required | Rule |
|---|---|---:|---|
| `catalogue_id` | Foreign key | Yes | References Catalogue entry |
| `classification_code` | Controlled reference | Yes | Canonical classification |
| `source_or_note` | Text | No | Optional provenance |

Composite primary key: `(catalogue_id, classification_code)`.

### `catalogue_designation`

| Field | Logical type | Required | Rule |
|---|---|---:|---|
| `catalogue_id` | Foreign key | Yes | References Catalogue entry |
| `designation_code` | Controlled reference | Yes | Canonical designation |
| `source_or_note` | Text | No | Optional provenance |

Composite primary key: `(catalogue_id, designation_code)`.

The initial controlled vocabulary must include `STRATEGIC_RESOURCE` as a designation, where applicable. It must not appear as a primary economic class.

## 6. Class-Specific Extensions

Class-specific tables contain only attributes relevant to that class. They must not duplicate common identity, classification, lifecycle, or canonical unit fields.

### 6.1 `good_definition`

| Field | Logical type | Required | Notes |
|---|---|---:|---|
| `catalogue_id` | Primary/foreign key | Yes | One-to-one with a Good Catalogue entry |
| `material_form` | Controlled value | Conditional | Solid, liquid, gas, particulate, discrete object, or supported form |
| `material_composition` | Structured optional data | No | Only where composition is modelled |
| `mass_per_unit` | Quantity + unit reference | Conditional | Must be dimensionally compatible |
| `volume_per_unit` | Quantity + unit reference | Conditional | Must be dimensionally compatible |
| `density` | Quantity + unit reference | Conditional | Required only where relevant conversion or simulation needs it |
| `quality_dimensions` | Structured references | No | Prefer typed attributes where used by core rules |
| `storage_requirements` | Structured references | No | Validated storage constraints |
| `handling_requirements` | Structured references | No | Validated handling constraints |
| `shelf_life` | Defined duration | Conditional | Requires explicit time convention |
| `hazard_classification` | Controlled reference | No | Where modelled |
| `durability_class` | Controlled reference | No | Where applicable |
| `instance_tracking_required` | Boolean | Yes | Whether individual instances must be tracked |
| `default_condition` | Controlled reference | Conditional | Only where a default is explicitly defined |

Do not require mass, volume, density, or individual-instance tracking for every Good. Requirements depend on the product definition and modelled mechanics.

### 6.2 `service_definition`

| Field | Logical type | Required | Notes |
|---|---|---:|---|
| `catalogue_id` | Primary/foreign key | Yes | One-to-one with a Service Catalogue entry |
| `delivery_mode` | Controlled value/reference | Conditional | Where delivery mode matters |
| `duration_basis` | Defined time basis | No | For duration-dependent services |
| `capacity_basis_unit_id` | Foreign key | No | Unit for relevant provider capacity |
| `quality_dimensions` | Structured references | No | Service quality attributes |
| `location_constraints` | Structured references | No | Geographic requirements |
| `provider_capabilities` | Structured references | No | Capability requirements |
| `consumption_semantics` | Controlled/validated definition | No | How service delivery/consumption is represented |

The canonical unit for a quantitative Service is `catalogue_entry.unit_of_measure_id`. Do not add a second authoritative service-unit field to `service_definition`. Service provider capacity may use a different unit where appropriate, but that capacity unit belongs to the provider/capacity model and must be explicitly related to the service-delivery basis through a validated rule or conversion; it does not replace the Catalogue entry's canonical unit.

A Service must never be assigned physical stock merely because it has a quantitative service unit. A transport service quantity, for example, is distinct from the cargo transported.

### 6.3 `designed_component_definition`

| Field | Logical type | Required | Notes |
|---|---|---:|---|
| `catalogue_id` | Primary/foreign key | Yes | One-to-one with a Designed Component entry |
| `design_id` | External/domain reference | No | References authoritative Research/design data |
| `interface_requirements` | Structured references | No | Compatibility requirements |
| `performance_attributes` | Structured references | No | Typed attributes used by simulation |
| `manufacturing_constraints` | Structured references | No | Applicable constraints |
| `revision_id` | External/domain reference | No | References authoritative design/product revision |

Research remains authoritative for technology progression and design-development state.

### 6.4 `armament_definition`

The Armament extension must carry the applicable typed attributes required by military simulation, including any Catalogue-owned common product properties and references to authoritative designs/specifications. Military effectiveness, readiness, inventory availability, and operational state must not be stored as Catalogue attributes.

The precise field set should be completed against the Military and Research specifications before the Armament schema is treated as implementation-complete.

## 7. Units and Conversion

### 7.1 `measurement_unit`

The shared unit registry should provide at least:

| Field | Logical type | Required | Rule |
|---|---|---:|---|
| `unit_id` | Stable identifier | Yes | Primary key |
| `unit_code` | Canonical code | Yes | Unique, immutable code |
| `unit_name` | Text | Yes | Human-readable name |
| `measurement_dimension` | Controlled reference | Yes | Mass, volume, count, time, energy, power, distance, compound unit, service unit, etc. |
| `is_discrete` | Boolean | Yes | Whether the unit convention requires discrete quantities |
| `status` | Controlled value | Yes | Active or retired under shared rules |
| `definition_version` | Stable version/reference | Yes | Supports reproducibility of conversions |

### 7.2 `unit_conversion`

| Field | Logical type | Required | Rule |
|---|---|---:|---|
| `unit_conversion_id` | Stable identifier | Yes | Primary key |
| `source_unit_id` | Foreign key | Yes | Source unit |
| `target_unit_id` | Foreign key | Yes | Target unit |
| `conversion_type` | Controlled value | Yes | Fixed, property-dependent, or other explicitly supported type |
| `conversion_definition` | Validated definition | Yes | Exact rule and parameters |
| `valid_from` | Simulation date/version | Conditional | Validity start |
| `valid_until` | Nullable simulation date/version | No | Validity end |
| `provenance` | Text/reference | No | Source or rationale |

`unit_conversion_id` identifies one immutable conversion definition/version. A change to its formula, parameters, property dependencies, precision semantics or validity interpretation requires a new ID; historical references remain available for reproducibility.

Conversion must be rejected when dimensions are incompatible or required product properties are unavailable. Density-dependent mass/volume conversion must use the applicable product/condition value rather than an implicit universal factor. Any noncanonical unit used for recipe, throughput, reservation, stock or transaction quantities must reference the exact applicable conversion when required by the owning schema.

The exact precision, rounding, conversion execution, and compound-unit representation are defined by the shared measurement contract, not improvised independently by Catalogue, Recipe, Inventory, or Economy.

## 8. Lifecycle and Historical Validity

Catalogue retirement controls whether a definition is available for new use under the applicable rules. It does not delete or rewrite historical references.

Production Executions must retain the exact recipe version and Catalogue identities used. If a product definition changes in a simulation-relevant way, the change must be versioned or represented as a new Catalogue entry according to the Catalogue's identity rules. Historical production must remain interpretable even after an entry is retired.

The implementation must define whether descriptive edits are versioned separately from simulation-relevant changes. That decision must not be left to incidental row updates.

## 9. Cross-Domain Contract

- Production recipe input/output lines reference `catalogue_id` and compatible units.
- Production Orders reference a Catalogue entry when the order targets a specific output.
- Production Execution output rows retain their own `catalogue_id`, unit, quantity, output role, and execution relationship.
- Inventory stock tables accept only Catalogue entries whose physical form and class permit physical stock.
- Economy prices and transactions reference Catalogue identity and explicitly identify the quantity/price denominator.
- Population demand rules may reference consumption intervals, but they cannot redefine a Catalogue physical unit.
- Strategic Resource is an additional designation, not a separate product identity system.

## 10. Acceptance Tests

The Catalogue schema must pass at least these tests:

1. Reject duplicate `catalogue_id` values.
2. Reject an invalid primary economic class.
3. Reject class/physical-form combinations that violate the classification framework.
4. Reject a Service being registered as physical stock.
5. Reject `Staple`, `Luxury`, or `Industrial` as a Good category for a non-Good primary class.
6. Permit a Strategic Resource designation without changing primary economic class.
7. Reject a unit conversion with incompatible dimensions.
8. Reject a conversion requiring a product property when that property is unavailable.
9. Reject invalid lifecycle ranges.
10. Preserve historical references after retirement.
11. Ensure aliases never substitute for `catalogue_id` in foreign-key relationships.
12. Ensure changes to Catalogue units or attributes do not silently rewrite historical Production Execution quantities.
13. Require a quantitative Service to use `catalogue_entry.unit_of_measure_id` as its single canonical delivery unit, with no duplicate `service_unit_id` field in `service_definition`.

## Cross-Domain Review Invariants

- All domain foreign keys use `catalogue_id`; aliases and local product codes are never canonical references.
- A quantitative Service's canonical delivery unit is `catalogue_entry.unit_of_measure_id`. Provider capacity may use a different unit only through a validated service-delivery conversion or relationship.
- Unit conversion definitions must be versioned and must reject incompatible dimensions or missing product properties.
- A Catalogue entry's unit does not determine stock eligibility. Inventory accepts only entries whose physical form and class permit physical stock.
- Production recipe versions and committed executions preserve the Catalogue identity, unit and conversion/configuration references required to interpret historical quantities.
- Changes to product identity or simulation-relevant measurement semantics must not silently rewrite historical Inventory, Production or Economy records.

## Governing Principle

The Catalogue defines what a product is and how its quantity is canonically expressed. It does not define how much is in stock, how much it sells for, or what a production process yields.
