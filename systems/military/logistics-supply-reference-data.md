# Logistics Supply Reference Data

**Project:** Project Sovereign  
**Canonical Path:** systems/military/logistics-supply-reference-data.md  
**Document ID:** MIL-LOG-DATA-001  
**Document Type:** Military logistics reference-data specification  
**Status:** Required data model and sourcing rules established; numeric capacity values and physical conversion records remain unpopulated pending sourced evidence  
**Historical Baseline:** 1 January 1955  
**Technology Horizon:** At least 2026, extensible beyond

## 1. Purpose and authority

This document defines the versioned reference data required to instantiate Military Logistics and Sustainment without hard-coding assumptions or inventing physical quantities. It is a supporting reference-data specification, not a second Logistics rules engine.

- Logistics owns demand calculation, supply planning, prioritisation, allocation, shipment coordination and delivery reconciliation.
- The Good and Service Catalogue owns canonical product identities, product classes and canonical units.
- Military Organisation owns unit size, authorised formation structure and formation-specific establishment.
- Equipment owns technical definitions, equipment assignments, compatibility and consumption parameters.
- Inventory owns physical stock quantities, reservations, locations, custody, receipts, issues, transfers, consumption and losses.
- Production owns manufacturing process definitions, capacity, execution and output.
- Movement and Transportation owns carrier movement, route traversal, progress and arrival.
- Infrastructure / Installations and Facilities owns facility identity, condition, shared capabilities and physical facility capacity.
- Government owns procurement authority, policy and budget approval; Economy owns commercial orders and financial transactions.

A reference-data record may describe a conversion or capacity, but must not duplicate or override live state owned by these systems.

## 2. Established canonical planning rules

### 2.1 Standard Stock per Unit Size per Day

The following generic planning factors are established and must remain unchanged unless a separately approved design decision versions them:

| Unit Size | Standard Stock per Unit Size per Day |
|---|---:|
| Brigade | 4 |
| Battalion | 2 |
| Company | 2 |
| Platoon | 1 |
| Section | 1 |

These values are abstract planning quantities, not physical units. They must never be labelled as tonnes, litres, rounds, rations, kilograms or individual goods.

### 2.2 Emergency request thresholds

| Supply category | Emergency trigger |
|---|---:|
| Ammunition | Available stock below 50% of the authorised one-day category baseline |
| Fuel | Available stock below 50% of the authorised one-day category baseline |
| Basics (Food & Hydration) | Available stock below 30% of the authorised one-day category baseline |

The denominator is the receiving Unit's authorised requirement for one day in the relevant category, derived from its current applicable formation, personnel, equipment, task and approved standard profile. The baseline definition and source versions must be recorded for every threshold evaluation.

For this calculation:
- Stock already reserved or otherwise committed is unavailable and is excluded from immediately available stock.
- Stock in transit is not available at the receiving Unit until confirmed receipt is committed through Inventory.
- The same stock must never count as available at both source and destination.
- The threshold triggers emergency classification only. It does not guarantee allocation, procurement, production, a feasible route or delivery.
- The comparison is strictly **below** the configured percentage; equality does not trigger the emergency condition.

## 3. Generic-to-physical conversion model

### 3.1 Principle

Physical demand is derived from formation-specific requirements and the actual relevant equipment/personnel/task profile, then mapped to compatible catalogue products through versioned mappings. A generic Standard Stock quantity is not itself a physical inventory holding.

The conversion process must:
1. Determine the receiving Unit's applicable formation, authorised personnel/equipment establishment, current task profile and relevant time basis.
2. Calculate its authorised one-day requirement for the requested supply category from approved, versioned requirement definitions.
3. Resolve the category into eligible physical Catalogue products, using product identity, canonical unit, compatibility, intended use and applicable substitution restrictions.
4. Calculate physical quantities using explicit, versioned conversion/requirement records with traceable source definitions.
5. Check actual stock, reservations, condition, location, availability and procurement/production eligibility through Inventory, Economy, Production and Government interfaces.
6. Preserve the generic planning quantity and the physical product quantities as linked but distinct records.
7. Reconcile issued, delivered, consumed, returned, lost and damaged quantities without double counting.

A mapping may allocate a generic category to multiple physical products. It must not assume a universal conversion across every Unit, equipment configuration, ammunition type, food/water product or fuel type.

### 3.2 Conversion record

Each approved military supply conversion record must contain, at minimum:

| Field | Requirement |
|---|---|
| conversion_id | Stable immutable identity |
| version | Immutable version for every outcome-affecting change |
| supply_category_id | Ammunition, Basics, Fuel, or a future approved category |
| unit_size_or_formation_scope | Applicable Unit size, formation definition, or explicit scope |
| personnel_profile_ref | Relevant authorised/current personnel basis where applicable |
| equipment_profile_ref | Relevant equipment configuration and quantities where applicable |
| task_profile_ref | Task/mission consumption profile where applicable |
| requirement_definition_refs | Approved definitions used to derive demand |
| catalogue_product_id | Canonical physical product |
| canonical_unit_id | Catalogue unit for physical quantity |
| quantity_expression_or_factor | Explicit formula/factor with named inputs and dimensional semantics |
| time_basis | Explicit requirement interval, including the one-day baseline where applicable |
| compatibility_constraints | Equipment, platform, recipient, storage or use restrictions |
| substitution_policy_ref | Approved substitutions, if any |
| valid_from / valid_until | Simulation-time validity interval |
| source_refs | Historical, technical, doctrinal or scenario evidence |
| confidence_status | Verified, estimated, provisional, disputed or unsupported |
| approval_status | Draft, reviewed, approved, retired |
| approval_provenance | Approver/decision record and effective time |

