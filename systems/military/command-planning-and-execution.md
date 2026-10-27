# Command Planning and Execution

**Project:** Project Sovereign  
**System:** Military Simulation  
**Document ID:** MIL-C2-001  
**Status:** Behavioural specification, cross-system contracts pending integration validation  
**Canonical path:** `systems/military/command-planning-and-execution.md`  
**Starting historical date:** 1 January 1955  
**Technology horizon:** Researchable capabilities through the 2026 state, subject to scenario and research rules  
**Dependencies:** Military Branches and Unit Organisation, Character, Communications, Sensors and Detection, Intelligence, World, Terrain, Weather and Environment, Movement, Logistics, Equipment, Combat, Doctrine, Time  
**Related specification:** [Military Branches and Unit Organisation](military-branches-and-unit-organisation.md)

---

## 1. Purpose

Command Planning and Execution models how military intent becomes orders, plans, subordinate tasks, physical actions, reports, reassessment, and revised action. It applies across land, naval, air, joint, strategic, and specialist forces while allowing branch-specific doctrine and operational constraints.

The central design rule is that **orders express intent; command interprets and coordinates; specialist systems execute authoritative effects**. Issuing an order must not directly teleport units, create resources, reveal hidden enemies, apply combat damage, or bypass physical and organisational constraints.

This specification incorporates general design lessons from Command Ops 2 without copying its interface, undocumented internal rules, or fixed timings. Project Sovereign defines its own simulation contracts and balances them for a world beginning on 1 January 1955 with a research horizon through the 2026 state.

## 2. Design principles

1. **Intent is distinct from execution.** An order is an instruction with purpose, priority, constraints, conditions, and provenance, not a guaranteed result.
2. **Hierarchical command, bounded autonomy.** Headquarters delegate tasks and intent. Subordinates may plan and react within their authority, doctrine, current orders, rules of engagement, and available information.
3. **Information is imperfect.** Planning uses the command's available operational picture, not omniscient access to authoritative enemy state.
4. **Feasibility precedes commitment.** Plans account for known capability, route, terrain, weather, timing, fuel, supply, formation, readiness, communications, and threat constraints.
5. **Plans adapt to change.** Periodic reassessment is supplemented by material-event triggers. Replanning is computationally and operationally meaningful, not instantaneous or free.
6. **Command has capacity and latency.** Staff, headquarters quality, commander judgement, workload, communication path, distance where relevant, mobility, disruption, and order complexity affect processing and execution.
7. **Doctrine shapes behaviour.** Formation, movement, engagement, reaction, withdrawal, emission control, and initiative follow the applicable doctrine and orders.
8. **Specialist systems own physical truth.** Command requests actions and consumes their results; it does not duplicate Movement, Combat, Communications, Logistics, or Sensors logic.
9. **Consequences propagate.** Fatigue, suppression, casualties, cohesion, supply, damage, disrupted communications, and changed terrain can invalidate tasks and trigger reassessment.
10. **Deterministic and auditable.** Scheduling, priority, conflict resolution, task transitions, and retries must be reproducible and save/load safe.

## 3. Core concepts

### 3.1 Command authority

A command relationship describes who may issue, approve, coordinate, or cancel orders for a unit. It is distinct from organic organisational parentage, physical proximity, temporary attachment, and communication connectivity.

The authority resolver must consider:
- current organic and operational command relationships;
- attachments, detachments, support relationships, and effective dates;
- order type and delegated authority;
- commander's intent, standing orders, doctrine, and rules of engagement;
- emergency authority explicitly permitted by doctrine;
- whether the sender is eligible to issue the instruction.

The **effective sender** is the authorised command entity whose intent the recipient is expected to act on after considering valid relays, staff processing, delegation, and command precedence. A relay may transport an order without becoming its originator. Conflicting orders must be resolved through a documented precedence policy and logged.

### 3.2 Order

An order is a durable record containing, at minimum:

