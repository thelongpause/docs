# Military Branches & Unit Organisation

**Project:** Project Sovereign  
**System:** Military Simulation  
**Legacy document ID (retired):** MIL-ORG-001 (reassigned to `military-branches-and-unit-organisation.md`)  
**Status:** System specification, draft 1  
**Historical baseline:** 1 January 1955  
**Technology horizon:** At least 2026

> **Legacy / superseded specification:** This concise draft is retained for historical comparison and is not the implementation authority. Use [`military-branches-and-unit-organisation.md`](military-branches-and-unit-organisation.md) as the canonical specification. Any unique requirements in this file must be reconciled into the canonical document before this file is archived or removed. See [`canonical-specification-register.md`](../../technical/architecture/canonical-specification-register.md).

## 1. Purpose and scope

This specification defines the shared organisational model for national armed forces: branches, commands, formations, units, installations, command appointments, organic structures, temporary attachments, unit condition, training frameworks, and force construction.

The model must support historically appropriate military organisations from 1955 onward, while remaining extensible to later doctrine, technology, and organisational practice. It must support land, naval, air, joint, and specialised forces without forcing them into identical tactical structures.

This document defines entities, relationships, and responsibilities. Detailed combat resolution, order execution, sensors, logistics, personnel simulation, construction, and equipment performance are owned by their respective systems.

## 2. Foundational principles

1. **Organisation is not combat power.** A unit's effectiveness emerges from its actual personnel, equipment, condition, training, experience, leadership, doctrine, support, and circumstances.
2. **Identity persists.** A unit retains its identity and history through changes of commander, location, attachment, or organic establishment.
3. **Permanent and temporary relationships are distinct.** Organic membership must not be overwritten by operational command or temporary attachment.
4. **Templates define intended establishments, not guaranteed strength.** Actual formations may be understrength, unequipped, untrained, or otherwise unready.
5. **The same framework supports every branch.** Branch-specific composition and behaviour are defined by data and connected systems.
6. **AI commanders exercise bounded judgement.** They may adapt plans and recommend changes within their authority; the precise order and authority rules belong to Doctrine, Command & Operations.
7. **Guidance is not a hard restriction.** The Player may create unconventional formations where structural constraints permit, with clear warnings about consequences.

## 3. Organisational hierarchy

A nation's military may contain a Military Establishment, Military Branches, Commands, Formations, Units, and Installations. Actual hierarchies vary by nation, era, and branch.

Illustrative land hierarchy:

- Military Establishment
  - Army
    - Field Army or Army Command
      - Corps
        - Division
          - Brigade or Regiment
            - Battalion
              - Company
                - Platoon
                  - Section or Squad

This is illustrative, not a mandatory universal tree. Some forces omit echelons, use different naming conventions, or organise specialist units differently. The model must support formations containing formations as well as formations containing units.

### 3.1 Military Branches

A Military Branch is an institutional service or branch within a nation's armed forces, such as Army, Navy, Air Force, Marines, or another nation-specific service. A branch may define its doctrine, establishment conventions, personnel career structures, training institutions, and equipment preferences.

Branch membership does not by itself define operational command. Cross-branch units may be assigned to joint commands while retaining their institutional identity.

### 3.2 Commands

A Military Command is an authority or organisational grouping. Commands may be administrative, territorial, operational, functional, or joint. These roles must be represented explicitly rather than inferred from a name.

Administrative command manages matters such as personnel, basing, and force generation. Operational command concerns the authority to direct units for missions. These relationships may be held by different headquarters.

### 3.3 Formations and units

A Formation is an organisational grouping that can contain subordinate formations, units, or both. A Unit is a persistent military entity with a defined identity, composition, condition, and capability profile. The distinction must be flexible enough to represent a headquarters, a tactical unit, a ship, an aircraft operating element, or a specialist detachment.

A unit may exist independently of a larger formation. Not every grouping requires a dedicated HQ, although groups without an appropriate HQ should have reduced staff and coordination capacity where relevant.

## 4. Unit classes

