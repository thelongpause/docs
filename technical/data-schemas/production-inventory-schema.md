# Project Sovereign: Production and Inventory Schema

**Status:** Draft logical schema  
**Authorities:** `systems/production/production-system.md`, `systems/production/production-processes-and-recipes.md`, and `systems/inventory/inventory-system.md`  
**Depends on:** Catalogue schema and shared measurement contract

## 1. Purpose

This document defines the relational data contract for recipe definitions, Production Orders, Production Executions, physical stock, reservations, and stock-changing transactions.

Production owns production planning and execution records. Production Processes and Recipes owns transformation definitions and immutable recipe versions. Inventory owns all authoritative physical stock quantities and stock changes.

The shared Production Execution transaction coordinates the physical outcome across Production and Inventory. It must not create competing ledgers or allow retries to duplicate stock movements.

This is a logical schema. Final migration DDL must use the identifier, simulation-time, measurement-unit, and transaction conventions established by the shared technical specifications.

## 2. Relationship Overview

```text
production_process
  └── recipe
       └── recipe_version
            ├── recipe_input ────── catalogue_entry
            └── recipe_output ───── catalogue_entry

industry_instance
  └── production_industry_assignment (where applicable)

canonical_facility (Installations and Facilities authority)
  └── production_facility (production capability extension; facility_id is both PK and FK)
       ├── facility_process_capability
       └── production_line (optional)

production_order
  ├── references recipe_version
  ├── references target Catalogue entry or explicit production basis
  ├── production_order_reservation ─── inventory_reservation
  └── production_execution
       ├── production_execution_input ── inventory_transaction_line
       ├── production_execution_output ─ inventory_transaction_line
       └── execution_commit/idempotency record

inventory_holding
  └── stock_lot
       ├── inventory_reservation
       └── inventory_transaction_line
            └── inventory_transaction
```

## 3. Recipe Definitions

### 3.1 `production_process`

One row defines a logical transformation type.

| Field | Required | Rule |
|---|---:|---|
| `process_id` | Yes | Stable primary identifier |
| `name` | Yes | Human-readable name |
| `description` | Yes | Transformation definition |
| `process_category` | Yes | Controlled category |
| `supported_modes` | Yes | Validated supported execution modes |
| `required_capabilities` | Conditional | Process-level capability requirements |
| `applicable_industry_types` | Conditional | Normalised relationship to Industry Type |
| `input_constraints` | No | Validated constraints, not a replacement for recipe input lines |
| `output_constraints` | No | Validated constraints, not a replacement for recipe output lines |
| `technology_requirements` | No | References to Research-owned technology |
| `environmental_constraints` | No | Validated conditions |
| `status` | Yes | Lifecycle state |
| `metadata` | No | Descriptive extension only |

Many-to-many requirements should use association tables when independently queried, versioned, or validated. Do not store foreign-key identities as unvalidated text arrays.

### 3.2 `recipe` and `recipe_version`

`recipe` represents the stable logical identity. `recipe_version` stores an immutable transformation definition.

**recipe**

| Field | Required | Rule |
|---|---:|---|
| `recipe_id` | Yes | Stable primary identifier |
| `process_id` | Yes | Foreign key to `production_process` |
| `name` | Yes | Human-readable name |
| `description` | No | Descriptive text |

**recipe_version**

| Field | Required | Rule |
|---|---:|---|
| `recipe_version_id` | Yes | Stable primary identifier for this immutable version |
| `recipe_id` | Yes | Foreign key to logical recipe |
| `version_label` | Yes | Human-readable or canonical version label |
| `production_mode` | Yes | Batch, continuous, discrete assembly, extraction, service, construction, or other supported mode |
| `basis_quantity` | Yes | Reference execution quantity or rate |
| `basis_unit_id` | Yes | Foreign key to measurement unit |
| `required_capabilities` | No | Validated requirements |
| `required_equipment` | No | Normalised equipment requirements where needed |
| `technology_requirements` | No | Research references |
| `operating_constraints` | No | Validated constraints |
| `valid_from` | Yes | Simulation validity start |
| `valid_until` | No | Optional simulation validity end |
| `status` | Yes | Draft, validated, active, retired, or approved lifecycle vocabulary |
| `provenance` | No | Source and assumptions |

