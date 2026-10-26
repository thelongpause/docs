# Military Logistics and Sustainment System

**Project:** Project Sovereign  
**Canonical Path:** `systems/military/logistics-and-sustainment.md`  
**System:** Military Systems  
**Document Type:** Canonical Behavioural Specification  
**Document ID:** MIL-LOG-001  
**Status:** Canonical initial specification; implementation schemas, detailed formulas, and interface conformance tests remain subject to integration review  
**Starting World Date:** 1 January 1955  
**Technology Horizon:** At least 2026, extensible beyond

---

## 1. Purpose

The Military Logistics and Sustainment System governs how military forces obtain, hold, move, distribute, consume, recover, and replenish the material resources required to operate over time.

It connects military demand to the physical and economic world. Food, hydration, fuel, ammunition, medical supplies, spare parts, construction materials, clothing, and other goods must come from valid stocks, production, procurement, capture, transfer, or other explicitly authorised sources. They must be transported and received through physical processes before becoming available to the intended recipient.

**Core principle: military sustainment is a physical and economic process, not an abstract replenishment bonus.** A unit cannot consume goods that do not exist, use stock it cannot access, or receive supplies that have not completed a valid delivery process. Purchases, production, transport, receipt, reservation, issue, consumption, loss, and disposal are distinct state transitions owned by their appropriate systems.

Logistics must support routine peacetime supply, training, mobilisation, expeditionary operations, prolonged campaigns, naval and air operations, strategic reserves, contested supply lines, blockades, infrastructure disruption, emergency resupply, and recovery after combat. It must be useful from the 1955 starting world through later technologies without assuming modern logistics methods exist at the beginning of the simulation.

This specification defines behavioural rules and interfaces. It does not replace the authoritative rules of Inventory, Economy, Production, Transportation, Infrastructure, Military Organisation, Equipment, Command, Combat, Government, or the Good and Service Catalogue.

## 2. Design Principles

1. **Physical reality:** stock has a quantity, product identity, location, ownership, custody, condition where applicable, and availability state.
2. **One authoritative owner per fact:** Logistics owns military supply planning, demand aggregation, allocation decisions, and delivery coordination. Inventory owns physical stock and stock movements. Other systems retain their own authoritative state.
3. **Economic integration:** military demand must connect to government procurement and eligible companies, goods, services, production, markets, budgets, and trade rules.
4. **Product-level eligibility:** an Industry Type alone does not prove that a company can supply a requested good. Supplier eligibility depends on the canonical product, company capabilities, production outputs, available stock, location, contracts, permissions, and delivery feasibility.
5. **Movement takes time:** transport capacity, route length, terrain, infrastructure, weather where modelled, congestion, threat, loading, unloading, and transfer operations constrain delivery.
6. **No teleportation:** a purchase, allocation, reservation, dispatch, or order is not equivalent to delivery or receipt.
7. **Demand is not consumption:** a request does not consume stock. Only an authorised and resolved consumption event does.
8. **Scarcity is meaningful:** insufficient supply creates explicit shortfalls, prioritisation, rationing, substitutions where valid, and operational consequences. The system must not silently create missing goods.
9. **Supply is task-dependent:** the same stock may be adequate for one unit and inadequate for another because their personnel, equipment, tasks, operating tempo, and time horizon differ.
10. **Branches share principles, not identical consumption profiles:** land, naval, air, strategic, support, and specialist formations may require different products, handling methods, replenishment points, and consumption calculations.
11. **Player and AI parity:** AI-controlled nations use the same procurement, production, inventory, transport, allocation, and information rules as the Player.
12. **Determinism and auditability:** logistics transitions must be reproducible, save/load safe, and protected against duplicate delivery or consumption.
13. **Historical progression:** capabilities depend on available technologies, equipment, infrastructure, organisations, doctrine, and institutions at the simulation date.
14. **Operational consequences emerge from causes:** reduced effectiveness follows from specific shortages, condition, maintenance, fatigue, or access constraints rather than an unexplained global supply penalty.

## 3. Scope and Responsibilities

### 3.1 Included

- Military supply demand and forecasting.
- Routine and emergency resupply requests.
- Supply categories and product-specific requirements.
- Military procurement requests and economic sourcing interfaces.
- Supply allocation, prioritisation, rationing, and reservation coordination.
- Military supply depots, supply bases, forward supply points, and unit-level holdings.
- Supply columns and other configured logistics formations.
- Cargo loading, movement, transfer, delivery, unloading, and receipt coordination.
- Supply routes, throughput, transport capacity, and delivery reliability.
- Unit consumption, expenditure reporting, losses, spoilage, contamination, damage, and wastage where modelled.
- Fuel demand for vehicles, ships, aircraft, generators, and other equipment through the appropriate equipment/operational interfaces.
- Ammunition resupply, while ammunition expenditure and combat effects remain owned by Combat and Inventory interfaces.
- Maintenance and spare-parts supply, while maintenance actions and equipment serviceability remain owned by their relevant systems.
- Supply support for bases, installations, training, mobilisation, deployment, and reconstitution.
- Contested logistics, interdiction, blockades, route disruption, and protection of supply movements.
- Strategic reserves and reserve policy interfaces.
- Logistics reporting, forecasts, shortfall warnings, and readiness implications.
- AI logistics planning and acceptance tests.

### 3.2 Excluded or Owned Elsewhere

- Canonical identity, unit of measure, and classification of goods and services: Good and Service Catalogue.
- Industry Types and company/industry establishment definitions: Industry Data and Economy.
- Company production capability, production recipes, production execution, capacity, and output generation: Production and associated industrial systems.
- Prices, payment, budgets, taxes, market transactions, contracts, international trade, sanctions, and government fiscal authority: Economy, Government, and trade systems.
- Physical stock quantity, ownership, custody, reservation records, stock transfers, receipt, issue, consumption, and loss transactions: Inventory.
- Road, rail, port, airfield, waterway, pipeline, and other infrastructure identity and physical capacity: Infrastructure and Transportation, as defined by their specifications.
- Unit identity, branch, organisation, command relationship, and formation composition: Military Branches and Unit Organisation.
- Equipment identity, configuration, equipment instance, condition, compatibility, and serviceability: Equipment and Materiel.
- Equipment fuel burn, powertrain requirements, ammunition compatibility, and equipment-specific operating requirements: Equipment, Combat, and other applicable domain systems.
- Order authority, command hierarchy, command delay, and operational tasking: Doctrine, Command and Operations.
- Engagement resolution, firing, ammunition expenditure at the point of use, combat damage, and interdiction effects: Combat, with stock changes reconciled through Inventory.
- Personnel health, nutrition-related effects, fatigue, morale, and other personnel state: relevant Character, Medical, Population, or Military state owners. Logistics provides relevant shortage or exposure inputs; it must not duplicate those state variables.

## 4. Core Concepts

### 4.1 Supply Requirement

A Supply Requirement describes the quantity and type of a good or service needed by a unit, installation, task, activity, or logistics organisation over a stated interval or for a defined event.

A requirement should specify, as applicable:

- Canonical product identifier.
- Required quantity and unit of measure.
- Recipient and receiving location.
- Required-by time or acceptable delivery window.
- Purpose, such as routine sustainment, operational task, training, maintenance, mobilisation, or emergency.
- Priority and consequence of shortfall.
- Handling, storage, compatibility, environmental, or safety constraints.
- Permitted substitutes, if any.
- Source or quality constraints, if applicable.
- Confidence and provenance of forecasts when uncertainty is material.

