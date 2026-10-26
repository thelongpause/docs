# Logistics Supply Reference Data

**Project:** Project Sovereign  
**Canonical Path:** systems/military/logistics-supply-reference-data.md  
**Document ID:** MIL-LOG-DATA-001  
**Document Type:** Military logistics reference-data specification  
**Status:** Required data model established; ration and variant-specific vehicle evidence is recorded in MIL-LOG-SRC-001 but remains unapproved. No generic physical conversion, depot throughput, depot capacity or Supply Column capacity values are approved; source-specific evidence requires scan-level verification and applicability review before use  
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

### 2.1 Retired Standard Stock factors

The following legacy planning factors may appear in older Military documentation:

| Unit Size | Legacy Standard Stock per Unit Size per Day |
|---|---:|
| Brigade | 4 |
| Battalion | 2 |
| Company | 2 |
| Platoon | 1 |
| Section | 1 |

These factors are **retired from authoritative simulation calculations**. They are not physical units and must not drive inventory, category demand, bulk delivery, replenishment, emergency thresholds or product allocation. They may be retained only as non-operative historical/documentation context until all references are migrated.

The active Player-facing supply quantities are bulk quantities in a category, normally expressed as mass. The physical simulation resolves these quantities to eligible Catalogue products using requirement profiles, available source stock, product unit mass, compatibility and allocation priorities. Product-level physical quantities remain authoritative in Inventory.

### 2.2 Emergency request thresholds

| Supply category | Emergency trigger |
|---|---:|
| Ammunition | Available physical category stock below 50% of the authorised one-day category baseline |
| Fuel | Available physical category stock below 50% of the authorised one-day category baseline |
| Basics (Food & Hydration) | Available physical category stock below 30% of the authorised one-day category baseline |

The authorised one-day baseline is derived from the receiving Unit's approved physical requirement profile, current relevant personnel/equipment/task inputs and the applicable interval. The comparison uses a common category mass basis where the eligible products have valid mass data. If the category cannot be reconciled to a common physical basis, the threshold result is unknown/unsupported rather than guessed.

The comparison is strictly **below** the configured percentage; equality does not trigger. Reserved or committed stock is excluded from immediately available stock. In-transit stock is not available at the destination until Inventory confirms receipt.

## 3. Bulk-to-physical supply allocation model

### 3.1 Principle

The Player manages three broad supply categories: Ammunition, Fuel and Basics (Food & Hydration). Bulk category quantities are normally expressed by mass for ordering, transport and allocation. They are a management abstraction, not an additional physical stock item.

At the source and destination, Inventory records actual canonical Catalogue products and quantities. Supply allocation distributes a bulk category delivery across eligible physical products according to the receiving Unit's shortages, authorised holdings, demand profile, explicit priorities and source availability. This is the retained purpose of Supply Conversion: **bulk-to-physical allocation at delivery**, not conversion of an abstract Standard Stock factor into invented goods.

The allocation process must:
1. Resolve the receiving Unit's approved requirement profile and current eligible shortages for the category.
2. Resolve the source's actual available, unreserved, usable physical products in that category.
3. Exclude products incompatible with the receiving Unit's equipment, intended use, fuel requirements or Basics profile.
4. Apply explicit allocation priorities or proportions from the requirement profile; do not distribute cargo equally by default.
5. Determine physical item quantities from the allocated mass and each product's approved unit mass, respecting indivisible item quantities and canonical units.
6. Ensure the total mass of allocated products does not exceed the delivered bulk category mass or the source stock available.
7. Record the physical cargo manifest and Inventory movements so stock is not simultaneously available at source, in transit and at destination.
8. Leave unallocated cargo explicitly at its valid source/in-transit holding, or mark the request partially fulfilled according to the delivery order. Never invent a product to absorb excess mass.

Bulk category allocation does not transform one physical product into another. Product transformation, if required, belongs to the authoritative Production/Inventory process. Physical quantities retain canonical Catalogue IDs and units before and after delivery.

Ammunition allocation must be compatible with weapons carried by the receiving Unit. Basics is one Player-facing category, but food and potable water remain distinct physical products. Fuel remains compatible with the relevant engines/equipment and is not treated as universally interchangeable.

### 3.2 Requirement profile and product allocation record

Each approved requirement/allocation profile must contain, at minimum:

