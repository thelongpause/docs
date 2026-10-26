# Canonical Specification Register and Military Interface Audit

**Project:** Project Sovereign  
**Document ID:** ARCH-SPEC-001  
**Status:** Canonical governance register, draft 1  
**Canonical path:** `technical/architecture/canonical-specification-register.md`  
**Last audit:** 26 October 2026  
**Scope:** Repository-wide specification ownership, with detailed audit of Military Systems  
**Historical baseline:** 1 January 1955  
**Technology horizon:** At least 2026, extensible beyond

## 1. Purpose

This register identifies the authoritative specification for each system, distinguishes behavioural specifications from catalogues and schemas, records duplicate or stale documents, and defines cross-system interfaces that must be reconciled before implementation.

It does not replace domain specifications. A system specification owns its rules; data catalogues own reference content under those rules; technical schemas define implementation-facing data contracts; architecture references describe dependencies and ownership.

**Governance rule:** There must be one authoritative owner for each simulation fact and each state transition. Other systems may reference, cache, report, or derive views of that fact, but must not create competing authoritative records.

## 2. Canonical status vocabulary

- **Canonical:** The designated authoritative source for the stated domain.
- **Supporting:** A catalogue, schema, or architecture document that supports a canonical system but does not replace its rules.
- **In progress:** A system whose principal specification exists but has known open interfaces or incomplete acceptance tests.
- **Missing:** A required system specification has not been found in the audited repository.
- **Legacy / superseded:** Retained for history or comparison, but not an implementation authority.
- **Review required:** Content may be valuable, but overlaps or conflicts must be resolved before implementation.

## 3. Military canonical register