Constraint: the logical recipe/version pair must be unique. Once a version is referenced by a committed execution, its simulation-relevant fields and child input/output rows must be immutable. A changed transformation requires a new version.

### 3.3 `recipe_input`

One row represents one input requirement for one recipe version.

| Field | Required | Rule |
|---|---:|---|
| `recipe_input_id` | Yes | Stable identifier |
| `recipe_version_id` | Yes | Foreign key to immutable version |
| `input_code` | Yes | Stable within the recipe version |
| `catalogue_id` | Yes when product/service input | Canonical Catalogue reference |
| `quantity` | Yes | Quantity relative to recipe basis |
| `unit_id` | Yes | Compatible with Catalogue unit and basis |
| `input_role` | Yes | Feedstock, component, consumable, fuel, utility, catalyst, auxiliary material, packaging, service input, or approved extension |
| `consumption_mode` | Yes | Consumed, incorporated, reusable, returned, service-consumed, or approved extension |
| `substitution_group` | No | Explicit permitted alternatives |
| `quality_constraints` | No | Grade, purity, specification, or condition |
| `minimum_quantity` | No | Validated minimum |
| `maximum_quantity` | No | Validated maximum |
| `notes` | No | Descriptive only |

Use a separate substitution association table if one input can have multiple permitted alternatives. A recipe must not treat a Service input as a physical stock item.

### 3.4 `recipe_output`

One row represents one defined output of one recipe version.

| Field | Required | Rule |
|---|---:|---|
| `recipe_output_id` | Yes | Stable identifier |
| `recipe_version_id` | Yes | Foreign key to immutable version |
| `output_code` | Yes | Stable within the recipe version |
| `catalogue_id` | Conditional | Required for Catalogue-defined product/service outputs; non-Catalogue outcomes require an explicit approved result type |
| `output_role` | Yes | Primary output, co-product, by-product, recovery, waste, or another defined role |
| `quantity_method` | Yes | Fixed, proportional yield, conversion yield, multi-output yield, conditional yield, discrete yield, or approved extension |
| `quantity` | Conditional | Required where the method specifies a fixed or reference quantity |
| `unit_id` | Yes for quantitative output | Compatible with the Catalogue entry or defined result |
| `calculation_definition` | Conditional | Versioned/validated parameters for yield or conditional calculations |
| `conditions` | No | Explicit execution conditions |
| `inventory_eligible` | Yes | Must agree with Catalogue physical form and Inventory rules |
| `notes` | No | Descriptive only |

A physical Catalogue output may be marked inventory-eligible only if its Catalogue entry is eligible for physical stock. A non-physical Service cannot be made stock-eligible by setting a flag. Process losses that are not physical materials must not be represented as fabricated stock items.

## 4. Production Facilities and Orders

### 4.1 `production_facility`

This is a Production-owned extension of a canonical Facility, not a second facility identity. The Installations and Facilities System owns the Facility record, including its stable identity, type, name, location/geometry references, ownership/operator/control relationships, shared physical condition, declared shared capabilities and common capacity definitions.

| Field | Required | Rule |
|---|---:|---|
| `facility_id` | Yes | Primary key and foreign key to the canonical Facility ID; never independently generated |
| `industry_id` | Conditional | Industry associated with this production capability, referencing Industry Data; model a separate association if one Facility serves multiple Industries |
| `industry_area_id` | No | Reference to an Industry Area owned by Industry Data |
| `production_eligibility_state` | Yes | Production-specific eligibility, such as eligible, restricted, suspended or ineligible; must not duplicate shared physical condition or Facility lifecycle state |
| `production_requirements` | No | Production-specific technology, infrastructure, utility, workforce and operating prerequisites, with authoritative references |
| `created_simulation_time` | Yes | Simulation time at which the Production extension was created |
| `last_updated_simulation_time` | Yes | Simulation time of the latest Production-owned change |