Unit classes are a classification and guidance structure, not immutable tactical roles. A unit's actual role depends on its composition, equipment, courses, commander, doctrine, orders, and circumstances.

### 4.1 Headquarters Units

HQ Units provide command, staff, planning, coordination, and control functions for subordinate forces. Their effectiveness depends on staff capacity, communications, procedures, experience, training, leadership, workload, and the complexity of assigned tasks.

### 4.2 Line Units

Line Units are principal fighting elements expected to engage and manoeuvre directly against an adversary. Examples include infantry, reconnaissance, armoured formations, specialised assault engineers, and special forces.

### 4.3 Line Support Units

Line Support Units contribute directly to combat in a supporting role. Examples include anti-tank platoons, mortar batteries, weapons companies, and military police assigned to security or population-control tasks. Their precise capabilities and permissible roles depend on their actual equipment, training, orders, and applicable rules.

### 4.4 Support Units

Support Units provide capabilities not primarily intended for direct engagement. Examples include indirect-fire artillery, intelligence, signals, construction engineers, logistics, medical support, and ceremonial units. Support units may still be attacked, defend themselves, or engage where their capabilities and orders allow.

### 4.5 Combat Base Units

Combat Base Units are a classification for fixed or semi-fixed military elements that provide combat-related capabilities from a location. The category may include some air-defence, coastal-defence, surveillance, or other base-based capabilities where the specific design calls for it. It does not replace the Installation class.

### 4.6 Installations

Installations are fixed-position facilities with persistent sites and facility identities. Examples include strategic radar sites, signals-monitoring stations, underwater surveillance facilities, bases, depots, and fixed command facilities. Installations do not move as ordinary field units do, but can be damaged, disabled, degraded, captured, abandoned, repaired, or decommissioned.

An installation's facility capabilities must be distinguished from the capabilities of its assigned staff. Its operational performance may depend on personnel, qualifications, equipment, power, communications, maintenance, and supplies.

## 5. Organic organisation and attachments

### 5.1 Organic structure

The Organic Structure is a unit's permanent organisational relationship. It defines the formation to which a unit institutionally belongs and its normal superior and subordinate relationships. For example, a company may belong organically to a battalion, which belongs to a brigade, which belongs to a division.

Organic relationships support institutional continuity, administration, doctrine, routine training, and familiarity. They should normally remain stable during operations, but can change through formal reorganisation. Reorganisation does not erase a unit's identity or history.

### 5.2 Organic superior and subordinate

A unit may have one organic superior unless the relevant organisational model explicitly permits otherwise. It may have zero or more organic subordinates. A top-level entity may have no organic superior. Prohibited cycles and self-parenting must be rejected.

### 5.3 Operational command and attachment

An Attached Unit is temporarily assigned to another command or formation for an operational, tactical, or administrative purpose. Examples include attaching engineers to an infantry formation for a crossing, anti-tank assets to a threatened brigade, or military police to a territorial command.

An attachment changes the relevant operational command relationship, not the attached unit's permanent identity or organic parent. A unit may be detached, returned to its organic superior, attached elsewhere, or assigned as an independent element. Permanent transfer to a new organic establishment is a separate action.

The data model must distinguish:
- Organic membership.
- Current operational command.
- Temporary attachment.
- Administrative authority.
- Direct Player orders.
- Formal transfer or disbandment.

### 5.4 Familiarity and coordination

Units that routinely train and operate together may coordinate more effectively due to joint training, shared doctrine, established procedures, leader familiarity, communications protocols, and shared operational history. Organic membership alone must not grant an unconditional bonus. Experienced attached units retain their proficiency, though unfamiliar command relationships may introduce temporary coordination costs.

### 5.5 Command without an HQ

Groups without a dedicated HQ are permitted. A designated lead unit or officer may coordinate the group, but it must not receive the full staff-processing and planning capability of a properly constituted headquarters. The consequence depends on the task's complexity and the command element's actual capacity.

## 6. Division Builder and force design

The Division Builder is the principal interface for creating, configuring, training, inspecting, and reorganising formations. Its underlying functionality must support multiple echelons, not divisions alone.

