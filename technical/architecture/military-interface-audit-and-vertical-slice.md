# Military Interface Audit and End-to-End Vertical Slice

**Project:** Project Sovereign  
**Canonical Path:** `technical/architecture/military-interface-audit-and-vertical-slice.md`  
**Document ID:** ARCH-MIL-INT-001  
**Document Type:** Canonical integration audit and test-plan specification  
**Status:** Audit baseline established; several required system specifications and executable tests remain open  
**Audit Date:** 26 October 2026  
**Historical Baseline:** 1 January 1955  
**Technology Horizon:** At least 2026, extensible beyond

---

## 1. Purpose and Outcome

This document records the repository-wide interface audit required to complete Military Systems, resolves currently identifiable ownership conflicts, defines the minimum cross-system contracts, and specifies an end-to-end vertical slice for procurement, production or stock sourcing, delivery, unit consumption, and operational consequences.

The audit covers the current repository tree, not unpublished notes or files outside the repository. Where a prior design decision is known from conversation but has no canonical repository source, it is recorded as provisional and must not be misrepresented as a verified canonical rule.

**Current outcome:** the principal Military behavioural specifications exist, including Logistics & Sustainment. Military is not yet integration-complete. The most significant remaining missing dependencies are Government procurement authority, Weather/Environment and Research specifications. Movement and Transportation, Character, Communications and Intelligence now have canonical initial behavioural specifications; their detailed schemas and conformance tests remain open. Government procurement authority also lacks a dedicated canonical system specification in the current repository. Existing Economy, Industry Data, Production, Inventory, Catalogue, Infrastructure, World Map, and Terrain specifications provide substantial foundations for the vertical slice.

## 2. Audit Method and Scope

The repository tree was inspected recursively on 26 October 2026. The audit checked canonical Military specifications and their related Economy, Production, Inventory, Infrastructure, World, Catalogue, Integration, schema, and architecture references.

A system is considered **missing from the repository** only when no dedicated canonical specification is present in the inspected repository tree. This does not prove that no design exists in other conversations, local files, or uncommitted material.

This document does not replace any domain specification. It records cross-system contracts and gaps, while each domain remains authoritative for its own state and rules.

## 3. Confirmed Canonical Foundations

The following specifications exist and provide usable foundations:

| Domain | Canonical specification | Audit conclusion |
|---|---|---|
| Military organisation, academies, Officer Characters, unit lifecycle and readiness | `systems/military/military-branches-and-unit-organisation.md` | Canonical; contains the broad organisational and officer-career model |
| Equipment and materiel | `systems/military/equipment-and-material-system.md` | Canonical; Inventory remains authoritative for physical stock |
| Equipment design, research integration, prototyping and fielding | `systems/military/equipment-design-research-and-production.md` | Canonical lifecycle; Production owns manufacturing execution |
| Doctrine, Command and Operations | `systems/military/doctrine-command-and-operations.md` | Canonical command and operational planning rules |
| Combat and Engagement | `systems/military/combat-and-engagement-systems.md` | Canonical engagement, damage and combat-expenditure events |
| Sensors and Detection | `systems/military/sensors-and-detection.md` | Canonical observations, detection and tracks |
| Electronic Warfare | `systems/military/electronic-warfare.md` | Canonical EW effects and interfaces |
| Logistics and Sustainment | `systems/military/logistics-and-sustainment.md` | Canonical supply planning, procurement coordination, shipment coordination and sustainment rules; integration remains open |
| Economy | `systems/economy/economy-system.md` | Owns economic/financial transaction state and pricing concepts |
| Industry Data | `systems/economy/industry-data.md` | Owns canonical Industry Types and establishment configuration |
| Production | `systems/production/production-system.md` | Owns production capacity, orders, scheduling and execution |
| Production Processes and Recipes | `systems/production/production-processes-and-recipes.md` | Owns transformation and recipe definitions |
| Inventory | `systems/inventory/inventory-system.md` | Sole authority for physical stock and stock-changing transactions |
| Good and Service Catalogue | `reference/economy/good-and-service-catalogue.md` | Canonical product/service IDs, units and product attributes |
| Installations and Facilities | `systems/infrastructure/installations-and-facilities-system.md` | Owns facility identity, shared capabilities, capacity definitions and physical state |
| World Map and Terrain Objects | `systems/world/world-map-system.md`; `systems/world/terrain-objects.md` | Own geographic truth and terrain feature definitions |
| National Territory and Control | `systems/world/national-territory-and-control-system.md` | Owns territorial sovereignty/control concepts |
| Shared data conventions | `technical/data-schemas/data-model-foundations.md` | Common identity, time, history and validation conventions |
| World dependency map | `technical/architecture/world-simulation-dependencies.md` | Useful dependency foundation, but its logistics/movement ownership wording requires alignment |