- stable order identifier and version;
- issuing authority and originator;
- recipient unit, headquarters, or task group;
- order type, objective, intent, priority, and constraints;
- issue time, earliest execution time, expiry or review condition where applicable;
- route, destination, area, target class, timing window, or other typed parameters;
- prerequisites and permitted contingencies;
- doctrine and rules-of-engagement references;
- transmission, receipt, acknowledgement, interpretation, cancellation, and completion status;
- provenance, supersession links, and audit events.

An order may be valid but not yet received, received but not understood, understood but infeasible, feasible but delayed, executing, partially completed, blocked, failed, cancelled, or completed. These are distinct states.

### 3.3 Plan

A plan is a revisable interpretation of one or more orders. It translates intent into phases, tasks, dependencies, resource requests, routes, timings, formation instructions, contingencies, and completion criteria.

A plan records assumptions and the information state used to create it. When material assumptions change, the plan is marked for review. A plan does not reserve or create a resource unless the owning system confirms a reservation.

### 3.4 Task

A task is an actionable unit of work assigned to a military unit or subordinate headquarters. Tasks can be sequential, concurrent, conditional, or mutually exclusive. Dependencies must be explicit.

Examples include move, occupy, reconnoitre, observe, screen, defend, attack, suppress, bombard, escort, withdraw, establish communications, resupply, recover, repair, or support another formation. Task types invoke specialist-system contracts rather than embedding every effect in the command system.

### 3.5 Operational picture

The operational picture is the command's time-bounded, source-attributed understanding of friendly and hostile forces, terrain, weather, routes, logistics, communications, and mission progress. It is built from received reports, sensors, intelligence assessments, subordinate reports, and permitted prior knowledge.

Each material report should preserve source, observation time, receipt time, confidence, freshness, and any known uncertainty. A stale report must not silently become current truth.

## 4. Order lifecycle

The baseline lifecycle is:

1. **Drafted:** Created by an eligible player, command process, or doctrine-governed automation.
2. **Validated:** Checked for authority, syntax, prerequisites, constraints, and obvious feasibility.
3. **Issued:** The issuing authority commits the order and its provenance.
4. **Transmitting:** Communications attempts delivery through an eligible path.
5. **Received:** Recipient headquarters or unit has the order; this does not imply comprehension.
6. **Interpreted:** Recipient resolves intent, ambiguity, priority, and constraints according to doctrine and capability.
7. **Planned:** One or more plans and tasks are prepared.
8. **Executing:** Tasks have been accepted by their owning systems and are progressing.
9. **Reassessing:** A scheduled or triggered review checks the order, plan, tasks, and situation.
10. **Completed, partially completed, blocked, failed, cancelled, or superseded:** Terminal or review states with a reason and record of achieved effects.

Not every order needs a separate visible transition for every stage. Internally, however, lifecycle state and timestamps must be available for simulation, debugging, and player feedback.

An order can be transmitted in fragments or relayed if Communications supports that model. If a path is unavailable, the order remains pending, may use an authorised alternative path, or expires according to its constraints. The system must not assume instant global connectivity.

## 5. Planning and feasibility

### 5.1 Required planning inputs

Planning should consider only relevant inputs for the task, including:

- unit identity, organisation, capability, subordinate composition, command relationship and current orders;
- commander and staff capability, doctrine, experience, workload and operational familiarity;
- current readiness, fatigue, cohesion, morale, suppression, casualties and equipment state;
- current and expected route, terrain, obstacles, crossings, bridges, road capacity, weather and visibility;
- movement mode, mobility, formation, spacing, pace and time window;
- known friendly and hostile observations, their age and confidence, and likely threats;
- fuel, ammunition, supply, medical, maintenance and other required support;
- communications availability and order latency;
- task priority, dependencies, rules of engagement and acceptable risk.

The planner may use estimates, but must distinguish estimated values from authoritative facts and record significant assumptions.

### 5.2 Feasibility outcomes

Planning returns a structured outcome rather than a binary success flag:

- feasible under current assumptions;
- feasible with a stated risk or reduced capability;
- feasible only after a prerequisite or resource request;
- infeasible with an identified blocking constraint;
- indeterminate because essential information is unavailable.

The planner may propose alternatives where doctrine and the order permit them. It must not silently alter the mission objective, violate rules of engagement, invent capability, bypass a blocked crossing, or assume supplies that do not exist.

