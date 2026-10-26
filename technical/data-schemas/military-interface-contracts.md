# Military Cross-System Interface Contracts

**Project:** Project Sovereign  
**Canonical Path:** technical/data-schemas/military-interface-contracts.md  
**Document ID:** SCHEMA-MIL-001  
**Document Type:** Supporting Logical Data Schema and Interface Contract  
**Status:** Logical implementation contract established for review alongside SCHEMA-WORLD-CMD-001; database-specific schema types, numeric supply reference data and runtime conformance tests remain open  
**Applies to:** Military Organisation, Equipment, Command, Combat, Sensors, EW, Logistics, Movement/Transportation, Character, Communications, Intelligence, Inventory, Economy, Production, Government and World systems

---

## 1. Purpose

This document establishes the shared logical contract for the most consequential Military cross-system interactions. It defines common event identity, time, provenance, idempotency, quantity rules, lifecycle separation and the minimum fields required for supply demand, procurement, shipment, movement, communications, intelligence and Character extension workflows.

This is a schema contract, not a replacement for domain specifications. The owning system remains authoritative for each entity and state transition. A schema may reference a fact owned elsewhere but must not become a second authoritative copy of it.

The initial implementation target follows the relational conventions in the Data Model Foundations document. Exact database types, constraints, migrations and indexes must be resolved consistently across the schema suite.

## 2. Governing Rules

1. **One owner per state:** each authoritative fact and state transition has one owner.
2. **Stable identifiers:** persistent entities and consequential events use stable IDs. Display names, array indexes, coordinates and mutable aliases are not identities.
3. **Explicit relationships:** cross-system links use IDs/references, not embedded name strings.
4. **Canonical products:** every physical product line references the Good and Service Catalogue's canonical catalogue_id.
5. **Explicit units:** every quantity is measured in a canonical unit or inherits a unit from a versioned definition with a documented conversion.
6. **Simulation time is authoritative:** simulation effective time is separate from real-world database/audit time.
7. **State transitions are explicit:** requests, approvals, commitments, dispatch, receipt, issue and consumption are not interchangeable.
8. **Idempotency is mandatory:** retrying or replaying an event must not duplicate physical stock, financial movements, message effects, damage or lifecycle changes.
9. **Unknown is not success:** unavailable or unconfirmed interfaces return explicit unknown, unsupported, rejected or unresolved states.
10. **Derived values are rebuildable:** readiness, estimated arrival, remaining demand, stock-days and assessment summaries are derived from owned state unless a documented cache is required.
11. **Historical results remain explainable:** definitions, configuration versions and source events used by a completed transition remain traceable.
12. **Player and AI parity:** the same contracts, authority checks, resource limits and information boundaries apply to all actors.

## 3. Common Identity and Time Conventions

### 3.1 Stable IDs

The implementation must choose compatible ID types centrally and use them consistently across foreign-key references. Do not mix incompatible representations for the same identity.

At minimum, the following logical entities require stable IDs:

- Character ID.
- Unit ID and formation ID where distinct.
- Equipment instance ID and equipment design ID/version.
- Catalogue ID.
- Facility ID, company ID and nation ID.
- Supply demand ID and procurement request ID.
- Economy commercial order/contract/transaction IDs.
- Production order and production execution IDs.
- Inventory holding, reservation and transaction IDs.
- Shipment ID and shipment cargo-line ID.
- Movement task ID.
- Message ID.
- Intelligence report ID and intelligence assessment ID/version.
- Command order ID.
- Combat event ID or equivalent firing/engagement event identity.
- Event ID or operation ID for every consequential transition.

Domain systems may use different internal names, but shared contracts must define the mapping and preserve one identity for the same entity.

### 3.2 Simulation Time

Every consequential state transition must identify its effective simulation time. Events may also carry recorded/audit time for operational diagnosis, but audit time must not determine simulation outcomes.

Use the shared simulation calendar and ordering convention. Do not infer event order from database insertion order, wall-clock timestamps or random IDs.

