# Military Branches and Unit Organisation System

**Canonical Path:** \`systems/military/military-branches-and-unit-organisation.md\`  
**System:** Military Systems  
**Document Type:** Behavioural Specification  
**Status:** Initial behavioural specification; cross-system architecture, reference data, balance values, and logical schema remain subject to review  
**Starting World Date:** 1 January 1955

---

## 1. Purpose

The Military Branches and Unit Organisation System defines the identity, classification, organisation, command relationships, personnel composition, training profile, readiness, and persistent organisational state of military forces.

It provides the common organisational foundation on which branch-specific systems and other military domains operate. It must support land formations, naval forces, air formations, specialist forces, support formations, mobile supply columns, and fixed installations without forcing every force into the same organisational or combat model.

The system is not a collection of fixed combat-stat multipliers. A unit's performance emerges from the interaction of its people, equipment, training, physical condition, organisation, leadership, orders, doctrine, environment, and immediate operational circumstances. The system records and exposes the relevant state and capability inputs; specialised systems calculate movement, detection, combat, logistics, equipment performance, and other effects.

This specification defines behaviour and ownership boundaries. It does not yet freeze the Military data schema, every global branch definition, every unit class, historical order of battle, course catalogue, character trait list, or numerical combat formula.

## 2. Design Principles

1. **Capabilities emerge from composition and condition.** A unit does not receive a universal combat rating that replaces the underlying causes of its performance.
2. **Classifications guide rather than constrain.** Unit classes support organisation, recruitment, recommendations, reporting, and interface guidance. Actual capabilities follow from validated personnel, equipment, training, command, and support.
3. **Common identity, specialised behaviour.** Military units share stable identity and common organisational contracts while Army, Navy, Air Force, Marines, and other configured branches may use different formation and equipment models.
4. **One authoritative owner per fact.** This system owns military organisation and unit-level organisational state, not the underlying authoritative records for people, equipment definitions, inventory, geographic geometry, or combat outcomes.
5. **Permanent organisation is distinct from current command.** Organic relationships represent established training and organisational relationships. Attachments and operational groupings represent changing command relationships.
6. **Orders are interpreted, planned, and executed through command.** Issuing an order does not teleport a unit or guarantee exact execution. Command capability, staff, communications, training, situation, and subordinate initiative can affect the plan and its execution.
7. **Characters are persistent participants in the simulation.** Commanders and senior military leaders are Character System entities assigned to military positions, not anonymous bonuses.
8. **Dynamic condition is not permanent capability.** Morale, cohesion, fatigue, and suppression are state variables that evolve during operations. Experience, training, fitness, courses, and traits are more persistent, but may change over time.
9. **A unit's actual capabilities are validated.** A label such as Infantry, Reconnaissance, or Special Forces does not grant a capability unsupported by the unit's equipment, qualified personnel, training, technology, and current condition.
10. **Historical and scenario validity matters.** Equipment, courses, branches, ranks, doctrine, and organisational templates must be available under the relevant scenario and simulation date. Illustrative examples are not automatically valid 1955 data.
11. **Events must be deterministic and auditable.** Recruitment, assignment, promotion, attachment, training completion, casualties, disbandment, and other authoritative transitions must not be applied twice after retries or save/load.
12. **Player guidance must not conceal trade-offs.** Builders and advisers can identify incompatibilities, risks, likely consequences, and alternative configurations, but must not imply that every suggested formation is universally optimal.

## 3. Scope

### 3.1 Included

- Canonical military branch catalogue and nation-specific branch activation.
- Military academies, branch leadership positions, and military administrative command roles.
- Military unit identity, broad unit classes, branch-specific organisational conventions, and unit composition references.
- Organic and operational/player command structures.
- Headquarters, command relationships, attachments, detachments, and temporary groupings.
- Unit organisational build, equipment-set references, staffing, qualifications, courses, and formation templates.
- Unit dynamic condition and persistent organisational statistics at the behavioural level.
- Officer Character assignments and command responsibilities.
- Training frameworks and course completion interfaces.
- Readiness, garrisoning, deployment eligibility, and unit formation validation.
- Fixed Installation staffing and the distinction between installations and mobile military units.
- Player-facing organisation builders, recommendations, validation, and reporting requirements.
- Interfaces to Equipment and Material, Personnel/Character, Training, Movement, Combat, Detection, Logistics, Installations and Facilities, Research, Government, and scenario initialisation.

### 3.2 Excluded or owned elsewhere

- Detailed equipment specifications, weapon characteristics, ammunition performance, platform characteristics, and material compatibility: Equipment and Material System.
- Physical stock, lots, reservations, cargo transfers, and replenishment transactions: Inventory and Logistics.
- Individual character identity, biography, general attributes, personal relationships, and non-military career transitions: Character System.
- Military personnel records, service status, qualifications, rank history, casualties, and personnel accounting: the agreed Military Personnel domain or explicit Military Systems contract; these must not be duplicated in aggregate Population records.
- Route traversal, continuous geographic position, terrain sampling, movement execution, and physical travel time: Movement and World Map contracts.
- Detection, sensor processing, electronic warfare, visibility, and intelligence confidence: Detection, Intelligence, and Electronic Warfare systems.
- Hit resolution, damage, casualties, suppression caused by fire, weapon effects, and tactical combat resolution: Combat and Weapons.
- Facility identity, shared physical condition, construction state, generic capacity, and installation-site geography: Installations and Facilities and World Map.
- Political authority, national budgets, procurement authorisation, mobilisation law, and government policy: Government and Political Systems.
- Research progression and technological discovery: Research.
- Numerical balance values, all historical ORBAT data, and the final relational schema.

## 4. Core Concepts and Definitions

### 4.1 Military Branch

A Military Branch is a canonical service category selected from a defined global catalogue. Nations activate or configure the branches they possess; players do not create arbitrary global branch identifiers during ordinary play.

Branch definitions support shared organisational expectations, academy defaults, recruitment and training rules, command positions, and branch-specific equipment or formation conventions.

National institutions may configure sub-branches, specialist arms, corps, communities, and qualification paths within the canonical framework. Examples include Army aviation, naval aviation, Marine aviation, and specialised signals or engineering corps. These do not automatically become new global branches.

The initial canonical branch catalogue remains a reference-data decision. It may include Army, Navy, Air Force, Marine Corps, and other globally defined service categories where required by the simulation. A scenario may omit a branch, activate it, or define national naming and institutional variations through configuration.

### 4.2 Military Academy

A Military Academy is a persistent institutional entity associated with a branch or explicitly configured training institution. When a nation activates a branch under the default rule, the simulation creates or resolves its associated academy and branch-chief position. Scenarios may supply pre-existing institutions, historical exceptions, shared academies, or alternative arrangements through explicit configuration.

An academy produces Officer Characters through a configured training and graduation process. It does not create anonymous command-stat bonuses. Graduation output, course capacity, staff, funding, admission rules, training quality, and graduation rank are configuration or simulation state, not hard-coded assumptions.

### 4.3 Officer Character

An Officer Character is a Character System character who has military service, rank, qualifications, command eligibility, and potentially one or more current military appointments.

The Character System remains authoritative for the character's identity, general attributes, biography, relationships, personality, and cross-domain career. Military Systems own military-specific service records, rank and appointment history, qualifications, military traits, and the relationship between a character and military command positions, subject to the final personnel-domain boundary.

Officer Characters may be promoted, assigned, reassigned, trained, decorated, relieved, dismissed, retired, arrested, killed, or transferred to another career or political role where supported by the owning systems and applicable rules. These are explicit state transitions, not deletion of character identity.

### 4.4 Military Unit

A Military Unit is a persistent, identifiable military organisation or operational formation with a declared organisational type, composition, command relationships, capabilities, state, and lifecycle.

A unit may represent a headquarters, combat formation, combat-support formation, combat base unit, or other configured mobile formation. Not every unit is intended to engage in direct combat. Not every unit is mobile. A unit's classification alone does not establish its capabilities.

Units may contain subordinate units, personnel, equipment/platforms, and assigned capability references. The exact composition model differs by branch and formation type, while common identity, ownership, command, status, and history remain consistent.

### 4.5 Unit Class, Role, and Capability

These concepts must not be conflated:

- **Unit Class:** A broad classification used for organisation, recommendations, reporting, and typical expectations.
- **Assigned Role:** The task or function the unit is configured or ordered to perform in a defined context.
- **Capability:** An evidenced ability to perform a task under specified conditions, based on composition, equipment, qualifications, training, technology, readiness, and support.
- **Formation Template:** A reusable proposed organisational and equipment configuration.
- **Unit Instance:** The actual persistent unit created from a template or assembled by the player.
- **Order or Mission:** An instruction or operational assignment that does not itself change the unit's permanent identity or organic organisation.

A unit can have multiple roles and capabilities. A role may be changed without changing the unit's identity, but doing so may require re-equipment, training, qualified personnel, and a period of reduced readiness.

### 4.6 Installation

An Installation is a fixed geographic entity with a specialised facility and operational capability. It is built or established at a specified location and does not move as an ordinary military unit.

Installations may host sensors, communications, surveillance, air defence, command, maintenance, medical, supply, research, or other configured capabilities. Examples include fixed radar and signals-intelligence sites and undersea surveillance facilities.

An Installation can have a military unit or staffing assignment associated with it. Its own facility capability is distinct from the personnel's qualifications and current condition. The Installation's physical facility makeup and construction state belong to Installations and Facilities; the staffing organisation and military command relationship belong to Military Systems; the detailed sensor or mission capability belongs to the relevant specialist system.

### 4.7 Combat Base Unit

A Combat Base Unit is a mobile or deployable military support formation organised around one or more configured depot or support capabilities. The initial intended core capability classes are Medical, Maintenance, and Supply.

Combat Base Units remain military units: they have identity, command, personnel, equipment, movement and deployment state, and may have defensive or other weapons where their configuration permits. They are not merely inventory containers or abstract service counters.

A unit may carry or operate multiple support capabilities if the configuration permits. The precise depot taxonomy and whether capabilities are combined in one unit or split among several units remain reference-data and logistics-design decisions.

## 5. Unit Classification Framework

The following classes provide a common classification framework. They are not an exhaustive list of all unit types, nor do they independently grant capabilities.

### 5.1 Headquarters Units

Headquarters Units command, coordinate, plan for, and support the execution of orders by subordinate units. Their effectiveness depends on staff capacity, command personnel, communications, doctrine, training, operational familiarity, information quality, and the number and complexity of assigned subordinate elements.

A headquarters may have organic subordinate units and temporary attached units. Headquarters capability should affect order processing, planning quality, coordination, and response time through explicit command-system calculations. A headquarters is not a magical range-free command multiplier.

Headquarters may be mobile, deployable, shipboard, airborne, or fixed according to configuration. A geographic command post or headquarters site must reference the relevant World Map and facility records where applicable.

### 5.2 Line Units

Line Units are primary fighting formations expected to close with, directly engage, or otherwise conduct the main tactical action against opposing forces. Typical classes include infantry, reconnaissance, armoured, mechanised, airborne, assault engineering, and special operations formations.

These are typical examples, not fixed capability grants. For example, calling a unit "Reconnaissance" does not make it effective at reconnaissance without suitable sensors, communications, trained personnel, doctrine, and command arrangements.

### 5.3 Line Support Units

Line Support Units can engage the enemy directly while supporting the action of other formations. Examples include anti-tank platoons, mortar batteries, weapons companies, and configured military-police units.

Military Police may perform combat support, security, route-control, detention, law-enforcement, or occupation duties where legally and organisationally configured. Their presence may influence local security outcomes only through explicit Political, Population, Intelligence, and Security-system rules. A generic Military Police label must not automatically suppress unrest, crime, or resistance.

### 5.4 Support Units

Support Units primarily provide specialised capabilities rather than direct close combat. Examples include indirect-fire artillery, military intelligence, construction engineering, signals, logistics, and ceremonial or morale-support formations.

Support Units may still carry weapons, suffer casualties, be detected, move, and be attacked. Their operational effects must arise from their actual equipment, staffing, qualifications, location, orders, and current state. A ceremonial band may influence a configured ceremonial or morale context, but must not provide an unexplained universal combat-stat bonus.

### 5.5 Combat Base Units

Combat Base Units provide deployable support capabilities such as medical treatment, maintenance, or supply. Their classification describes their support purpose, not an exemption from movement, command, vulnerability, or personnel requirements.

### 5.6 Installations

Installations are fixed entities rather than mobile combat formations. They are assigned a geographic site, constructed or established, staffed, activated, maintained, and potentially damaged, degraded, disabled, repaired, or decommissioned.

An Installation can be unique in its physical and technical design. A fixed early-warning radar, signals-intelligence site, and undersea surveillance system may all be Installations but use different specialist capability definitions. The shared Installation concept must not force these systems into identical sensor, crew, or operating models.

## 6. Nation Branch Activation and Military Administration

### 6.1 Branch Activation

A nation’s starting scenario configuration defines which canonical branches exist and their initial organisational state. During play, creating or activating a branch requires an authorised action and validation of relevant legal, financial, personnel, training, and institutional prerequisites.

Under the default branch-creation rule, activating a branch establishes or resolves:
- A branch identity for that nation.
- Its associated Military Academy or explicitly configured academy arrangement.
- A Branch Chief appointment in the military administrative command structure.
- Initial institutional configuration, including available training and graduation rules.

These are related but separate entities. Creating a branch must not create an arbitrary number of officers, units, facilities, or equipment without explicit initialisation rules.

### 6.2 Administrative Command Structure

The administrative structure represents institutional authority, service leadership, personnel administration, readiness policy, doctrine, and military advice. It is distinct from the operational command structure that controls units in the field.

A nation may have configured positions such as:
- Commander-in-Chief, where applicable to its constitutional and military arrangement.
- Defence Minister or equivalent civilian political authority.
- Chief of Defence or equivalent joint-service leadership.
- Branch Chiefs, such as Chief of Army or Chief of Marines.
- Other configured senior appointments, staff offices, and specialist commands.

The existence, title, precedence, appointment authority, and relationship between these roles are nation- and scenario-configurable. The simulation must not assume that every country has an identical hierarchy or that the Commander-in-Chief is always the same constitutional office.

Positions are institutional entities; the characters occupying them are separate. Appointment and vacancy rules must be explicit. A person may not simultaneously occupy incompatible appointments unless the scenario expressly permits it.

### 6.3 Political and Military Authority

Government and Political Systems own legal authority, civilian oversight, defence budgets, mobilisation law, and political policy. Military Systems own military command positions, professional advice, force organisation, and the execution of authorised military actions.

A senior officer may advise, resist, delay, reinterpret, or unlawfully act only through explicit rules for the relevant character, command, political, and operational context. A character's personality does not silently override formal authority. Coups, purges, unlawful orders, mutiny, arrests, war-crimes investigations, and political transitions require explicit state transitions and interfaces with the relevant systems.

### 6.4 Branch Chief and Institutional Influence

Branch Chiefs and senior military leaders are Officer Characters assigned to institutional appointments. Their traits, experience, relationships, professional competence, doctrine, and access to resources may influence branch-specific advice, training standards, force development, personnel appointments, procurement recommendations, readiness, and institutional culture.

These effects must be expressed through defined decisions or modifiers with traceable causes. A Branch Chief does not automatically improve every unit in their branch by a fixed universal percentage.

## 7. Military Academies and Officer Careers

### 7.1 Academy Output

Academies produce Officer Characters through configured intake, training, graduation, and assignment processes. Output depends on available places, instructor and facility capability, funding, training standards, candidate availability, institutional quality, and other explicitly modelled inputs.

The player may configure academy graduation output within the supported policy limits. Graduation rank, qualifications, branch eligibility, and available specialist pathways are defined by the applicable academy and course configuration. The system must not assume all graduates emerge at the same rank or with identical qualifications.

### 7.2 Quality and Funding

Government owns the defence budget and authorised expenditure. Military Systems consumes the resulting funding and resource availability to determine what training, academy capacity, instructor quality, equipment access, and institutional support can be sustained.

Budget is one input to officer development, not a direct deterministic guarantee of officer quality. Training design, staff, institutional continuity, candidate attributes, course availability, leadership, and other configured factors may also matter. Quality calculations must be transparent enough for diagnosis and player understanding.

### 7.3 Character Continuity and Career Transitions

Officer Characters persist after graduating or leaving a command appointment. Their service history, rank history, qualifications, awards, relationships, and prior assignments remain available where supported.

Supported transitions may include promotion, reassignment, advanced education, dismissal, retirement, transfer to a political or civilian career, arrest, imprisonment, death, or other configured outcomes. A character leaving active service does not automatically erase their influence, relationships, or public role.

### 7.4 Decorations and Awards

Awards and decorations are persistent personnel-history events. They may be awarded manually by the player or through authorised rules. Eligibility, authority, criteria, timing, and consequences must be explicit. Awards may affect reputation, morale, career progression, or unit identity only where a defined rule supports that outcome.

## 8. Unit Identity, Composition, and Lifecycle

### 8.1 Stable Identity

Each Military Unit has a stable identity independent of its display name, current commander, parent HQ, location, role, and equipment. Renaming or reorganising a unit must not erase its history or cause references to point to a different unit.

A unit's branch, class, composition, role, and command relationships are separately represented. A unit may change role or composition over time without necessarily becoming a new unit. Whether a major reorganisation preserves identity or creates a successor unit must follow a declared rule and preserve lineage.

### 8.2 Unit Composition

A unit's composition may include:
- Subordinate units.
- Personnel and authorised personnel slots.
- Officer and other command appointments.
- Equipment and platforms.
- Assigned equipment sets or loadout configurations.
- Qualifications and specialist roles.
- Completed courses and current training programmes.
- Support, communications, sensor, and logistics capabilities.
- Garrison, HQ, installation, or other geographic assignments where applicable.

The composition model must not duplicate authoritative equipment definitions, inventory quantities, personnel identities, or facility records. It references those entities and records the unit's authorised assignments and configuration.

### 8.3 Lifecycle

The system must support explicit lifecycle states appropriate to the entity, such as proposed, forming, training, ready, deployed, reconstituting, restricted, inactive, disbanded, or retired. The final state catalogue is reference data and may vary by entity type.

Lifecycle state is not identical to operational readiness, deployment status, or physical condition. A unit can be active but not ready, deployed but temporarily ineffective, or in garrison while undergoing training.

Transitions require validation and an audit trail. Disbanding a unit must define the disposition of its subordinate units, personnel, equipment, assigned inventory, appointments, and historical record. It must not silently delete those entities.

### 8.4 Unit Creation

A unit may be created from a scenario starting order of battle, a saved Formation Template, an authorised recruitment/formation process, or a player-created configuration.

Creation must validate:
- Nation and branch eligibility.
- Unit class and organisational type.
- Required command appointments and eligible personnel.
- Personnel establishment and qualifications.
- Equipment and material availability and compatibility.
- Technology and course prerequisites.
- Required communications, transport, support, or other capabilities.
- Garrison, HQ, base, or geographic assignment where required.
- Authorised funding and training capacity.
- Valid organic superior and command structure, unless the entity type explicitly permits none.

A template describes a desired build; it does not create free equipment, personnel, inventory, or facilities. Missing requirements must be reported as unmet, waived by an authorised rule, or explicitly configured as optional. They must not be silently fabricated.

## 9. Organic Organisation and Operational Command

### 9.1 Organic Structure

The Organic Structure represents a unit's established parent-child organisation and habitual relationships. It describes how formations are intended to train, administer, prepare, and normally operate together.

Examples include a division's organic brigades and battalions, a regiment's subordinate battalions, or a squadron's flights. Not every unit needs an organic superior, and the allowed structure differs by branch and organisational type.

Organic relationships should normally persist when a unit is temporarily assigned elsewhere. Changes to the permanent structure require an explicit reorganisation or establishment transition.

### 9.2 Operational or Player Command Structure

The Operational Command Structure represents current command and attachment relationships for an operation or period. Units may be attached to a different HQ, detached from their organic superior, grouped into a temporary task force, or returned to their normal organisation.

The player and authorised commander AI may create or alter these relationships within their authority. An attachment must not silently rewrite the organic structure, unit identity, or administrative ownership.

### 9.3 Attachment and Detachment

Attachment assigns a unit to an operational superior for a defined purpose or period. Detachment removes or suspends its current operational relationship with its prior superior. A unit can be organically subordinate to one HQ and operationally attached to another.

Attachment and detachment require validation of:
- Authority to change the relationship.
- Compatibility with existing command assignments.
- Effective time and any end condition.
- Operational control and support responsibilities.
- Communications, command capacity, and staff implications.
- Any restrictions imposed by doctrine, scenario, or national organisation.

A unit must not have incompatible simultaneous operational superiors unless an explicitly modelled command arrangement permits it. If a unit is directly assigned to the player, this is represented as an explicit command relationship or authority state, not inferred from a display colour.

### 9.4 Temporary Groupings and HQ-less Groups

Players may create temporary groups from selected units. A group may include a headquarters or, where permitted, operate without a dedicated HQ.

If a group has no HQ, the system may designate an eligible senior or otherwise suitable unit commander as the temporary group leader. This does not transform the unit into a fully staffed HQ. The absence of HQ staff and planning capability should have explicit consequences for order processing, coordination, and response time.

The rules for selecting the temporary leader, resolving equal eligibility, and returning units to prior command relationships must be deterministic and visible to the player.

### 9.5 Command Load and Familiarity

Command effectiveness depends on more than rank or unit count. Relevant factors may include HQ staff capacity, staff training, commander competence, communications, subordinate count and complexity, span of control, familiarity from training together, organic versus attached relationships, information quality, and current workload.

Organic subordinates that train together may impose lower coordination and planning costs than unfamiliar attachments, all else being equal. This is a modelling principle, not an unconditional bonus. Specialist requirements, staff procedures, communication limits, and actual operational context may outweigh familiarity.

The command system must define how workload affects planning and order execution before numeric balance values are fixed. No unit should be assigned an unexplained universal command multiplier.

### 9.6 Order Delegation and Planning

An order to a HQ may be interpreted as an instruction to plan and coordinate execution by its current operational subordinates. The HQ may develop subordinate tasks and routes within the authority and constraints of the order.

A commander may exercise bounded discretion to select routes, sequencing, formations, deployment, rest, reorganisation, support requests, and other execution details where the order permits. The commander's authority, competence, doctrine, available information, communications, and mission constraints determine the allowed discretion.

A subordinate's order is not automatically inherited from its organic superior if it is currently detached or attached elsewhere. Conflicts between orders must be resolved through explicit authority, precedence, timing, and command-state rules.

### 9.7 Order Delay and Communications

Orders may incur delays from command processing, staff capacity, communications, headquarters movement, operational complexity, subordinate familiarity, and other configured factors. Orders may be delayed, misunderstood, rejected as impossible, or executed with deviation where explicit rules support those outcomes.

Command delay must be distinguished from physical movement time, sensor/report delay, and player interface delay. Movement owns route traversal and movement execution; command systems calculate the organisational and decision components and provide the applicable order or task to Movement and other receiving systems.

## 10. Unit State and Performance Inputs

The system separates dynamic operational state from persistent organisational characteristics. These categories are not claims that every variable is permanently fixed or updated at the same frequency.

### 10.1 Dynamic State

#### 10.1.1 Morale

Morale represents the unit's current willingness and psychological capacity to continue its assigned task under pressure. It can be affected by casualties, losses, isolation, perceived threat, leadership, supply, fatigue, cohesion, experience, training, recent successes or failures, and other explicitly modelled conditions.

Low morale may increase the likelihood of hesitation, withdrawal, surrender, refusal, or rout when the relevant tactical circumstances arise. Morale does not by itself decide combat outcomes, and a single threshold must not automatically force every unit to rout regardless of situation.

Morale recovery may occur through rest, withdrawal from immediate pressure, reinforcement, successful action, leadership, and other defined effects. The relative rates and exact triggers remain combat and reference-data decisions.

#### 10.1.2 Cohesion

Cohesion represents the unit's current internal organisation and ability to act as a coordinated formation. Low cohesion can reduce the effectiveness of fire and other coordinated actions, increase vulnerability, slow or disrupt order execution, and create a need to pause, reorganise, or re-form.

Cohesion may be degraded by casualties, incoming fire, fatigue, fear, rapid movement, separation, disrupted communications, unfamiliar attachments, and loss of key personnel. Training, experience, leadership, rest, and explicit reorganisation actions may support recovery.

Reorganisation is a meaningful order or commander-initiated action, not merely passive recovery at a faster arbitrary rate. It may consume time, limit other actions, change exposure, and interrupt or supersede lower-priority orders where the command rules permit. Player orders, higher command intent, and emergency conditions must be reconciled through explicit precedence rules.

#### 10.1.3 Fatigue

Fatigue represents accumulated physical and operational exhaustion. Higher fatigue can reduce movement performance, rate and accuracy of fire, recovery, attention, morale retention, cohesion retention, and other capabilities through the relevant systems.

Fatigue accumulation depends on activity, duration, movement mode, terrain, pace, environmental conditions, fitness, carried load, and other configured factors. Foot movement may impose different fatigue costs from motorised movement; rough terrain, forced marches, and night operations may increase costs when supported by the relevant model.

Fatigue may decline while stationary or resting, with a dedicated Rest order allowing the recovery rules to prioritise recovery over other activity. Rest is not instantaneous and must be compatible with threat, mission requirements, and supply conditions.

#### 10.1.4 Suppression

Suppression represents a temporary reduction in a unit's ability to act effectively while under threatening or incoming fire. It may impair observation, aimed fire, movement, command execution, and the ability to coordinate indirect-fire or air-support requests.

Suppression is distinct from casualties, damage, morale, and cohesion, although it may contribute to changes in those variables. It should normally decline when the suppressive cause ends, subject to the actual fire situation and configured recovery behaviour.

Combat and Weapons own the calculation of suppression caused by weapon effects. This system exposes and maintains the appropriate unit-level state or interface without duplicating the authoritative combat resolution.

### 10.2 Persistent and Slowly Changing Characteristics

#### 10.2.1 Experience

Experience represents accumulated operational and combat learning at unit level. It is distinct from individual personnel experience and from formal training.

Experience may improve a unit's ability to retain and recover morale and cohesion, employ its equipment, execute orders, dig in, coordinate with familiar formations, and adapt to battlefield conditions. The specific effects must be calculated through explicit rules rather than a single global multiplier.

Experience can develop internal esprit de corps and unit identity over time. Unit culture may be influenced by shared service history, institutional norms, leadership, and personnel composition. The simulation must not assume that a commander's ideology automatically becomes the unit's ideology. Any transfer of outlook or traits must be a gradual, explicit, bounded rule with defined evidence, exposure, and time basis.

#### 10.2.2 Training

Training represents learned collective proficiency and preparedness. It complements experience but is not identical to it: a new unit may be well trained but inexperienced, while a veteran unit may be experienced but have outdated or insufficient training for a new role.

Training affects execution only through defined capabilities and rules, including relevant weapon use, formation manoeuvre, coordination, order execution, and assigned specialities. Training levels must not grant abilities unsupported by equipment, technology, qualified personnel, or physical capability.

Training can decay, be refreshed, or change when equipment, doctrine, personnel composition, or mission changes make prior training less applicable. Decay and retraining rules require explicit configuration.

#### 10.2.3 Courses and Qualifications

Completed courses are persistent records of instruction or qualification. They are not interchangeable with an undifferentiated Training score.

A course may confer a qualification, unlock a supported task, improve proficiency under defined conditions, or satisfy an organisational requirement. Course effects must state the applicable personnel or unit scope, prerequisites, validity, recertification needs, and resulting capability.

Examples include urban operations, night marksmanship, parachute assault, specialist reconnaissance, forward air control, signals interpretation, aircraft detection, and advanced leadership. These are examples for future course reference data, not a committed canonical catalogue.

A capability requiring a qualified specialist must validate that the relevant personnel are actually assigned and available. Completing a unit course must not silently create a qualified person or equipment item.

#### 10.2.4 Fitness

Fitness represents the physical conditioning of the unit's personnel as an aggregate or appropriately derived unit-level measure. It is distinct from current fatigue.

Fitness may affect sustainable movement, fatigue accumulation and recovery, physical tasks, and resistance to suppression or operational stress where supported by explicit rules. It depends on personnel composition, physical training, health, nutrition, rest, and other configured inputs.

The unit-level Fitness measure must have a declared aggregation and denominator. It must not obscure severe deficiencies in a critical subpopulation or specialist role where those deficiencies matter to the task.

#### 10.2.5 Aggression and Initiative

Aggression represents a unit's tendency to act assertively within the scope of its orders and doctrine. It may influence the likelihood of taking initiative, pressing an attack, exploiting an opportunity, or accepting risk.

Aggression is not a universal bonus to morale or combat effectiveness. High aggression may produce valuable initiative or unwanted escalation, overextension, excessive risk, or unlawful conduct depending on command, doctrine, traits, discipline, and the situation. The consequences must be context-dependent and explainable.

Player authority and order constraints remain meaningful. A unit must not automatically disregard a valid explicit order merely because its aggression value is high. Any deviation must follow defined command discretion, communication, doctrine, and discipline rules.

#### 10.2.6 Traits and Unit Identity

Units may develop or possess traits reflecting institutional culture, doctrine, esprit de corps, operational history, discipline, and collective tendencies. Traits must have explicit definitions, scope, valid ranges or categories, provenance, and effects.

Unit traits are distinct from commander traits and individual character traits. Commander influence may contribute to unit culture through defined and gradual mechanisms, but does not overwrite the unit's own history or automatically replicate every personal trait.

Trait effects must be context-bound and traceable. Traits that concern prejudice, political extremism, indiscipline, or hostility may influence defined social, political, occupation, or civilian-treatment outcomes. Such effects must be modelled as simulation consequences, not presented as endorsements or unconditional tactical advantages.

### 10.3 No Universal Combat Score

The system may expose derived summaries for player comprehension, comparison, and AI planning, but such summaries must not replace the underlying state and capability calculations.

Any summary must identify its scope and relevant assumptions. A unit can be strong in one task and weak in another. A unit-level readiness or capability report should distinguish at least relevant personnel, equipment, training, command, logistics, and current-condition deficiencies where those inputs are available.

Combat effectiveness is calculated by Combat and Weapons and other relevant specialist systems using the unit's actual state, equipment, personnel, training, command, orders, and battlefield context. This specification does not define a single universal formula.

## 11. Military Courses and Training Frameworks

### 11.1 Course Definition and Versioning

Courses are defined as versioned reference data or authorised research outputs. Each course definition specifies its purpose, prerequisites, eligible branch or unit types, required personnel or equipment, duration, resource costs, instructors and facility needs where modelled, completion criteria, qualification outputs, and effects.

A course version used by a unit or personnel training event must remain identifiable after the course is revised. Historical completions must not silently inherit later course effects.

### 11.2 Training Frameworks

Players may create reusable Training Frameworks composed of ordered phases, parallel modules, prerequisites, and completion criteria. A framework may target a unit class, branch, role, specialist task, or command appointment.

For example, an infantry framework might combine physical conditioning, marksmanship, night operations, urban operations, and a selectable specialist phase. A special operations framework might prioritise unconventional warfare, infiltration, reconnaissance, survival, language, or other configured modules. These examples are illustrative only.

The framework must validate course compatibility and ordering. It must identify the expected time, cost, resource demands, qualification outcomes, and capabilities that remain unavailable until completion.

### 11.3 Unit and Individual Training

Training can target a whole unit, a subunit, a selected personnel group, or an individual where the relevant system supports that granularity.

A unit-level course must not assume every assigned person completes every module if the course is intended for selected roles. Conversely, a specialist qualification must not be treated as present merely because one person in a unit has completed a course, unless the capability rule only requires one available qualified person.

### 11.4 Leadership Courses and Specialist Roles

Officer Characters may undergo leadership, staff, command, and specialist courses. These affect the qualifications and competencies recorded by Military Systems and may inform assignment eligibility and command planning.

Specialist roles such as forward air control or signals interpretation require an explicit role/qualification contract with the receiving operational system. A unit may request air support, interpret a sensor feed, or perform another specialist task only when the required personnel, equipment, communications, doctrine, and current conditions are valid.

### 11.5 Training Sustainment and Refresh

Commanders may be responsible for maintaining training standards while units are in garrison or otherwise not engaged in operations. This requires explicit scheduled or event-driven training activities, resource use, available instructors, course validity, and unit availability.

A commander may improve training only through valid training activity and authorised resources. Trait-based effects may influence the quality or frequency of training, but must not create proficiency without a causal mechanism.

## 12. Formation Templates and Player Builders

### 12.1 Formation Template

A Formation Template is a reusable blueprint for creating or reorganising a unit or hierarchy of units. It can define:
- Proposed command and subordinate structure.
- Unit classes, roles, and establishment targets.
- Personnel and specialist-role requirements.
- Equipment sets and platform/loadout requirements.
- Required courses, training framework, and qualifications.
- Command appointments and preferred leadership qualifications.
- Garrison, HQ, base, or installation assignment requirements.
- Support attachments and logistics expectations.
- Optional and mandatory components.
- Validity, version, and scenario/technology availability.

Templates are not live units and do not reserve resources unless an explicit planning or procurement operation does so.

### 12.2 Division and Formation Builder

The player should be able to assemble a division or other formation at an appropriate level of detail without being forced to manually manage every item before the basic structure can be evaluated. The builder should support hierarchy, subordinate formations, unit classes, equipment sets, courses, commanders, HQ assignments, garrison, and administrative/operational command placement.

The interface may offer recommended structures and warnings from eligible Officer Characters or command staff. Recommendations must be tied to their expertise, available information, doctrine, assumptions, and relevant character or institutional traits. The player must be able to inspect why a recommendation was made and which constraints it assumes.

The builder should support custom formations while retaining validation, warnings, and capability analysis. It must not hard-code one historical or national divisional organisation as the only valid option.

### 12.3 Validation Levels

The builder should distinguish:
- **Structurally valid:** identifiers and parent-child relationships are valid.
- **Organisationally valid:** branch, class, rank, role, and command relationships are permitted.
- **Technically feasible:** required equipment, qualifications, technology, and capabilities are compatible.
- **Resourced:** required personnel, equipment, materials, facilities, and funding are available or explicitly planned.
- **Training complete:** required courses and training standards are satisfied.
- **Ready for deployment:** the unit meets its configured deployment criteria.
- **Mission suitable:** the unit is plausibly suited to a particular mission and context.

Passing one level does not imply passing the next. A formation can be structurally valid while understrength, untrained, poorly equipped, or unsuitable for its intended mission.

### 12.4 Recommendations and Warnings

Warnings should identify the deficiency, affected capability, likely operational consequence, severity, source of the rule, and available remediation where known. Recommendations should not silently alter the player's design.

Examples include missing anti-armour capability, insufficient qualified crew, incompatible equipment, lack of night training, weak HQ staffing, inadequate communications, missing logistics support, or a course not available under current technology.

## 13. Readiness, Garrison, and Deployment

### 13.1 Readiness

Readiness is a derived or explicitly maintained assessment of whether a unit can perform its designated tasks at the expected standard and within the expected timeframe. It is distinct from unit lifecycle, current deployment status, and any single dynamic statistic.

Readiness evaluation may consider personnel fill, equipment availability and serviceability, ammunition and supply state, training, qualifications, command appointments, cohesion, fatigue, morale, maintenance, communications, and other mission-relevant factors.

Readiness reports must expose the main causes of restriction. A unit should not be labelled fully ready if a required capability is absent merely because its other statistics are high.

### 13.2 Garrison and Headquarters Assignment

Units may be assigned to a garrison, base, headquarters, installation, or other valid geographic location according to their type. A unit's garrison assignment is distinct from its current physical position when deployed.

Headquarters and fixed installations may require a specific facility or site. Mobile units may use a base, barracks, depot, port, airfield, or other configured support location. These references must use canonical World Map and Installations and Facilities identities.

### 13.3 Deployment and Reconstitution

Deployment, redeployment, withdrawal, reconstitution, and return to garrison require explicit transitions and operational tasks. They must account for the actual time, movement, training, equipment, supply, and personnel constraints owned by the relevant systems.

A unit cannot instantly become deployed at a new location because its status changed. Movement owns physical movement and authoritative continuous position. Military Systems requests and tracks the organisational intent and receives movement outcomes through an explicit interface.

## 14. Installation Staffing and Operation

### 14.1 Construction and Site Assignment

An Installation is established at a selected valid geographic location through the relevant construction or establishment process. Construction progress and facility state are owned by Installations and Facilities and applicable construction systems.

The establishment request may specify intended staffing, command appointment, qualifications, courses, equipment, and operating requirements. The request must not treat staff as part of the Installation's immutable physical structure.

### 14.2 Staffed Capability

An Installation's effective operational capability may depend on:
- Its configured physical and technical capability.
- Installed and serviceable equipment.
- Assigned personnel and staffing levels.
- Personnel qualifications, course completions, and experience.
- Command and supervisory appointments.
- Communications and power or other required services.
- Readiness, maintenance, and current operational condition.
- Technology and doctrine prerequisites.

The specialist domain calculates the installation-specific result. For example, a radar installation's sensor performance belongs to Detection and Sensors; its fixed-site identity and construction state belong to Installations and Facilities; its staffing and military appointments belong to Military Systems.

### 14.3 Staffing Changes

Staff may be assigned, replaced, rotated, trained, or withdrawn through explicit personnel transitions. A staffing change may reduce or restore capability depending on required roles and qualification coverage.

The installation must not retain a capability that depends on a specialist role when no qualified and available staff satisfy that role, unless a defined automation or remote-operation capability explicitly permits it.

### 14.4 Activation, Restriction, and Decommissioning

Completion of construction does not automatically prove that an Installation is operational. Activation validates required equipment, personnel, qualifications, support, and specialist-domain conditions.

An Installation may be active, restricted, degraded, disabled, under repair, or decommissioned according to its lifecycle and the relevant owning systems. These states must not be conflated with its staffing status. A decommissioned Installation retains historical identity and event history.

## 15. Character Advice, Delegation, and Institutional Behaviour

### 15.1 Advice and Recommendations

Officer Characters and senior military leaders may produce intelligence-informed assessments and recommendations on force organisation, training, appointments, equipment gaps, doctrine, readiness, operational planning, and allocation of units.

Recommendations should reflect the character's role, competence, experience, doctrine, personality, relationships, information access, and uncertainty. They must identify assumptions and should not imply omniscient access to hidden enemy or world state.

Intelligence quality and confidence are owned by Intelligence and Detection systems. Military advisers consume the information actually available to them, including its age, source, confidence, and uncertainty.

### 15.2 Delegated Command Decisions

A commander AI may make decisions within its assigned authority and the discretion permitted by the player's orders. Examples include local route planning, subordinate tasking, reorganisation, rest, routine support requests, and execution adjustments.

The decision process must respect explicit order constraints, command precedence, rules of engagement, available information, capability limits, and political or legal restrictions. High initiative is not unlimited authority.

### 15.3 Character Relationships and Institutional Conflict

Characters may develop rivalries, alliances, loyalty, resentment, ambition, and competing institutional outlooks through the Character and Political systems. These relationships may influence advice, appointments, cooperation, discipline, and institutional stability through explicit rules.

Purges, arrests, coups, civil conflict, unlawful orders, and command disobedience must be modelled as distinct political or military events with prerequisites, consequences, and audit history. They must not be triggered solely by a single personality trait.

### 15.4 Human and Civilian Consequences

Military conduct can affect civilians, occupation conditions, political stability, and legitimacy. Such outcomes require explicit interactions with Population, Government, Political, Intelligence, Security, and Combat systems.

Unit and commander traits may affect conduct where a defined behavioural rule supports it. The simulation must distinguish lawful orders, unlawful conduct, authorised policy, individual action, institutional tolerance, reporting, and accountability where those distinctions are modelled.

## 16. Cross-System Interfaces and Ownership

| Domain | Authoritative responsibility | Military Branches and Unit Organisation interface |
|---|---|---|
| Character System | Character identity, general attributes, biography, relationships, non-military careers | References Officer Characters; publishes relevant character attributes and events |
| Military Personnel domain | Service records, military personnel status, ranks, qualifications, appointments, casualty and personnel accounting, subject to final architecture | Requests and references personnel assignments; does not create duplicate personnel records |
| Government | Defence budget, lawful expenditure, mobilisation policy, civilian authority | Consumes authorised funding and policy; publishes military resource and force requirements |
| Political Systems | Political organisations, civil-military political outcomes, public support and political authority | Supplies institutional events and leadership transitions through explicit contracts |
| Research | Technology, discoveries, availability dates, research outcomes | Checks technology prerequisites for equipment, training, roles, and capabilities |
| Equipment and Material | Equipment definitions, platform characteristics, compatibility, performance parameters | References equipment sets and assigned platforms; publishes capability requirements |
| Good and Service Catalogue | Canonical goods, services, and units of measure | Uses canonical identities for equipment, materials, supplies, and services where applicable |
| Inventory | Physical stock, lots, reservations, transfers, consumption and custody | Requests equipment/material issue and transfer; does not maintain a competing stock ledger |
| Logistics | Supply planning, transport, movement of cargo, routing and delivery | Provides resupply and support tasks; receives unit demand, priority, destination and timing |
| Installations and Facilities | Facility identity, physical condition, lifecycle, construction and generic capacity | References garrisons, bases, HQ sites and Installation facilities without duplicating facility state |
| World Map | Geographic identity, geometry, spatial queries and shared spatial model | References canonical geographic entities and locations |
| Movement | Authoritative continuous unit position, routes and movement execution | Issues movement intent and receives progress, arrival, interruption and loss-related events |
| Weather and Environment | Weather and environmental state | Supplies conditions that affect movement, fatigue, equipment and operation through relevant systems |
| Detection and Sensors | Sensor capabilities, detection processing and confidence | Supplies unit and Installation sensor requirements and consumes resulting observations |
| Intelligence | Intelligence records, source quality, confidence, dissemination and knowledge state | Supplies information actually available to commanders and advisers |
| Electronic Warfare | EW effects, emissions, jamming and countermeasures | References unit platforms, operators, training and mission configuration |
| Combat and Weapons | Weapon effects, hit/damage resolution, casualties, fire-induced suppression and tactical combat | Supplies unit composition, state and capability inputs; receives combat events and resulting condition changes |
| Population | Civilian demographics, settlement population and aggregate social state | Receives/produces explicit interfaces for civilian harm, occupation and security outcomes; no duplicate civilian or military population ledger |
| Economy | Company/industry/economic state, costs and financial accounting | Consumes authorised cost data; does not maintain duplicate financial accounts |
| Simulation Scheduler | Deterministic event timing, update ordering and replay conventions | Registers transitions and update requirements without inventing independent time rules |

The table is a behavioural boundary map, not the final Military Systems architecture or schema. More detailed ownership and dependency decisions will be formalised in the architecture/domain-map stage.

## 17. Determinism, History, and Save/Load

All persistent units, branches, academies, command positions, appointments, templates, courses, and installations require stable identities.

Authoritative changes must use explicit events or equivalent auditable transitions, including:
- Branch activation or disbandment.
- Academy establishment and configuration change.
- Officer graduation, promotion, appointment, removal, retirement, or death.
- Unit creation, renaming, reorganisation, attachment, detachment, transfer, and disbandment.
- Course assignment and completion.
- Readiness or lifecycle transitions.
- Installation staffing, activation, restriction, and decommissioning.

Each event must identify its source, effective simulation time, relevant configuration version, and idempotency scope. Retrying an event must not duplicate a unit, officer, assignment, course completion, or state change.

Save/load must preserve stable identity, active command relationships, unit state, course history, staffing, template/configuration versions, and any state needed for deterministic continuation. Derived reports may be rebuilt if their authoritative inputs and calculation versions are retained.

## 18. Open Decisions

The following require later resolution and must not be silently hard-coded during implementation.

1. Canonical global branch catalogue and rules for national sub-branches.
2. Exact administrative position catalogue and constitutional variations.
3. Military Personnel ownership boundary and rank/appointment model.
4. Canonical unit-class taxonomy and branch-specific organisational types.
5. Unit identity continuity rules for major reorganisations and successor formations.
6. Detailed model for personnel slots, actual personnel, specialists, casualties, and vacancies.
7. Formal definitions, scales, update rates, and dependencies for morale, cohesion, fatigue, suppression, experience, training, fitness, aggression, and unit traits.
8. Which unit-level values are stored state, event-derived state, or cached derived summaries.
9. Rules for gradual development of unit identity, esprit de corps, political outlook, and commander influence.
10. Command capacity, staff models, span of control, order delay, communications, command discretion, and organic-familiarity calculations.
11. Order precedence when player intent, higher HQ orders, local commander initiative, and emergency reorganisation conflict.
12. Academy capacity, funding, candidate generation, graduation output, graduation rank, and national exceptions.
13. Course catalogue, research dependencies, training cost, duration, instructor requirements, validity, and qualification scope.
14. Training decay, refresher training, and requalification.
15. Formation Template format, versioning, sharing, and scenario availability.
16. Readiness categories and criteria for different missions and unit types.
17. Installation lifecycle and the exact interface to Installations and Facilities.
18. Whether all Combat Base Unit capabilities are mobile formations, fixed facilities, or may be configured as either.
19. Detailed command AI and recommendation-generation approach.
20. Rules and interfaces for occupation security, civilian harm, misconduct, accountability, and political instability.
21. Update ordering and frequency for unit condition, command planning, training, and readiness.
22. Historical reference-data standards for 1955 and later technological eras.
23. Numerical calibration, UI summaries, and player-facing presentation of complex capability state.

## 19. Acceptance Criteria

This specification is ready to inform the next architecture and schema stages when:

- [ ] Branches use a canonical global catalogue with explicit national activation and configuration.
- [ ] Branch activation, academy creation, and Branch Chief appointment follow explicit rules.
- [ ] Officer Characters remain persistent Character System entities with explicit military appointments and service history.
- [ ] Military administrative authority is distinct from operational command.
- [ ] Unit identity is stable and separate from name, location, commander, role, and parent HQ.
- [ ] Unit classes guide organisation without granting unsupported capabilities.
- [ ] Headquarters, Line Units, Line Support Units, Support Units, Combat Base Units, and Installations have distinct behavioural expectations.
- [ ] Organic and operational command relationships are separately represented.
- [ ] Attachment, detachment, temporary grouping, and restoration of relationships are explicit and auditable.
- [ ] Headquarters staff and command load influence planning through defined rules rather than unexplained universal bonuses.
- [ ] Orders can be planned and delegated with bounded commander discretion and explicit precedence.
- [ ] Dynamic condition is distinct from persistent characteristics and readiness.
- [ ] Morale, cohesion, fatigue, suppression, experience, training, courses, fitness, aggression, and traits have separate meanings and owners.
- [ ] No universal unit combat score replaces underlying capability calculations.
- [ ] Course frameworks support ordered phases, prerequisites, qualifications, training costs, and versioned outcomes.
- [ ] Formation Templates do not create unearned personnel, equipment, inventory, facilities, or funding.
- [ ] Formation builders distinguish structural validity, technical feasibility, resourcing, training, readiness, and mission suitability.
- [ ] Installation staffing and specialist capability are distinct from physical facility identity and condition.
- [ ] Readiness and deployment eligibility are explicit and do not equate to a single statistic or lifecycle state.
- [ ] Military Systems do not duplicate authoritative Character, Personnel, Equipment, Inventory, Facility, World Map, Movement, Intelligence, or Combat records.
- [ ] Cross-system transitions are deterministic, idempotent, and auditable.
- [ ] Scenario reference data can represent historical and national variation without arbitrary global branch or unit definitions.
- [ ] Open decisions remain explicit until resolved through the agreed architecture and schema workflow.

## 20. Related Specifications

- [Installations and Facilities System](../infrastructure/installations-and-facilities-system.md)
- [World Map System](../world/world-map-system.md)
- [Terrain Objects](../world/terrain-objects.md)
- [National Territory and Control System](../world/national-territory-and-control-system.md)
- [Population System](../population/population-centres-system.md)
- [World Simulation Dependency Map](../../technical/architecture/world-simulation-dependencies.md)
- Equipment and Material System: to be authored next.
- [Equipment and Material System](equipment-and-material-system.md)
- [Equipment Design, Research & Production](equipment-design-research-and-production.md)
- [Doctrine, Command & Operations](doctrine-command-and-operations.md)
- [Sensors and Detection System](sensors-and-detection.md)
- [Electronic Warfare System](electronic-warfare.md)
- [Combat & Engagement Systems](combat-and-engagement-systems.md)
- [Canonical Specification Register and Military Interface Audit](../../technical/architecture/canonical-specification-register.md)
