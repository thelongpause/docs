# Command Ops 2 Design Review: Cross-System Integration Register

**Project:** Project Sovereign  
**Document ID:** INT-MIL-C2-001  
**Status:** Integration review and implementation checklist  
**Created:** 2026-10-27  
**Scope:** Existing Military System and its authoritative interfaces with other Project Sovereign systems  
**Primary specification:** [Command Planning and Execution](../military/command-planning-and-execution.md)

---

## 1. Decision

Incorporate the useful behavioural principles identified in the Command Ops 2 review into Project Sovereign's existing Military System and its cross-system contracts. Do not create a separate Command Ops-inspired simulation layer and do not reproduce undocumented engine rules.

The reviewed concepts cover planning; orders and tasks; replanning; routes; formations; resource allocation; time and space; reassessment; reactions; order delay; effective sender; distance; staff capacity and workload; movement; formation and single-unit movement; direct fire; anti-personnel and anti-armour fire; area fire; bombardment; registration; and airstrikes.

The implementation is staged. This register identifies system ownership and required interfaces; it does not declare every connected specification complete.

## 2. Shared behavioural contract

- Orders express intent and constraints; they do not guarantee outcomes.
- Command interprets orders, creates plans, delegates tasks, and reassesses using information legitimately available to it.
- Communications owns delivery, delay, loss, and link state.
- Movement owns physical position, route traversal, movement timing, terrain interaction, and arrival.
- Combat and Weapons own engagement eligibility, firing resolution, damage, suppression, and tactical effects.
- Sensors and Detection own observations, detection, classification, identification, tracking, and uncertainty.
- Intelligence owns fusion and assessment according to its specification.
- Logistics and Inventory own supply demand, stock, reservations, movement of goods, delivery, and consumption.
- Terrain, World, and Weather own authoritative geographic and environmental inputs.
- Characters and Military Organisation own command-person identity, appointments, organisational relationships, and relevant persistent attributes within their respective boundaries.
- Doctrine configures permitted reactions, formation behaviour, rules of engagement, initiative, and withdrawal.
- Research and Equipment determine available capabilities; research does not silently retrofit fielded equipment.
- Every cross-system effect is requested, acknowledged, and reported through a durable contract. Retried events must not duplicate effects.

## 3. System-by-system integration register