### 5.3 Resource allocation

When several tasks compete for the same unit, route, fire support, communications capacity, supply stock, or other scarce resource, allocation must use explicit priority and compatibility rules. Candidate factors include order priority, task urgency, capability match, readiness, fatigue, distance, risk, existing commitments, and commander's intent.

The exact scoring policy is configuration or system design, not an unexplained universal multiplier. Ties must be resolved deterministically. Reservations and consumption remain owned by Logistics, Inventory, Communications, or the relevant system.

## 6. Hierarchy, delegation, and staff workload

A headquarters coordinates its assigned units and may delegate planning to subordinate headquarters. It should communicate objective, constraints, priorities, timing, coordination requirements, and permitted initiative rather than micromanaging every low-level action in every circumstance.

The amount of detail delegated depends on doctrine, task type, command capability, staff capacity, communications, operational tempo, and scenario configuration. A low-capacity headquarters may simplify plans, process fewer simultaneous tasks, delay lower-priority work, or request support. It must not silently process unlimited complexity.

Staff workload can be affected by:
- number of subordinate units and active tasks;
- order complexity and ambiguity;
- number of concurrent operations;
- volume and quality of reports;
- map, route, and coordination complexity;
- communications disruption and repeated failed coordination;
- casualties, displacement, damage, or loss of key staff.

Commanders are persistent Character entities. Judgement, experience, training, traits, fatigue, stress, and relevant relationships may influence command behaviour only through defined, testable rules. Avoid a single opaque commander score that overrides the underlying causes.

## 7. Order delay and effective command

Order delay is composed from explicit stages rather than a single arbitrary delay where practical:

- preparation and staff queue;
- transmission and relay;
- receipt and acknowledgement;
- interpretation and decision;
- planning and task allocation;
- subordinate coordination and execution start.

Not every stage needs a separate clock in the first playable implementation, but the model must preserve the distinction between issued and acted-upon orders. Delay can be affected by headquarters capacity, order complexity, communications quality, path length or relay structure where relevant, unit mobility, EW, damage, staff availability, and scenario rules.

Distance is not a universal penalty independent of mechanism. It matters where it affects transmission, liaison, travel, coordination, sensor coverage, or command-post movement. A functioning long-range network may outperform a nearby but disconnected unit. Use the Communications system's actual link state instead of duplicating radio range calculations.

## 8. Reassessment and replanning

### 8.1 Periodic reassessment

Each headquarters has a reassessment policy based on echelon, doctrine, commander and staff effectiveness, workload, mobility, and operational tempo. Smaller units may review local conditions more frequently than large formations, but no fixed interval is universally correct. Defaults and ranges must be configurable and calibrated through simulation tests.

For the initial model, use a scheduled next-review time plus event-triggered invalidation. Do not hard-code a universal company/division interval as a rule for all armies, eras, or task types.

### 8.2 Event-triggered reassessment

Trigger or prioritise review when a material event affects feasibility, risk, or task progress, such as:
- route blocked, bridge destroyed, crossing capacity changed, or significant terrain state change;
- new credible threat observation or material change in contact confidence;
- subordinate unit lost, detached, delayed, suppressed, exhausted, or rendered ineffective;
- fuel, ammunition, medical capacity, or required supply falls below a task threshold;
- loss or restoration of communications;
- order changed, superseded, cancelled, or expired;
- unexpected movement delay, congestion, formation separation, or failed task;
- casualties, leader loss, equipment damage, morale/cohesion change, retreat, or rout;
- change in weather or environmental conditions that materially affects task assumptions.

Event triggers request reassessment; they do not force every headquarters to recompute every plan immediately. The scheduler must coalesce repeated events, prioritise urgent changes, and respect processing capacity.

### 8.3 Replanning outcomes

Reassessment may confirm the plan, adjust a route or timing, reallocate tasks, request support, hold position, use an authorised contingency, withdraw, or report inability to continue. A material plan change should preserve the prior version and reason. Avoid oscillation by using hysteresis, minimum commitment windows, or doctrine-governed thresholds where repeated small fluctuations would otherwise cause thrashing.