Do not copy the canonical Facility's type, display name, location, owner, operator, shared operational/condition state or generic capacity fields into this extension. Production obtains those values through the canonical Facility reference and its interface.

Production-specific throughput is represented separately in `facility_process_capability` and, where required, in `production_line`. A throughput value must identify its process or compatible process family, output or process basis, canonical unit, time basis, applicable conditions and source/version. It is not a duplicate of generic facility capacity and does not imply that output has been produced.

The Industry relationship is an operational/economic association, not ownership of the canonical Facility. If a facility can serve multiple Industries, use a normalised association rather than overwriting a single authoritative relationship.

### 4.2 `facility_process_capability`

One row declares that a canonical Facility, or an independently modelled production line within it, is configured to perform a defined Production Process. It does not establish that the capability is currently available or that a production order can execute.

| Field | Required | Rule |
|---|---:|---|
| `facility_process_capability_id` | Yes | Stable identifier, or a declared composite key if the implementation standardises that pattern |
| `facility_id` | Yes | Canonical Facility ID |
| `production_line_id` | No | Optional line reference; if set, the line must belong to the same Facility |
| `process_id` | Yes | Production Process reference |
| `capability_state` | Yes | Configured, restricted, suspended or retired under the declared lifecycle |
| `throughput_basis_id` | Conditional | Required when process-specific throughput is declared |
| `nominal_throughput` | Conditional | Production-specific throughput for the stated basis and interval |
| `throughput_unit_id` | Conditional | Canonical compatible unit |
| `throughput_period` | Conditional | Explicit simulation interval or rate basis |
| `valid_from` | Yes | Simulation validity start |
| `valid_until` | No | Optional simulation validity end |
| `configuration_version` | Yes | Version/provenance for simulation-relevant capability configuration |

Constraints:
- The same capability scope and validity interval must not have conflicting duplicate rows.
- Throughput values are not interchangeable across different output/process bases merely because their units share a dimension.
- Nominal throughput is not effective capacity, allocated capacity or actual output.
- Effective throughput is derived by Production from the configured capability and authoritative operating constraints.
- Changes to simulation-relevant capability must be versioned so completed executions remain reproducible.

### 4.3 `production_line`

A production line is optional and exists only where the facility is modelled with independently scheduled lines.

| Field | Required | Rule |
|---|---:|---|
| `production_line_id` | Yes | Stable primary identifier |
| `facility_id` | Yes | Foreign key to canonical Facility ID and Production facility extension |
| `name` | Yes | Production-line name; does not rename the canonical Facility |
| `production_eligibility_state` | Yes | Production-line-specific operating eligibility, distinct from parent Facility condition |
| `process_capabilities` | Conditional | Normalised relationship to supported processes; use association rows when queried or versioned independently |
| `installed_throughput` | Conditional | Process-specific nominal throughput, only where line-level throughput is modelled independently |
| `throughput_basis_id` | Conditional | Required with installed throughput; identifies process/output basis and compatible unit |
| `throughput_unit_id` | Conditional | Required with installed throughput |
| `throughput_period` | Conditional | Required with installed throughput |
| `maintenance_reference` | No | Reference to authoritative maintenance/condition state; do not duplicate it |

### 4.4 `production_order`