| Domain | Canonical path | Document ID | Status and authority |
|---|---|---|---|
| Branches, unit organisation, academies, Officer Character appointments, training profiles, unit lifecycle | `systems/military/military-branches-and-unit-organisation.md` | MIL-ORG-001 | Canonical behavioural specification. It contains the broader academy and career model and should be the base document for consolidation. |
| Equipment, materiel, equipment instances, configuration, condition, serviceability, inventory allocation | `systems/military/equipment-and-material-system.md` | MIL-EQM-001 | Canonical equipment/materiel behavioural specification. Inventory owns authoritative stock quantities and stock movements; Military owns equipment identity/configuration and military allocation semantics. |
| Component design, equipment design, research integration, prototypes, tests, qualification, manufacturing and fielding lifecycle | `systems/military/equipment-design-research-and-production.md` | No formal ID found in header | Canonical design and development lifecycle specification. Production owns execution and capacity; Inventory owns physical stock. |
| Doctrine, command relationships, headquarters, orders, command delay, operational planning, mission execution | `systems/military/doctrine-command-and-operations.md` | MIL-OPS-001 | Canonical command and operations specification. |
| Combat and engagements across domains | `systems/military/combat-and-engagement-systems.md` | MIL-CBT-001 | Canonical combat and engagement specification. |
| Sensors, detection, observations, contacts, tracks, classification and uncertainty | `systems/military/sensors-and-detection.md` | MIL-SENS-001 | Canonical sensor behaviour specification. |
| Sensor definitions, initial historical examples, signatures, calibration and technology progression | `systems/military/sensor-detection-catalogue.md` | MIL-SENS-DATA-001 | Supporting reference-data catalogue, not a second detection rules engine. |
| Electronic warfare, emissions, electronic attack/protection, jamming, countermeasures and EW effects | `systems/military/electronic-warfare.md` | MIL-EW-001 | Canonical EW behaviour specification. |
| Cross-system decision architecture, AI observations/actions, integration risks and tests | `systems/integration/simulation-integration-and-ai-decision-architecture.md` | INT-AI-001 | Canonical cross-system integration reference; domain rules remain with domain owners. |
| Military logistics and sustainment | `systems/military/logistics-and-sustainment.md` | MIL-LOG-001 | Canonical behavioural specification. Owns military supply demand, allocation, procurement coordination, shipment planning and delivery coordination. Inventory, Economy, Production, Infrastructure, Movement/Transportation and other systems retain their authoritative state. Interface audit and vertical-slice plan are documented; executable tests remain open. |
| Movement and Transportation | `systems/world/movement-and-transportation-system.md` | MOV-TRANS-001 | Canonical behavioural specification, initial draft. Owns physical movement execution, route feasibility, carrier traversal and arrival/interruption events. Detailed mode calibration, schemas and end-to-end conformance tests remain open. |
| Character System | `systems/characters/character-system.md` | CHAR-001 | Canonical initial behavioural specification. Owns persistent person identity, general attributes, personality, relationships, biography and global lifecycle. General attribute catalogue, schemas and domain-extension contracts remain open. |
| Communications and message delivery | `systems/communications/communications-system.md` | COMM-001 | Canonical initial behavioural specification. Owns message transport, routing, latency, delivery, acknowledgement and communication access state; source domains retain payload meaning. Schemas, channel catalogue and conformance tests remain open. |
| Intelligence | `systems/intelligence/intelligence-system.md` | INTEL-001 | Canonical initial behavioural specification. Owns intelligence requirements, source evaluation, assessments, confidence, dissemination decisions and actor-specific knowledge state; Sensors owns observations/tracks and Communications owns delivery. Schemas, calibration and conformance tests remain open. |
| Weather and Environment | `systems/world/weather-and-environment-system.md` | ENV-001 | Canonical initial behavioural specification. Owns actual weather/climate state, environmental events and forecasts; consuming domains own their effects. Variable catalogue, climate data, numerical models and conformance tests remain open. |
| Government procurement and mobilisation interface | `technical/architecture/government-procurement-and-mobilisation-interface.md` | GOV-IF-001 | Supporting interface contract defines Government authority decisions required by Military Logistics. Full Government System specification remains missing; Economy owns financial/commercial transactions and Inventory owns physical stock. |
| Research and Technology Progression interface | `technical/architecture/research-technology-progression-interface.md` | RES-IF-001 | Supporting interface contract defines technology knowledge/unlock boundaries needed by Military and equipment design. Full Research System specification remains missing; research progression rules, technology catalogue and data remain open. |
| Military training and course catalogue | Training concepts are included in the organisation specification; no separate canonical course catalogue found | TBD | Decide whether the initial course framework remains in the organisation system or requires a supporting catalogue. Do not create a duplicate training-state owner. |

## 4. Duplicate and legacy document resolution

The repository contains two shorter files that duplicate the domains covered by more comprehensive specifications:

| Retained legacy path | Canonical path | Resolution |
|---|---|---|
| `systems/military/branches-and-unit-organisation.md` | `systems/military/military-branches-and-unit-organisation.md` | Legacy / superseded as an implementation authority. Semantic delta review is complete for the known distinctive requirements: cross-branch/joint applicability and shared organisational concepts were consolidated into the canonical document. The file remains retained for history and comparison. |
| `systems/military/equipment-and-materiel-system.md` | `systems/military/equipment-and-material-system.md` | Legacy / superseded as an implementation authority. Semantic delta review is complete for the known distinctive requirements: equipment lots/batches, operational availability, custody, capture/transfer and disposal were reconciled into the canonical document. The file remains retained for history and comparison. |

The canonical choice favours the more comprehensive documents because they include broader domain coverage, explicit scope boundaries, dependencies, acceptance criteria and related-specification links. This is a documentation authority decision, not a claim that the shorter files contain no unique value.

**Legacy status is now explicit and the known semantic deltas have been reconciled.** Keep these files read-only as deprecated references until a separate repository-housekeeping decision archives or removes them. No implementation agent should treat them as current authority.

## 5. Non-military authorities used by Military