A requirement is not a stock record, purchase order, shipment, or consumption event.

### 4.2 Supply Demand

Supply Demand is the time-bounded quantity a recipient is forecast or has requested. Demand can be forecast, requested, validated, approved, allocated, partially fulfilled, deferred, denied, cancelled, or expired. These states must be explicit.

Demand can arise from baseline personnel sustainment, equipment operation, combat expenditure, maintenance, construction, training, deployment, planned operations, recovery, or strategic stock targets.

Demand aggregation must preserve its underlying source and urgency. Combining requests for planning must not erase the recipient, purpose, delivery deadline, priority, or audit trail.

### 4.3 Supply Category

The Player-facing bulk supply model has exactly three top-level categories:

- **Ammunition:** compatible ammunition products, including distinct natures, calibres, charges, fuzes and grenades where represented as ammunition products.
- **Fuel:** specific fuel products compatible with the receiving equipment and intended use.
- **Basics (Food & Hydration):** food and potable water, which remain distinguishable physical Catalogue products beneath the shared Player-facing category.

These categories simplify routine bulk ordering, transport and replenishment. A category is not a physical product and is not a substitute for a Catalogue ID. Bulk category quantities are resolved into actual physical products and a cargo manifest during shipment preparation/loading, no later than dispatch. Inventory retains the authoritative physical quantities.

Other goods, including clothing such as field jackets, medical supplies, spare parts, engineering materials and specialist equipment, remain valid physical Catalogue products and may be stocked, procured, transported and issued through product-specific workflows. They are **not automatically forced into Ammunition, Fuel or Basics** merely to fit the three-category bulk model. The existence of such goods does not create additional top-level bulk categories unless a later explicit design decision adds one.

The Catalogue determines product identity, canonical unit and product classification. Equipment and Combat determine compatibility and use. Logistics may group eligible products into the three bulk categories for supply allocation, but must not create duplicate goods, make incompatible products interchangeable or treat the category abstraction as extra physical stock.

### 4.4 Supply Holding

A Supply Holding is an Inventory-backed quantity of a canonical product associated with a location, owner, and custody arrangement. Logistics may present military-specific views of holdings but must not independently maintain a competing authoritative quantity.

Holdings may exist at company warehouses, commercial distributors, government stores, strategic depots, military bases, forward supply points, mobile supply columns, ships, aircraft, vehicles, units, or other authorised locations.

### 4.5 Supply Node

A Supply Node is a logistics planning representation of a place where goods may be sourced, stored, transferred, handled, or delivered. It references the authoritative entity and location rather than duplicating them.

Examples include industrial suppliers, commercial warehouses, ports, railheads, depots, supply bases, forward distribution points, military installations, mobile supply units, and unit receiving locations.

Each node may expose relevant capabilities such as eligible products, available stock, storage capacity, loading/unloading throughput, transfer capability, security, operating status, and connection to transport networks. The owning systems remain authoritative for each capability and its state.

### 4.6 Supply Route and Supply Path

A Supply Route is a planned or configured path between nodes. A Supply Path is the currently feasible sequence of transport legs and transfer operations used for a specific shipment or supply task.

Routes may use roads, railways, navigable waterways, sea lanes, air routes, pipelines, or other transport modes where the simulation supports them. A route is not automatically traversable merely because its endpoints exist. Its feasibility depends on the transport system, infrastructure, cargo, available carriers, access rights, operating conditions, and current disruption.

### 4.7 Shipment and Delivery

A Shipment is a coordinated movement of specified cargo from a source to a destination through one or more transport legs. A Delivery is the process of tendering and transferring cargo to the intended receiving entity. Receipt is recorded only when the appropriate Inventory receipt transition succeeds.

A shipment can be planned, approved, reserved, prepared, loaded, dispatched, in transit, delayed, diverted, intercepted, damaged, partially delivered, delivered, returned, cancelled, or lost. Valid state transitions and terminal outcomes must be explicit.

### 4.8 Supply Unit and Supply Column

A **Supply Unit** is an organisational military combat-support formation, trained, equipped, staffed and commanded like other military Units, whose role includes holding and distributing supplies. When configured with a Supply Depot, the Supply Unit is a **Supply Base Unit**. Its Depot holds the military-facing stock categories Ammunition, Basics (Food & Hydration), and Fuel. It is distinct from the physical stock records that Inventory owns.

A **Supply Column** is a separate, physical mobile transport Unit tasked to carry supplies from a Supply Base to a drawing Unit, deliver and unload the supplies, then return to its originating or assigned supply base according to its task. A Supply Column is not the Supply Base Unit and does not replace the base's stored stock.

Supply Columns travel through the world as actual map units. Their dispatch, route, loading, travel, delivery/unloading, return, suspension and abort states must be explicit. They require valid personnel, equipment, fuel, carrying capacity, command/tasking and a feasible route. They may be delayed, rerouted, interrupted, attacked, damaged, captured or destroyed.

A Supply Column is not a teleportation service. Cargo remains Inventory-backed and must be reconciled through loading, in-transit custody, delivery/receipt, loss or return. The carrier's arrival does not itself complete delivery or issue goods to the drawing unit.

### 4.9 Supply Base Units and Supply Depots

A **Supply Base Unit** is a Combat Base Unit configured with a **Supply Depot**. This is a specialised configuration of the canonical Combat Base Unit, not a separate unit identity or parallel organisational class. The Combat Base Unit remains a military formation with identity, command, personnel, equipment, movement and deployment state; the Supply Depot is the configured supply-handling capability it provides.

A **Supply Base** may refer to the operational supply function at a site. A Supply Base Unit is mobile/deployable; a fixed depot or installation may provide supply-base functions without being a mobile formation. The model must preserve the distinction between a unit, its depot capability, the site/facility, and the physical Inventory holdings stored there.

A Supply Base Unit's handling, payload and bulk-fuel capacities are distinct. The model must not collapse them into one universal cargo-capacity field. Handling throughput is measured in tonnes loaded/unloaded per hour; Payload capacity applies to Ammunition and Basics; Bulk Fuel capacity applies to Fuel. The lower applicable handling rate governs the total loading/unloading completion time where the operation requires both stages.

## 5. Demand Generation and Forecasting

### 5.1 Sources of Demand

Demand may be generated by:

1. Personnel numbers and sustainment profiles.
2. Equipment inventory and expected operating hours, distance, sorties, patrols, or other activity.
3. Planned and actual movement, including transport operations.
4. Combat and training ammunition expenditure.
5. Maintenance forecasts, faults, scheduled servicing, and repair work.
6. Medical and casualty support requirements.
7. Construction, fortification, engineering, recovery, and deployment tasks.
8. Supply base and transport-unit operating requirements.
9. Strategic reserve targets and government policy.
10. Expected campaign duration, operational tempo, and contingency margins.

Each source must identify the owning system and its demand interface. Logistics may aggregate and forecast requirements but must not reimplement the source system's authoritative consumption or activity model.

### 5.2 Physical Requirements and Bulk Category Demand