## 4. Missing Specifications and Required Decisions

The following canonical specifications were not present in the repository tree at audit time:

| Missing or incomplete domain | Required purpose | Recommended action |
|---|---|---|
| Movement and Transportation | Continuous positions, route traversal, carrier movement, movement modes, travel time, route feasibility, access, capacity interfaces, interruptions and loss | `systems/world/movement-and-transportation-system.md` (MOV-TRANS-001) now defines the canonical initial behavioural contract; detailed schemas, calibration and conformance tests remain open |
| Character System | `systems/characters/character-system.md` (CHAR-001) | Canonical initial specification now exists; general attribute catalogue, schemas and conformance tests remain open |
| Communications | Message transport, latency, delivery, disruption, priority, acknowledgement, relay and information access | Author the shared communications contract used by Command, Sensors, EW, Intelligence and AI |
| Intelligence | `systems/intelligence/intelligence-system.md` (INTEL-001) | Canonical initial specification exists; source catalogue, assessment calibration, schemas and conformance tests remain open |
| Weather and Environment | Environmental state, temporal/spatial variation, uncertainty and effects consumed by movement, sensors, equipment, combat and logistics | Author or recover the Environment specification; World geometry remains separate |
| Government and Political authority | Treasury/budget authority, procurement permissions, mobilisation and reserve-release decisions, regulation and policy | Confirm whether a dedicated Government specification exists outside the current tree; otherwise author the minimum Government authority contract needed by procurement and mobilisation |
| Research | Technology/knowledge progression, discovery and unlock authority | Equipment design references Research, but no canonical Research specification was found in the current tree; locate or author before technology-dependent equipment implementation |
| Medical/health | Personnel health, medical treatment and physiological consequences | Define the owner and interface scope before implementing Basics, hydration and medical-supply effects; do not duplicate health state in Logistics |
| Course catalogue | Reusable course definitions, prerequisites, duration, cost, qualification and historical availability | Current training concepts remain inside Military Organisation; decide whether a supporting catalogue is necessary before duplicating course data |

Missing specifications should not be replaced by vague references or silently embedded into whichever system is currently being written. A missing domain may initially be represented by a narrow interface contract, but its authoritative state and future extension path must be explicit.

## 5. Resolved Ownership Rules

### 5.1 Character and Officer Characters

- Character System owns one persistent identity per person, general character statistics, personality fields, general relationships, biography and global lifecycle.
- Military owns officer status, military identifier, branch, rank, appointments, service history, military education and qualifications, military-specific traits, command experience, decorations, military succession and military-specific career transitions.
- Military defines how general Character attributes and military-owned traits affect judgement, planning, delegation, command latency, leadership and execution.
- A general character statistic must not be copied into a second authoritative Military field. Derived military evaluations may cache inputs but must retain provenance and be rebuildable.
- Officer appointments are explicit, time-bounded relationships. Appointment loss does not delete the person.
- The Character specification is missing from the current repository, so the conceptual boundary is agreed but the schema and lifecycle event contract remain open.

### 5.2 Combat Base Units and Supply Bases

The canonical Military Organisation specification defines a **Combat Base Unit** as a mobile or deployable support formation whose initial core capability classes include Medical, Maintenance and Supply. The Logistics specification has been aligned to this model.

