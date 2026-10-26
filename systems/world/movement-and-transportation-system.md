# Movement and Transportation System

**Project:** Project Sovereign  
**Canonical Path:** `systems/world/movement-and-transportation-system.md`  
**System:** World Simulation / Movement and Transportation  
**Document Type:** Canonical Behavioural Specification  
**Document ID:** MOV-TRANS-001  
**Status:** Canonical initial specification; detailed movement equations, data schemas and mode-specific calibration remain open  
**Starting World Date:** 1 January 1955  
**Technology Horizon:** At least 2026, extensible beyond

---

## 1. Purpose

The Movement and Transportation System governs the physical movement of units, people, platforms, vehicles, vessels, aircraft, cargo carriers and other mobile entities through the simulated world. It translates movement intent into feasible routes, time-dependent travel, position changes, interruptions, arrival events and transport outcomes.

The system connects the World Map and Terrain model to operational movement. It must account for the actual entity moving, its movement capabilities, the route and terrain, infrastructure, weather and environmental conditions, access permissions, transport capacity, operating constraints, and the state of the world during travel.

**A route plan is not movement, and arrival is not delivery.** An accepted order does not teleport an entity. A route that appears geometrically connected is not necessarily traversable. A carrier arriving at a destination does not prove that cargo has been unloaded, received or made available to the recipient.

This specification is intended to support military operations, logistics, civilian transport, industrial supply chains, passenger movement and other world systems through shared movement rules. Domain systems continue to own their specialised decisions and consequences.

## 2. Design Principles

1. **World geometry is authoritative:** positions, terrain geometry and spatial queries come from the World Map and Terrain systems.
2. **Physical movement is time-dependent:** position changes follow elapsed simulation time, movement mode, capabilities, route conditions and explicit operating rules.
3. **Intent and execution are separate:** orders and route plans describe desired movement; this system resolves feasibility and actual progress.
4. **Movement does not create authority:** a valid route does not grant territorial access, border permission, port clearance or right of passage.
5. **Transport capacity is finite:** vehicles, vessels, aircraft, rail consists and other carriers have declared capabilities and constraints; infrastructure and facilities impose additional limits.
6. **Cargo is not duplicated:** cargo movement references Inventory-backed physical stock and must preserve stock accounting while in transit.
7. **No universal movement score:** movement mode and entity-specific constraints matter. Foot infantry, wheeled vehicles, tracked vehicles, trains, ships, aircraft and pipelines do not share one interchangeable movement model.
8. **Environmental effects are explicit:** weather and environment influence movement only through defined interfaces and applicable mode-specific rules.
9. **One authoritative position:** each mobile entity has one authoritative current position or explicitly defined spatial state. Presentation and planning systems may cache or forecast but must not write a competing position.
10. **Player and AI parity:** all actors use the same movement feasibility, access, capacity, timing and event-resolution rules.
11. **Deterministic and replay-safe:** movement progress and state transitions must be save/load safe and must not duplicate arrivals, cargo transfers or other consequences.
12. **Historical progression:** available movement technologies, infrastructure, communications, navigation, equipment and operating procedures depend on the simulated date and configured scenario.

## 3. Scope and Ownership

### 3.1 Movement and Transportation Owns

- Authoritative movement execution for supported mobile entities.
- Current position, movement state and progress, referencing World Map coordinates or supported spatial representations.
- Movement mode and route traversal state.
- Route feasibility evaluation using geometry, terrain, infrastructure, entity capability, environment and access results from owning systems.
- Travel-time and movement-rate calculations, within the supported movement models.
- Movement orders/tasks as execution references, while command systems own order authority and lifecycle.
- Transport assignments and carrier/cargo movement coordination where explicitly configured.
- Boarding, loading-for-transport, embarkation, disembarkation, transfer, unloading readiness and movement-related handling transitions, while Inventory owns stock changes and relevant facility systems own capacity/condition.
- Movement interruptions, delays, diversion, rerouting requests, arrival, breakdown/stranding reports and movement completion events.
- Movement-related fuel or operating demand requests to Equipment, Logistics and Inventory interfaces, without independently changing physical stock.
- Transport capacity evaluation from carrier configuration, cargo properties and applicable infrastructure/facility constraints.
- Movement simulation settings, fidelity levels and aggregation policies, provided they preserve required outcomes and authority boundaries.