Baseline requirements cover the configured routine sustainment of a Unit or installation. Activity-based requirements cover additional or different consumption caused by operating tempo, mission, equipment use, environmental conditions, or other explicitly modelled factors.

The Player-facing Logistics categories are:

- **Ammunition**
- **Fuel**
- **Basics (Food & Hydration)**

The Player may order, allocate and transport supply in bulk category quantities, normally expressed as mass (for example, tonnes). The simulation maintains the actual physical Catalogue products and quantities beneath each category. Category-level cargo is an allocation instruction or bulk planning view; it is not an additional physical product and must not be counted alongside the physical products it represents.

The former **Standard Stock per Unit Size per Day** factors (Brigade 4, Battalion 2, Company 2, Platoon 1, Section 1) are retired from authoritative demand, inventory and replenishment calculations. They must not be interpreted as physical quantities or used as an intermediate conversion layer. If retained for legacy-document compatibility, they are informational only and have no effect on simulation outcomes.

Physical requirements are derived directly from the receiving Unit's actual personnel, equipment, authorised holdings, task, activity and approved requirement profiles. Demand profiles identify the physical products or eligible product classes and the required quantity or mass over the applicable interval. Product-level requirements must respect weapon/ammunition compatibility, fuel compatibility, and applicable Basics suitability.

When a Player or AI requests a bulk category quantity, the receiving Unit's eligible shortages and the source's actual compatible inventory determine how the bulk order is allocated to physical products. The simulation resolves the physical cargo manifest during shipment preparation/loading, no later than dispatch, so carrier mass and compatibility are known during transit. Receipt then adds the manifested physical products to the destination Inventory. The allocation must not create goods, transform one product into another, or allocate products the source does not possess. See [Logistics Supply Reference Data](logistics-supply-reference-data.md) for the bulk-to-physical allocation rules, item mass data and validation requirements.

Physical product quantities and their canonical units remain authoritative in Inventory. Where a product has an approved unit mass, the system can calculate its contribution to cargo mass. Bulk category mass is reconciled against the physical products allocated to it. Food and potable water remain distinguishable physical products even when the Player manages both under Basics.

Unit size may inform a requirement profile where explicitly configured, but must not replace actual composition or approved requirement definitions. Activity-based demand is calculated separately and reconciled with baseline demand without double-counting.

### 5.3 Forecast Horizons

The system should support multiple horizons, including immediate demand, near-term resupply, operational planning horizon, and longer-term procurement and strategic reserve planning. The exact time windows are configurable and must not be hard-coded into every subsystem.

Forecasts must distinguish known consumption, approved future tasks, probable demand, contingency reserves, and policy targets. Forecast uncertainty must be represented where it can change procurement or allocation decisions.

### 5.4 Demand Reconciliation

Forecasts are recalculated when meaningful state changes occur, including unit activation, reorganisation, attachment, task change, equipment changes, deployment, casualties, loss of a supply route, changes in operating tempo, or updated intelligence about disruption.

Recalculation must not retroactively rewrite already completed transactions. It may create new demand, amend uncommitted requests, or cancel superseded requirements under explicit rules.

## 6. Procurement and Economic Integration

### 6.1 Core Rule

Military supplies must be sourced through the simulation's economic and physical systems. Governments and authorised military purchasing organisations may procure goods from eligible companies, use existing government or military holdings, transfer stock from another authorised owner, or obtain goods through other explicitly permitted mechanisms.

**A company is not a valid supplier solely because its Industry Type sounds relevant.** Supplier eligibility must be established at the product and capability level.

### 6.2 Supplier Eligibility

A prospective supplier may be considered only when relevant conditions are satisfied:

- The supplier is a valid company or other authorised source.
- The company produces, trades, stocks, or is otherwise permitted to supply the requested canonical good.
- The product and specification meet recipient requirements, including compatibility and quality constraints.
- Sufficient available stock exists, or a feasible production order can create it by the required time.
- The source can legally sell or transfer the good to the purchaser.
- Contract, export-control, sanctions, embargo, wartime restriction, and other applicable rules permit the transaction.
- The price, payment, budget, credit, or procurement authorisation requirements are satisfied under the Economy and Government systems.
- Delivery can be performed through feasible transport and handling capacity.
- The goods can be received, stored, and handled at the destination.

A supplier may be eligible for one product and ineligible for another, even if both are broadly associated with the same Industry Type.

### 6.3 Industry Type as a Discovery Hint

Industry Type may help identify candidate suppliers, but it is a classification and discovery aid, not a product-capability guarantee.

For example, a company in the canonical `consumer_goods` Industry Type may be a candidate source for packaged food or personal goods if its actual catalogue, production, and stock records support those products. A company in the canonical `oil` Industry Type may be a candidate for petroleum products, but crude petroleum is not interchangeable with refined fuel compatible with a military vehicle or aircraft.

The same principle applies to ammunition, medicines, spare parts, clothing, construction materials, and other goods. A supplier may also sell goods it does not itself manufacture, if its commercial and inventory records support that role.

### 6.4 Procurement Request and Contract

A Procurement Request communicates a validated military requirement to the relevant Government, Economy, or purchasing interface. It should identify products, quantities, delivery locations and windows, priorities, permitted specifications or substitutes, and funding or authorisation references.

A procurement request is not itself a purchase, contract, production order, or stock receipt. Those transactions are resolved by their owning systems.

A contract may establish price, quantity, delivery schedule, quality, penalties, priority, allocation rights, and other terms. Contract execution must not bypass production capacity, inventory, transport, export restrictions, or physical delivery requirements.

### 6.5 Central Procurement and Delegated Purchasing

**Default policy:** government-level procurement is the normal route for major military replenishment, strategic reserves, and national-scale mobilisation. Government may aggregate demand across branches, negotiate contracts, coordinate scarce goods, and balance military requirements against civilian needs.

The architecture must also support explicitly authorised delegated purchasing by military commands, bases, units, or agencies where the relevant nation, period, doctrine, law, or scenario permits it. Delegation is a policy and authority setting, not an assumption that every unit can buy freely.

Delegated purchasing remains subject to budgets, permissions, contracts, supplier eligibility, stock availability, and receipt rules. It must not create a separate economy or bypass the authoritative transaction systems.

### 6.6 Existing Stocks, Reserves, and Procurement

The logistics planner should compare the validated requirement with usable holdings, stock already reserved for higher-priority purposes, goods in transit with credible arrival times, feasible production, and feasible new procurement.

Existing stock may be preferable to purchasing more, but stock reserved for another purpose must not be silently appropriated. Strategic reserves may be released only under the relevant authority or policy. A purchase must not be counted as available supply until the physical goods exist and are accessible at the relevant stage of the supply chain.

### 6.7 Civilian Competition and Economic Effects

Military procurement should affect the economy through the same transaction rules as other demand. Where the Economy simulates them, military purchases can affect prices, company revenues, production queues, input availability, labour demand, trade flows, civilian shortages, and government finances.

The Logistics System does not independently calculate market prices or company profitability. It submits demand and consumes the outcomes returned by the economic systems.

Wartime mobilisation may increase military priority or redirect production only through explicit government, legal, industrial, or production rules. A military demand signal must not automatically seize a company's stock or production capacity without authority and a resolved transaction.

### 6.8 Foreign and Captured Supplies