| Field | Required | Rule |
|---|---:|---|
| `production_order_id` | Yes | Stable primary identifier |
| `industry_id` | Yes | Requesting/responsible Industry |
| `facility_id` | No | Assigned facility |
| `production_line_id` | No | Assigned line; must belong to facility where both are set |
| `process_id` | Yes | Process to execute |
| `recipe_id` | Yes | Logical recipe |
| `recipe_version_id` | Yes | Exact selected immutable version |
| `target_output_catalogue_id` | Conditional | Required when completion is measured against a target product output |
| `production_basis_type` | Yes | Target output or recipe-defined input/throughput basis |
| `production_basis_reference` | Yes | Explicit basis definition/recipe line when not target-output based |
| `requested_quantity` | Yes | Quantity of the stated basis |
| `requested_unit_id` | Yes | Unit compatible with the stated basis |
| `priority` | Yes | Explicit scheduling priority |
| `order_state` | Yes | Controlled lifecycle state |
| `created_simulation_time` | Yes | Simulation time, not wall-clock timestamp |
| `earliest_start_time` | No | Simulation time |
| `deadline` | No | Simulation time |
| `allocated_capacity` | No | Derived or explicitly allocated capacity with unit and interval |
| `completed_quantity` | Yes | Quantity measured in the order's requested basis |
| `rejected_quantity` | Yes | Quantity measured in the same basis where supported |
| `cancelled_quantity` | Yes | Quantity measured in the same basis |
| `failure_reason_code` | No | Structured failure code |
| `last_updated_simulation_time` | Yes | Simulation time |

The target output or production basis must be explicit. For a target-output order, the target Catalogue entry and unit must be compatible with an output of the selected recipe version. For a throughput-based order, the referenced basis must exist in the selected recipe version and use a compatible unit.

Do not sum quantities of different co-products to calculate completion. Order progress is measured against the declared target output or production basis only.

### 4.5 `production_order_reservation`

Use an association table instead of treating `input_reservation_ids` as an unvalidated array.

| Field | Required | Rule |
|---|---:|---|
| `production_order_id` | Yes | Foreign key to order |
| `reservation_id` | Yes | Foreign key to Inventory Reservation |
| `purpose` | No | Explicit role of the reservation for the order |
| `created_simulation_time` | Yes | Simulation time |

Composite primary key: `(production_order_id, reservation_id)`. The reservation must refer to eligible stock and quantities appropriate to the recipe's required inputs. Exact matching may require an explicit input-allocation relation when a reservation supplies a particular recipe input line.

## 5. Production Execution Records

### 5.1 `production_execution`

One row represents one attempted logical execution. The record must distinguish pending, rejected, failed, and committed outcomes.

| Field | Required | Rule |
|---|---:|---|
| `production_execution_id` | Yes | Stable unique identifier and idempotency scope |
| `production_order_id` | Yes | Originating order |
| `facility_id` | Yes | Executing facility |
| `production_line_id` | No | Executing line |
| `process_id` | Yes | Process used |
| `recipe_version_id` | Yes | Exact immutable recipe version |
| `start_simulation_time` | Yes | Execution interval start |
| `end_simulation_time` | Conditional | Required when execution interval ends |
| `execution_state` | Yes | Prepared/pending, committed, rejected, failed, or approved lifecycle state |
| `requested_basis_quantity` | Yes | Basis quantity evaluated for this execution |
| `basis_unit_id` | Yes | Unit for basis quantity |
| `allocated_capacity` | Yes | Capacity used or allocated |
| `capacity_unit_id` | Yes | Capacity unit |
| `deterministic_input_snapshot` | Yes | Versioned references and resolved inputs needed to explain/reproduce result |
| `execution_result` | Conditional | Final outcome payload/reference |
| `failure_reason_code` | No | Structured failure |
| `committed_simulation_time` | No | Set only when committed |
| `payload_fingerprint` | Yes | Stable fingerprint of the execution request/inputs for idempotency checks |

The execution record is not itself a stock ledger. Authoritative stock effects are recorded in Inventory transactions and referenced by execution input/output rows.

### 5.2 `production_execution_input`

One row per resolved input for an execution.