### 3.2 Owned Elsewhere

- Spatial geometry, coordinates, map indexing and spatial queries: World Map.
- Terrain object identity and terrain properties/state: Terrain Objects and related environment owners.
- Weather and environmental truth: Weather/Environment, once specified.
- Unit identity, organisational hierarchy, personnel, orders and operational authority: Military Organisation and Doctrine/Command/Operations.
- Equipment identity, design, configuration, serviceability, engine/propulsion characteristics, fuel compatibility and rated transport capability: Equipment and Materiel.
- Facility identity, physical condition, access authority and shared facility capability/capacity definitions: Installations and Facilities.
- Infrastructure network identity and physical condition: applicable Infrastructure/World systems.
- Legal access, territorial control, border permissions, treaties, transit rights and occupation: World Territory/Control and relevant Government/Political/Diplomatic systems.
- Cargo product identity, quantity, ownership, custody, reservations, stock movements, receipts, consumption and losses: Good and Service Catalogue and Inventory.
- Supply demand, procurement, allocation, cargo manifests, supply priority and delivery reconciliation: Logistics and Sustainment.
- Commercial transportation-service pricing, contracts, payments and settlement: Economy.
- Production of vehicles, fuel, parts, rails, ships, aircraft and other goods: Production and Economy.
- Combat damage, interdiction, destruction, capture and engagement resolution: Combat, with resulting entity and inventory changes committed by the authoritative state owners.
- Detection, observation and intelligence about routes, carriers and threats: Sensors, Intelligence and Communications.

## 4. Core Concepts

### 4.1 Mobile Entity

A Mobile Entity is an entity whose spatial position may change through a supported movement process. Examples include military units, individual platforms, road vehicles, trains, vessels, aircraft, civilian groups, transport carriers and mobile installations where the scenario supports them.

Every moving entity must reference a stable identity and expose or reference its current mobility capabilities, operating state, size or movement footprint, movement restrictions and applicable control/access context.

A unit can contain many individual vehicles or personnel. The system must state whether movement is resolved for the unit as one aggregate entity, for its constituent platforms, or through a mixed model. Aggregate movement must not imply that every constituent has identical speed, fuel, condition or route compatibility.

### 4.2 Movement Intent

Movement Intent expresses a desired destination, route, time, movement mode, speed/tempo, formation, constraints or other operational conditions. It may originate from a player command, military headquarters, civilian transport planner, Logistics shipment plan, or other authorised domain.

Intent is not authority to bypass constraints. The owning order/policy system validates who may issue or modify it. Movement evaluates feasibility and reports accepted, constrained, deferred, rejected or interrupted execution.

### 4.3 Route

A Route is a sequence of spatial edges, segments, waypoints, network links or supported free-movement intervals connecting an origin and destination. It references authoritative geometry and network features rather than copying their physical state.

A route plan may be valid for one entity and invalid for another because of movement mode, dimensions, load, draft, runway needs, terrain, infrastructure, weather, access or operating limits.

### 4.4 Movement Mode

A Movement Mode identifies the physical method of travel and its applicable rules. The system must support a data-driven set of modes, including as appropriate:

- Foot movement.
- Wheeled road movement.
- Tracked movement.
- Cross-country or terrain traversal.
- Rail movement.
- Inland-waterway movement.
- Maritime surface movement.
- Subsurface maritime movement where modelled.
- Air movement.
- Helicopter/vertical-lift movement where modelled.
- Strategic air/sea transport and tactical movement as distinct operating contexts where needed.
- Pipeline or fixed conveyance movement for goods, where supported by the infrastructure/economy model.
- Other future modes through extensible definitions.

Mode availability depends on equipment, technology, infrastructure, environment, training and scenario configuration. The system must not assume that every mode exists in every historical period or location.