A typical workflow allows the Player to:
1. Select a template or begin from a blank establishment.
2. Choose formation class, intended roles, and organisational structure.
3. Add and arrange subordinate units and HQ elements.
4. Allocate weapons, vehicles, equipment, and other materiel.
5. Assign commanders and subordinate Officer Characters.
6. Configure specialist personnel and qualifications.
7. Select military courses and training frameworks.
8. Set organic superior, administrative command, garrison, and HQ location.
9. Review personnel, equipment, training, maintenance, and logistics requirements.
10. Validate and save the proposed establishment.
11. Begin recruitment, equipment allocation, training, and force generation.
12. Assess readiness and deploy when sufficiently prepared.

The interface should permit different creation orders where practical. A template defines intended organisation; it does not guarantee that the Player has the personnel, equipment, or training to fill it.

### 6.1 Validation

**Hard constraints** prevent invalid or unsupported configurations, such as invalid references, command cycles, or impossible equipment definitions. **Soft constraints** warn of weaknesses such as insufficient logistics, overloaded HQ staff, inadequate anti-armour capability, or training mismatches.

Soft recommendations can generally be overridden. An override does not grant missing capabilities. The interface should explain the operational consequences.

### 6.2 Templates and variants

Templates should support historical establishments, national standards, Player-designed formations, specialised missions, experimental designs, and saved variants. Variants should be traceable to their source template without requiring them to share the same actual strength or state.

### 6.3 Leadership recommendations

Relevant Officer Characters and military leaders may recommend composition, training, equipment, or organisational changes. Recommendations should be grounded in their expertise, role, experience, doctrine, personality, and biases. Characters may disagree. Recommendations are advisory unless the character has actual authority to approve or reject the design.

### 6.4 Force-generation costs

Formation creation may require recruitment, transfers, officer appointments, procurement, specialist training, facilities, ammunition, supplies, maintenance capacity, and time. Completing the Builder interface does not instantly make a formation operational.

## 7. Officer Characters and command appointments

Commanders are persistent Officer Characters, not interchangeable numerical modifiers. Their identity, career, skills, experience, personality, relationships, traits, and political outlook are managed by the Character and Personnel Systems.

A Command Appointment records the officer, unit or HQ commanded, role and authority, effective period, relevant qualifications, restrictions, and succession arrangement. Acting appointments and emergency succession must be supported where appropriate.

Commanders may influence morale, cohesion, fatigue management, training standards, initiative, planning, specialisation, unit culture, and administration. Effects should be domain-specific rather than a universal multiplier. A commander can be strong tactically but poor administratively, or excellent at training but cautious in offensive action.

Unit identity and culture develop through leadership, personnel, doctrine, shared experience, traditions, and events. A commander's ideology must not instantly transform every subordinate; cultural change emerges over time and may encounter resistance. Traits can persist across command changes while gradually evolving.

When a commander is killed, captured, incapacitated, removed, or transferred, the system should use establishment, seniority, qualifications, and doctrine to determine succession. Vacancies may reduce effectiveness without making the unit cease to function.

## 8. Unit statistics and condition

The model distinguishes Dynamic Statistics, which describe current operational condition, from Static Statistics, which describe more persistent proficiency and characteristics. “Static” does not mean immutable: experience, training, fitness, and traits can change over time.

### 8.1 Dynamic statistics

**Morale** represents willingness to continue performing a mission under pressure. It may be affected by casualties, isolation, encirclement, leadership, supply, perceived prospects, successful action, and rest. Low morale increases the likelihood of withdrawal, disobedience, surrender, or rout according to circumstances; it should not force a uniform outcome at a single threshold.

**Cohesion** represents the unit's current ability to function collectively. It affects coordination, formation integrity, order execution, weapon employment, and response to unexpected events. Casualties, suppression, fatigue, poor communications, unfamiliar attachments, and leadership losses may reduce it. Rest and reorganisation help restore cohesion. Reorganisation prioritises recovery and may delay other tasks; it does not restore lost equipment or personnel.