### 3.3 Versioned Definitions

References to configurable definitions must include a stable definition ID and, where changes can affect outcomes, a version/configuration reference. This includes unit standard requirements, physical supply requirement/allocation profiles, product unit-mass and transport-mass definitions, movement modes, capacity definitions, message channels, intelligence confidence rules and technology prerequisites.

Updating a current definition must not silently rewrite prior transactions or historical outcomes.

## 4. Common Event Envelope

Every cross-system command or event that can change authoritative state or produce a consequential side effect must carry the following logical fields:

| Field | Required meaning |
|---|---|
| event_id | Stable identity of this event instance |
| event_type | Canonical event type and versioned enumeration |
| schema_version | Version of the event contract |
| source_system | System issuing the event or command |
| source_entity_id | Originating entity when applicable |
| initiating_actor_id | Character, institution, AI agent or system actor where applicable |
| authority_reference | Command, policy, contract or permission reference when applicable |
| simulation_effective_time | Time the event takes effect in the simulated world |
| recorded_time | Optional operational/audit timestamp; never used to resolve simulation order |
| causation_id | Direct event/command that caused this event, where applicable |
| correlation_id | Stable chain identifier for the originating workflow |
| subject_references | Typed IDs of entities affected |
| outcome_status | Explicit success, partial, rejected, failed, unresolved or other defined result |
| reason_code | Stable reason category for rejection, failure, delay or constraint |
| configuration_version | Relevant rule/data version where required for reproducibility |
| idempotency_key | Stable scope/key preventing duplicate side effects |
| provenance_reference | Source evidence or audit record where required |

Fields may be normalised into related tables rather than stored in one physical record, but the contract must preserve these semantics.

### 4.1 Event Versus Command

A command/request asks an owning system to perform or validate an action. An event/result records a confirmed outcome from that owner. A caller must not treat an unconfirmed command as a committed state change.

Examples:
- A stock reservation request is not the authoritative reservation result.
- A movement request is not proof that movement started or arrived.
- A delivery request is not proof of an Inventory receipt.
- Sending an order is not proof of Communications delivery.
- Communications delivery is not proof that Command execution occurred.

### 4.2 Duplicate and Retry Handling

The owner must recognise repeated idempotency keys within the declared scope. A retry may return the original result but must not repeat its side effect. If a genuinely new action is required, it receives a new operation ID and explicit causation link.

## 5. Quantity and Product Contract

### 5.1 Physical Product Line

Every physical goods line must reference:
- Canonical Catalogue ID.
- Quantity and canonical unit, or explicit inherited unit.
- Product specification/variant where relevant.
- Lot/batch reference when required.
- Condition/quality reference where relevant.
- Source and destination holding/location references when moving stock.
- Relevant compatibility/handling constraints through canonical product/equipment data.

A military-facing category such as Basics, Fuel or Ammunition is not a substitute for a Catalogue ID.

### 5.2 Bulk Category Quantity and Physical Allocation

The Player-facing categories are Ammunition, Fuel and Basics (Food & Hydration). A bulk category order or shipment quantity is normally expressed as mass and is a management/allocation abstraction, not an additional physical Catalogue product.

Legacy Standard Stock per Unit Size per Day factors (Brigade 4, Battalion 2, Company 2, Platoon 1, Section 1) are retired from authoritative demand, inventory, replenishment and emergency-baseline calculations. They must not be used as a physical conversion layer.

The receiving Unit's approved physical requirement profile, eligible shortages, explicit allocation priorities and source inventory determine how a bulk category delivery is allocated to canonical Catalogue products. Each physical product line retains its Catalogue ID, canonical inventory unit, quantity, product variant and applicable unit-mass reference. Missing mappings, missing mass data or incompatible products produce an explicit unsupported/partial result; they must not generate stock.

