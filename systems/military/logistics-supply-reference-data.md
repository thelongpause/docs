# Logistics Supply Reference Data

**Project:** Project Sovereign  
**Canonical Path:** systems/military/logistics-supply-reference-data.md  
**Document ID:** MIL-LOG-DATA-001  
**Document Type:** Military logistics reference-data specification  
**Status:** Bulk-to-physical allocation model established; physical requirement profiles, product unit masses, Handling Rate coefficients and depot/carrier capacity values remain unapproved. Source-specific evidence requires scan-level verification and applicability review before use  
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

At the source and destination, Inventory records actual canonical Catalogue products and quantities. Supply allocation resolves a bulk category order into eligible physical products according to the receiving Unit's shortages, authorised holdings, demand profile, explicit priorities and source availability. The physical manifest is resolved during shipment preparation/loading, no later than dispatch, so carrier payload and compatibility can be checked. Receipt adds those manifested products to destination Inventory. This is the retained purpose of Supply Conversion: **bulk-to-physical allocation**, not conversion of an abstract Standard Stock factor into invented goods.

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

A profile may allocate a category to multiple physical products. Product unit mass must be explicit and versioned when needed to reconcile item counts with bulk cargo mass. Use one canonical simulation mass per Catalogue product for all Logistics cargo calculations. Do not maintain separate net/gross, packaging, pallet or container mass. Do not infer mass from product names or treat projectile-only mass as the mass of a different complete product.

### 3.3 Mass, units and cargo reconciliation

- Bulk order and transport quantities use a declared mass unit, with tonnes as the standard Logistics display/handling unit unless a specific interface defines otherwise.
- Physical Inventory quantities continue to use each Catalogue product's canonical unit. A product may be stocked as each, round, pack, litre, kilogram or another supported unit.
- Product unit mass provides the conversion needed to calculate that product's contribution to cargo mass. It does not change the product's canonical inventory unit.
- Total cargo mass is the sum of each physical line's quantity multiplied by its applicable transport unit mass.
- Vehicle/depot payload limits apply to the sum of Catalogue simulation masses, subject to any explicitly defined volume or compatibility constraints. Packaging and pallet mass are outside the model.
- Bulk category mass must reconcile to the mass of the allocated physical products within the defined rounding/indivisibility rules.
- Any unallocated remainder remains explicit. Do not force allocation beyond eligible demand or source availability simply to exhaust a bulk order.
- Missing, ambiguous or conflicting unit mass data returns an unresolved/unsupported result. Do not treat missing mass as zero.

### 3.4 Fuel-specific requirements

Fuel demand is calculated from assigned equipment's fuel type, consumption rate and expected operating activity. Do not use a universal fuel-per-Unit factor. Fuel products must be compatible with the receiving equipment. Use defined tank capacity where available; otherwise leave the tank limit unspecified until the Equipment record provides it.

Use the Catalogue's canonical simulation mass for cargo accounting. Do not add packaging, pallet or container mass. Keep fuel storage in the unit used by the tank, normally litres. Convert volume to mass only when needed and only when the Catalogue provides a conversion value.

### 3.5 Ammunition-specific requirements

Ammunition allocation is based on compatible weapons and equipment actually carried by the receiving Unit, authorised holdings/basic loads where defined, explicit orders and confirmed expenditure. An authorised basic load is not automatically a daily expenditure rate. Combat owns firing/expenditure events; Inventory reconciles physical stock reduction.

For routine bulk allocation, prioritise: (1) explicit player/commander priority, (2) products below authorised load, (3) products with confirmed recent expenditure, then (4) remaining eligible compatible demand. Do not invent a universal daily ammunition consumption rate. If no compatible product or unit-mass record exists, leave that line unresolved and deliver other eligible products.

### 3.6 Basics-specific requirements

Basics remains one Player-facing category for Food & Hydration, with food and potable water represented as distinct physical Catalogue products.

Use these **provisional gameplay baselines** when no more specific national, formation or task profile exists:

- Food: **2 kg per person per day**.
- Potable water: **5 litres per person per day** under routine temperate conditions.
- Hot conditions or strenuous activity: increase the water baseline by **50%**. If both apply, use the higher single modifier rather than stacking them; cap the combined modifier at 2× the routine baseline.

These are simplified gameplay assumptions, not universal historical claims. More specific profiles may override them. If present personnel are known but task allocation is not, use all present personnel for Basics demand. Food and water remain separate physical products even when ordered together under Basics.

Keep the model to quantity and sufficiency. Do not model calories, macronutrients, vitamins, individual meals, food preparation or individual nutritional composition. Quality grades and special ration effects are out of scope for the initial implementation unless a clear operational need is established.

### 3.7 Missing or invalid data

If the requirement profile, product mapping, unit mass, compatibility rule or source availability is missing or conflicting, return an explicit unknown, unsupported, infeasible or partially fulfilled result. Do not generate products, assume a zero mass, silently substitute incompatible goods or invent quantities.

A bulk delivery may be accepted as a category-level order while physical fulfilment remains partial or unresolved; the system must expose the reason and preserve all source stock and cargo states.

## 4. Supply Base Unit capacity reference data

A Supply Base Unit is a Combat Base Unit configured with a Supply Depot. Keep three separate dimensions: handling throughput, dry/general storage capacity for Ammunition and Basics, and bulk Fuel capacity.

### 4.1 Handling throughput