| Domain | Canonical path | Interface rule |
|---|---|---|
| World geometry, coordinates, spatial queries and Movement Grid | `systems/world/world-map-system.md` | Owns spatial truth; Military must not maintain duplicate geometry. |
| Terrain objects and terrain state | `systems/world/terrain-objects.md` | Owns terrain features and persistent terrain state; Movement, Sensors and Combat consume them. |
| Movement and transportation | `systems/world/movement-and-transportation-system.md` | MOV-TRANS-001 owns physical movement execution and route traversal; detailed schemas and conformance tests remain open. |
| Weather and environment | `systems/world/weather-and-environment-system.md` | ENV-001 owns actual weather/climate state, environmental events and forecasts; consuming domains own their effects. |
| Territorial claims, sovereignty, control, occupation and frontlines | `systems/world/national-territory-and-control-system.md` | Military presence does not automatically change legal sovereignty or control. |
| Characters | `systems/characters/character-system.md` | CHAR-001 owns persistent person identity and general attributes; Military owns officer role, rank, branch, appointments, service record, qualifications, military-specific traits and military decision behaviour. |
| Communications | `systems/communications/communications-system.md` | COMM-001 owns message delivery, latency, disruption and acknowledgement; source domains own payload meaning. |
| Intelligence | `systems/intelligence/intelligence-system.md` | INTEL-001 owns assessments, confidence, dissemination and actor-specific knowledge; Sensors owns observations/tracks and Communications owns delivery. |
| Government procurement and mobilisation authority | `technical/architecture/government-procurement-and-mobilisation-interface.md` | GOV-IF-001 defines the minimum authority contract for procurement/mobilisation; the full Government System remains missing. |
| Research and technology progression | `technical/architecture/research-technology-progression-interface.md` | RES-IF-001 defines the minimum technology-availability boundary; the full Research System remains missing. |
| Facilities and installations | `systems/infrastructure/installations-and-facilities-system.md` | Owns common facility identity, location/footprint, shared capacity, physical condition and lifecycle. Military owns specialised military functions and requirements. |
| Industry and economic capacity | `systems/economy/industry-data.md`, `systems/economy/economy-system.md` | Industry defines establishment types/configuration; Economy owns financial state and transactions. |
| Production processes and execution | `systems/production/production-processes-and-recipes.md`, `systems/production/production-system.md` | Recipe definitions and production execution remain separate from equipment design and military procurement intent. |
| Physical stock and movements | `systems/inventory/inventory-system.md` | Inventory owns stock quantities, reservations, transfers and stock-changing transactions. Military must not create a parallel ammunition or equipment stock ledger. |
| Population and labour | `systems/population/population-centres-system.md`, `technical/data-schemas/population-schema.md` | Population, demographic state and labour availability remain owned outside Military. |
| Shared relational data conventions | `technical/data-schemas/data-model-foundations.md` | Domain schemas must follow common identity, time, history, validation and transaction conventions. |
| Military cross-system interface contracts | `technical/data-schemas/military-interface-contracts.md` | SCHEMA-MIL-001 defines the initial shared event envelope and logical contracts for supply demand, procurement, shipment, movement, Inventory, communications, intelligence and Character extensions; implementation schemas remain open. |
| World simulation dependencies | `technical/architecture/world-simulation-dependencies.md` | Dependency map is aligned to MOV-TRANS-001 and MIL-LOG-001; detailed movement schemas and conformance tests remain open. |

## 6. Military interface ownership matrix