### 4.5 Transport Carrier

A Transport Carrier is a mobile entity or transport service capable of moving other entities or cargo. Examples include trucks, cargo ships, transport aircraft, trains, barges and other configured carriers.

A carrier has finite capacity and constraints. Its payload must respect mass, volume, dimensions, vehicle slots, passenger seats, deck/hold configuration, hazardous-material compatibility, loading arrangement, stability, access and any other relevant restrictions. Not all capacity dimensions can be reduced to one generic number.

Carrier identity and condition remain owned by Equipment and the applicable entity system. Movement owns execution of carrier movement and capacity validation against referenced capability data. Inventory owns cargo stock.

### 4.6 Transport Network

A Transport Network is the set of connected, mode-specific edges, nodes, facilities and services that can be traversed or used for movement. It may include roads, tracks, railways, navigable rivers, canals, ports, sea lanes, air routes, airports, airfields, pipelines, transshipment facilities and other supported networks.

Network edges must reference their geometry and authoritative condition. A road line on the map does not automatically establish a connected road network, legal access, passability, vehicle suitability or capacity.

### 4.7 Route Feasibility

Route Feasibility is the result of evaluating whether a specific entity or carrier can traverse a proposed route under current conditions and within a stated time horizon.

The result must distinguish:
- Feasible now.
- Feasible with delay or constraints.
- Feasible only after a required action, repair, permission or configuration change.
- Unknown because required data is unavailable.
- Infeasible under current conditions.
- Temporarily blocked or disrupted.

Unknown must not be treated as feasible. Feasibility may be estimated by an AI from imperfect information, but authoritative execution uses actual world state and the same resolution rules for all actors.

### 4.8 Movement Progress

Movement Progress describes the committed state of an active movement task, including current position, active route segment, elapsed time, completed distance or traversal progress, current mode/speed, delays, interruptions and estimated remaining time where available.

Estimated arrival time is a forecast, not a guaranteed event. Actual progress must be updated according to the simulation clock and relevant changes in state.

### 4.9 Cargo in Transit

Cargo in Transit is physical stock being transported between locations. It remains represented by Inventory-backed records and shipment/cargo references. Movement provides the physical movement progress of the carrier; Logistics coordinates the shipment; Inventory ensures the cargo is not simultaneously available at origin and destination.

Cargo loss, damage, capture, diversion and receipt must produce explicit state transitions in the owning systems. A carrier's position alone does not determine the cargo's ownership or stock quantity.

## 5. Movement Lifecycle

A movement task may follow the following lifecycle, with valid branches for different modes and task types:

1. **Requested:** an authorised actor requests movement.
2. **Validated:** entity identity, authority, destination, movement mode and constraints are checked.
3. **Planned:** a route or movement plan is generated.
4. **Evaluated:** route feasibility, timing, access and capacity are checked.
5. **Accepted, constrained, deferred or rejected:** the outcome is returned to the requester.
6. **Prepared:** any required staging, loading, embarkation, refuelling, crew preparation, permissions or facility operations are completed.
7. **Started:** movement execution begins.
8. **In progress:** position and progress evolve with simulation time.
9. **Interrupted, delayed or diverted:** a disruption or changed condition requires explicit handling.
10. **Arrived:** the entity reaches the movement task's defined endpoint.
11. **Completed or handed off:** the task is closed or the entity is transferred to another operational process.

Not every task uses every state. A foot unit moving to an adjacent position may not need a loading phase. A cargo aircraft or ship may require loading, facility handling, departure clearance, travel and unloading. Any skipped state must be valid for the movement type.

Movement acceptance is not proof that the entity can complete the journey. A route can become blocked or conditions can change after departure.

## 6. Route Planning and Feasibility

### 6.1 Inputs

Route planning may use:

- Origin and destination.
- Movement mode and carrier capability.
- Entity dimensions, weight, footprint, draft, altitude envelope, turning limits or other relevant geometry.
- Cargo load and distribution.
- Terrain geometry, elevation, slope, crossings and surface state where modelled.
- Network connectivity, condition, operating restrictions and capacity.
- Facilities and transfer nodes required by the route.
- Weather and environmental conditions.
- Territorial control, legal permissions, border/transit rules and access restrictions.
- Expected speed, operating tempo, fuel/range, endurance and serviceability.
- Threat or risk assessments actually available to the planner.
- Time window, priority, delivery deadline and acceptable risk.
- Congestion and competing route use where simulated.

### 6.2 Pathfinding

The system must support route-finding over appropriate graphs and/or spatial movement representations. Different modes may use different network models and pathfinding constraints. The architecture must not force air, maritime, rail and cross-country land movement into one road-network graph.

The route planner may optimise for distance, time, fuel/energy, cost, risk, reliability, exposure, capacity, or a weighted/lexicographic policy. The selected objective and constraints must be inspectable. A shorter route is not always better, and a minimum-cost route must not violate hard safety, legal, technical or access constraints.

### 6.3 Route Revalidation

Routes must be re-evaluated when meaningful state changes occur, including a bridge or road failure, terrain change, weather change, facility closure, border/access change, carrier damage, payload change, fuel shortage, hostile control/interdiction, or changed destination.

Revalidation must preserve movement history and actual position. It may create a new route segment or task revision; it must not teleport the entity to the new route or reset completed travel.

### 6.4 Imperfect Information

The simulation may maintain actual route state separately from the route planner's beliefs. An AI may plan using stale maps, uncertain infrastructure reports or incomplete threat information. The actual movement engine still resolves physical outcomes against world truth. An AI must not query hidden state through a convenience path that bypasses Sensors, Intelligence or Communications.

## 7. Movement Execution and Timing

### 7.1 Time-Based Progress

Movement progresses over simulation time. The movement rate depends on mode, entity capability, route conditions, payload, fatigue or crew factors where relevant, environmental state, serviceability and configured operating constraints.

Exact equations, coefficients and calibration must be mode-specific and data-driven. The initial implementation may use simplified models, but must retain explicit inputs and avoid arbitrary universal multipliers that conceal causal effects.

### 7.2 Continuous Position and Discrete Networks

The system must support continuous or suitably fine-grained spatial positions while using discrete network edges or route segments where appropriate. A movement grid may support spatial indexing and simulation queries, but grid adjacency must not automatically grant traversability.

Position updates must preserve valid spatial references, movement footprints, crossing constraints and domain-specific collision or occupancy rules where enabled.

### 7.3 Speed, Tempo and Formation

Requested speed or tempo is constrained by the slowest relevant member, formation integrity, route conditions, control policy and equipment state where the chosen aggregate model requires it.

A formation may stretch, split, reorganise, pause or continue at reduced speed only through explicit movement and command rules. If the system models a unit as one aggregate entity, it must still report when constituent equipment or personnel cannot keep pace or remain operationally available.

### 7.4 Fuel and Operating Resources

Movement may generate fuel, energy, consumable or operating-service demand. The relevant Equipment or domain system determines consumption from the actual platform and activity. Inventory records stock changes; Logistics forecasts and coordinates resupply.

Movement must not independently subtract fuel from inventory. If an entity cannot continue because it lacks compatible fuel or energy, the owning operational/equipment rules determine the consequences, and Movement records the resulting pause, stranding, reduced capability or other defined outcome.

### 7.5 Fatigue and Wear

Movement may report duration, distance, tempo, terrain and environmental exposure to Military, Character/Personnel, Equipment and maintenance systems. Those systems calculate their owned fatigue, health, wear and serviceability state.

Movement must not duplicate character fatigue or equipment wear as an independent authoritative record.

## 8. Transport Capacity and Cargo Handling

### 8.1 Carrier Capacity

A carrier's effective capacity is constrained by its authoritative configuration, condition, load, cargo compatibility, crew, fuel, operating rules and the relevant facility/route limits.