Foreign procurement must respect international trade, diplomatic, legal, and transport restrictions. Captured supplies must be recorded through an authorised capture or transfer event and their ownership/custody updated through Inventory and the relevant Military/Combat interfaces.

Captured stock is not automatically usable. Its product identity, condition, compatibility, contamination, security, accessibility, and any required inspection or conversion must be assessed before issue. Captured goods may be unusable, unsuitable, or useful only to specific recipients.

## 7. Allocation, Prioritisation, and Rationing

### 7.1 Allocation Inputs

When available supply cannot meet all validated demand, allocation should consider, as applicable:

- Explicit command and government priorities.
- Emergency status and consequences of failure.
- Unit mission, operational phase, and task criticality.
- Time until the requirement becomes critical.
- Remaining usable supply and expected consumption.
- Whether the unit or installation can be resupplied by another route.
- Feasibility and expected time of delivery.
- Personnel safety and preservation of life.
- Strategic reserve policy and protected stock.
- Product compatibility, perishability, storage, and handling limits.
- Fairness or distribution rules configured by the nation or scenario.

No single universal priority score should erase explicit authority, hard constraints, or explainable allocation decisions.

### 7.2 Routine and Emergency Requests

The system must distinguish routine requests from emergency requests. Emergency requests indicate that delay may cause immediate loss of capability, unacceptable risk to personnel, mission failure, or another defined severe consequence.

The established design rule is that an emergency request triggers when a Unit's immediately available stock falls **below 50% of its authorised one-day category baseline for Ammunition or Fuel**, or **below 30% of that baseline for Basics (Food & Hydration)**. The denominator is derived from the applicable formation, personnel, equipment, task and approved standard profile. Reserved or otherwise committed stock is unavailable and excluded. In-transit stock does not count as available at the receiving Unit until Inventory confirms receipt. The comparison is strictly below, so equality does not trigger. Store these rules once in the authoritative supply-demand configuration; the versioned data contract is defined in [Logistics Supply Reference Data](logistics-supply-reference-data.md).

Emergency classification does not guarantee delivery or permit the system to create stock. It changes the request's urgency and allocation consideration, subject to authority, physical feasibility, and competing emergencies.

### 7.3 Partial Fulfilment and Rationing

A request may be fulfilled in full, partially fulfilled, deferred, denied, or cancelled. The recipient must receive an explicit result with the quantity allocated, quantity expected, shortfall, estimated delivery, and reason for any restriction where available.

Rationing must be explicit. It may involve reduced issue rates, mission-specific allocation, prioritisation of essential equipment, preservation of reserves, or use of approved substitutes. The system must not silently treat a partially fulfilled request as fully satisfied.

### 7.4 Allocation and Reservation

Allocation is the Logistics decision that stock or a future supply commitment is intended for a recipient or purpose. Reservation is an Inventory state transition that restricts specified existing stock from competing uses.

An allocation decision does not create a reservation until Inventory confirms it. A reservation does not move or consume stock. If a reservation expires, is cancelled, or becomes infeasible, its release must be explicit and auditable.

### 7.5 Substitution

Substitution is allowed only when the relevant product, technical, medical, safety, legal, and operational constraints permit it. Similar names or broad categories do not prove interchangeability.

Substitution must preserve the identity of the actual good supplied and record the relationship to the original requirement. Where substitution changes capability, performance, storage, consumption, maintenance, or safety, the receiving system must be informed.

## 8. Supply Nodes, Bases, and Physical Distribution

### 8.1 Node Capabilities

Supply nodes may support some or all of the following:

- Receipt and inspection.
- Storage and stock accounting.
- Loading and unloading.
- Issue and transfer.
- Product-specific handling.
- Consolidation and deconsolidation of shipments.
- Temporary staging and transshipment.
- Preservation, refrigeration, fuel handling, or hazardous-material handling where modelled.
- Repair, recovery, or medical support when separately configured.
- Security and protection.
- Connection to one or more transport modes.

A node's actual capabilities depend on infrastructure, equipment, personnel, stock, power, access, operating status, and other relevant systems. A node label must not automatically grant all capabilities.

### 8.2 Capacity

Capacity must be represented in the appropriate units for the operation being constrained. Supply Base Unit Handling throughput is expressed in tonnes per hour, with loading and unloading rates recorded separately where their performance differs. Payload capacity for Ammunition and Basics and Bulk Fuel capacity remain separate dimensions.

**Depot Handling Rate is derived from four inputs:**

- **Personnel:** personnel present and available to contribute to handling.
- **Logistics equipment:** type, quantity, capability and serviceability of assigned handling equipment.
- **Cargo characteristics:** the cargo category and its handling profile, such as Ammunition, packaged Basics, bulk Fuel or mixed cargo.
- **Operational condition:** the Unit's current condition, including whether it is engaged, suppressed or otherwise operationally impaired.

The logical calculation is a configurable rate derived from available personnel and equipment capability, modified by the applicable cargo profile and operational-condition factor. In compact form:

`Effective Handling Rate = Personnel/Equipment Base Rate × Cargo Factor × Operational Factor`

The exact base-rate coefficients and modifiers are reference data and must be supported by evidence or explicitly approved scenario assumptions. Personnel and equipment are complementary contributors; the formula must not assume that the Unit's entire headcount performs handling. The model does not simulate individual workers, loading bays, warehouse layouts, queues or detailed shift organisation.

Engagement, suppression and other explicitly modelled operational effects may reduce effective throughput. Do not add separate micro-management of facilities or work organisation unless later gameplay requirements demonstrate a material benefit.

A supply base may have abundant stock but insufficient handling throughput. A route may have adequate throughput but lack a suitable carrier. A depot may have storage capacity but lack compatible equipment for a particular cargo. Each meaningful constraint is evaluated separately. Do not invent numeric rates or collapse Handling, Payload and Bulk Fuel into one universal capacity field.

### 8.3 Supply Bases and Depots

Supply bases and depots support storage and distribution across strategic, operational, and tactical echelons. The model should permit national depots, regional depots, theatre depots, forward supply points, temporary depots, mobile supply units, and shipboard or airbase holdings where supported.

The presence of a base does not guarantee that it is stocked, connected, accessible, operational, secure, or capable of handling every product. Base status and capacity must be derived from actual stock, infrastructure, personnel, equipment, transport links, damage, access, and other relevant conditions.

### 8.4 Supply Columns

Supply columns are mobile formations with finite carrying capacity, movement capability, personnel, equipment, fuel requirements, and vulnerability. They must be tasked, loaded, dispatched, routed, tracked, and unloaded through the relevant command, movement, and Inventory interfaces.

A supply column's cargo manifest must reconcile to actual stock removed from the source and held in transit or in the carrier's inventory. Its load must not exceed valid capacity, including any relevant restrictions on cargo mix, weight, volume, handling, compatibility, or hazardous goods.

Loss, damage, interception, capture, diversion, breakdown, delay, or abandonment of a supply column must resolve into explicit cargo and equipment state changes. The system must not both deliver and lose the same cargo.

### 8.5 Loading, Unloading, and Transfer

Loading and unloading consume time according to the applicable Handling Rate, the quantity of cargo and the cargo's handling profile. Effective rate is derived from available personnel, assigned logistics equipment, cargo characteristics and the Unit's operational condition, including engagement or suppression. Detailed loading-bay, queue, individual-worker or shift simulation is out of scope.