The physical products allocated must not exceed source availability or the delivered bulk mass. The mass of allocated products is reconciled to the category shipment, with any unallocated remainder represented explicitly. Food and potable water remain separate physical products under the high-level Basics category. Fuel must be compatible with receiving equipment. Ammunition must be compatible with weapons actually carried by the Unit.

Product unit mass supports cargo and payload calculations without changing the Catalogue product's canonical Inventory unit. Where packaging affects transport mass, the applicable gross transport mass must be distinguished from net product mass. The exact profiles, unit masses and priority data remain reference-data work under MIL-LOG-DATA-001.

### 5.3 Available Stock

Available stock must be derived from Inventory's authoritative holdings and must respect:
- Current physical quantity.
- Condition, expiry and usability.
- Ownership and custody.
- Reservation and commitment state.
- Location and access.
- Compatibility with the requested use.
- Valid in-transit representation and expected arrival, without counting it as physically received stock.

For emergency-threshold evaluation, the denominator is the receiving Unit's authorised one-day requirement for the relevant category, derived from applicable formation, personnel, equipment, task and standard-profile definitions. Reserved or otherwise committed stock is unavailable and excluded from immediately available stock. In-transit stock counts as available at the receiving Unit only after Inventory confirms receipt. The threshold is strictly below 50% for Ammunition/Fuel and below 30% for Basics; equality does not trigger. Forecasts may represent expected in-transit stock separately, but must not count it as physically received or available at both ends.

## 6. Supply Demand Contract

A logical Supply Demand record should include:

- Demand ID.
- Source event ID and source system.
- Recipient Unit or facility ID.
- Destination location reference.
- Canonical Catalogue ID or a versioned military supply-category requirement with an explicit product-mapping reference.
- Requested quantity and unit.
- Baseline quantity or standard-requirement reference where applicable.
- Demand interval or defined event basis.
- Required-by simulation time.
- Purpose code, such as routine sustainment, operation, training, maintenance, mobilisation or emergency.
- Priority and consequence/severity reference.
- Permitted substitution rules.
- Forecast/actual designation.
- Confidence/provenance when based on uncertain forecasts.
- Status and revision/version.
- Fulfilled, allocated, in-transit, received and outstanding quantities as distinct values.

Demand lifecycle states should distinguish at least: generated, validated, approved, allocated, partially fulfilled, fulfilled, deferred, rejected, cancelled, expired and unresolved. Domain-specific states may extend this list, but must not redefine an existing state incompatibly.

Demand creation does not reserve, purchase, dispatch or consume stock.

## 7. Procurement and Production Contract

### 7.1 Government Approval

A Government procurement decision or equivalent must reference the originating request, authority/policy, authorised buyer, approved scope/quantity/value, effective time, conditions, outcome and reason code. The minimum contract is documented in the Government Procurement and Mobilisation Interface (GOV-IF-001).

Government approval does not create a commercial order, payment, production output or physical stock.

### 7.2 Commercial Transaction

Economy owns the commercial order/contract, transaction price, payment obligation and settlement. The military/Logistics workflow should reference Economy's stable order/contract/transaction IDs rather than maintain a parallel financial ledger.

Supplier eligibility must be checked against actual product capability, stock/production feasibility, specification, permissions, delivery schedule and contract terms. Industry Type may aid discovery but is not proof of product supply.

### 7.3 Production

Where stock is not already available, the workflow may reference a Production Order and Production Execution. Production Processes and Recipes own the transformation definition; Production owns execution and capacity; Inventory commits input consumption and output receipt.

Production order creation is not output creation. Completed execution must be reconciled with the Inventory result.

## 8. Shipment and Cargo Contract

### 8.1 Shipment Header

A logical Shipment record should include:
- Shipment ID.
- Originating Demand ID.
- Procurement/order/contract reference where applicable.
- Source and destination node/facility/location references.
- Carrier Unit ID or transport assignment reference.
- Movement Task ID.
- Priority and required-by time.
- Planned route reference and route revision.
- Planned start, dispatch, expected arrival and actual arrival simulation times where applicable.
- Shipment lifecycle status and reason.
- Expected, dispatched, received, lost, damaged, returned and outstanding quantities as distinct values.
- Inventory transaction references for source issue/transfer and destination receipt.
- Audit, correlation and idempotency references.