Capacity may be expressed as payload mass, cargo volume, vehicle spaces, passenger seats, deck positions, container slots, axle load, train length, aircraft payload, vessel displacement/draft or another mode-specific measure. These measures are not interchangeable.

A shipment must be rejected, split or replanned if the load exceeds a hard constraint. The system must not silently overfill a carrier.

### 8.2 Infrastructure and Facility Constraints

Infrastructure and facilities may impose limits such as bridge load, road/rail clearance, track gauge, axle load, tunnel dimensions, port draught, berth dimensions, loading rate, runway length, pavement strength, parking capacity, fuel-transfer rate or cargo-handling throughput.

The owning infrastructure/facility system defines the physical state and shared capacity. Movement evaluates whether the moving entity or carrier can use that capacity under current conditions. Movement does not create or repair infrastructure.

### 8.3 Loading, Boarding and Transfer

Loading and transfer operations take time and consume applicable handling capacity. They may require staff, equipment, a suitable facility, access permission and a compatible transport mode.

The system records movement-related readiness states such as prepared, loaded, embarked, disembarked or unloaded, while Inventory commits the actual physical stock transfer/receipt and Logistics updates the shipment lifecycle. A loading animation or movement-state flag is not an inventory transaction.

### 8.4 Split Loads and Multiple Carriers

Cargo may be split across multiple carriers or shipments when allowed by the source contract and recipient requirements. Each cargo line and shipment must reconcile to the originating demand without duplicate fulfilment.

A split load may arrive in parts. The receiving system must distinguish partial receipt from complete delivery. Cargo that has been loaded onto a carrier must not remain available as unrestricted source stock.

### 8.5 Commercial Transport

A company may provide a transportation Service or operate a carrier. Economy owns the service price, contract and financial settlement. Movement executes physical travel under the same capability and route constraints as other transport. Logistics coordinates military supply shipments. The existence of a transportation Industry Type or a signed service contract does not prove that a specific carrier, route, capacity or delivery time is available.

## 9. Mode-Specific Behaviour

### 9.1 Foot and Land Vehicle Movement

Land movement must account for terrain, roads/tracks, slopes, crossings, obstacles, surface conditions, vehicle mobility, footprint, payload, congestion and access. Road movement and cross-country movement are separate capabilities. A vehicle designed for road travel must not automatically traverse all terrain.

Wheeled, tracked, foot and specialist movement may use distinct capability profiles and rate calculations. The actual rules depend on equipment design and configuration.

### 9.2 Rail Movement

Rail movement requires connected, compatible railway infrastructure and appropriate locomotives, rolling stock, loading facilities and operating permissions. Gauge, axle load, train length, signalling/traffic capacity and route condition may constrain movement when modelled.

Rail freight capacity is not equivalent to road carrier capacity. Loading onto rail, shunting, transfers and unloading consume time and facility capacity.

### 9.3 Maritime Movement

Maritime movement requires a feasible navigable route and a vessel capable of operating in its current state. Relevant constraints may include draft, depth, channel access, port compatibility, weather/sea state, endurance, fuel, speed, navigation and legal access.

Port presence does not automatically grant berth, cargo handling, refuelling, repair or rearming capability. Maritime routes may be disrupted by blockades, mines, hostile control, weather or other explicit world events.

### 9.4 Air Movement

Air movement requires a valid aircraft/platform, operating condition, crew and mission constraints, fuel/endurance, compatible departure and arrival facilities where required, and relevant weather/airspace permissions.

Air routes may use waypoint/airspace models rather than physical road-like edges. Runway length, surface, loading, payload, range, altitude, weather, navigation, air traffic and destination capability may constrain flight.

Strategic transport and tactical flight may use different planning abstractions while preserving actual time, capacity and operating costs. Aircraft arrival does not establish cargo receipt until unloading and Inventory transitions complete.

### 9.5 Pipelines and Fixed Conveyance

Where pipelines or fixed conveyance networks are modelled, movement represents flow through a constrained network rather than a mobile carrier's position. Network topology, product compatibility, pressure/flow capacity, pumping, energy, storage and failures are owned by the relevant infrastructure and domain systems.