Where loading and unloading are separate sequential stages, calculate each stage's duration using its applicable rate and quantity; do not treat one rate as a universal throughput for all cargo and operations. A planned loading operation does not make cargo available to the carrier before loading completes.

A carrier's arrival does not establish delivery. Cargo must be unloaded and reconciled through Inventory's receipt transition before becoming available at the receiving Unit. Partial delivery, loss, damage, return and unallocated cargo must remain explicit.

### 8.6 Route Selection

Route selection should compare feasible paths using relevant factors such as delivery time, capacity, cost, risk, security, infrastructure quality, congestion, access rights, expected losses, fuel consumption, and strategic priority.

The optimal route is context-dependent. A faster route may be more vulnerable; a safer route may be too slow; a high-capacity route may be unavailable to a specific vehicle or cargo. The planner should explain significant constraints and trade-offs to the player and AI.

### 8.7 Infrastructure and Access

Movement and Transportation owns physical route traversal, movement progress and arrival/interruption events. Infrastructure owns shared physical facility state and capacity definitions. Logistics chooses supply routes and coordinates shipment/cargo state, then requests movement and reconciles confirmed arrival. It must not independently invent road, rail, port, airfield, pipeline, or waterway capacity.

Border permissions, transit rights, territorial control, port access, neutral-country rules, and other access constraints are resolved by the relevant political, legal, diplomatic, and transportation systems. Military control of an area does not automatically make every route safe or operational.

## 9. Consumption and Expenditure

### 9.1 Consumption Authority

Logistics forecasts and coordinates supply. The authoritative stock reduction is performed through Inventory's authorised consumption transaction. The consuming activity may originate in Military, Combat, Equipment, Production, Medical, or another domain, depending on the product and use.

Each consumption event must identify the product, quantity, source holding, consuming entity, purpose, time, and originating activity or event. Rejected or failed transactions must not reduce stock.

### 9.2 Basics: Food and Hydration

Basics support the routine sustainment of military personnel. Preserve the existing generic Basics planning/issue abstraction while backing physical supply with canonical Catalogue products and Inventory stock.

The model should distinguish only the properties that can change an operational decision:

- **Quantity/sufficiency:** available Basics compared with the authorised requirement for the recipient and period.
- **Ration type:** a small, extensible set of broad types where suitability matters, such as standard provision, field/operational ration, or a genuinely specialised ration for particular conditions.
- **Quality grade:** optionally distinguish degraded, standard and superior provision when the scenario or source data supports a consequential difference. Quality effects should be modest, gradual and routed to the owning personnel/readiness/morale system; superior supply is not a direct combat-stat bonus.
- **Operating conditions:** use relevant conditions and task intensity to select an approved requirement profile or modifier where evidence supports it.

These are lightweight requirement/suitability properties, not a separate nutrition simulator. Do not model calories, macronutrients, vitamins, individual meals, food preparation, or item-level dietary composition as part of the core Logistics model. Do not add a ration type or modifier unless it changes suitability, demand, handling, or a meaningful downstream outcome.

Food and potable water remain distinct physical products where their units, availability, handling or operational consequences differ. They may be grouped under Basics for high-level planning, but must not be treated as physically interchangeable. A missing or unsupported requirement profile must be reported as unknown/unsupported rather than filled with an invented quantity.

Personnel sustainment requirements derive from the authoritative personnel count, approved profile, relevant duration, task and operating conditions. Logistics reports sufficiency or shortfall; the owning personnel, medical or readiness systems resolve health, fatigue, morale and other human effects.

### 9.3 Fuel

Fuel requirements arise from actual equipment and activity, including movement, engine operation, flight, patrol, generation, heating, or other modelled uses. Equipment and operational systems calculate or supply the relevant consumption demand; Inventory records the corresponding stock reduction.

Fuel products must respect equipment compatibility, storage and handling requirements, and any modelled quality constraints. A unit with fuel in its holdings may still be unable to operate a specific platform if the fuel is incompatible, inaccessible, or not available at the required location.

### 9.4 Ammunition

Combat and training activity can generate ammunition demand and expenditure. Combat owns the firing and engagement event; Inventory owns the stock transaction; Logistics forecasts and coordinates replenishment. These responsibilities must be integrated so one shot, burst, salvo, launch, or other expenditure event cannot consume the same ammunition more than once.

Ammunition compatibility and actual usable stock must be respected. General ammunition planning categories do not permit a weapon to use an incompatible product.

### 9.5 Other Consumables

Medical supplies, spare parts, lubricants, clothing, engineering stores, batteries, food-service materials, and specialist consumables follow the same principles: canonical product identity, valid stock, authorised use, applicable compatibility, and a recorded stock transaction.

The consuming system defines the need and the consequences of a shortfall. Logistics determines sourcing and delivery. Inventory records the physical stock change.

### 9.6 Loss, Spoilage, Damage, and Waste

Goods may be lost, damaged, contaminated, spoiled, stolen, destroyed, or rendered unusable during storage, handling, transport, combat, or other events where the simulation models these outcomes.

Losses must be recorded as explicit Inventory state changes with provenance and a cause. The system must distinguish physical loss from reduced availability, expired stock, damaged goods, inaccessible stock, and goods reserved for another purpose.

Do not apply generic loss percentages to all goods and routes without regard to the underlying model. Loss rates should be grounded in product properties, handling, duration, storage conditions, transport mode, security, and relevant event resolution.

## 10. Maintenance, Repair, Recovery, and Reconstitution

### 10.1 Interface with Maintenance

Maintenance organisations may demand spare parts, tools, consumables, replacement assemblies, and other materials. Logistics supplies the required goods and tracks their delivery; the maintenance system determines whether a repair or service task can proceed and whether it succeeds.

A repair request does not automatically create parts. Parts reserved for a maintenance task are not available for other uses unless their reservation is explicitly released. Completion of maintenance does not itself authorise an inventory change beyond the material consumption and receipts recorded through Inventory.

### 10.2 Equipment Readiness

Equipment availability depends on its authoritative serviceability and condition, compatible crew and configuration, fuel, ammunition, mission requirements, and other constraints. Logistics supplies relevant availability inputs but must not maintain a competing equipment-serviceability flag.

A formation may have enough total fuel but still lack the distribution, location, compatible product, or delivery capacity to operate all of its equipment. Aggregate stock totals must not conceal these constraints.

### 10.3 Recovery and Reconstitution

Following combat, withdrawal, deployment, or disruption, units may require replacement equipment, personnel, ammunition, fuel, Basics, medical supplies, spare parts, and time for reorganisation. Logistics supports the material side of reconstitution; Military Organisation, Personnel/Character, Equipment, Medical, and Command systems own their respective states and actions.

Reconstitution must not be instantaneous. It is constrained by available goods, personnel, training, equipment, infrastructure, repair capacity, transport, command decisions, and other relevant resources.

## 11. Contested Logistics and Operational Risk

### 11.1 Supply-Line Vulnerability

Supply routes, depots, columns, ports, railheads, pipelines, and other logistics assets may be exposed to detection, attack, sabotage, interdiction, capture, blockade, infrastructure failure, or political closure where the relevant systems support those events.