- A **Supply-capable Combat Base Unit** is a military unit with a configured Supply capability.
- A **Supply Base** is a site or operational logistics function, which may be provided by a fixed depot, installation or configured support formation.
- Do not create a separate organisational entity called Supply Base Unit unless a future explicit design decision requires it.
- Unit identity, command, personnel, equipment and movement belong to Military and its related owners. Product stock belongs to Inventory. Facility identity and shared capacity belong to Installations and Facilities. Logistics coordinates supply functions and shipments.
- The repository does not contain a canonical numeric table for separate ammunition, Basics and bulk-fuel payload capacities. Such values must come from the original design notes or explicit user decision, not be invented by the integration layer.

### 5.3 Basics, Fuel, Ammunition and Emergency Thresholds

- Military-facing supply categories such as Basics, Fuel and Ammunition may be retained for planning and UI, but physical stock and procurement must map to canonical Catalogue IDs and explicit units.
- Basics may aggregate food and hydration for military planning while preserving the actual product-level records beneath that category.
- Fuel must be product-specific and compatible with the consuming equipment. An Industry Type of Oil is not proof that a company supplies the required refined fuel.
- Ammunition expenditure is triggered by a Combat event, recorded once through Inventory, and reconciled with Logistics demand. Logistics must not deduct ammunition independently from the Combat event.
- The available design context records intended emergency thresholds of 50% below baseline for ammunition and fuel, and 30% below baseline for Basics. The repository audit did not find a canonical source defining the thresholds, their denominator or scope. They remain provisional until the original rule is recovered and then must be defined once in canonical configuration.
- No authoritative numeric stock/day baselines or Supply Base capacity table were located in the current repository.

### 5.4 Logistics Versus Movement and Transportation

The current architecture distinguishes Movement from Logistics, but the exact ownership wording is inconsistent across documents and there is no canonical Movement/Transportation specification.

Adopt the following target boundary:

- **World Map:** geographic geometry, coordinates, spatial indexing and spatial queries.
- **Terrain and Environment:** terrain features/state and weather/environmental state.
- **Movement and Transportation:** actual unit/platform/carrier position, movement mode, route traversal, travel progress, speed/operating constraints, arrival, interruption, and movement-related events. It should expose transport assets and route/edge feasibility/capacity from the relevant equipment, infrastructure and access state without owning those source facts.
- **Installations and Facilities:** facility identity, shared capabilities/capacity definitions, condition, access and operating state.
- **Logistics:** supply demand, sourcing coordination, allocation, shipment/cargo manifest lifecycle, logistics route choice, movement requests, cargo/shipments in transit, expected delivery, receipt reconciliation and shortfall handling.
- **Inventory:** physical stock quantity, location/holding, reservation, ownership/custody, transfer, receipt, issue, consumption, loss and condition.
- **Economy/Government:** commercial transaction, price/payment, contract, budget and authority.
- **Production:** production execution and capacity.

A Logistics route plan is not the same as actual movement execution. A shipment is not received merely because a carrier's route ends at the destination. Movement reports progress and arrival; Logistics coordinates unloading and delivery; Inventory confirms receipt.

The existing `world-simulation-dependencies.md` has been revised to separate physical movement traversal from Logistics shipment planning and delivery reconciliation. The canonical Movement/Transportation specification now exists, but detailed carrier/network capacity interfaces remain open until schemas and conformance tests are completed.

### 5.5 Company Procurement and Economic Supply

The canonical specifications already establish the necessary foundations:

- Industry Data defines Industry Types and establishment configuration. Its taxonomy includes `transportation` and `consumer_goods`; an Industry Type is not a complete product catalogue.
- The Good and Service Catalogue owns stable `catalogue_id` and canonical quantitative units.
- Production and Recipes define feasible transformations and production execution.
- Economy owns commercial orders, transaction prices, payment obligations and financial settlement.
- Inventory owns stock quantities, reservation, physical transfer and receipt.
- Logistics owns military demand and coordinates sourcing and delivery.
- Government must supply the authority and policy decision for government procurement and military prioritisation; the Government specification is not present in the current tree.