This mode must not be forced into vehicle movement state. It may share route/network concepts but requires a distinct execution model.

### 9.6 Future Modes

New movement modes must declare their supported entities, route representation, feasibility constraints, capacity model, time model, environmental dependencies, operating-resource demands and event outputs. Adding a mode must not require rewriting unrelated movement modes or bypassing common ownership rules.

## 10. Disruption, Failure and Recovery

The system must represent explicit outcomes for:

- Route or network disconnection.
- Obstacle, terrain or infrastructure blockage.
- Bridge, road, railway, port, runway or other facility closure/damage.
- Weather/environmental restriction.
- Loss of access rights or territorial control.
- Carrier breakdown, damage, fuel/energy shortage or capacity reduction.
- Overloaded or incompatible cargo.
- Missed departure window, congestion, delay or unavailable transfer.
- Movement orders that conflict with current position or mode capability.
- Destination closure or lack of suitable receiving facility.
- Carrier interception, capture or destruction through Combat or other authorised events.
- Movement task cancellation or supersession while in transit.
- Save/load during any movement phase.
- Repeated event delivery or failed transaction retry.

The system must distinguish a rejected plan from a task interrupted after departure. It must preserve actual position, elapsed movement and cargo status. A reroute does not reset travel or duplicate the entity.

Breakdown, damage, combat loss, capture, cargo loss and inventory changes are applied by their authoritative owners and linked to the movement event. A carrier cannot be both destroyed and continue moving unless the damage model explicitly supports a residual movement state.

## 11. Interface and State Ownership

| State or decision | Authoritative owner | Movement/Transportation role |
|---|---|---|
| Geometry, coordinates and spatial queries | World Map | Query and reference |
| Terrain object and terrain state | Terrain Objects / applicable environment owner | Evaluate as movement constraints |
| Weather/environment state | Weather/Environment, when specified | Consume relevant state; do not duplicate |
| Unit identity, composition and command authority | Military Organisation / Command | Validate references and execute authorised movement intent |
| Equipment mobility, carrier capability, propulsion, condition and fuel compatibility | Equipment and Materiel | Query actual capability and operating state |
| Facility identity, shared capacity definitions and physical condition | Installations and Facilities | Evaluate departure, arrival, loading and transfer feasibility |
| Transport infrastructure network and physical state | World/Infrastructure and relevant network owner | Query connected routes and constraints |
| Legal access and territorial control | National Territory and Control / Government / political systems | Enforce returned permissions and restrictions |
| Cargo product identity and unit | Good and Service Catalogue | Validate cargo metadata |
| Physical stock, custody, reservation, transfer, receipt and loss | Inventory | Request/consume confirmed movement-related transitions only through owner interfaces |
| Military supply demand, priority, procurement, shipment manifest and delivery reconciliation | Logistics | Accept route feasibility, issue movement requests, track carrier/shipment progress and reconcile arrival |
| Commercial transport contracts and financial state | Economy | Consume contract/service references; do not create financial records |
| Physical movement position, progress, route traversal and arrival events | Movement and Transportation | Authoritative |
| Combat engagement and damage | Combat | Receive event; apply resulting movement interruption through owner event |
| Sensor observation and intelligence about routes/threats | Sensors / Intelligence / Communications | Expose only information permitted to the decision actor |

### 11.1 Required Query/Command Interfaces

The initial interface must support:

- Query current position, movement state, mode, active route segment and remaining movement constraints.
- Evaluate route feasibility for a specified entity/carrier, route, payload, time and policy.
- Request route planning with an explicit objective and hard constraints.
- Request, modify, cancel or pause movement under the relevant authority.
- Query carrier capacity and cargo compatibility against a specified manifest.
- Request loading, embarkation, disembarkation, unloading or transfer handling where supported.
- Receive movement started, progress, delayed, interrupted, diverted, arrived, completed and failed events.
- Query facility and network capacity/access through authoritative owners.
- Notify Logistics and Inventory-related workflows of carrier arrival or disruption without directly committing cargo stock.
- Request operating-resource demand from the appropriate domain system.
- Reconcile state after save/load without replaying completed events.