**Fatigue** represents accumulated physical and operational exhaustion. It influences movement, endurance, weapon handling, observation, cohesion, and morale resilience. Its accumulation depends on activity, fitness, transport, terrain, weather, night operations, and tempo. Rest reduces fatigue; an explicit Rest order prioritises recovery. Motorised units may experience less marching fatigue but incur vehicle wear, fuel use, and crew fatigue.

**Suppression** represents the immediate disruptive effect of hostile fire and threatening stimuli. It can impair observation, firing, movement, coordination, and fire-support direction. It usually rises under effective incoming fire and decays when the immediate threat subsides. Suppression is distinct from casualties and does not automatically cause retreat.

### 8.2 Persistent statistics and profiles

**Experience** represents accumulated practical experience. It may improve resilience, weapon employment, order execution, reorganisation, fieldcraft, and coordination. Combat experience must be distinguished from exercise and other service experience where relevant. Experience gains should reflect actual participation; a HQ must not automatically gain the same direct combat experience as its subordinate infantry.

**Training** represents acquired proficiency from formal instruction, exercises, collective training, and professional development. It is task-specific and complements experience. A unit may be highly trained but combat-inexperienced, or experienced but untrained in a newly introduced capability.

**Fitness** represents physical conditioning and endurance. It affects dismounted movement, fatigue accumulation, recovery, load carriage, and physically demanding tasks. Its impact must reflect the unit's actual roles.

**Aggression and initiative** represent the tendency to favour assertive action and exercise discretion. They are not capability scores. Aggression may influence risk tolerance, advance, pursuit, and exploitation, but can also lead to overextension or actions inconsistent with Player intent. Aggression alone must not cause war crimes. Criminal conduct belongs to the interaction of traits, institutional culture, incentives, orders, discipline, oversight, and rules.

**Courses and qualifications** record completed instruction and resulting competencies. Qualifications have a defined scope and may apply only to particular personnel. A unit does not gain universal proficiency because a small number of its members completed a specialist course.

**Traits and esprit de corps** represent persistent identity and behavioural tendencies arising from tradition, leadership, recruitment, doctrine, training, experience, and events. Traits require defined effects and conditions rather than arbitrary bonuses. Esprit de corps can strengthen through achievement and solidarity, or deteriorate through defeat, hardship, and poor leadership.

### 8.3 Additional state

Other systems may supply or own personnel strength, equipment availability, ammunition, fuel, supply, medical capacity, communications, readiness, staff workload, and specialist coverage. These must be referenced from authoritative sources rather than duplicated as competing records.

### 8.4 No universal combat-strength multiplier

Combat effectiveness must emerge from relevant processes. Movement evaluates terrain, transport, fatigue, mobility, and orders. Observation evaluates sensors, visibility, proficiency, suppression, and information. Firing evaluates weapon design, ammunition, target, crew proficiency, and conditions. Command execution evaluates staff capacity, communications, cohesion, experience, and command relationships. UI summaries may estimate effectiveness, but must not replace the underlying models.

## 9. Military courses and training frameworks

A Military Course is a defined programme that develops a skill, qualification, or capability. A Training Framework is an ordered programme of courses and phases for a unit or personnel group.

Courses may specify eligibility, prerequisites, objectives, unit/personnel scope, duration, instructors, facilities, equipment, ammunition, cost, assessment, qualification outcome, validity, refresher requirements, and research prerequisites. Frameworks support sequential phases, parallel courses where feasible, specialist streams, and refresher training.

Illustrative infantry battalion framework:

| Phase | Course | Intended outcome |
|---|---|---|
| I | Physical conditioning | Fitness and endurance |
| II | Marksmanship | Proficiency with assigned weapons |
| II | Night marksmanship | Weapon proficiency in low light |
| III | Urban combat | Movement and collective combat in urban terrain |
| IV | Parachute assault | Qualification for eligible personnel and airborne tasks |
| V | Collective exercises | Integration of personnel, equipment, subunits, and command |

Special forces frameworks may prioritise small-unit tactics, reconnaissance, unconventional warfare, infiltration, survival, specialist communications, mission planning, and independent execution.