| Source / consumer | Authoritative owner | Required contract |
|---|---|---|
| Branches and organisation ↔ Character System | Character System owns person identity and general stats; Military owns military appointment and service state | Stable character ID, branch/rank/role, appointment history, military qualifications, command authority, career events. |
| Organisation ↔ Equipment | Equipment System owns design/instance/configuration/condition; Military owns establishment and unit assignment semantics | Unit equipment establishment, actual allocations, crew/operator prerequisites, serviceability and availability references. |
| Organisation ↔ Command | Organisation owns persistent and organic hierarchy; Command owns operational relationships and order processing | Distinct organic superior, administrative authority, operational control, attachment, order issuer/recipient and command succession. |
| Command ↔ Communications | Communications owns message transport and delivery state | Sender/recipient, message type, priority, latency, disruption, acknowledgement and report delivery. |
| Command ↔ Intelligence/Sensors | Sensors own observations and tracks; Intelligence owns intelligence assessments and dissemination rules, if established | Source, observation time, confidence, track state, report provenance, permitted recipients and uncertainty. |
| Sensors ↔ Equipment | Equipment owns sensor definitions/instances and physical condition; Sensors owns detection resolution and contact tracks | Installed sensor capability, operating state, signatures, detection inputs and observation outputs. |
| Sensors ↔ EW | EW owns EW emissions/effects; Sensors owns observations and contact tracks | Emitter observations, interference conditions, track degradation, deception evidence and uncertainty without direct hidden-state access. |
| EW ↔ Combat | EW owns the electronic effect; Combat owns engagement and terminal weapon outcomes | Compatibility, geometry, timing, seeker/receiver susceptibility, defensive countermeasure state and one authoritative resolution per effect. |
| Command ↔ Combat | Command owns orders, mission intent, authority and ROE policy lifecycle; Combat owns physical engagement resolution | Target/contact reference, authority, engagement policy, status and outcome events. An order is not an engagement result. |
| Combat ↔ Equipment/Inventory | Equipment owns platform/weapon condition and configuration; Inventory owns physical quantities and stock transactions; Combat owns firing/expenditure events | Weapon instance, ammunition type/lot, firing event, expenditure transaction, damage/loss event and idempotent application. |
| Equipment design ↔ Research | Research owns knowledge and research progress; design system owns candidate designs and engineering integration | Prerequisites, design versions, component compatibility, research unlocks, test evidence and uncertainty. |
| Equipment design ↔ Production ↔ Inventory | Design owns approved configuration; Production owns process/capacity/execution; Inventory owns stock outcomes | Qualification, bill of materials, manufacturing route, order/execution IDs, output acceptance, stock receipt and fielding state. |
| Military ↔ Logistics | Logistics must own movement and allocation of supply through the network; Inventory owns physical stock; Military owns demand and operational priority | Demand, stock reservation, transport, delivery, consumption, loss, shortage, priority and resupply forecast. |
| Military ↔ World/Terrain/Environment | World/Terrain owns geography and features; Environment owns environmental state when specified; Military owns units and military orders | Position/footprint references, movement routes, obstruction, terrain effects, weather/environment state and time. |
| Military ↔ National AI | National AI owns national objectives and policy; Military owns military feasibility, force readiness and operational execution | Resource demand, readiness estimates, force options, risk/cost forecasts, mobilisation requirements, operational outcomes and uncertainty. |

## 7. Canonical data and state ownership rules

1. **Person identity:** one persistent Character record per person. Officer is a military role and service state, not a duplicate person entity.
2. **Unit identity:** Military owns the persistent unit and formation identity, composition, organisational relationships and military condition.
3. **Equipment identity:** Equipment owns design, instance, component, configuration and condition. Military references equipment in unit establishments and allocations.
4. **Stock truth:** Inventory owns physical quantities, reservations, transfers, expenditure and receipts. Combat or Military may request and report consumption but cannot maintain a competing stock ledger.
5. **Facility identity:** Infrastructure owns the shared facility entity and common capacities. Domain systems own specialised usage.
6. **Geographic truth:** World owns geometry and spatial queries. Military stores references and operational state, not a second map geometry.
7. **Knowledge truth:** the simulation may store actual state; each actor's knowledge is a separate observation/report state governed by Sensors, Intelligence and Communications.
8. **Order truth:** Command owns order identity, authority, provenance and lifecycle. Combat owns engagement lifecycle. Both reference each other through stable IDs and events.
9. **Research/design/production separation:** research progress, design validity, production execution, inventory receipt and introduction into service are distinct states.
10. **Event accountability:** state-changing events must have one owner, stable identity, provenance, simulation time and idempotent handling.
11. **Derived values:** strength, readiness, combat effectiveness, production forecasts and AI evaluations should be derived from owned state. They must not become competing authoritative facts.
12. **Player parity:** Player and AI actions pass through the same authority checks, domain rules, resource constraints, information boundaries and simulation resolution.