### 11.2 Failure and Unknown Data

An interface that cannot confirm route, access, capacity, current position or arrival must return an explicit unknown, unsupported, stale or failed result. Missing data must not be interpreted as unlimited capacity, free access, zero travel time or successful arrival.

A movement request is not complete until the system commits or confirms the relevant state transition. Retrying the same operation must not create duplicate movement tasks or duplicate arrival/transfer events.

## 12. AI, Information Boundaries and Player Parity

The Player and AI use the same physical movement and transport rules. AI may differ in planning quality, risk tolerance, route selection, scheduling, coordination and ability to respond to disruption, but it must not receive hidden world state or ignore constraints.

National AI and domain planners may optimise movements according to strategic objectives, costs, deadlines and risk. Military Command defines the intent and authority of military movement. Logistics plans and prioritises supply shipments. Movement/Transportation determines physical feasibility and execution.

An AI's estimated route or ETA may differ from actual conditions because of incomplete information. The execution system must still resolve actual movement against world truth, then deliver the resulting events according to Communications and reporting rules when those systems are implemented.

## 13. Simulation Time, Persistence and Determinism

### 13.1 Simulation Clock

Movement is time-based. The system must integrate with the shared simulation clock and support relevant resolution frequencies for strategic, operational and tactical movement. The exact event cadence must not be independently hard-coded across unrelated subsystems.

### 13.2 Event Ordering

Concurrent events such as route closure, movement progress, carrier damage, arrival and task cancellation require deterministic ordering or explicit conflict resolution. A movement event must not both report a completed arrival and remain active on an earlier route segment.

### 13.3 Save/Load

Persist the minimum authoritative state required to resume movement exactly, including entity position, active task, route/segment reference, progress, movement mode, relevant committed transitions and unresolved outcomes.

On load, reconcile with Military, Equipment, Infrastructure, Inventory, Logistics and Combat. Do not repeat a completed loading, departure, arrival, cargo receipt, loss or other transition.

### 13.4 Event Provenance

Movement-related events should carry stable event and task IDs, entity/carrier IDs, source/recipient references, simulation time, route/task revision, causation/correlation references, outcome and idempotency scope. The common event envelope is defined by the shared data-model conventions and cross-system integration contracts.

## 14. Minimum Acceptance Tests

1. A unit cannot move before an authorised movement task is accepted.
2. A geometrically connected route is rejected when the terrain, mode or carrier makes it infeasible.
3. A wheeled vehicle and tracked vehicle may evaluate the same terrain differently when their capabilities differ.
4. A route that is feasible at planning time can become blocked; the moving entity remains at its actual committed position and receives an explicit interruption/reroute outcome.
5. A road line does not automatically create a connected or maintained road network.
6. A carrier cannot exceed its mass, volume, slot, dimensional or other configured capacity constraints.
7. A cargo ship cannot enter a port or berth that is incompatible with its actual draft/dimensions/access permissions.
8. An aircraft cannot use a departure/arrival facility that does not support its operating requirements.
9. A route plan does not grant legal border, territorial, port or transit permission.
10. A movement task does not itself consume fuel; the appropriate equipment/operational system generates demand and Inventory records consumption.
11. A carrier arriving at a destination does not automatically receipt cargo or settle a commercial transport contract.
12. Loading/unloading requires appropriate facility capacity and consumes the configured time.
13. A Logistics shipment references the actual carrier/movement task and cannot claim arrival before Movement confirms it.
14. Cargo remains accounted for exactly once across source, carrier/in-transit state, destination, consumption and loss.
15. A combat event that destroys or disables a carrier interrupts movement and produces consistent outcomes across Equipment, Combat, Movement, Logistics and Inventory.
16. A saved movement task resumes without duplicating distance, fuel consumption, arrival, loading, unloading or cargo receipt.
17. AI and Player obey identical route feasibility, capacity, access and movement execution rules.
18. An AI cannot route around a hidden threat unless its permitted information supports that decision; physical movement still resolves against actual world state.
19. A missing route or capacity value returns an explicit unknown/infeasible result rather than unlimited capacity or zero travel time.
20. Adding a new movement mode does not alter the authoritative state ownership of World, Equipment, Inventory, Logistics or Economy.