### 8.2 Cargo Lines

Each Shipment Cargo Line must identify:
- Cargo-line ID and Shipment ID.
- Canonical Catalogue ID.
- Quantity and unit.
- Lot/batch where required.
- Handling and compatibility constraints.
- Source holding and destination holding/location references.
- Loading, dispatch, receipt, loss/damage and return quantities or linked event references.

Cargo lines must reconcile to the originating demand and Inventory holdings. Split shipments and partial delivery must not duplicate fulfilment.

### 8.3 Shipment States

The logical shipment lifecycle should support:
- Planned.
- Approved.
- Stock-reserved/committed where applicable.
- Preparing.
- Loading.
- Loaded.
- Dispatched.
- In transit.
- Delayed.
- Interrupted.
- Diverted/rerouted.
- Arrived.
- Unloading.
- Partially received.
- Received/closed.
- Returned.
- Cancelled.
- Lost/destroyed/captured where applicable.
- Unresolved.

The exact transition graph must be defined for each transport/task type. A state transition must not skip required physical handling or Inventory transactions.

### 8.4 Supply Base Unit and Supply Column

A Supply Base Unit is a Combat Base Unit configured with a Supply Depot. The depot's Handling, Payload and Bulk Fuel capacities are separate:
- Handling: tonnes loaded/unloaded per hour.
- Payload: Ammunition and Basics.
- Bulk Fuel: Fuel.

A Supply Column is a distinct physical mobile Unit. It travels from the Supply Base to the drawing Unit, delivers/unloads the assigned supply, and returns according to its task; it may be suspended or aborted. The Supply Column's actual movement is owned by Movement and Transportation. Logistics owns shipment/cargo coordination; Inventory owns physical stock transitions.

The lower applicable Handling rate governs total loading/unloading completion time where both stages apply. Numeric capacity values remain reference data and are not specified by this contract.

## 9. Movement and Transportation Contract

A logical Movement Task should include:
- Movement Task ID.
- Moving entity/carrier ID.
- Authorised command/request reference.
- Origin and destination references.
- Movement mode.
- Route/route revision reference where applicable.
- Payload/cargo manifest reference where applicable.
- Planned start and required-by time.
- Current status and reason.
- Current position/route progress references owned by Movement and Transportation.
- Estimated arrival and confidence as derived values.
- Actual start, interruption, arrival and completion event references.
- Access/permission result references.
- Idempotency and provenance references.

Movement and Transportation owns actual position, route traversal, progress, arrival and interruption events. Logistics requests movement for shipments and reconciles confirmed movement events with shipment state. A movement task does not itself receipt cargo.

The route/capacity result must distinguish feasible, feasible with constraints, unknown, temporarily blocked and infeasible. Unknown must not be treated as feasible.

## 10. Inventory Transaction Contract

Inventory remains the sole authority for physical stock. A cross-system stock operation must reference:
- Inventory transaction ID.
- Operation type.
- Canonical product and quantity/unit.
- Source/destination holding or location.
- Ownership/custody references where relevant.
- Reservation/lot references where relevant.
- Originating demand/order/shipment/combat/production event.
- Simulation effective time.
- Outcome and reason.
- Idempotency key.

Relevant operations include receipt, reservation, release, issue, transfer, consumption, loss, capture and disposal. The precise operation schema remains owned by Inventory.

Combat expenditure must have a single authoritative transaction boundary. A firing event may generate an expenditure request, but Inventory confirms the stock change once. Logistics consumes the confirmed expenditure event to update replenishment demand and must not independently subtract the same ammunition.

## 11. Communications and Intelligence Contract

### 11.1 Message