| System / domain | Command integration requirement | Authoritative owner / boundary | Validation evidence |
|---|---|---|---|
| Military Organisation | Separate organic structure from operational command, attachments, and temporary task groups; identify headquarters, subordinate units, command appointments and authority. | Military Branches and Unit Organisation owns organisational identity and relationships. | Conflicting command relationships and attachment expiry tests. |
| Command Planning and Execution | Order lifecycle, effective sender, planning, task delegation, workload, latency, reassessment and bounded initiative. | This behavioural specification within Military. | Order lifecycle, authority, latency, event-trigger and save/load tests. |
| Doctrine | Formation, reaction, initiative, engagement, withdrawal, EMCON and task-specific constraints must be explicit and scenario-configurable. | Doctrine policy; Command applies it without inventing doctrine. | Same task under different doctrine yields explainable differences. |
| Communications | Orders and reports use actual communication availability, paths, latency, relay, disruption and failure outcomes. | Communications owns message delivery. EW provides interference inputs only. | Delayed/lost order, relay provenance, and restored-link tests. |
| Sensors and Detection | Planning consumes friendly observations, tracks, classification, confidence and freshness rather than hidden target truth. | Sensors owns observation/track state. | Unknown, misclassified, stale and newly confirmed contact tests. |
| Intelligence | Reports and assessments retain source provenance, confidence, age and uncertainty. | Intelligence owns fusion and assessment. | Correlated reports do not become independent confirmation. |
| Electronic Warfare | EW can affect compatible sensing or communication functions and may degrade the command's information and delivery. | EW supplies method-specific effects; Sensors and Communications resolve their own outcomes. | Jamming affects only compatible systems and does not rewrite knowledge directly. |
| World Map / Geography | Plans reference canonical locations, coordinates, spatial entities and valid traversable geometry. | World Map owns geographic truth. | No duplicate or conflicting route geometry. |
| Terrain Objects | Areas, lines, crossing points, obstacles and terrain state influence route feasibility and local reactions. Destroyed bridges and blocked routes invalidate plans. | Terrain owns terrain objects/state; Movement resolves traversal. | Crossing blocked mid-move triggers reassessment without teleportation. |
| Weather and Environment | Current and forecast/estimated conditions affect visibility, mobility, fire, aviation and task risk according to each owning system. | Weather/environment supplies conditions; specialist systems calculate effects. | Weather changes invalidate only affected assumptions. |
| Movement | Route selection, traversal, position, pace, travel time, congestion, crossing capacity, movement mode and arrival. | Movement is the sole authority for physical movement. | Formation separation, route blockage, congestion and arrival variance tests. |
| Logistics and Inventory | Plans request supply, ammunition, fuel, medical and maintenance support; tasks remain pending until allocation/delivery is confirmed. | Logistics and Inventory own stock and transactions. | No stock creation; shortage and delayed convoy propagate to command. |
| Combat and Weapons | Orders request fire/engagement missions; Combat resolves direct, anti-personnel, anti-armour, area, bombardment, registration and airstrike effects where supported. | Combat/Weapons own engagement and physical outcomes. | No damage outside Combat; ammunition and effect events applied once. |
| Equipment and Material | Capability, compatibility, ammunition, mobility, sensors, communications, reliability and damage constrain task feasibility. | Equipment defines technical capability; installed state is distinct from definition. | Class labels cannot grant unsupported capability. |
| Personnel and Characters | Commanders and staff are persistent entities with appointments, qualifications, experience and relevant state; loss or fatigue can affect command. | Character owns identity; Military owns military appointment/qualification boundaries. | Commander replacement and staff loss update capacity without deleting character history. |
| Research | Technology unlocks capability/designs and informs doctrine and equipment options, but does not automatically upgrade existing units. | Research owns progression; Equipment/production and Logistics govern fielding and sustainment. | Research unlock alone does not alter installed equipment. |
| Production / Industry / Economy | New equipment, ammunition, replacements and construction require production, procurement, finance and delivery where applicable. | Economy/Industry and Government/procurement contracts remain authoritative. | Command request cannot manufacture or requisition nonexistent materiel. |
| Government / Political Systems | Mobilisation, deployment authority, rules of engagement, national command arrangements and political restrictions constrain orders. | Government/political authority defines authorisation; Military executes permitted orders. | Unauthorised deployment/order rejected or held for approval. |
| Population Centres / Civilian systems | Operations in populated areas may create route, access, security, civilian harm, infrastructure, unrest and political consequences through explicit rules. | Population/civilian systems own civilian state; Command cannot represent civilians as generic combat resources. | Civilian effects occur through owned systems and are auditable. |
| Time / Scheduling / Save State | Scheduled reviews, task deadlines, delivery events and triggers use authoritative simulation time and survive save/load. | Time/event infrastructure owns scheduling primitives; each system owns its event semantics. | Replays deterministic; no frame-rate dependency or duplicate events. |
| UI / Advisers | Distinguish issued, delivered, understood, planned, accepted, executing, blocked and completed; show uncertainty and reasons without exposing hidden state. | UI presents authoritative state; it does not cause simulation effects. | UI actions cannot reroll outcomes or grant omniscience. |

## 4. Key end-to-end scenarios

### Scenario A: Bridge destroyed during a formation movement

1. Terrain records the bridge state change.
2. Movement recalculates affected traversability and reports the blocked route to the relevant task.
3. The task becomes blocked or degraded; no unit is moved through the destroyed crossing.
4. The event is routed to the responsible headquarters, subject to the command's information and communication state.
5. Command schedules reassessment within workload and urgency rules.
6. A new route is proposed only if it is physically and operationally feasible.
7. The formation may halt, split only where permitted, reroute, wait for engineering support, or report inability to continue.
8. All actual positions and arrival times remain Movement-owned.

### Scenario B: A new hostile contact appears

1. Sensors detect and track the contact according to available sensing capability and conditions.
2. Classification, identification and confidence remain separate.
3. The report reaches command only through applicable reporting and communication rules.
4. Intelligence may fuse it with other evidence, preserving provenance and uncertainty.
5. Command evaluates whether the contact materially changes task feasibility or risk and triggers reassessment where warranted.
6. Rules of engagement, target confidence, weapon eligibility and task constraints determine permitted responses.
7. Combat resolves any engagement; command does not use hidden true target identity as a shortcut.

