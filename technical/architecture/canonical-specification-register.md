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
| Military logistics and sustainment | No canonical system specification found | TBD | **Missing.** Must be authored before Military is considered complete. |
| Communications and command-message delivery | No dedicated canonical system specification found in audited Military folder | TBD | External dependency required by command, sensors and EW. Determine whether a general Communications System exists elsewhere or author the missing system. |
| Military intelligence and intelligence operations | No dedicated canonical system specification found in audited Military folder | TBD | External dependency for national AI, command and contact/report interpretation. Must be located or scoped explicitly. |
| Military training and course catalogue | Training concepts are included in the organisation specification; no separate canonical course catalogue found | TBD | Decide whether the initial course framework remains in the organisation system or requires a supporting catalogue. Do not create a duplicate training-state owner. |

## 4. Duplicate and legacy document resolution

The repository contains two shorter files that duplicate the domains covered by more comprehensive specifications:

| Retained legacy path | Canonical path | Resolution |
|---|---|---|
| `systems/military/branches-and-unit-organisation.md` | `systems/military/military-branches-and-unit-organisation.md` | Legacy / superseded as an implementation authority. Preserve it for history and comparison. Any unique requirements should be merged into the canonical document before archival or deletion is considered. |
| `systems/military/equipment-and-materiel-system.md` | `systems/military/equipment-and-material-system.md` | Legacy / superseded as an implementation authority. Preserve it for history and comparison. Any unique requirements should be merged into the canonical document before archival or deletion is considered. |

The canonical choice favours the more comprehensive documents because they include broader domain coverage, explicit scope boundaries, dependencies, acceptance criteria and related-specification links. This is a documentation authority decision, not a claim that the shorter files contain no unique value.

**Do not delete the legacy files yet.** First complete a semantic delta review and merge any unique, valid requirements into the canonical files. After that, either move them to a clearly marked archive or retain a short deprecation notice at their existing paths. No implementation agent should treat them as current authority.

## 5. Non-military authorities used by Military

| Domain | Canonical path | Interface rule |
|---|---|---|
| World geometry, coordinates, spatial queries and Movement Grid | `systems/world/world-map-system.md` | Owns spatial truth; Military must not maintain duplicate geometry. |
| Terrain objects and terrain state | `systems/world/terrain-objects.md` | Owns terrain features and their simulation properties; movement, sensors and combat consume them. |
| Territorial claims, sovereignty, control, occupation and frontlines | `systems/world/national-territory-and-control-system.md` | Military presence does not automatically change legal sovereignty or control. |
| Weather/environment | Dedicated canonical path not verified in this audit | Must be located or explicitly specified; relevant to movement, sensors, aviation, naval operations and combat. |
| Characters | Dedicated canonical Character System path not verified in this audit | Owns persistent person identity and general character stats. Military owns officer role, rank, branch, appointment, service record, military qualification, military-specific traits and command behaviour. |
| Facilities and installations | `systems/infrastructure/installations-and-facilities-system.md` | Owns common facility identity, location/footprint, shared capacity, physical condition and lifecycle. Military owns specialised military functions and requirements. |
| Industry and economic capacity | `systems/economy/industry-data.md`, `systems/economy/economy-system.md` | Industry defines establishment types/configuration; Economy owns financial state and transactions. |
| Production processes and execution | `systems/production/production-processes-and-recipes.md`, `systems/production/production-system.md` | Recipe definitions and production execution remain separate from equipment design and military procurement intent. |
| Physical stock and movements | `systems/inventory/inventory-system.md` | Inventory owns stock quantities, reservations, transfers and stock-changing transactions. Military must not create a parallel ammunition or equipment stock ledger. |
| Population and labour | `systems/population/population-centres-system.md`, `technical/data-schemas/population-schema.md` | Population, demographic state and labour availability remain owned outside Military. |
| Shared relational data conventions | `technical/data-schemas/data-model-foundations.md` | Domain schemas must follow common identity, time, history, validation and transaction conventions. |
| World simulation dependencies | `technical/architecture/world-simulation-dependencies.md` | Existing architecture reference is authoritative for its stated World/Population/Infrastructure/Production scope; extend it or add a Military dependency map rather than duplicating its ownership rules. |

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

- **Military Logistics & Sustainment is missing as a canonical system specification.** Doctrine and Combat refer to logistics, supply, ammunition, fuel, maintenance, transport and readiness, but the dependency is not yet backed by a dedicated owner specification.
- **Character System authority is unresolved in the repository index.** Prior design decisions require general character stats and person identity to remain in Character System, while military role, branch-specific traits, career and command behaviour belong to Military. The Character System's actual path and contract must be linked before schemas are frozen.
- **Communications and Intelligence ownership need to be located or specified.** Sensors, EW, Command and AI depend on report delivery and intelligence assessment. Do not embed those systems independently in each military document.
- **Duplicate Military specifications can produce contradictory implementation instructions.** The two shorter files must be explicitly marked legacy and their unique requirements reconciled.

### High

- **No single Military interface contract/schema is established.** Existing documents contain conceptual entities and acceptance criteria, but stable cross-system identifiers, event envelopes, lifecycle enums and transaction conventions need a shared contract.
- **Ammunition and combat expenditure require an idempotent transaction boundary.** A firing event must not consume ammunition twice after save/load, retries or event replay.
- **Order and engagement state must remain separate.** Command acceptance, order execution, weapon release, impact and mission effect are separate transitions.
- **Military course definitions need a clear data owner.** Organisation may own assigned course profiles and training state; the course catalogue should define course identity, prerequisites, duration, costs and outcomes if this needs to scale.
- **The 1955-to-2026 equipment and doctrine horizon needs data-driven progression.** Calendar dates must not directly grant technology or upgrade existing formations.

### Medium / pending detailed review

- Canonical IDs and versioning for branch, unit class, formation, course, officer appointment, equipment design/instance, sensor, EW installation, order, contact, engagement, damage event and supply transaction.
- Common simulation time and event ordering across movement, command, detection, EW, combat and logistics.
- Domain-specific combat/art presentation must be separated from authoritative simulation state.
- Acceptance tests need to exercise combined systems, not only isolated subsystem behaviour.

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

- [ ] Canonical and legacy files are reconciled and clearly marked.
- [ ] Logistics & Sustainment specification is authored and linked.
- [ ] Character System path and shared person/Officer contract are confirmed.
- [ ] Communications and Intelligence owners are identified or specified.
- [ ] Shared military interface contracts and lifecycle/event rules are documented.
- [ ] Equipment, Inventory, Production and Military ownership is reconciled for ammunition, spares, procurement and fielding.
- [ ] Doctrine, command, sensors, EW and combat pass an end-to-end vertical-slice test.
- [ ] AI nations can request, allocate and sustain forces without bypassing production, stock, transport or readiness.
- [ ] Character stats remain in Character System; military appointments and branch-specific traits remain in Military.
- [ ] A Military Art / visual-reference scope is recorded as presentation-only and does not duplicate character or simulation data.
- [ ] Cross-system tests cover save/load, event replay, idempotency, information boundaries and Player/AI parity.

## 11. Recommended next system

**Next Military system to author: Logistics & Sustainment.**

This is the largest missing military domain because command, operations and combat already rely on supply, transport, fuel, ammunition, maintenance, recovery, replacement, medical support and operational reach. Its specification must integrate with Inventory, Production, Infrastructure, World/Movement, Equipment, Military organisation, Command and Combat without duplicating any of their state.

After Logistics & Sustainment, complete the remaining Military interfaces and the end-to-end vertical slice. Do not move to the next top-level game system until the Military completion checklist is satisfied.