The expression language must be deterministic, unit-aware and auditable. Arbitrary executable expressions or silent dimensional coercions are not permitted.

### 3.3 Fuel-specific requirements

Fuel mappings must resolve to a compatible physical product and its canonical unit. Compatibility must account for the receiving equipment/platform's fuel specification and any explicit approved alternatives. If quantities are converted between volume and mass, the conversion must reference the applicable product density and conditions/version. Fuel products must not be treated as universally interchangeable.

### 3.4 Ammunition-specific requirements

Ammunition mappings must resolve to actual compatible ammunition products, not only a broad category or calibre label. Applicable weapon/equipment compatibility, authorised load, consumption basis, packaging, and any modelled restrictions must be explicit. Generic demand must not cause a second generic ammunition deduction when the same physical rounds are consumed.

### 3.5 Basics-specific requirements

Basics mappings must specify the relevant food and potable-water products and the personnel/time basis. Food and hydration may have different physical units and consumption profiles; a single generic Basics category must not obscure those distinct product-level requirements. Nutrition, packaging, storage, spoilage and environmental constraints apply where the underlying systems model them.

### 3.6 Missing or invalid conversion

If a required conversion is absent, expired, ambiguous, dimensionally invalid, or incompatible with the receiving Unit, the result must be unknown, unsupported, or infeasible with a reason code. The simulation must not create a default physical quantity, silently select an incompatible product, or treat generic stock as physical inventory.

## 4. Supply Base Unit capacity reference data

A Supply Base Unit is a Combat Base Unit configured with a Supply Depot. Its capacity definition must preserve separate dimensions.

### 4.1 Handling throughput

- Measure in tonnes loaded/unloaded per hour, as established by the current design.
- Represent the applicable loading and unloading throughput separately where the facility/equipment can differ by operation.
- Record product/category restrictions, operating conditions, concurrent-operation assumptions, staffing/equipment dependencies, effective period and source.
- For an operation requiring both loading and unloading, calculate completion using the applicable operation-specific constraints; where both stages are sequential, the slower applicable rate constrains throughput, with stage times calculated explicitly rather than blindly taking a minimum when quantities or stage requirements differ.
- Actual usable throughput may be constrained by condition, staffing, power, access, safety restrictions, congestion and other owned state. A nominal capacity is not proof of current availability.

### 4.2 Payload capacity: Ammunition and Basics

Payload capacity applies to the Depot's Ammunition and Basics stocks, separate from Bulk Fuel.

The capacity definition must state its physical basis and constraints. Mass and volume may both matter. Product packaging, segregation, hazardous-material requirements, stacking, storage conditions and incompatibilities may impose additional constraints. Until a canonical capacity basis and actual values are sourced, the fields remain unpopulated; do not infer capacity from Handling throughput.

### 4.3 Bulk Fuel capacity

Bulk Fuel capacity applies to Fuel products and is separate from Payload capacity.

The record must define the capacity unit and the applicable product/unit conversions. If mass-based, product-specific density and its provenance are required where conversion from volume is needed. If volume-based, the volume unit and any temperature/measurement assumptions must be defined. Tank count, tank capacity, product segregation, unusable reserve, transfer rate and current usable capacity belong in the appropriate facility/equipment/live-state records, not in an invented universal value.

### 4.4 Depot capacity record

Each supply depot capacity profile must contain:

- Stable ID and immutable version.
- Depot/Supply Base Unit configuration reference.
- Separate Handling, Payload and Bulk Fuel capacity definitions.
- Explicit numeric value and canonical unit for each approved capacity; null/unsupported until evidence exists.
- Operation type, product/category scope, concurrent-use assumptions and limiting constraints.
- Required equipment, staffing, infrastructure and operating-state references.
- Validity interval, source references, confidence status, review status and approval provenance.
- Links to live facility/Inventory state used to calculate current available capacity.

Capacity values are not inferred from unit size alone unless a sourced and approved class-level rule explicitly establishes that relationship.

## 5. Supply Column carrying-capacity reference data

A Supply Column is a physical mobile Unit, not an abstract teleportation or shipment mechanism. Its carrying capacity should derive from its actual assigned and serviceable vehicle/equipment composition where possible.

Each supply column capacity profile must contain:

- Stable ID/version and Supply Column or configuration-class reference.
- Vehicle/equipment instance or type references and assigned quantities.
- Payload mass limit and canonical mass unit where supported.
- Cargo volume limit and canonical volume unit where supported.
- Any vehicle-space, pallet, standard-load, axle/load distribution or other constraint if the relevant equipment model supports it.
- Separate constraints for fuel, ammunition, Basics and any restricted goods.
- Compatibility, hazardous cargo, packaging, segregation and load-distribution constraints.
- Personnel/crew and equipment readiness prerequisites.
- Effective interval, source references, confidence status and approval provenance.