A supplier is eligible only if the company or distributor has the requested canonical product in available stock or a feasible production route, has authority to sell it, meets the product specification, can satisfy the contract and timing, and can deliver it through a feasible transport/handling path. Industry Type is a candidate-discovery hint, not evidence of product availability.

A company may fulfil a contract in stages. Purchase, payment, production completion, inventory receipt, dispatch, arrival and destination receipt are distinct transitions. Each must be confirmed by its authoritative owner.

### 5.6 Weather, Intelligence and Communications

- Weather/Environment owns environmental truth and uncertainty. Movement, Sensors, Equipment, Combat and Logistics consume only the relevant state and calculate their domain-specific effects.
- Sensors owns observations and tracks. Intelligence owns assessments, source evaluation, confidence, dissemination and actor-specific knowledge, once its specification is authored.
- Communications owns message delivery, latency, disruption, acknowledgement and relay state. Command owns order identity and authority; Communications determines whether/when the order or report is delivered.
- Logistics may use weather and intelligence-informed risk estimates only within the information available to the decision-making actor. It must not receive perfect knowledge of enemy threats by reading hidden world state directly.
- These owners must not be reimplemented as separate mini-systems inside Military, Combat, EW or Logistics.

## 6. Shared Interface Contract Minimum

Before implementation, each cross-system request or event must carry enough information to preserve identity, time, authority and idempotency. Exact schema details are still to be formalised in the relevant data-schema documents, but the minimum conceptual fields are:

### 6.1 Common event envelope

- Stable event/operation ID and event type.
- Schema version and source-system identity.
- Simulation effective time and creation/record time where distinct.
- Initiating actor and authorisation/policy reference where applicable.
- Subject entity references and relevant canonical IDs.
- Causation/correlation reference to the originating order, demand, transaction or event.
- Explicit status/outcome, including rejection or unresolved state.
- Idempotency scope and duplicate-handling rule.
- Provenance and configuration/data version where required for replay.

### 6.2 Supply demand

- Demand ID, source event/system, recipient unit/site and destination reference.
- Canonical product ID and unit of measure.
- Quantity, demand interval or event basis, required-by time.
- Purpose, urgency/priority, operational consequence and allowed substitutions.
- Forecast/actual designation, confidence and status.
- Fulfilled, allocated, in-transit, received and outstanding quantities as separate values.

### 6.3 Procurement and production

- Procurement request/order ID and authorised buyer.
- Supplier/company ID, canonical product ID, quantity and price/contract reference.
- Required specification, delivery schedule and destination.
- Stock availability or production feasibility result, with expected completion/availability time and uncertainty.
- Explicit commercial status and separate physical inventory status.
- Production order/execution references where production is required.
- Payment and stock events linked by causation/correlation, not collapsed into one transaction.

### 6.4 Shipment and delivery

- Shipment ID, originating demand and procurement/order references.
- Cargo-line IDs with canonical product IDs, quantities and units.
- Source/destination node and facility references.
- Carrier/transport assignment, route or route-plan reference and access constraints.
- Loading, dispatch, transit, delay, diversion, arrival, unloading and receipt status.
- Expected and actual times, loss/damage quantities and reason.
- Inventory transaction IDs for source issue/transfer and destination receipt.
- Reconciliation status ensuring each unit of cargo exists in one authoritative physical state.

### 6.5 Consumption and combat expenditure

- Unique originating activity/event ID.
- Unit/equipment/weapon reference, canonical product/lot reference and quantity/unit.
- Validated stock holding and consumption authority.
- Inventory transaction ID and confirmed result.
- Reconciliation state for partial expenditure, misfire, abort, cancelled activity, event replay or save/load.
- No separate Logistics-side stock decrement.

## 7. End-to-End Vertical Slice: Company-to-Unit Sustainment

### 7.1 Objective

Demonstrate that a military unit can request Basics or Fuel, a government-authorised procurement process can source goods from a company based on actual product capability, goods can be produced or drawn from stock, physical inventory can move through a real transport process, the receiving unit can consume the goods, and the resulting operational state changes through the owning systems.

The scenario must run with Player and AI as equivalent actors. The initial test may use a small deterministic world and simplified prices or travel estimates, but it must not bypass ownership boundaries or create goods through a shortcut.