A logical Message should include stable Message ID, source/recipient endpoint references, message type, priority, payload reference, creation time, channel/route reference, security/access requirements, delivery status, delivery/acknowledgement times, originating event/correlation IDs and idempotency scope.

Communications owns transport, delivery, latency, disruption and acknowledgement. The source domain owns payload meaning. Delivery does not imply comprehension or execution.

### 11.2 Intelligence Report and Assessment

A logical Intelligence Report should reference source/observation IDs, received time, provenance, content/claim, source evaluation and delivery state. An Intelligence Assessment should have stable ID/version, subject/question, time/geographic scope, supporting/contradicting report references, assumptions, confidence/uncertainty, validity/review status and dissemination restrictions.

Sensors owns observations and tracks. Intelligence owns analytical assessments and actor-specific knowledge. Communications owns delivery. A report not delivered to an actor must not silently become part of that actor's knowledge.

## 12. Character Extension Contract

A domain extension references the stable Character ID and stores only domain-owned state. The Military Officer extension may include military identifier, branch, rank, appointments, service history, military qualifications, military-specific traits, command experience and succession state.

Character owns persistent person identity, general attributes, personality, general relationships and global lifecycle. Military owns the Officer extension and defines how general Character attributes and military-specific state influence judgement, planning, delegation, command latency and execution.

The Character specification is CHAR-001. General attribute definitions and exact schema remain open; no implementation may create parallel Military general-stat fields.

## 13. State Transition and Conservation Invariants

The following invariants are mandatory:

1. A request is not a committed transaction.
2. A government approval is not a commercial order.
3. A commercial order is not physical stock.
4. A production order is not completed output.
5. Production completion is not Inventory receipt.
6. Inventory source issue/transfer is not destination receipt.
7. Dispatch is not arrival.
8. Arrival is not unloading, receipt or issue to the drawing Unit.
9. Receipt does not imply consumption.
10. Consumption occurs once through Inventory's authorised transaction.
11. A shipment cannot be physically available at origin and destination at the same time.
12. A movement event cannot be both completed and still active on an earlier route segment.
13. A command order is not message delivery; message delivery is not execution.
14. A sensor observation is not an intelligence assessment; an assessment is not actual world truth.
15. General Character attributes are not duplicated in domain extensions.
16. A technology unlock is not an approved design, production capacity or fielded equipment.
17. AI uses the same authority, resource and information constraints as the Player.
18. Every state-changing event has stable identity, effective simulation time, provenance and idempotent retry semantics.
19. A rejected/unknown interface result cannot be silently converted into success.
20. Derived status/forecast fields must be rebuildable from authoritative state and versioned rules.

## 14. Minimum Schema Conformance Tests

1. Every product line references a valid Catalogue ID and explicit/inherited unit.
2. Legacy Standard Stock factors do not affect authoritative demand, physical inventory, bulk orders, replenishment or emergency baselines.
3. Bulk category allocation is resolved to compatible physical Catalogue products using approved requirement profiles, source stock, unit-mass data and explicit priorities; the category abstraction is not counted as additional physical stock.
4. Emergency thresholds use the configured unit/category baseline and an explicit definition of available stock.
5. Government approval, Economy order, Production execution, Inventory receipt, Movement, Logistics delivery and final consumption have separate IDs/statuses.
6. A retry of any committed transaction returns the prior result and does not duplicate its side effect.
7. Partial procurement, production, delivery and consumption preserve outstanding quantities.
8. Shipment cargo totals reconcile with source issue, in-transit state, destination receipt, losses and returns.
9. Movement and Transportation owns route progress and arrival; Logistics cannot write physical position.
10. Inventory owns stock receipt and consumption; Logistics cannot directly alter stock quantities.
11. Communications retry does not duplicate Command execution.
12. Duplicate Intelligence report delivery does not create duplicate source evidence or repeated confidence gain.
13. Character domain extensions reference one stable Character ID and do not duplicate general attributes.
14. A Combat firing event cannot consume ammunition twice after save/load or event replay.
15. A technology unlock does not automatically create a design, prototype, production output or fielded equipment.
16. Player and AI requests pass the same schema validation and authority checks.
17. Unknown units, missing product mappings and unsupported states are rejected or explicitly unresolved.
18. Saved state reconciles with source-domain event IDs and does not replay completed receipts, movements or lifecycle changes.