### Scenario C: Headquarters issues a fire-support order

1. Authority and order constraints are validated.
2. The order is transmitted and received through Communications.
3. The headquarters or delegated fire-support element interprets and plans the task.
4. Target information, freshness, rules of engagement, range, weapon capability, ammunition, weather and timing are checked by the appropriate systems.
5. Logistics confirms available ammunition; Combat/Weapons determines engagement eligibility and resolution.
6. Combat outcomes and ammunition expenditure are reported back once.
7. Casualties, suppression, fatigue, changed target information, or mission failure may trigger reassessment.

### Scenario D: Supply convoy is delayed or interdicted

1. Logistics and Movement determine the convoy's actual progress and delivery estimate.
2. Detection and Combat resolve any observed and engaged interdiction through their own contracts.
3. Inventory is updated only when actual transactions occur.
4. Receiving units retain their real shortages until delivery is confirmed.
5. Command reassesses affected tasks, priorities, routes or support requests.
6. No order or planned arrival creates supply automatically.

### Scenario E: Communications are jammed

1. EW identifies a compatible effect against a communication capability under applicable conditions.
2. Communications resolves link degradation, delay or loss.
3. Orders and reports are delayed or lost according to Communications rules; previously received information is not automatically erased.
4. Subordinate initiative is limited by standing orders, doctrine and local information.
5. Headquarters workload and reassessment continue under the defined scheduling model.
6. When connectivity returns, queued and expired messages are resolved according to delivery and order validity rules, without duplicating effects.

## 5. Reassessment policy

Reassessment should use both scheduled review and material-event triggers. The initial implementation must not hard-code one interval for every echelon or historical period. Review cadence is configurable and affected by echelon, task complexity, command/staff capability, workload, mobility, and operational tempo.

Events are prioritised and coalesced. An event can mark a plan stale without immediately forcing a full recomputation. High-urgency events may pre-empt lower-priority work if the headquarters has capacity. Replanning must preserve previous plan versions, reasons, and already-executed effects.

## 6. Integration priorities

1. **Military closure first:** order/task lifecycle, authority, communications interface, Movement interface, Combat interface, reassessment, and persistence.
2. **Terrain and Movement:** obstacle and crossing changes, route invalidation, formation movement and event feedback.
3. **Sensors, Intelligence and EW:** imperfect information, contact freshness, report provenance, and communications disruption.
4. **Logistics and Inventory:** resource requests, reservations, delivery confirmation, consumption, and task feasibility.
5. **Doctrine, Characters and Equipment:** command capacity, bounded initiative, commander/staff effects, technical capability and constraints.
6. **Government, Economy/Industry and Population:** authorisation, mobilisation, production and civilian/political consequences.
7. **Shared technical contracts:** event identity, deterministic scheduling, save/load, idempotency, and end-to-end automated tests.

This order is a validation sequence, not a claim that downstream systems may be ignored. Military remains open until the critical interfaces and end-to-end scenarios are validated.

## 7. Definition of done

- The Command Planning and Execution behavioural specification is incorporated into the canonical Military documentation.
- Military Organisation points to the command contract and preserves organic versus operational command distinctions.
- Each listed system has an explicit ownership boundary and a defined request/result interface where relevant.
- The five end-to-end scenarios can be represented without duplicated authority or impossible actions.
- Tests cover latency, reassessment, route invalidation, uncertain intelligence, supply shortages, combat feedback, event idempotency, determinism, and save/load.
- No claim is made that an interface is implemented until its owning specification and tests confirm it.

**Status note:** This register is a design and integration checklist. It does not close the Military System or certify implementation completeness.

## Follow-on Specification Alignment (2026-10-27)

The following canonical specifications now carry explicit Command Ops 2 integration requirements in addition to this register: Command Planning and Execution; Military Branches and Unit Organisation; Doctrine, Command and Operations; Equipment and Materiel; Sensors and Detection; Electronic Warfare; Combat and Engagement; Movement and Transportation; Terrain Objects; Weather and Environment; Communications; Intelligence; Logistics and Sustainment; Inventory; World and Command Interface Contracts; and the Military Interface Audit and End-to-End Vertical Slice.

This register remains a design checklist. As implementation proceeds, add schema references and test identifiers to each relevant row. Do not mark a row complete merely because its specification addendum exists.