## 15. Open Decisions

The following must be settled before implementation is considered complete:

- Exact route/network graph schemas and their relationship to World Map spatial indexing.
- Exact movement integration cadence and event ordering across strategic, operational and tactical resolution.
- Mode-specific movement equations, calibration sources, and supported levels of fidelity.
- Representation of aggregated units versus individual platform positions.
- Exact capacity and throughput contracts for roads, bridges, rail, ports, airfields and other infrastructure.
- Route reservation/congestion model and simultaneous movement conflict handling.
- Exact boarding, loading, embarkation, disembarkation, unloading and transfer schemas.
- Legal/access permission query contract and its relationship to territorial control and diplomacy.
- Fuel/energy consumption request contract with Equipment, Inventory and Logistics.
- Weather/Environment state schema and movement effects.
- Movement map uncertainty and how stale/unknown infrastructure is represented to AI.
- Detailed civilian/passenger movement model and its relation to military movement.
- Whether pipelines/fixed conveyance are implemented as a movement mode or a dedicated network-flow subsystem.

Until these are defined, implementation must preserve the explicit ownership boundaries and return unknown or unsupported states rather than inventing authoritative values.

## 16. Related Specifications

- [World Map System](world-map-system.md) — authoritative geometry, coordinates, spatial indexing and spatial queries.
- [Terrain Objects](terrain-objects.md) — terrain feature identity, geometry and terrain state.
- [National Territory and Control](national-territory-and-control-system.md) — sovereignty, control, occupation and territorial permissions.
- [Installations and Facilities](../infrastructure/installations-and-facilities-system.md) — facilities, physical condition, access and capacity definitions.
- [Military Branches and Unit Organisation](../military/military-branches-and-unit-organisation.md) — unit identity, command organisation and military movement intent.
- [Doctrine, Command and Operations](../military/doctrine-command-and-operations.md) — command authority, orders and operational plans.
- [Equipment and Materiel](../military/equipment-and-material-system.md) — equipment capability, serviceability, propulsion and carrier capacity data.
- [Logistics and Sustainment](../military/logistics-and-sustainment.md) — supply demand, shipment coordination, cargo manifests, route choice and delivery reconciliation.
- [Inventory System](../inventory/inventory-system.md) — authoritative stock and cargo transactions.
- [Good and Service Catalogue](../../reference/economy/good-and-service-catalogue.md) — product identity, units, transport properties and compatibility.
- [Economy System](../economy/economy-system.md) — transport-service contracts, prices and financial settlement.
- [Simulation Integration and AI Decision Architecture](../integration/simulation-integration-and-ai-decision-architecture.md) — shared decision and information boundaries.
- [Military Interface Audit and End-to-End Vertical Slice](../../technical/architecture/military-interface-audit-and-vertical-slice.md) — cross-system contract and end-to-end acceptance plan.

---

## 17. Completion Criteria

Movement and Transportation is ready for implementation when:

1. The ownership boundaries in this specification are accepted by World, Terrain, Infrastructure, Equipment, Logistics, Inventory, Military, Economy and AI integration.
2. Route feasibility, movement task, progress, interruption, arrival, capacity and loading/transfer interfaces have stable schemas.
3. At least one land movement path and one cargo-transport path pass deterministic acceptance tests.
4. Save/load and retries preserve movement position, progress, event identity and cargo conservation.
5. Player and AI use the same authoritative movement rules and information constraints.
6. The company-to-unit sustainment vertical slice can use this system for actual cargo transport and delivery.
7. Open mode-specific decisions are tracked explicitly and are not silently replaced by arbitrary defaults.

The existence of this specification does not prove that movement has been implemented. Military integration remains open until the required shared dependencies and end-to-end tests pass.