| Field | Requirement |
|---|---|
| profile_id / version | Stable identity and immutable version |
| effective period / applicability | Date range, nation/service, formation or equipment scope where relevant |
| recipient applicability | Unit, formation class or explicit rule selector |
| supply_category | Ammunition, Fuel or Basics |
| eligible product IDs/classes | Canonical Catalogue IDs or constrained product classes |
| requirement basis | Personnel, equipment, task, activity, authorised holding or approved standard profile |
| required quantity / interval | Physical quantity, canonical unit and time/event basis |
| unit mass | Approved product mass per canonical item/unit where needed for bulk mass reconciliation |
| allocation priority | Explicit ranking or proportion, where more than one product is eligible |
| compatibility rules | Weapon/ammunition, fuel/equipment and Basics suitability constraints |
| source and provenance | Source, page/table, units, applicability, confidence and approval |
| missing-data behaviour | Unknown/unsupported/infeasible outcome; no generated stock |

A profile may allocate a category to multiple physical products. Product unit mass must be explicit and versioned when needed to reconcile item counts with bulk cargo mass. The definition must distinguish net product mass from packaged/gross transport mass where the difference is material. Do not infer mass from product names or treat projectile-only mass as complete-round mass.

### 3.3 Mass, units and cargo reconciliation

- Bulk order and transport quantities use a declared mass unit, with tonnes as the standard Logistics display/handling unit unless a specific interface defines otherwise.
- Physical Inventory quantities continue to use each Catalogue product's canonical unit. A product may be stocked as each, round, pack, litre, kilogram or another supported unit.
- Product unit mass provides the conversion needed to calculate that product's contribution to cargo mass. It does not change the product's canonical inventory unit.
- Total cargo mass is the sum of each physical line's quantity multiplied by its applicable transport unit mass.
- Vehicle/depot payload limits apply to the appropriate total cargo mass, subject to any additionally supported volume, packaging or compatibility constraints.
- Bulk category mass must reconcile to the mass of the allocated physical products within the defined rounding/indivisibility rules.
- Any unallocated remainder remains explicit. Do not force allocation beyond eligible demand or source availability simply to exhaust a bulk order.
- Missing, ambiguous or conflicting unit mass data returns an unresolved/unsupported result. Do not treat missing mass as zero.

### 3.4 Fuel-specific requirements

Fuel profiles must resolve to products compatible with the receiving equipment and intended use. Product canonical units and mass basis must remain explicit. If fuel is stored/consumed by volume but transported or capacity-limited by mass, an approved product-specific density and unit-conversion definition is required. Do not use a universal density or infer that all fuel products are interchangeable.

### 3.5 Ammunition-specific requirements

Ammunition profiles must identify ammunition products compatible with the weapons and equipment actually carried by the receiving Unit. Product identity must distinguish materially different ammunition natures, calibres, fuzes, charges, packaging or variants when those differences affect compatibility, use, mass or handling.

An authorised basic load is not automatically a daily expenditure rate. Combat owns firing/expenditure events; Inventory reconciles the physical stock reduction. Logistics uses confirmed expenditure and approved demand profiles to request replenishment.

### 3.6 Basics-specific requirements

Basics remains one high-level Player-facing category for Food & Hydration. Its physical allocation can include separate food and potable-water products because their units, mass, storage, transport and availability differ.

Keep the model lightweight:
- Quantity and sufficiency against an approved personnel/task/condition requirement.
- Broad ration type only where operationally meaningful.
- Optional quality grade only where supported by evidence and a modest, bounded downstream effect.
- Operating-condition/task modifiers only where an approved profile supports them.

Do not model calories, macronutrients, vitamins, individual meals, food preparation or individual nutritional composition. A reported calorific value cannot be converted into physical stock mass. Specialised ration types do not automatically imply superior quality.

### 3.7 Missing or invalid data

If the requirement profile, product mapping, unit mass, compatibility rule or source availability is missing or conflicting, return an explicit unknown, unsupported, infeasible or partially fulfilled result. Do not generate products, assume a zero mass, silently substitute incompatible goods or invent quantities.

A bulk delivery may be accepted as a category-level order while physical fulfilment remains partial or unresolved; the system must expose the reason and preserve all source stock and cargo states.

## 4. Supply Base Unit capacity reference data

A Supply Base Unit is a Combat Base Unit configured with a Supply Depot. Its capacity definition must preserve separate dimensions.

### 4.1 Handling throughput

- Measure Handling Rate in tonnes loaded or unloaded per hour.
- Derive effective rate from four inputs: available personnel, assigned logistics equipment, cargo characteristics/category, and the Unit's operational condition.
- A simple configurable model is:

`Effective Handling Rate = Personnel/Equipment Base Rate × Cargo Factor × Operational Factor`