### 7.2 Scenario fixture

Use configurable test fixtures rather than hard-coded production game data:

- A national Government/Treasury with a valid defence procurement authority and sufficient budget for the baseline success case.
- One military unit with known personnel/equipment, a valid location, an active task and a declared supply requirement.
- One company whose Industry Type is `consumer_goods` and whose product/stock or recipe explicitly supports the selected food good.
- A second supplier whose Industry Type is `oil` and whose product data distinguishes crude petroleum from a compatible refined fuel. This tests that Industry Type alone is not supplier eligibility.
- A production facility and recipe only if the chosen product is not already in available stock.
- An Inventory holding at the source and an Inventory holding/receiving location for the military unit or supply base.
- A configured supply-capable Combat Base Unit or a fixed supply base with the appropriate handling capability.
- A transport asset with explicit cargo capacity and fuel/operating constraints, plus a valid traversable route connecting source and destination.
- A canonical Good and Service Catalogue entry for every physical good and its quantity unit.
- A deterministic simulation clock and stable fixture IDs.

The scenario must explicitly configure all capacities, units, costs, and travel times. It must not present arbitrary illustrative numbers as historically authoritative data.

### 7.3 Baseline success path

1. **Demand creation:** Military Organisation/Logistics derives a unit requirement from personnel, equipment, task and configured baseline. Logistics creates a demand ID referencing the canonical product and unit.
2. **Validation:** Logistics verifies destination, required-by time, urgency, allowed products/substitutes and authority. It queries Inventory for accessible usable holdings and existing commitments.
3. **Procurement decision:** Logistics sends a request to the authorised Government/Economy interface. The Government authority confirms that procurement is permitted and budget is available.
4. **Supplier discovery:** Economy/Industry Data identifies candidate suppliers. Eligibility is checked against canonical product ID, company stock or feasible Production capability, contract permission, timing and destination.
5. **Product-level rejection test:** the crude-petroleum supplier is rejected for a request requiring refined compatible fuel unless a valid refining recipe, production capacity and lead time can fulfil the request. A `consumer_goods` company without the requested food product is also rejected.
6. **Commercial commitment:** the selected supplier and buyer establish the appropriate commercial order/contract. Economy records commercial and financial state. No physical stock is created by the contract.
7. **Production or stock allocation:** if goods must be manufactured, Production executes a valid recipe using available inputs and capacity. Inventory records authorised input consumption and output receipt. If stock already exists, Inventory validates its available quantity and any reservation.
8. **Source reservation and loading:** Inventory confirms the source stock reservation/issue transition. Logistics creates the shipment and cargo manifest. The transport asset and source facility confirm compatibility and capacity. Loading consumes time and handling capacity.
9. **Dispatch and transit:** Movement/Transportation confirms route feasibility and executes the carrier's movement. Logistics tracks the shipment and expected delivery. The cargo must not remain simultaneously available at the source and destination.
10. **Arrival and receipt:** Movement reports arrival. Logistics coordinates unloading and destination handling. Inventory confirms receipt once, using an idempotent transaction ID.
11. **Issue to unit:** if the receiving stock is held at a supply base, Logistics issues or transfers the required quantity to the unit's authorised holding through Inventory. The unit must not use cargo that has merely arrived but has not been received/issued under the chosen model.
12. **Consumption:** the owning activity reports actual Basics/Fuel use. Inventory commits the consumption transaction once. Logistics updates remaining demand and forecast from the confirmed stock result.
13. **Operational consequence:** the relevant Military/Equipment/Movement systems evaluate actual availability. Adequate compatible fuel may permit the planned movement; inadequate or incompatible fuel must constrain it. Basics shortfall is reported to the relevant personnel/medical owner rather than Logistics inventing character health state.
14. **Economic consequence:** Economy records the transaction and financial outcome. Where the economy model supports it, company revenue, production inputs/capacity and competing demand reflect the transaction through their normal rules.
15. **Audit:** every transition is linked by stable IDs. Source, in-transit and destination quantities reconcile; no stock is created, lost or double-counted without an explicit event.