## 8. Findings from the interface audit

### Critical

- **Movement/Transportation specification now exists as `systems/world/movement-and-transportation-system.md` (MOV-TRANS-001).** Its schemas, detailed mode calibration and conformance tests remain open.
- **Character System now has a canonical initial specification at `systems/characters/character-system.md` (CHAR-001).** Its general attribute catalogue, schemas, and integration tests remain open.
- **Communications and Intelligence now have canonical initial specifications** at `systems/communications/communications-system.md` (COMM-001) and `systems/intelligence/intelligence-system.md` (INTEL-001). Their schemas, catalogues, calibration and conformance tests remain open.
- **The end-to-end vertical slice has not yet been executed.** A documented plan exists, but runtime behaviour, transaction reconciliation and save/load idempotency remain unproven.

### High

- **Government procurement authority now has a supporting interface contract** at `technical/architecture/government-procurement-and-mobilisation-interface.md` (GOV-IF-001). The full Government System specification remains missing; Treasury/policy schemas and integration tests remain open.
- **Weather/Environment now has a canonical initial specification at `systems/world/weather-and-environment-system.md` (ENV-001).** Its variable catalogue, calibration and conformance tests remain open. A supporting Research/Technology Progression interface exists at `technical/architecture/research-technology-progression-interface.md` (RES-IF-001); the full Research System remains missing.
- **A draft Military interface schema now exists** at `technical/data-schemas/military-interface-contracts.md` (SCHEMA-MIL-001). Stable ID primitives, physical table schemas, lifecycle constraints and cross-system conformance tests still require implementation review.
- **Supply Base nomenclature is reconciled; capacity and baseline conversion data remain open.** A Supply Base Unit is a Combat Base Unit configured with a Supply Depot. Prior user design specifies Standard Stock per Unit Size per Day factors (Brigade 4, Battalion 2, Company 2, Platoon 1, Section 1) and emergency thresholds below 50% of baseline for Ammunition/Fuel and below 30% for Basics. Category-specific physical conversion, threshold denominator, reserved/in-transit treatment and numeric Handling/Payload/Bulk Fuel capacities still require explicit data contracts.
- **World dependency wording needs alignment.** Movement owns physical unit/platform traversal; Logistics owns supply planning, shipment coordination and delivery reconciliation. The precise transport asset/network capacity interfaces must be established in the missing Movement/Transportation specification.

### Medium / pending detailed review

- Canonical IDs and versioning for branch, unit class, formation, course, officer appointment, equipment design/instance, sensor, EW installation, order, contact, engagement, damage event, supply demand, shipment and inventory transaction.
- Common simulation time and event ordering across movement, command, detection, EW, combat and logistics.
- Domain-specific combat/art presentation must be separated from authoritative simulation state.
- Acceptance tests must exercise combined systems, not only isolated subsystem behaviour.

See [Military Interface Audit and End-to-End Vertical Slice](military-interface-audit-and-vertical-slice.md) for the detailed gap register, state boundaries, shared contract minimum, scenario fixture, failure branches and completion order.

## 9. Officer Characters and Military AI: explicit ownership decision

The intended boundary is:

### Character System owns

- Persistent person identity and general character lifecycle.
- General character statistics and attributes that are meaningful outside military service.
- General personality/psychology fields, if included in the shared Character model.
- General relationships and personal history where these are not exclusively military records.
- Death and other global character lifecycle events.

### Military Systems owns

- Officer status, branch affiliation, rank, service number or military identifier, appointment and command position.
- Military career and posting history, command assignments, decorations, military education and qualifications.
- Branch-specific military traits and professional competencies.
- Military doctrine familiarity, command experience, operational record and military-specific reputation.
- Officer succession, command authority, military promotion recommendations and military service transitions.
- The rules by which general character attributes and military-specific state influence military judgement, leadership, command latency, planning and execution.