Officer and specialist courses may include leadership, staff college, command planning, artillery coordination, Forward Air Control/JTAC, signals, intelligence analysis, engineering, airborne, and amphibious qualifications.

Research may unlock courses; it does not automatically train units. Experienced Officer Characters may help develop training programmes, but validation and dissemination are required before a capability becomes widespread. Training consumes time, instructors, facilities, equipment, ammunition, and other resources. Qualification coverage must reflect the personnel actually trained. Training may be interrupted and resumed; partial completion does not grant a full qualification unless defined by the course. Skills may deteriorate without practice, and some qualifications may require renewal.

## 10. Installations

An Installation is a fixed-position facility intended to provide persistent military capability. Examples include strategic radar, signals intelligence, underwater surveillance, communications relay, air defence, naval basing, logistics, training, maintenance, and command facilities.

Facility capabilities derive from design, construction, installed systems, infrastructure, and physical condition. Staff capabilities derive from personnel, qualifications, equipment, training, experience, command, and condition. These are distinct inputs. A sophisticated facility may underperform when understaffed or poorly maintained; highly trained staff cannot create hardware the facility does not possess.

Construction orders define type/configuration, site, components, planned staffing, commander where appropriate, specialist qualifications, administrative authority, and intended assignment. Construction consumes materials, labour, equipment, funds, and time. Completion does not necessarily mean operational activation: commissioning, testing, power, communications, staff training, and other prerequisites may be required.

Installations have fixed-site references and persistent condition. They can be damaged, degraded, disabled, sabotaged, captured, abandoned, repaired, or decommissioned. Staff can be assigned in functional groups, and their qualifications and condition use the same underlying personnel systems as other military personnel. Support needs should reflect the facility's actual design rather than treating it as a field infantry unit.

An installation may be administratively part of one command while providing operational services to another. Physical location does not automatically determine command authority.

## 11. Readiness, garrison, and lifecycle

Readiness is task- and timeframe-dependent, not a universal measure of combat effectiveness. It is derived from personnel availability, equipment serviceability, ammunition and supplies, fuel, transport, training, qualifications, HQ staffing, communications, formation integrity, and outstanding force-generation requirements.

The model should support states such as Proposed, Authorised, Forming, Recruiting and Equipping, Training, Ready, Deployed, Recovering or Refitting, Reorganising, Inactive Reserve, and Decommissioned or Disbanded. These are not necessarily a single linear progression.

A garrison is a unit's normal administrative or peacetime station where applicable. The HQ location is the current physical location of its command element. These may differ, and subordinate elements may be distributed across locations. Installations have fixed sites; mobile units change location through movement.

## 12. Cross-branch applicability

**Army:** infantry, armour, mechanised forces, reconnaissance, artillery, engineers, air defence, signals, intelligence, logistics, military police, special forces, and other era-appropriate organisations.

**Navy:** vessels, shore establishments, naval bases, fleets, task forces, task groups, squadrons, flotillas, and other national structures. Ship inventory and condition remain distinct from the task group commanding it.

**Air Force:** aircraft and operating elements, flights, squadrons, wings, groups, air commands, and airbases. Aircraft inventory is distinct from the organisation operating the aircraft. Crew proficiency, maintenance, weapons, training, and sortie generation are handled by relevant systems.

**Joint and specialised commands:** may command units from multiple branches without erasing their organic identities. Joint operations must account for doctrine, communications, procedures, and inter-service familiarity.

## 13. Conceptual data model