## 15. Open Decisions

- Exact identifier primitive and schema namespace conventions for all new domains.
- Exact physical schemas for demand, procurement, shipment, movement task, message, intelligence report/assessment and Character extension.
- Approved physical requirement profiles, category-to-product eligibility and allocation-priority data, and canonical product unit-mass/transport-mass records needed for demand-driven bulk-to-physical allocation.
- Source-backed numeric Supply Base Unit Handling, Payload and Bulk Fuel capacities and Supply Column carrying capacity, including their canonical units and constraints. Requirements are specified in MIL-LOG-DATA-001; unsupported values must remain unset rather than invented.
- Detailed movement-mode capacity and route feasibility schemas.
- Communications channel catalogue and Intelligence confidence/source calibration.
- Character general-attribute catalogue and scales.
- Government Treasury/policy schema and full Research progression schema.
- Event-ordering policy for simultaneous movement, combat, receipt, loss and consumption.
- Schema migration and historical replay strategy.

These decisions remain open until the authoritative owner supplies them. They must not be filled with arbitrary implementation defaults that alter simulation outcomes.

## 16. Related Specifications

- [Data Model Foundations](data-model-foundations.md) — shared identifiers, units, time, history and idempotency.
- [Logistics Supply Reference Data](../../systems/military/logistics-supply-reference-data.md) — bulk-to-physical allocation, product-mass reconciliation, emergency-threshold semantics, capacity record requirements and sourcing policy.
- [Military Interface Audit and End-to-End Vertical Slice](../architecture/military-interface-audit-and-vertical-slice.md) — integration gaps and test scenario.
- [Logistics and Sustainment](../../systems/military/logistics-and-sustainment.md) — supply demand, procurement coordination, shipments and delivery.
- [Movement and Transportation](../../systems/world/movement-and-transportation-system.md) — physical movement and route execution.
- [World and Command Interface Contracts](world-and-command-interface-contracts.md) — Movement, Character, Communications, Intelligence and Weather/Environment logical schemas.
- [Inventory System](../../systems/inventory/inventory-system.md) — authoritative physical stock transactions.
- [Communications System](../../systems/communications/communications-system.md) — message delivery and acknowledgement.
- [Intelligence System](../../systems/intelligence/intelligence-system.md) — source assessment, confidence and actor knowledge.
- [Character System](../../systems/characters/character-system.md) — persistent identity and general attributes.
- [Government Procurement and Mobilisation Interface](../architecture/government-procurement-and-mobilisation-interface.md) — procurement authority and mobilisation policy.
- [Research and Technology Progression Interface](../architecture/research-technology-progression-interface.md) — technology knowledge and unlock boundaries.

---

## 17. Completion Criteria

This contract is ready for implementation when:

1. The shared event envelope and ID conventions are accepted by all relevant domain owners.
2. Supply demand, procurement, shipment/cargo, movement task, Inventory transaction, message, intelligence assessment and Character extension schemas are versioned and validated.
3. Bulk-to-physical allocation, product-mass reconciliation and emergency-threshold semantics are explicit; physical manifests are resolved no later than dispatch and all used requirement profiles, product unit masses and allocation priorities are versioned and approved.
4. Inventory, Economy, Production, Government, Movement/Transportation and Logistics transaction boundaries are tested.
5. Save/load and retries preserve stock conservation, movement state, message delivery and actor knowledge.
6. The company-to-unit sustainment and command-to-intelligence vertical slices pass with Player/AI parity.
7. Every unresolved domain field has an explicit unsupported/unknown state rather than an arbitrary default.

The existence of this contract does not prove that runtime code or the vertical slice has been implemented.