### Shared boundary

Military references the Character ID and consumes permitted general attributes. Military does not duplicate those attributes as independent state. Military-specific traits should be stored in military-owned extensions or linked military records, with clear provenance and progression rules.

If “Military Art” means visual art, it is not a Character System simulation responsibility. Visual portraits, uniform depiction, rank insignia, equipment illustration and military visual assets belong to art/content presentation specifications. Military owns the semantic requirements for rank, branch, uniform rules, insignia, equipment and historical plausibility. A separate Character Art / Military Visual Reference catalogue may be warranted when visual production begins, but it must not own simulation rules. If the intended term was “Military AI,” it should not become a single standalone magic-brain system: national strategy belongs to National AI; command and operational planning belong to Doctrine, Command & Operations; engagement decisions belong to Combat; local execution belongs to units/platforms; and Officer Character judgement belongs at the Character/Military boundary. The common decision interface and information boundaries are already established conceptually in INT-AI-001 and should be implemented through these domain owners.

## 10. Military completion checklist

Military should not be declared complete until:

- [x] Canonical and legacy Military files are reconciled and clearly marked.
- [x] Logistics & Sustainment specification is authored, registered and linked.
- [x] Movement/Transportation behavioural specification and target route/traversal ownership contract are canonical.
- [ ] Movement/Transportation schemas, detailed mode calibration and conformance tests are complete.
- [x] Character System path and shared person/Officer ownership boundary are canonical.
- [ ] General attribute catalogue, lifecycle schemas and shared person/Officer conformance tests are complete.
- [x] Communications and Intelligence ownership boundaries are specified.
- [ ] Communications and Intelligence schemas, calibration and integrated conformance tests are complete.
- [x] Minimum Government procurement/mobilisation authority interface contract is documented.
- [ ] Full Government System specification, Treasury/policy schemas and conformance tests are complete.
- [x] Minimum Research/Technology Progression interface contract is documented.
- [ ] Full Research System specification, technology catalogue and progression rules are complete.
- [x] Weather/Environment behavioural ownership boundary is canonical.
- [ ] Weather/Environment variable catalogue, calibration and conformance tests are complete.
- [x] Shared military interface contracts and lifecycle/event rules are documented in a draft logical schema.
- [ ] Implementation-ready schemas, constraints and cross-system conformance tests are complete.
- [ ] Equipment, Inventory, Production, Economy, Government and Military ownership is reconciled for ammunition, spares, procurement and fielding.
- [ ] The company-to-unit sustainment vertical slice passes, including failure branches.
- [ ] Doctrine, command, sensors, EW and combat pass integrated acceptance tests.
- [ ] AI nations can request, allocate and sustain forces without bypassing production, stock, movement or readiness.
- [x] Character stats remain in Character System; military appointments and branch-specific traits remain in Military as the agreed ownership decision.
- [x] Military visual/art scope is presentation-only and does not duplicate character or simulation data.
- [ ] Cross-system tests cover save/load, event replay, idempotency, information boundaries and Player/AI parity.

## 11. Recommended next system

**Next integration tasks: shared schemas, calibration and vertical-slice execution.**

Initial canonical behavioural specifications now exist for Movement and Transportation (MOV-TRANS-001), Character (CHAR-001), Communications (COMM-001) and Intelligence (INTEL-001). Their schemas, calibration, domain integration and conformance tests remain open.

The Government procurement authority and Research/Technology Progression interface contracts are documented, but the full Government and Research Systems remain outside scope until Military is complete. Next, freeze the shared event/transaction contracts, complete Weather/Environment calibration, recover the numeric supply capacity data and category conversion rules, and execute the company-to-unit and command-to-intelligence vertical slices. Do not move to the next top-level game system until the Military completion checklist is satisfied.