| Entity | Responsibility |
|---|---|
| `MilitaryEstablishment` | National armed forces and overarching structure |
| `MilitaryBranch` | Institutional service or branch |
| `MilitaryCommand` | Administrative, territorial, operational, functional, or joint command |
| `Formation` | Organisational grouping of formations and units |
| `Unit` | Persistent military entity with composition, condition, and capabilities |
| `UnitTemplate` | Reusable intended establishment |
| `UnitMembership` | Organic membership and parent-child structure |
| `CommandRelationship` | Current command authority and superior-subordinate relationships |
| `Attachment` | Temporary assignment to another formation or command |
| `CommandAppointment` | Officer-to-command appointment and effective period |
| `UnitPersonnelAssignment` | Personnel allocations and specialist qualifications |
| `EquipmentAllocation` | References to authoritative equipment records |
| `UnitTrainingProfile` | Training levels and collective qualifications |
| `MilitaryCourse` | Course prerequisites, duration, costs, and outcomes |
| `TrainingFramework` | Ordered programme of courses |
| `TrainingEnrolment` | Participation and progress |
| `UnitCondition` | Morale, cohesion, fatigue, suppression, and other dynamic condition |
| `UnitExperienceProfile` | Accumulated experience categories |
| `UnitTraitAssignment` | Traits, provenance, and effects |
| `Installation` | Fixed facility and site |
| `InstallationFacility` | Facility components and capabilities |
| `InstallationStaffAssignment` | Personnel and qualifications assigned to installation |
| `GarrisonAssignment` | Designated garrison or station |
| `OrganisationalChange` | Formal changes to establishment or relationships |

Each unit has a persistent identifier independent of name, commander, location, template, and parent. The establishment, actual composition, equipment allocation, condition, training profile, and command graph must remain separate. Relationship integrity must reject self-parenting, prohibited cycles, invalid references, and unsupported command relationships.

## 14. System interfaces

- **Equipment & Materiel:** authoritative equipment designs, allocations, condition, and availability.
- **Equipment Design, Research & Production:** researched designs and manufactured output; research does not automatically upgrade existing units.
- **Doctrine, Command & Operations:** command authority, order processing, autonomy, staff capacity, and command delay.
- **Combat & Engagement:** consumes relevant capabilities, condition, ammunition, training, experience, and command context.
- **Sensors & Detection / Electronic Warfare:** consumes sensor, electronic equipment, staff proficiency, installation state, and communications.
- **Personnel and Character:** personnel identity, skills, traits, career history, assignment, recruitment, and losses.
- **Military Logistics:** supply, ammunition, fuel, maintenance, transport, and sustainment.
- **Military Construction:** infrastructure and permanent facilities.
- **Readiness and Deployment:** task-specific readiness and deployment state.

Each system owns its authoritative data and exposes the required information to the others. Avoid duplicated competing inventories or condition records.

## 15. Acceptance criteria

- Multiple branches and national organisational conventions can be represented.
- Formations may contain formations and/or units at multiple echelons.
- Organic membership, operational command, attachments, administration, and direct Player orders are distinguishable.
- Unit identity and history persist across reassignment and reorganisation.
- Templates describe intended establishments separately from actual composition.
- Hard constraints and soft recommendations are distinct.
- Units can be created without a dedicated HQ, with appropriate limitations.
- Command appointments support vacancies and succession.
- Morale, cohesion, fatigue, and suppression vary independently.
- Training and experience remain distinct; specialist qualifications have explicit scope.
- Aggression does not act as a universal bonus or automatically cause criminal conduct.
- Courses have prerequisites, costs, duration, outcomes, and interruption rules.
- Installations distinguish facility capabilities from staff capabilities and construction from activation.
- Readiness derives from relevant requirements, not one arbitrary statistic.
- Land, naval, air, and joint forces use the same foundational organisational model.
- Cross-system references use authoritative records and valid identifiers.

## 16. Open design considerations

The following should be resolved in the detailed implementation specifications:

1. **Personnel granularity:** individually simulate officers and relevant specialists, with aggregation for other personnel where appropriate for performance.
2. **Statistical scales:** define separate ranges, recovery rates, and thresholds for each condition field; do not assume all fields share one scale.
3. **Template evolution:** support national and historical variants without duplicating every template.
4. **Command autonomy:** define authority boundaries, conflicting orders, and AI reassignment rules in Doctrine, Command & Operations.
5. **Historical progression:** support the 1955 starting state and technological/organisational change through at least 2026.
6. **Simulation cadence:** update condition and relationships at event-appropriate intervals without unnecessary work for unaffected entities.