| Field | Required | Rule |
|---|---:|---|
| `production_execution_input_id` | Yes | Stable identifier |
| `production_execution_id` | Yes | Parent execution |
| `recipe_input_id` | Conditional | Recipe input line, where applicable |
| `catalogue_id` | Conditional | Catalogue identity for product/service input |
| `quantity_requested` | Yes | Calculated required quantity |
| `quantity_committed` | Yes | Quantity actually committed as consumed/used |
| `unit_id` | Yes | Unit for the quantity |
| `consumption_mode` | Yes | Semantics from recipe version |
| `inventory_transaction_line_id` | Conditional | Link to authoritative stock consumption line when physical stock changes |
| `source_reservation_id` | No | Inventory reservation fulfilled by the input |
| `result_state` | Yes | Validated, consumed, not consumed, or other approved outcome |

Service inputs can be recorded for execution provenance but must not reference physical stock transactions unless a separate physical product is actually consumed.

### 5.3 `production_execution_output`

One row per calculated output, regardless of whether it becomes physical stock.

| Field | Required | Rule |
|---|---:|---|
| `production_execution_output_id` | Yes | Stable identifier |
| `production_execution_id` | Yes | Parent execution |
| `recipe_output_id` | Conditional | Source recipe output line |
| `catalogue_id` | Conditional | Required for Catalogue-defined output |
| `output_role` | Yes | Primary, co-product, by-product, recovery, waste, or defined role |
| `quantity_calculated` | Yes | Calculated result |
| `quantity_committed` | Yes | Quantity committed to its authoritative destination |
| `unit_id` | Yes | Unit for output |
| `inventory_transaction_line_id` | Conditional | Required for a physical stock output |
| `result_state` | Yes | Calculated, committed, rejected, non-stock result, or approved state |

Each output keeps its own Catalogue identity, unit, quantity, role, and execution link. The requested order quantity does not overwrite the quantities of co-products or by-products.

## 6. Inventory Records

### 6.1 `inventory_holding`

Represents a stock context, not a financial asset.

| Field | Required | Rule |
|---|---:|---|
| `inventory_holding_id` | Yes | Stable primary identifier |
| `catalogue_id` | Yes | Canonical Catalogue entry eligible for physical stock |
| `location_id` | Yes | Physical stock location |
| `owner_id` | Conditional | Stock owner |
| `custodian_id` | Conditional | Responsible custodian |
| `unit_id` | Yes | Canonical or compatible unit |
| `status` | Yes | Controlled holding state |
| `created_simulation_time` | Yes | Simulation time |
| `updated_simulation_time` | Yes | Simulation time |

`quantity_on_hand`, `quantity_reserved`, and `quantity_available` may be cached aggregates but must be reconcilable with lots and reservations. The authoritative quantities must not be independently mutated in conflicting records.

### 6.2 `stock_lot`

| Field | Required | Rule |
|---|---:|---|
| `stock_lot_id` | Yes | Stable primary identifier |
| `inventory_holding_id` | Yes | Parent holding |
| `catalogue_id` | Yes | Must agree with holding |
| `quantity_on_hand` | Yes | Non-negative physical quantity |
| `unit_id` | Yes | Compatible with Catalogue unit and holding |
| `location_id` | Yes | Current physical location |
| `owner_id` | Conditional | Current owner |
| `custodian_id` | Conditional | Current custodian |
| `condition_code` | Conditional | Supported condition |
| `quality_attributes` | No | Validated quality data |
| `origin_type` | Yes | Production, purchase, extraction, transfer, or approved origin |
| `origin_reference` | Yes | Stable reference to originating operation |
| `created_simulation_time` | Yes | Simulation time |
| `expiry_simulation_time` | No | Optional simulation expiry |
| `restrictions` | No | Explicit allocation/handling/legal constraints |

A lot split must create traceable child lot(s) or equivalent lineage. Lot merges are permitted only when all behaviourally relevant attributes remain compatible.

### 6.3 `inventory_reservation`