### 7.4 Required failure branches

Run the same scenario with each failure injected independently:

- **Insufficient budget:** procurement is rejected/deferred by Government/Economy. No payment or stock transfer occurs.
- **No eligible supplier:** the request remains unfulfilled with a reason; Logistics must not infer supply from Industry Type.
- **Production shortfall:** missing inputs or capacity delay/reduce output. The military request remains outstanding.
- **Source stock reserved elsewhere:** competing demand is respected; no double reservation.
- **Route unavailable before dispatch:** no dispatch occurs until a valid alternative is chosen or the request is deferred.
- **Route blocked in transit:** Movement reports interruption; Logistics reroutes, stages, returns or records failure without duplicating cargo.
- **Carrier capacity too small:** loading is rejected or split into multiple valid shipments.
- **Receiving facility unavailable/full:** cargo remains in transit/staging or is diverted; it is not silently received.
- **Partial delivery:** Inventory records actual received quantity; the outstanding demand reflects the shortfall.
- **Consumption retry:** replaying the same consumption event does not consume stock twice.
- **Save/load during transit:** the shipment resumes at the same authoritative position/state and does not repeat dispatch or receipt.
- **Combat interdiction:** Combat resolves the engagement; the carrier/cargo outcome is applied once and Inventory records the resulting loss/capture/transfer.
- **AI parity:** repeat baseline and selected failure branches with AI as buyer/commander; all restrictions and transaction outcomes remain the same.
- **Information boundary:** an AI logistics planner cannot avoid a hidden enemy threat unless its own observation/intelligence state supports that belief.

### 7.5 Acceptance criteria

The vertical slice passes only if all of the following are true:

- All products reference canonical Catalogue IDs and units.
- Industry Type is used only as a candidate-discovery aid, never as proof of product capability.
- Government authority, commercial commitment, production, inventory, movement, delivery, receipt and consumption are distinct authoritative transitions.
- Every stock-changing operation is authorised, traceable and idempotent.
- Stock conservation reconciles source, transit, destination, consumption and explicit losses.
- Transport capacity, route feasibility, access and handling constraints are respected.
- The destination cannot use goods before valid receipt and issue/availability.
- Shortage and delay are visible and have causal explanations.
- Operational consequences flow through the owning Military/Equipment/Movement/Personnel systems.
- Player and AI outcomes obey the same rules.
- Save/load and transaction retry preserve the exact committed outcome.
- Economic consequences occur through Economy/Production/Inventory, not a parallel Logistics ledger.

## 8. Additional Military Integration Tests

The company-to-unit scenario is necessary but not sufficient. The following tests are also required before declaring Military integration complete:

1. **Command and Communications:** an order is authorised by Command, sent through Communications, delayed or disrupted as configured, and only then received/executed. Order existence does not imply delivery.
2. **Sensors, Intelligence and EW:** sensor observations create tracks; Intelligence produces assessments from permitted reports; Communications controls dissemination; EW affects the relevant electronic process without granting perfect knowledge.
3. **Officer Character:** two commanders with different general Character attributes and Military-specific traits may make different bounded planning or delegation decisions, but neither can bypass authority, stock, information or physical constraints.
4. **Combat expenditure:** a valid firing event consumes compatible ammunition once, applies equipment/damage consequences once, and generates Logistics replenishment demand without a second stock decrement.
5. **Movement and Environment:** a unit/carrier route uses World geometry and terrain, current environmental state, transport/equipment limits and access permissions. A road line or route plan alone does not guarantee movement.
6. **Readiness:** unit readiness is derived from personnel, equipment, training, command, supply, fuel, ammunition, maintenance and mission requirements; no duplicate readiness ledger overrides source state.
7. **Capture and recovery:** capture, loss, repair, recovery and reconstitution reconcile unit, equipment, personnel, stock and shipment state across their respective owners.
8. **Historical availability:** a product, equipment design, doctrine, communications method or transport capability is not usable before its configured historical/technical prerequisites, except where a scenario explicitly supplies it.

## 9. Interface Gap Register