## 9. Reactions and initiative

Immediate local reactions may be permitted when a unit observes a threat, faces imminent danger, encounters an obstacle, loses a subordinate, or detects that an order is impossible. Reactions must be defined by doctrine, rules of engagement, current order constraints, unit capability, and available information.

Possible responses include halt, seek cover, report contact, continue under reduced speed, disperse, deploy, return fire, break contact, request support, or take a locally authorised alternative route. No unit should automatically choose the tactically optimal response from perfect knowledge.

A reaction may temporarily interrupt a task. It must report the interruption, retain or revise the task state, and trigger reassessment where required. Local initiative does not grant authority to ignore strategic constraints or undertake unrelated operations.

## 10. Movement and formation coordination

Command owns the requested movement objective and constraints. Movement owns route traversal, position, terrain interaction, movement time, and physical movement outcomes.

A movement plan can specify route or route preferences, destination or area, timing, formation, order of march, pace, spacing, halt conditions, crossing priority, concealment requirements, and allowed contingencies. The Movement system evaluates actual traversability and reports progress or failure.

Formation movement must coordinate:
- subordinate sequence and order of march;
- pace and mobility differences;
- cohesion, spacing, and acceptable separation;
- route bottlenecks, road capacity, bridges, ferries, and crossings;
- halts, congestion, traffic, obstacles, damage, and weather;
- supply, fatigue, and arrival windows;
- explicit detachments and authorised split/rejoin behaviour.

A formation's estimated arrival is not a guarantee. Subordinate units may arrive at different times, and a formation may become separated or lose cohesion. The system must distinguish the headquarters' planned route from each unit's actual route and position.

Single-unit movement follows the same authoritative Movement contract without unnecessary formation coordination. Neither movement mode may bypass obstacles or alter geographic truth.

## 11. Combat tasking and feedback

Command issues missions and constraints; Combat and Weapons resolve engagements and effects. Supported task families may include direct fire, anti-personnel fire, anti-armour fire, area fire, bombardment, registration, airstrikes, and other branch-appropriate actions.

A fire-support task may specify target or target class, area, timing, priority, ammunition constraints, desired effect, engagement rules, and abort conditions. Target validity and confidence depend on available information. Combat determines eligibility, weapon/munition effects, hit or damage outcomes, and relevant losses. Logistics determines ammunition availability and consumption.

Registration or preparatory fire is a distinct task when the weapon and doctrine support it. It may improve later fire effectiveness only through a documented Combat effect model. Airstrikes require the relevant air-unit availability, mission planning, weather, basing, fuel, weapon, detection/intelligence, and air-defence interactions; an order alone must not guarantee arrival or success.

Combat feedback to command can include:
- casualties, equipment and platform damage;
- suppression and temporary loss of effective action;
- cohesion, morale, fatigue and readiness changes;
- ammunition expenditure and remaining capacity;
- friendly-fire or target-identification uncertainty where modelled;
- withdrawal, retreat, rout, surrender, or inability to continue;
- changed threat estimates and reports from participating units.

The owning systems determine these effects and publish events. Command uses them to reassess task viability, request support, change priorities, or issue authorised follow-on orders. Avoid duplicate application of combat or morale effects.

## 12. Information, detection, and electronic warfare

Command plans against the information available to it. Sensors and Detection own sensor observations, detection, classification, identification, tracking, and uncertainty. Intelligence owns fusion and assessments according to its specification. Communications owns report delivery and delay. Electronic Warfare supplies compatible interference or deception effects; it does not directly rewrite command knowledge.

A detected contact is not automatically identified. A stale track is not current truth. A report generated by a subordinate cannot be treated as received before the relevant communication event. If EW disrupts communications, orders and reports may be delayed or lost according to Communications rules, while already received information remains known unless a separate rule explicitly changes memory or confidence.

## 13. Logistics and sustainment

Plans must express supply and support requirements without creating stock. Logistics determines demand, allocation, movement of supplies, delivery, and consumption; Inventory owns authoritative holdings and transactions. A plan may request replenishment, estimate a shortage, prioritise a convoy, or delay a task pending delivery.