- Personnel contribution is based on personnel present and available for handling, not automatically the entire Unit headcount.
- Equipment contribution depends on equipment type, quantity, capability and serviceability.
- Cargo factors distinguish meaningful differences such as Ammunition, packaged Basics, bulk Fuel and mixed cargo.
- Operational factors may reduce throughput when the Unit is engaged, suppressed or otherwise operationally impaired.
- Loading and unloading rates may differ and must be recorded separately when evidence or equipment characteristics justify it.
- Coefficients and modifiers require traceable evidence or explicitly approved scenario assumptions. Do not invent values and present them as historical facts.
- Do not model individual workers, loading bays, warehouse layouts, queues or detailed shifts. This is intentionally a Unit-level capability model.

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

1. Legacy Standard Stock factors are not used to create physical inventory, category demand, bulk quantities or emergency baselines.
2. A one-day physical category baseline is derived from the applicable authorised formation/personnel/equipment/task definitions and records the definition versions used.
3. Exactly 50% of Ammunition/Fuel baseline does not trigger a strictly-below threshold; a value below it does.
4. Exactly 30% of Basics baseline does not trigger a strictly-below threshold; a value below it does.
5. Reserved stock is excluded from immediately available stock for emergency evaluation.
6. In-transit stock is excluded from receiving-unit availability until confirmed Inventory receipt.
7. A missing requirement profile, product eligibility mapping or unit mass returns an explicit unsupported/infeasible result without generating stock or treating missing mass as zero.
8. Multi-product allocation reconciles the delivered bulk mass to physical product quantities without double counting.
9. Allocation never exceeds eligible shortage, source availability or delivered category mass; any remainder remains explicit.
10. Fuel compatibility and any mass-volume conversion use the approved product-specific definition and version.
11. Generic Ammunition allocation includes only products compatible with the receiving Unit's weapons and equipment.
12. Generic category cargo is not counted as additional stock alongside the physical products representing it.
13. Depot Handling, Payload and Bulk Fuel are evaluated as separate constraints.
14. Handling throughput responds to available personnel, assigned logistics equipment, cargo category and operational condition using approved coefficients/modifiers.
15. Engagement/suppression affects handling only through the configured operational modifier; no loading-bay or individual-worker simulation is required.
16. Sequential loading and unloading time is computed from each stage's quantity and applicable rate.
17. A Supply Column load satisfying mass but exceeding a supported volume limit is rejected, and vice versa.
18. An assigned carrier's actual composition and serviceability constrain capacity; unavailable vehicles do not contribute nominal capacity.
19. Simultaneous shipments cannot reserve the same capacity or stock twice.
20. A changed definition does not retroactively alter previously committed simulation outcomes.
21. Every numeric value is traceable to a source or explicitly approved scenario estimate.
22. Player and AI produce the same result for equivalent state, configuration, authority and inputs.
23. Basics requirements do not depend on calorie or nutrient simulation; a reported calorific value cannot be converted into physical stock quantity.
24. Ration type and quality affect outcomes only when an approved profile defines meaningful suitability or a bounded downstream effect; specialised does not automatically mean superior.

## 9. Outstanding data records

The following records are deliberately not populated with invented numbers:

- Approved physical requirement profiles for Ammunition, Fuel and Basics, scoped by applicable formation/personnel/equipment/task/activity where needed.
- Canonical product unit masses and, where relevant, separate net and gross/packaged transport mass.
- Explicit product eligibility, compatibility and allocation-priority data for multi-product category deliveries.
- Supply Depot Handling Rate coefficients/modifiers, derived from personnel, logistics equipment, cargo category and operational condition.
- Payload capacity values and approved mass/volume basis.
- Bulk Fuel capacity values, units and applicable density/unit conversions.
- Supply Column carrying-capacity values and supported capacity dimensions based on actual carrier/equipment definitions. EXT-004 and EXT-005 in MIL-LOG-SRC-001 contain candidate capacities for special-purpose US CCKW-353 tank bodies and two CCKW cargo-truck variants. They are not depot capacities or complete Supply Column capacities and remain unapproved pending source and applicability review.
- Initial source dataset and value-by-value historical applicability/confidence review.

These are data acquisition and approval tasks, not permission to substitute arbitrary defaults.

## 10. Related specifications

- [Logistics and Sustainment](logistics-and-sustainment.md)
- [Logistics Supply Source Register](logistics-supply-source-register.md)
- [Military Cross-System Interface Contracts](../../technical/data-schemas/military-interface-contracts.md)
- [World and Command Interface Contracts](../../technical/data-schemas/world-and-command-interface-contracts.md)
- [Military Interface Audit and End-to-End Vertical Slice](../../technical/architecture/military-interface-audit-and-vertical-slice.md)
- [Equipment and Materiel System](equipment-and-material-system.md)
- [Inventory System](../inventory/inventory-system.md)
- [Movement and Transportation](../world/movement-and-transportation-system.md)
- [Good and Service Catalogue](../../reference/economy/good-and-service-catalogue.md)