| ID | Gap | Severity | Required resolution | Status |
|---|---|---|---|---|
| IF-MIL-01 | Movement/Transportation implementation contract incomplete | Critical | Initial canonical specification authored; freeze schemas and test route/traversal/capacity contracts across World, Terrain, Infrastructure, Equipment and Logistics | Specification authored; conformance open |
| IF-MIL-02 | Character System schemas and attribute catalogue incomplete | Critical | Initial canonical specification authored at `systems/characters/character-system.md` (CHAR-001); finalise general attribute catalogue, lifecycle schemas and Military extension conformance tests | Specification authored; schema/tests open |
| IF-MIL-03 | Communications schemas and channel model incomplete | Critical | Initial specification authored at `systems/communications/communications-system.md` (COMM-001); finalise endpoint/channel schemas, historical catalogue and integrated tests | Specification authored; schema/tests open |
| IF-MIL-04 | Intelligence schemas and assessment calibration incomplete | Critical | Initial specification authored at `systems/intelligence/intelligence-system.md` (INTEL-001); finalise source/assessment schemas, confidence calibration and integrated tests | Specification authored; schema/tests open |
| IF-MIL-05 | Government authority specification missing/unverified | High | Confirm canonical owner for budgets, procurement, mobilisation and reserve-release authority | Open |
| IF-MIL-06 | Weather/Environment specification missing | High | Define environmental state and domain-consumer contracts | Open |
| IF-MIL-07 | Research specification missing/unverified | High | Confirm knowledge/progression owner and equipment design unlock interface | Open |
| IF-MIL-08 | Supply Base nomenclature conflict | Medium | Use canonical Combat Base Unit with Supply capability; distinguish it from fixed Supply Base | Resolved in Logistics specification |
| IF-MIL-09 | Emergency thresholds and capacity table not found in repository | High | Recover source rules; confirm denominator, units and configuration ownership | Open |
| IF-MIL-10 | Shared event envelope and cross-system schema not frozen | Critical | Extend data-model foundations and define stable IDs, time, provenance, idempotency and outcomes | Open |
| IF-MIL-11 | World dependency map wording conflated Logistics network planning and Movement traversal | High | Updated the dependency map to separate route planning, physical traversal and cargo state; complete detailed movement interfaces through schemas and conformance tests | Resolved at architecture-text level; detailed contract tests open |
| IF-MIL-12 | End-to-end vertical slice not yet executed | Critical | Implement/run baseline and failure scenarios with deterministic state reconciliation | Open |
| IF-MIL-13 | Medical/personnel effects of Basics and medical supplies lack confirmed owner interface | High | Confirm Character/Personnel/Medical authority; Logistics only publishes shortage/availability facts | Open |
| IF-MIL-14 | Course catalogue scope unresolved | Medium | Keep within Military Organisation initially or define a supporting catalogue without duplicate course state | Open |

## 10. Recommended Completion Order

1. Complete Movement/Transportation schemas, detailed mode calibration and conformance tests; align the World dependency map and canonical register.
2. Complete the Character general-attribute catalogue and schemas, and validate the Military Officer extension against the Character System.
3. Complete Communications and Intelligence schemas/catalogues and integrate them with Sensors, EW, Command and AI.
4. Confirm Government procurement/budget authority and author or recover Weather/Environment and Research specifications.
5. Resolve the original Supply/Base Unit capacity and emergency-threshold rules from their source.
6. Freeze common event, transaction and lifecycle schemas.
7. Execute the company-to-unit and command-to-intelligence vertical slices and the additional Military integration tests.
8. Update the canonical register with evidence-based completion status. Do not mark Military complete merely because the required documents have been authored.

---

## 11. Completion Rule

Military is complete only when the canonical Military specifications, required shared dependencies, state ownership contracts and acceptance tests agree. The minimum proof is an executable, save/load-safe vertical slice demonstrating procurement or valid stock sourcing, production where required, physical transport, receipt, consumption, and resulting operational consequences under both Player and AI control.

A documented but unimplemented interface is an explicit integration dependency, not evidence that the simulation behaviour already works.