| Field | Required | Rule |
|---|---:|---|
| `reservation_id` | Yes | Stable primary identifier |
| `catalogue_id` | Yes | Must match reserved stock |
| `stock_lot_id` | Conditional | Required for lot-specific reservations |
| `inventory_holding_id` | Yes | Reserved holding |
| `quantity_reserved` | Yes | Positive quantity |
| `unit_id` | Yes | Compatible unit |
| `requesting_entity_id` | Yes | Requesting system/entity |
| `purpose_type` | Yes | Production, shipment, military supply, customer order, or approved purpose |
| `purpose_reference` | Yes | Stable reference to order/requirement |
| `priority` | Conditional | Explicit priority |
| `status` | Yes | Active, partially fulfilled, fulfilled, released, expired, or cancelled |
| `created_simulation_time` | Yes | Simulation time |
| `expires_simulation_time` | No | Optional expiry |
| `fulfilled_quantity` | Yes | Quantity fulfilled against reservation |

Active reservations must not exceed eligible stock. A reservation is a claim on stock, not stock itself.

## 7. Inventory Transaction Header and Lines

A single inventory transaction may change several products, lots, or stock locations. Use a header plus lines rather than one transaction row that can reference only one Catalogue product.

### 7.1 `inventory_transaction`

| Field | Required | Rule |
|---|---:|---|
| `inventory_transaction_id` | Yes | Stable primary identifier |
| `transaction_type` | Yes | Receipt, consumption, transfer, output, loss, adjustment, transit dispatch/receipt, or approved type |
| `transaction_state` | Yes | Prepared, committed, rejected, failed, or approved lifecycle state |
| `initiating_domain` | Yes | Production, Logistics, Economy, or other authorised domain |
| `initiating_reference` | Yes | Stable originating operation reference |
| `production_execution_id` | Conditional | Shared Production Execution reference for production commits |
| `idempotency_key` | Yes for retriable commands | Unique within the declared scope |
| `payload_fingerprint` | Yes for idempotent commits | Detects key reuse with different payload |
| `created_simulation_time` | Yes | Simulation time |
| `committed_simulation_time` | Conditional | Present only after commitment |
| `failure_reason_code` | No | Structured failure reason |

### 7.2 `inventory_transaction_line`

| Field | Required | Rule |
|---|---:|---|
| `inventory_transaction_line_id` | Yes | Stable primary identifier |
| `inventory_transaction_id` | Yes | Parent transaction |
| `catalogue_id` | Yes | Canonical physical product identity |
| `quantity_delta` | Yes | Signed stock change or explicit direction/quantity representation, selected consistently for all transaction types |
| `unit_id` | Yes | Compatible measurement unit |
| `stock_lot_id` | Conditional | Existing or newly created affected lot |
| `source_location_id` | Conditional | Source location |
| `destination_location_id` | Conditional | Destination location |
| `source_owner_id` | No | Source owner |
| `destination_owner_id` | No | Destination owner |
| `reservation_id` | No | Reservation fulfilled or released |
| `line_role` | Yes | Input consumption, primary output, co-product, by-product, recovery, waste, transfer out/in, or approved role |
| `related_line_id` | No | Related line for paired movements or lineage |

The final convention for signed quantity deltas versus explicit direction and positive quantity must be selected once and used consistently. Transfer and transit logic must ensure a physical quantity is not available at both origin and destination.

## 8. Shared Production Execution Transaction

### 8.1 Transaction Identity

A Production Execution has one stable idempotency identity. The transaction record stores a payload fingerprint that covers the order, recipe version, deterministic execution inputs, input allocations, and calculated output set.

- Same key, same payload: return or resolve to the same committed outcome.
- Same key, different payload: reject as an idempotency conflict.
- Different key: a distinct execution, subject to normal order and capacity validation.

A new retry is not a new Production Execution.

### 8.2 Commit Contract

The shared logical commit must:

1. Validate the order state, exact recipe version, requested output/basis, units, facility eligibility, and execution identifier.
2. Validate all input reservations or other explicitly supported allocation records.
3. Revalidate available quantities and reservations against authoritative Inventory state at commit time.
4. Validate the complete set of output lines, including each Catalogue identity, unit, quantity, and output role.
5. Commit input stock reductions and all physical output additions as one atomic logical operation.
6. Record Production Execution inputs/outputs and their Inventory transaction-line references.
7. Update order progress consistently with the committed execution.
8. Mark the execution and inventory transaction committed.
9. Publish success events only after commitment.

Production owns the operational execution record. Inventory owns stock changes. Neither may claim a successful result before the shared physical outcome is committed.

### 8.3 Failure and Recovery

If validation fails, no stock changes occur. If commit fails, the implementation must either roll back all affected stock changes or use a durable prepare/commit/recovery mechanism that prevents partial outcomes from being treated as committed.

If a successful commit response is lost, a retry resolves to the existing committed result. It must not re-consume inputs, re-register outputs, or update order completion twice.

A long-running process may have several Production Executions. Each committed stage has its own execution identity and transaction. Completion of one stage does not imply completion of a later stage.

The exact database or event-based implementation is a Technical Architecture decision; these invariants are mandatory regardless of implementation.

## 9. Quantity and State Invariants

1. Every physical quantity references a Catalogue entry eligible for stock and uses a compatible unit.
2. Quantity values are non-negative at the lot level; signed deltas, if selected, exist only in transaction lines.
3. Active reservations cannot exceed eligible stock.
4. Available stock is consistent with on-hand stock and active reservations.
5. In-transit stock is accounted for once and is not simultaneously available at origin and destination.
6. Inventory transactions are immutable after commitment; corrections use explicit compensating transactions rather than silent edits.
7. Each committed Production Execution has a complete, auditable input/output record.
8. Multi-output executions store every output independently, with its own Catalogue ID, unit, quantity, role, and execution link.
9. Production order completion is measured against the declared target output or production basis, never the sum of unlike output quantities.
10. Recipe versions referenced by committed executions remain available for historical interpretation.
11. Inventory never infers production completion from output stock alone.
12. Production never mutates stock outside the Inventory transaction interface.

## 10. Required Integration Tests

Before this schema is considered implementation-ready, tests must demonstrate:

- A recipe with two or more outputs produces separate output rows with independent quantities and units.
- A target-output order completes based on the selected target, not co-product totals.
- A throughput-based order resolves its basis to an explicit recipe input or other supported basis.
- A service output is recorded as a non-stock result and does not create an Inventory Holding or Stock Lot.
- A physical output is registered once and linked to its Production Execution.
- A Production facility extension cannot exist without a canonical Facility, and its `facility_id` is the same primary/foreign key rather than a separately generated ID.
- Production facility extensions do not store duplicate canonical Facility type, name, location, ownership/operator/control, shared physical condition/lifecycle or generic capacity fields.
- Process-specific throughput is tied to an explicit process/output basis, compatible unit, time basis and configuration version; it is not treated as generic Facility capacity.
- An input reservation cannot be consumed by two competing executions beyond its reserved quantity.
- Two simultaneous reservations cannot exceed eligible stock.
- A retry after successful commit does not duplicate any input consumption or output.
- Reusing an idempotency key with a different payload is rejected.
- Failure between preparation and commitment leaves no unintended partial stock outcome.
- A multi-stage process preserves separate execution and transaction identities for each committed stage.
- Save/load does not replay committed stock transactions.
- Transfer and in-transit logic never makes the same quantity available at two locations.
- A Catalogue unit change does not rewrite the unit or quantity recorded by a historical execution.

## Governing Principle

Production defines and records the transformation event. Inventory records the physical stock changes. A shared idempotent transaction links them, while the Catalogue provides the canonical identity and units for every product involved.