Fuel, ammunition, medical support, maintenance, crew fatigue, and repair availability can limit task feasibility and endurance. A planned resupply is not completed until Logistics confirms delivery. A convoy or supply formation is itself subject to command, movement, detection, interdiction, damage, and capacity constraints.

## 14. Time, scheduling, and simulation performance

Command processing is event-driven where possible, with scheduled reassessment and bounded work queues. It must not recalculate every headquarters and every plan on every rendered frame.

Use the authoritative simulation clock. Store explicit event times, review times, deadlines, and task progress. Coalesce redundant triggers; prioritise time-critical tasks; avoid unbounded queues; and make ordering deterministic where events share a timestamp.

A command cycle should:
1. collect eligible new orders, reports, and material events;
2. update the headquarters' permitted operational picture;
3. select work according to urgency and doctrine;
4. validate authority and feasibility;
5. plan or revise tasks within capacity;
6. dispatch task requests to owning systems;
7. consume accepted/rejected/progress/outcome reports;
8. schedule the next review and persist audit state.

The runtime may optimise implementation, but observable results must not depend on frame rate or incidental iteration order.

## 15. Data contracts

Conceptual records; these are behavioural contracts, not a frozen relational schema.

```text
Order
  order_id
  order_version
  originator_command_id
  effective_sender_id
  issuing_authority_id
  recipient_id
  order_type
  intent
  priority
  constraints
  parameters
  doctrine_reference
  rules_of_engagement_reference
  issued_at
  expires_at?
  status
  supersedes_order_id?
  provenance

OrderDelivery
  delivery_id
  order_id
  sender_id
  recipient_id
  communication_path_reference?
  sent_at
  received_at?
  acknowledged_at?
  delivery_status
  failure_reason?

Plan
  plan_id
  plan_version
  headquarters_id
  source_order_ids[]
  operational_picture_version
  assumptions[]
  tasks[]
  resource_requests[]
  created_at
  review_at
  status
  revision_reason?

Task
  task_id
  plan_id
  task_type
  assigned_unit_id
  priority
  prerequisites[]
  parameters
  owning_system
  requested_at
  accepted_at?
  started_at?
  completed_at?
  status
  blocking_reason?
  outcome_reference?

CommandEvent
  event_id
  event_type
  occurred_at
  observed_or_received_at
  source_entity_id?
  affected_order_ids[]
  affected_plan_ids[]
  urgency
  provenance
```

### 15.1 Invariants

1. Every order has a valid originator and auditable authority.
2. Transmission, receipt, acknowledgement, interpretation, and execution are distinct states.
3. The effective sender is resolved through authority and provenance, not inferred solely from proximity.
4. Plans use only information legitimately available to the relevant command.
5. Every task has one authoritative owning system for its physical or economic effects.
6. A task cannot complete before its prerequisites and owning-system outcome permit it.
7. Resource requests do not create or reserve resources without owning-system confirmation.
8. Movement tasks cannot mutate position outside Movement.
9. Fire tasks cannot apply damage outside Combat.
10. Communications alone determines message delivery and communication availability.
11. Reports preserve provenance, age, and uncertainty where relevant.
12. Reassessment is scheduled or triggered by defined causes and respects command capacity.
13. Competing orders and scarce-resource allocations use explicit deterministic precedence.
14. Repeated event delivery cannot duplicate a state transition or physical effect.
15. Cancelled or superseded orders stop initiating new work, while already executed effects remain in the historical record.
16. Save/load preserves in-flight orders, tasks, queues, review times, reservations, and event identifiers.
17. A hidden enemy entity is not revealed merely because a planner internally models a possible threat.
18. User-interface actions do not reroll combat or alter simulation outcomes.

## 16. Player feedback and observability

The interface should explain what the command believes, what it ordered, what has been accepted, what is currently happening, and what is blocking progress. Distinguish:

- order issued versus received;
- received versus interpreted;
- plan created versus accepted by the relevant system;
- task in progress versus complete;
- planned route versus actual route;
- estimated arrival versus actual arrival;
- known contact versus uncertain estimate;
- resource requested versus allocated versus delivered;
- task failure versus task not yet attempted.