Logistics provides the assets, routes, cargoes, and operational dependencies needed for other systems to resolve threats. Sensors and Intelligence own their observations and assessments; Combat owns engagements and damage; Infrastructure and Transportation own physical network state; Inventory records resulting stock losses and transfers.

### 11.2 Route Disruption

When a route or node becomes unavailable or constrained, affected shipments must be reassessed. The system may reroute, delay, stage, return, divert, or cancel shipments if permitted and feasible. Replanning must not duplicate cargo or reset the shipment to an earlier state without an auditable transition.

Disruption may affect only specific transport modes, cargo classes, directions, time windows, or access permissions. The system must not treat every route through a region as identically blocked unless the underlying event justifies that conclusion.

### 11.3 Interdiction and Loss Resolution

A combat or other authorised event may damage, destroy, capture, or disrupt a logistics asset or cargo. The event must resolve once, with the outcome reflected consistently across the carrier, shipment, cargo, equipment, personnel, and Inventory state.

An interdiction attempt does not guarantee successful interception. Detection, identification, engagement, hit, damage, and cargo loss remain distinct stages as defined by Sensors, Intelligence, Combat, and relevant damage systems.

### 11.4 Security and Protection

Military commands may assign escorts, guards, air defence, patrols, deception, dispersion, or other protection measures. Logistics exposes the relevant routes and assets and plans around assigned protection; Command and Combat determine orders and operational effects.

Protection has costs and opportunity costs. Escorts and guards are not assumed to be available everywhere, and protection does not guarantee immunity from attack.

## 12. Strategic Reserves and Mobilisation

### 12.1 Reserve Policy

Governments may define target reserves for selected goods, locations, services, or strategic purposes. A reserve target is a policy requirement, not physical stock. The system must distinguish the target quantity from current stock, reserved stock, stock in transit, and stock that is actually usable.

Reserve policy may be expressed in absolute quantities, days of expected demand, scenario-defined contingencies, or other supported measures. The method must identify its assumptions and demand basis.

### 12.2 Mobilisation

Mobilisation can sharply increase demand through personnel expansion, equipment activation, training, transport operations, ammunition expenditure, and deployment. Logistics must expose forecasts early enough for procurement and production systems to respond, while respecting production lead times and transport constraints.

The simulation must permit a nation to mobilise beyond what its supply network can immediately sustain. The consequence is a visible gap between authorised force generation and supported operational capacity, not automatic creation of supplies.

### 12.3 Reserve Release and Replenishment

Reserve release requires appropriate authority and must record the change in allocation or reservation status. Replenishment requires valid procurement, production, receipt, or transfer events. Strategic reserve stock must not be simultaneously counted as freely available for routine issue and committed to a protected contingency.

## 13. Supply Requests and Lifecycle

A supply request should follow an explicit lifecycle, adapted to whether it is a forecast, routine request, emergency request, or planned operational requirement:

1. **Generated:** a source system creates a forecast or requirement.
2. **Validated:** product, quantity, recipient, location, time, and authority are checked.
3. **Prioritised:** routine/emergency status and applicable priority rules are applied.
4. **Approved, deferred, or rejected:** the responsible authority resolves the request.
5. **Sourcing:** existing holdings, reserves, contracts, production, and eligible suppliers are assessed.
6. **Allocated:** stock or future deliveries are assigned to the requirement.
7. **Reserved or committed:** Inventory reservation or an appropriate economic/production commitment is confirmed where applicable.
8. **Prepared:** cargo and transport are made ready.
9. **Loaded and dispatched:** physical movement begins after valid loading and dispatch transitions.
10. **In transit:** movement, delays, risk, and route status are tracked.
11. **Delivered and received:** the destination and Inventory receipt are confirmed.
12. **Issued or transferred:** stock moves to the recipient holding as required.
13. **Consumed, returned, cancelled, or otherwise closed:** the final state is recorded with quantity reconciliation.

A request may branch into partial deliveries or multiple shipments. Each child shipment must reconcile to the original requirement without double-counting fulfilment.

Not every request must pass through every state. For example, issuing already-held stock may not require commercial procurement or long-distance transport. Any skipped state must be valid for the specific transaction and remain auditable.

## 14. State Ownership and Interfaces

### 14.1 Ownership Matrix

| Fact or state | Authoritative owner | Logistics responsibility |
|---|---|---|
| Product identity, specification, units, canonical classification | Good and Service Catalogue | Reference products; define military demand mappings |
| Industry Type, company identity, establishment | Economy / Industry Data | Discover candidate sources through their declared capabilities |
| Company output, recipe, production queue, capacity and completion | Production and industrial systems | Submit forecasts/requirements; consume availability estimates |
| Price, payment, budget, contract and trade permission | Economy / Government / trade systems | Request procurement; respect returned decisions |
| Physical stock quantity and stock holding | Inventory | Query holdings; request authorised reservations, transfers, receipts and consumption |
| Stock ownership and custody | Inventory and applicable legal/economic systems | Use authorised stock; request valid ownership/custody changes |
| Military unit identity, organisation, task and command relationship | Military Organisation / Command | Resolve demand recipients and destinations |
| Equipment identity, compatibility, condition and serviceability | Equipment and Materiel | Request compatible goods and expose supply constraints |
| Physical movement, route traversal, progress and arrival events | Movement and Transportation | Plan supply routes, issue movement requests and reconcile confirmed arrival
| Facility identity, physical condition and shared capacity definitions | Infrastructure / Installations and Facilities | Check relevant facility and handling constraints |
| Combat engagement, damage, interdiction and combat expenditure event | Combat | Expose logistics assets/cargo to the combat interfaces and reconcile outcomes |
| Observation, track, intelligence assessment and uncertainty | Sensors / Intelligence | Use authorised information to estimate route and supply risk |
| Personnel numbers, health and individual state | Character / Personnel / Medical systems | Forecast configured requirements and report shortages |
| Military supply demand, priority, allocation plan and delivery coordination | Logistics & Sustainment | Authoritative within this system |
| Shipment planning state and supply-task coordination | Logistics & Sustainment, referencing transport execution | Track lifecycle and reconcile delivery against Inventory |

Where another canonical system defines a more precise owner, that system's ownership contract takes precedence. This table must be reconciled during the interface audit, not used to create duplicate state.

### 14.2 Required Interfaces

Logistics requires query or command interfaces for:

- Validated unit, installation, task, and equipment demand.
- Canonical products, units, compatibility, and handling requirements.
- Available stock by product, location, owner, custodian, reservation state, condition, and access.
- Supplier capabilities, product availability, production lead time, contracts, prices, budgets, and procurement authorisation.
- Production and delivery forecasts, with uncertainty and commitment status.
- Route feasibility, carrier availability, transport capacity, estimated travel time, movement state, access, and disruption.
- Loading/unloading capacity and handling constraints at source, transfer, and destination nodes.
- Authorised stock reservations, transfer, receipt, issue, consumption, loss, release, and reconciliation transactions.
- Combat or other events that affect carriers, shipments, routes, depots, or cargo.
- Command tasking, priority, destination, timing, and changes to operational plans.
- Unit readiness and supply-shortfall reporting interfaces.

A query result is not a state change. A request to perform a transaction is not considered successful until the owning system confirms the authoritative transition.

### 14.3 Interface Failure