The feasible load is constrained by every applicable dimension. Mass and volume are not interchangeable; a load must satisfy all applicable limits. If the underlying equipment data does not support a capacity dimension, the result for that constraint is unsupported rather than unlimited.

A configured class profile may provide a versioned baseline, but actual carrier capacity must account for the composition, condition, configuration and mission load of the assigned vehicles. Capacity must not be counted twice across concurrent assignments.

## 6. Source and evidence policy

Numeric reference values must be supported by traceable evidence. Appropriate evidence may include contemporary military manuals, tables of organisation and equipment, technical vehicle/equipment manuals, national procurement specifications, logistics doctrine, engineering data, and documented scenario assumptions.

Each value must carry:
- Source title/identifier, publisher/issuing authority, publication date and applicable page/table/section where available.
- Historical period, country/service, equipment or formation scope, units and interpretation notes.
- Confidence status and reviewer/approval provenance.
- Any conversion assumptions, uncertainty bounds or differences between nominal, maximum, rated and operationally usable capacity.

Do not silently apply modern equipment capacity to a 1955 configuration, or apply one nation's doctrine to another without an explicit applicability rule. Where evidence conflicts, retain the conflicting source records and make the selected interpretation explicit. A provisional estimate may be used only if explicitly labelled as an estimate and approved for a stated scenario/use; it must never be presented as verified historical canon.

## 7. Versioning, effective dates and deterministic resolution

- Outcome-affecting definitions are immutable once used by a committed simulation event; changes create a new version.
- Resolution selects the approved version valid at the relevant simulation time and applicable to the receiving formation/equipment/product.
- Every calculated demand, emergency threshold, allocation and capacity decision records the definition versions and input-state references used.
- Historical results remain reproducible after reference data changes.
- Conflicting equally applicable definitions return an explicit conflict requiring resolution; arbitrary precedence is prohibited.
- Reference data must be available to Player and AI through the same authoritative rules.

## 8. Required validation and conformance tests

1. Standard Stock factors remain generic planning quantities and are not written into physical Inventory as tonnes, litres, rounds or rations.
2. A one-day baseline is derived from the applicable authorised formation/personnel/equipment/task definitions and records the definition versions used.
3. Exactly 50% of Ammunition/Fuel baseline does not trigger a strictly-below threshold; a value below it does.
4. Exactly 30% of Basics baseline does not trigger a strictly-below threshold; a value below it does.
5. Reserved stock is excluded from immediately available stock for emergency evaluation.
6. In-transit stock is excluded from receiving-unit availability until confirmed Inventory receipt.
7. A missing or incompatible product conversion returns an explicit unsupported/infeasible result without generating stock.
8. Multi-product mappings reconcile generic demand allocation to product quantities without double counting.
9. Fuel compatibility and any mass-volume conversion use the approved product-specific definition and version.
10. Generic ammunition demand does not duplicate physical round consumption.
11. Depot Handling, Payload and Bulk Fuel are evaluated as separate constraints.
12. Sequential loading and unloading time is computed from each stage's quantity and applicable rate.
13. A Supply Column load satisfying mass but exceeding volume is rejected, and vice versa where both limits are supported.
14. An assigned carrier's actual composition/condition constrains capacity; unavailable vehicles do not contribute nominal capacity.
15. Simultaneous shipments cannot reserve the same capacity or stock twice.
16. A changed definition does not retroactively alter previously committed simulation outcomes.
17. Every numeric value is traceable to a source or explicitly approved scenario estimate.
18. Player and AI produce the same result for equivalent state, configuration, authority and inputs.

## 9. Outstanding data records

The following records are deliberately not populated with invented numbers:

- Approved physical conversion mappings for Ammunition, Basics and Fuel by applicable formation/equipment/personnel/task profile.
- Supply Depot Handling throughput values and operation-specific rates.
- Payload capacity values and approved mass/volume basis.
- Bulk Fuel capacity values, units and applicable density/unit conversions.
- Supply Column carrying-capacity values and the capacity dimensions supported by actual carrier/equipment definitions.
- Initial source dataset and value-by-value historical applicability/confidence review.

These are data acquisition and approval tasks, not permission to substitute arbitrary defaults.

## 10. Related specifications

- [Logistics and Sustainment](logistics-and-sustainment.md)
- [Military Cross-System Interface Contracts](../../technical/data-schemas/military-interface-contracts.md)
- [World and Command Interface Contracts](../../technical/data-schemas/world-and-command-interface-contracts.md)
- [Military Interface Audit and End-to-End Vertical Slice](../../technical/architecture/military-interface-audit-and-vertical-slice.md)
- [Equipment and Materiel System](equipment-and-material-system.md)
- [Inventory System](../inventory/inventory-system.md)
- [Movement and Transportation](../world/movement-and-transportation-system.md)
- [Good and Service Catalogue](../../reference/economy/good-and-service-catalogue.md)