Where appropriate, show delay, workload, stale information, unavailable subordinates, route constraints, low readiness, resource shortage, disrupted communications, or a command's lack of authority. Do not expose hidden enemy information as a debugging convenience in normal player-facing views.

## 17. Acceptance criteria

### Orders and authority
- An unauthorised entity cannot issue an order unless explicit delegated or emergency authority permits it.
- Conflicting orders resolve under a documented precedence rule and retain provenance.
- A relayed order preserves its originator while recording the relay path.
- An order may be delayed or lost when Communications reports that outcome.
- An order does not cause execution before receipt and interpretation conditions are satisfied.

### Planning and reassessment
- An order that requires an unavailable capability is reported infeasible or indeterminate, not silently fulfilled.
- A blocked bridge invalidates affected routes and triggers relevant reassessment without teleporting units.
- A new credible threat may trigger review but does not automatically reveal the threat's true identity.
- Repeated events are coalesced and do not cause unbounded replanning.
- Staff workload can delay or prioritise work according to documented policy.
- Replanning retains previous plan versions and records material reasons.

### Movement and formations
- Formation movement respects route, crossing, terrain, weather, and unit mobility constraints.
- A subordinate may arrive later than the formation's estimate without being forced to the planned arrival time.
- Formation separation, congestion, halt, and reorganisation are represented through explicit state and rules.
- Single-unit movement uses the same Movement authority without formation-only overhead.

### Combat and sustainment
- Direct fire, anti-personnel, anti-armour, area fire, bombardment, registration, and airstrike orders resolve through supported Combat/Weapons contracts.
- Ammunition consumption and resupply are confirmed by Logistics/Inventory.
- Combat effects feed back to command once, with no duplicated casualties, suppression, fatigue, or damage.
- A unit that is suppressed, exhausted, out of ammunition, or otherwise incapable may fail, pause, or revise a task according to doctrine rather than being guaranteed success.

### Determinism and persistence
- Equivalent saved states and event streams produce reproducible command decisions under the simulation's determinism policy.
- Save/load preserves pending orders, plans, task state, and scheduled reviews.
- Replayed or retried events do not duplicate delivery, movement, combat, or logistics effects.
- Simulation outcomes do not depend on rendering frame rate or UI interaction.

## 18. Implementation sequence

1. **Contracts and lifecycle:** Order/task states, authority, provenance, deterministic transitions, save/load.
2. **Basic command loop:** One headquarters, direct subordinate orders, task requests and outcome reports.
3. **Communications and latency:** Delivery states, delays, loss, relay and effective sender.
4. **Movement and routes:** Single-unit tasks, route feasibility, obstacles and blocked-route feedback.
5. **Formation coordination:** Order of march, cohesion, spacing, bottlenecks, subordinate arrival and reorganisation.
6. **Periodic and event-triggered reassessment:** Review scheduling, trigger coalescing, bounded workload.
7. **Combat and logistics feedback:** Fire tasks, resource constraints, casualties, suppression, fatigue, supply and maintenance.
8. **Doctrine and initiative:** Local reactions, bounded autonomy, withdrawal and other doctrine-governed behaviours.
9. **Advanced operations:** Registration, bombardment coordination, airstrike planning, deception-aware planning and joint operations.
10. **Performance and calibration:** Large formations, high event rates, determinism, acceptance tests and scenario balancing.

## 19. Definition of done

The initial command implementation is complete when orders have durable lifecycle and provenance; authorised headquarters interpret and delegate them; plans account for known capability and constraints; Communications controls delivery; Movement and Combat own their outcomes; Logistics owns stock and delivery; periodic and event-triggered reassessment work without omniscient knowledge; subordinate initiative follows doctrine; and automated tests verify determinism, save/load, cross-system ownership, and failure handling.

**Design objective:** Command should feel capable but fallible. Good organisation, staff, doctrine, communications, leadership, information, and sustainment should improve execution, while friction, uncertainty, workload, physical constraints, and enemy action create meaningful operational decisions.