If an interface is unavailable, rejects a transaction, returns stale information, or cannot confirm a transition, Logistics must preserve the last confirmed state and represent uncertainty or failure explicitly. It must not assume that a purchase, reservation, receipt, transfer, or consumption succeeded.

Retrying a request must not duplicate its effects. Transactions must carry stable operation identifiers or equivalent idempotency protection where needed.

## 15. AI and Player Interaction

### 15.1 Common Rules

Player-controlled and AI-controlled nations use the same Logistics rules, resource constraints, supplier eligibility, procurement authority, stock accounting, transport capacity, information boundaries, and delivery resolution.

AI must not gain invisible stock, perfect knowledge of enemy routes, instant procurement, or unrestricted supplier access. AI may estimate hidden enemy supply status only from information available through its observation and intelligence systems.

### 15.2 National and Military Planning

National AI determines strategic goals and resource trade-offs through the national decision architecture. Government and Economy resolve policy, budgets, procurement authority, and market interactions. Military command defines operational priorities and tasks. Logistics turns those demands into feasible sourcing, allocation, and delivery plans.

Logistics may recommend force posture changes, reserve release, route changes, procurement urgency, or reduced operational tempo when supply cannot support the plan. It does not independently decide national strategy or override command authority.

### 15.3 Planning and Explainability

The interface should present, where relevant:

- Current holdings versus baseline and forecast demand.
- Days or hours of supply, with the calculation basis shown.
- Requested, allocated, reserved, in-transit, expected, received, and consumed quantities as separate values.
- Expected delivery time and confidence.
- Supplier, production, route, transport, and handling constraints.
- Shortfalls, consequences, and available alternatives.
- Why an emergency request or allocation was prioritised.
- Whether a shortage is caused by missing goods, lack of budget/authority, production delay, route disruption, transport shortage, handling bottleneck, compatibility, or access restriction.

AI planning should consider the same classes of information and trade-offs. It need not expose private enemy information to the Player or privileged internal reasoning, but its observable actions must follow the same world rules.

## 16. Time, Simulation, and Persistence

### 16.1 Time-Based Processing

Demand generation, stock consumption, loading, unloading, production lead times, transport movement, delivery, spoilage, and recovery operate on the simulation clock and their owning systems' resolution schedules.

The system must not require every logistics process to execute only once per day. Some activities need finer temporal resolution, particularly combat resupply, aviation, naval operations, fuel use, and time-sensitive delivery. Resolution cadence should be configurable by subsystem and consistent with the simulation architecture.

### 16.2 Event Ordering

When multiple events affect the same stock, shipment, or recipient in one simulation interval, the system must use a deterministic order or explicit transactional conflict resolution. For example, a stock reservation, combat loss, and planned dispatch must not all independently assume the same units of stock remain available.

The authoritative result must reconcile quantity conservation across the source, in-transit cargo, destination, consumption, and loss records.

### 16.3 Save and Load

Persist the authoritative state necessary to resume logistics without duplicating events. This includes outstanding requests, allocations, reservations as owned by Inventory, shipments, cargo manifests, shipment positions or movement references, committed deliveries, and unresolved outcomes.

On load, the system must reconcile with Inventory, Transportation, Production, Economy, and Military state. It must not replay a completed receipt, purchase, consumption, or loss as if it were new.

### 16.4 Audit Trail

Materially important transactions should retain provenance, including originating demand, approving authority, source and destination, product and quantity, relevant economic/production commitment, shipment identifiers, time, and final outcome. The implementation may use a shared event/audit architecture, but must not create duplicate accounting ledgers that disagree.

## 17. Failure Modes and Edge Cases

The system must explicitly handle at least:

- A request for a product that has no eligible supplier.
- A company that can produce a good but lacks inputs, capacity, stock, or time to fulfil the order.
- A purchase that succeeds commercially but has not yet produced or delivered physical stock.
- A supplier that becomes unavailable after a contract or reservation.
- A source stock that is already reserved, damaged, expired, inaccessible, or held by an unauthorised owner.
- A route that becomes blocked after dispatch.
- A shipment that is delayed, diverted, partially delivered, captured, damaged, or destroyed.
- A carrier that breaks down, runs out of fuel, loses capacity, or is reassigned.
- A receiving base that is full, damaged, closed, insecure, or unable to handle the cargo.
- An incompatible fuel, ammunition, spare part, or other product.
- Emergency requests that compete with other emergencies.
- A unit whose demand changes while a shipment is in transit.
- A shipment arriving after the original request was cancelled or superseded.
- A partial receipt or discrepancy between dispatched and received quantities.
- A duplicated event or retry following save/load.
- A transfer between different owners, custodians, or legal jurisdictions.
- Captured stock that requires inspection, reclassification, conversion, or is unusable.
- Strategic reserves that are simultaneously targeted for routine use and protected contingency.
- A product whose unit of measure differs across supplier, transport, and recipient interfaces.
- A mismatch between generic military supply categories and product-level Inventory holdings.
- AI procurement or routing attempts that violate the same restrictions applied to the Player.

Every failure must produce an explicit outcome, retry path, escalation, or unresolved state. It must not silently create stock, discard a shortfall, or leave the source and destination both claiming the same cargo.

## 18. Minimum Acceptance Tests

The following tests are mandatory for the initial implementation and integration audit.

### 18.1 Product and Supplier Eligibility

1. A military demand for food identifies canonical food products and finds eligible suppliers only where product capability and stock/production records permit.
2. A company classified as `consumer_goods` but lacking the requested product cannot satisfy the demand merely because of its Industry Type.
3. A company classified as `oil` with crude petroleum but no compatible refined fuel cannot fulfil a request for a specific refined military fuel unless a valid production or refining chain can fulfil it.
4. A supplier with stock but no legal or contractual permission to sell to the buyer is rejected or requires the appropriate authorisation.
5. A supplier can fulfil a portion of an order when only a portion is available, and the remainder remains explicitly unresolved.

### 18.2 Physical and Economic Separation

6. A purchase contract does not create physical stock.
7. A production order does not create output stock before production completion is confirmed.
8. A dispatch does not create a destination receipt.
9. A confirmed receipt increases the destination holding once and only once.
10. A failed receipt leaves stock in the correct prior state or unresolved transit state according to the authoritative transaction result.
11. A transaction retry cannot duplicate stock, payment, receipt, or consumption.

### 18.3 Distribution and Capacity

12. A shipment cannot exceed carrier capacity or product-specific handling limits.
13. Depot Handling Rate changes deterministically when available personnel, assigned handling equipment, cargo category or operational condition changes; no loading-bay or individual-worker simulation is required.
14. Engagement or suppression applies only the configured operational modifier and does not silently create or destroy cargo.
15. A bulk Ammunition delivery is allocated only to compatible ammunition products held by the source and eligible for the receiving Unit; allocation follows authorised shortages/priorities and cannot create unsupported products.
16. The physical mass of allocated products cannot exceed the bulk category mass delivered; any remainder stays explicitly unallocated or at its valid prior/in-transit holding.
17. Product unit mass converts physical item quantities to cargo mass without changing the Catalogue quantity/unit or duplicating stock.
18. A 5,000 kg payload with a 43.2 kg per-item mass supports at most 115 such items by mass, before packaging/other load constraints; a 5,000 lb payload supports at most 52. The actual ammunition cargo mass must use the complete stocked item's defined mass, not assume projectile-only mass is a complete round.
13. Loading and unloading consume time and respect the applicable throughput constraints.
14. A route closure after dispatch causes a valid delay, reroute, return, diversion, or failure state without duplicating cargo.
15. A receiving base lacking storage or handling capacity cannot silently accept unlimited cargo.
16. A supply column's fuel and other operating requirements generate valid demand through the same system interfaces as other equipment.