Use this simple initial gameplay model. These are **provisional gameplay defaults**, not claims about universal historical performance. They are editable configuration values and should not block implementation.

- Base manual handling rate: **0.25 tonnes per available handling person per hour**.
- Equipment contribution: use the serviceable assigned equipment's handling-rate value where defined. For a simple initial model, use the greater of the personnel-derived rate and the assigned equipment's combined rated throughput; do not add both values together, which would double-count the same handling work.
- Cargo factor: Ammunition **0.75**; Basics **1.0**; bulk Fuel **1.0**; mixed cargo **0.7**.
- Operational factor: normal **1.0**; engaged **0.5**; suppressed **0.25**. If engaged and suppressed, use the lowest applicable factor rather than multiplying penalties repeatedly.
- Loading and unloading use the same rate initially. Add separate rates only when a specific equipment profile requires it.

Formula:

`Effective Handling Rate = max(Personnel Rate, Equipment Rate) × Cargo Factor × Operational Factor`

If there are no available handling personnel and no serviceable handling equipment, throughput is zero. A unit's full headcount is not assumed available for handling: use the personnel assigned as available for the task, or 10% of present personnel as the initial fallback when no task allocation exists. Round durations up to the next simulation time step; never round throughput up.

This deliberately does not simulate individual workers, loading bays, warehouse layouts, queues, packaging, pallets or shifts.

### 4.2 Storage and payload capacity: Ammunition and Basics

Use the configured capacity of the Supply Depot or its assigned storage equipment when present. If no explicit capacity exists, use a provisional class default of **100 tonnes** for a Supply Base Unit's combined Ammunition and Basics storage. This is a gameplay fallback, not a historical claim. Current occupied stock reduces available capacity.

Do not track packaging or pallet mass. Use the canonical Catalogue item's simulation mass as its cargo mass. Do not infer a depot's storage capacity from its handling rate.

### 4.3 Bulk Fuel capacity

Use the actual capacity of assigned tanks, fuel bowsers, tank vehicles or fixed fuel-storage equipment when defined. If no capacity exists, use a provisional class default of **20,000 litres** for a Supply Base Unit's bulk Fuel storage. This is a gameplay fallback, not a historical claim. Current usable Fuel stock reduces available capacity. Fuel capacity is separate from general Ammunition/Basics storage.

Where the Catalogue defines Fuel by mass rather than volume, use the Catalogue's canonical conversion/density value if available. If it is absent, keep Fuel storage and delivery in litres for that product rather than inventing a density.

### 4.4 Depot capacity record

The implementation needs only:

- Depot/Supply Base Unit reference.
- Handling rate and current operational factor.
- Combined Ammunition/Basics storage capacity and current occupied amount.
- Bulk Fuel capacity and current occupied amount.
- Assigned personnel/equipment inputs.
- Value status: sourced, provisional gameplay default, or explicitly configured.
- Effective date/version where definitions change.

Do not create separate records for loading bays, package types, pallet layouts, shifts or individual handlers.

## 5. Supply Column carrying-capacity reference data

A Supply Column is a physical mobile Unit. Its capacity derives from its actual assigned, serviceable vehicles and their Equipment definitions.

- Use each vehicle's canonical payload capacity and applicable cargo compatibility.
- Add the capacities of vehicles actually assigned and available for the shipment; exclude vehicles that are damaged, detached, committed elsewhere or otherwise unavailable.
- Use canonical Catalogue item simulation mass for cargo accounting. Do not add a separate packaging or pallet allowance.
- If an assigned vehicle lacks a payload value, use a provisional gameplay fallback of **2.5 tonnes per generic cargo truck** only when the vehicle is explicitly classified as a generic cargo truck and no more specific value exists. Mark the result provisional. Do not apply this fallback to tankers, passenger vehicles, aircraft, ships or specialist carriers.
- If volume or a specific compatibility constraint is defined for the vehicle/product, respect it. Otherwise the initial model constrains cargo by payload mass and compatibility only; missing volume data does not block routine cargo.
- Capacity cannot be counted twice across simultaneous shipments. Vehicle availability and load state must be checked at dispatch.

This is sufficient for the initial game. Detailed axle loads, packaging, pallets, load distribution and vehicle-space accounting are out of scope unless a later mechanic demonstrates a meaningful gameplay need.

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

These are follow-on refinements, not blockers to implementing Logistics:

- Populate Catalogue simulation mass and units for products used by the initial playable slice. Do not track packaging or pallet mass.
- Add more specific national/formation requirement profiles incrementally. The generic Basics baseline is 2 kg food and 5 litres potable water per person per day.
- Add fuel consumption/tank data and ammunition authorised-load data to relevant equipment/product profiles as those entries are built. Do not invent universal daily ammunition expenditure or fuel consumption rates.
- Populate product eligibility and compatibility for the initial equipment set. Unresolved lines must not prevent other eligible products from being delivered.
- The provisional Handling Rate, storage, bulk Fuel and generic cargo truck capacity values in Sections 4 and 5 are usable initial defaults. More specific equipment/depot values override them.
- EXT-004, EXT-005 and EXT-006 in MIL-LOG-SRC-001 remain historical research evidence only. They may refine particular equipment records later but are not prerequisites for the initial system.
- Historical source verification and detailed constraints are optional refinement tasks. Do not expand the runtime model into packaging, pallet, bay, shift or individual-worker tracking.

The system distinguishes provisional gameplay values from verified historical values, while continuing to operate with provisional values where a safe generic rule exists.

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