### 18.4 Demand and Shortage

17. Routine and emergency requests are distinguished and use the configured thresholds and priorities.
18. A request below its emergency threshold is not automatically delivered if no feasible stock or route exists.
19. Partial fulfilment is shown as partial; a request is not marked complete while a material shortfall remains.
20. A shortage of Basics produces the appropriate shortage signal to the owning personnel/medical systems without Logistics duplicating character health state.
21. Fuel shortage constrains the relevant operation through equipment and operational rules rather than a universal, unexplained penalty.
22. Ammunition expenditure is recorded once when a combat event resolves and is reconciled with Logistics replenishment demand.

### 18.5 Integration, AI, and Persistence

23. AI and Player procurements obey the same supplier, budget, trade, transport, and inventory rules.
24. AI does not use hidden enemy supply information unless its observation/intelligence state permits the inference.
25. Saving and loading during loading, transit, unloading, or receipt does not duplicate or lose cargo through event replay.
26. Interdiction, capture, or destruction updates shipment, carrier, and Inventory state consistently.
27. Strategic reserve targets are not reported as physical stock when the stock does not exist.
28. A company shortage or production disruption can propagate into delayed military procurement and a visible military supply consequence.
29. A government procurement surge can compete with civilian demand where the Economy models the relevant market and production constraints.
30. Every rejected or delayed request exposes a reason category sufficient for debugging and player-facing explanation.

## 19. Open Decisions and Configuration Requirements

The following details should be settled in data contracts and integration work rather than improvised separately by each subsystem:

- Exact schema and units for demand, request, allocation, shipment, cargo manifest, and delivery events.
- Define and populate approved physical requirement profiles, product unit masses, and category-to-product allocation rules. These support bulk-to-physical allocation during shipment preparation/loading, no later than dispatch; the retired Standard Stock factors are not an operational conversion layer.
- Populate source-backed Supply Base Unit Handling throughput and operation-specific loading/unloading rates, Payload capacity with its physical basis, Bulk Fuel capacity with canonical units, and Supply Column carrying-capacity constraints. Do not invent values; see [Logistics Supply Reference Data](logistics-supply-reference-data.md).
- Establish canonical product eligibility/compatibility, product unit-mass records, category membership, and explicit substitution/allocation priorities. Missing mappings or source stock must produce an unresolved/partial result, never invented cargo.
- Which branch-specific consumption profiles and transport modes are initially supported.
- The exact set of national procurement authorities, delegated purchasing permissions, contract types, and reserve-release rules.
- Which storage degradation, spoilage, contamination, and loss mechanisms are modelled at launch.
- Transportation and Infrastructure interface contracts, including route feasibility, capacity reservations, transit time, and disruption events.
- Detailed unit consumption formulae and event ordering, ensuring Combat, Equipment, Inventory, and Logistics do not double-count expenditure.
- The extent to which market prices, civilian shortages, and industrial competition are simulated in the initial economic implementation.

These are not permission to use arbitrary placeholders in authoritative state. Until a detailed contract exists, an interface must return an explicit unknown, unsupported, or unresolved state rather than silently invent a value.

## 20. Related Canonical Specifications

- [Military Branches and Unit Organisation](military-branches-and-unit-organisation.md) — unit identity, supply-related military formations, unit requirements, command and readiness context.
- [Equipment and Materiel System](equipment-and-material-system.md) — equipment identity, compatibility, condition, serviceability, and operational availability.
- [Equipment Design, Research and Production](equipment-design-research-and-production.md) — design lifecycle and equipment fielding interfaces.
- [Doctrine, Command and Operations](doctrine-command-and-operations.md) — tasking, priorities, movement plans, and command authority.
- [Combat and Engagement Systems](combat-and-engagement-systems.md) — combat expenditure, interdiction, damage, and operational effects.
- [Sensors and Detection](sensors-and-detection.md) — detection and observation of logistics assets and threats.
- [Electronic Warfare](electronic-warfare.md) — relevant electronic effects on communications and operations.
- [Movement and Transportation System](../world/movement-and-transportation-system.md) — physical carrier/unit traversal, route feasibility, movement progress and arrival/interruption events.
- [Weather and Environment System](../world/weather-and-environment-system.md) — actual environmental conditions and forecast interfaces.
- [Economy System](../economy/economy-system.md) — commercial orders, prices, financial obligations and settlement.
- [Government Procurement and Mobilisation Interface](../../technical/architecture/government-procurement-and-mobilisation-interface.md) — public authority, budget approval, mobilisation and reserve-release decisions.
- [Inventory System](../inventory/inventory-system.md) — authoritative physical stock and stock transactions.
- [Industry Data](../economy/industry-data.md) — canonical Industry Types and company/industry configuration.
- [Good and Service Catalogue](../../reference/economy/good-and-service-catalogue.md) — canonical goods, services, specifications, and units.
- [Military Cross-System Interface Contracts](../../technical/data-schemas/military-interface-contracts.md) — demand, procurement, shipment, inventory and movement interface contracts.
- [Logistics Supply Reference Data](logistics-supply-reference-data.md) — bulk-to-physical allocation, product-mass reconciliation, emergency-threshold semantics, capacity schemas and evidence requirements.
- [World and Command Interface Contracts](../../technical/data-schemas/world-and-command-interface-contracts.md) — movement task and transport/cargo integration contracts.
- [Military Interface Audit and End-to-End Vertical Slice](../../technical/architecture/military-interface-audit-and-vertical-slice.md) — integration gap register and company-to-unit/command-to-intelligence acceptance scenarios.
- [Simulation Integration and AI Decision Architecture](../integration/simulation-integration-and-ai-decision-architecture.md) — shared interfaces, decision architecture, AI parity, and integration testing.

---

## 21. Completion Criteria

The Logistics and Sustainment specification is ready for implementation when:

1. Its state ownership boundaries are accepted by the related canonical systems.
2. Product-level procurement is connected to company capabilities, Economy, Production, Inventory, and the canonical Good and Service Catalogue.
3. Demand, reservation, shipment, receipt, issue, consumption, and loss interfaces have stable contracts.
4. Bulk category ordering, demand-driven allocation to physical products, product-mass reconciliation, the one-day physical category baseline, reserved-stock exclusion, receipt-only treatment of in-transit stock, and composition-derived carrier capacity are defined in [Logistics Supply Reference Data](logistics-supply-reference-data.md). Product mass/requirement profiles and numeric handling/capacity inputs remain reference-data tasks.
5. The required acceptance tests can be executed in an end-to-end scenario.
6. Player and AI logistics use the same authoritative rules.
7. Save/load and transaction retries preserve stock conservation and event idempotency.

Military should not be declared complete solely because this document exists. Completion requires the remaining interface audit, any missing cross-system dependencies, and an integrated scenario demonstrating procurement, production or stock sourcing, transport, receipt, unit consumption, and resulting operational consequences.
