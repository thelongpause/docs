# Doctrine, Command & Operations

**Project:** Project Sovereign  
**System:** Military Simulation  
**Document ID:** MIL-OPS-001  
**Status:** System specification, draft 1  
**Historical baseline:** 1 January 1955  
**Technology horizon:** At least 2026  
**Canonical path:** `systems/military/doctrine-command-and-operations.md`

## 1. Purpose and scope

This specification defines how armed forces are directed, prepared, supplied, informed, and employed in pursuit of operational and strategic objectives. It connects the organisational model of forces to the operational-wargaming experience without prematurely fixing the detailed mechanics of combat resolution.

It establishes the concepts and rules for:

- Doctrine, standard operating procedures, and force employment.
- Command relationships, headquarters, command capacity, and command latency.
- Orders, intent, planning, execution, revision, and cancellation.
- Strategic, operational, and tactical levels of military activity.
- Operations, missions, objectives, task organisation, and coordination.
- Rules of engagement, authority, discipline, and constraints on force.
- Intelligence, reconnaissance, surveillance, uncertainty, and the Player's knowledge.
- Logistics, supply, transport, readiness, maintenance, and sustainment.
- Joint and combined operations across branches and national forces.
- Terrain, weather, infrastructure, and other conditions affecting plans.
- Operational assessment, after-action analysis, and strategic consequences.

Detailed weapon employment, detection probability, fire-control solutions, hit probability, damage, destruction, and engagement-resolution algorithms belong to the Combat & Engagement Systems and their supporting specifications. This document defines how forces request, authorise, coordinate, and exploit those capabilities.

The intended experience combines the operational depth and command friction of military simulations with the wider strategic, industrial, and political scope of Project Sovereign. The simulation should be authentic, highly configurable, and immersive without making the Player manually issue every trivial action.

## 2. Core principles

1. **Command is a process, not a teleporting instruction.** Orders require formulation, transmission, receipt, interpretation, and execution. Time, communications, headquarters workload, command arrangements, and circumstances can affect each stage.
2. **Intent and execution are distinct.** The Player may define objectives and constraints while subordinate commanders determine how to act within delegated authority.
3. **Organisation and operational control are distinct.** Organic membership, operational command, temporary attachment, administrative authority, and direct Player orders must remain separate relationships.
4. **Doctrine guides behaviour; it does not guarantee it.** Doctrine shapes training, planning, force employment, and decisions but must interact with equipment, experience, leadership, terrain, logistics, enemy actions, and available information.
5. **Information is not omniscience.** The simulation maintains a distinction between the world's actual state, what a force has observed, what it assesses, what has been reported, and what the Player knows.
6. **Plans are hypotheses about the future.** Plans can be incomplete, delayed, disrupted, contradicted by new information, or rendered invalid by enemy action and changing conditions.
7. **Logistics constrain operations.** Units cannot sustain unlimited movement, firing, flight, sailing, construction, or combat independent of supply, maintenance, transport, and infrastructure.
8. **Readiness is multidimensional.** A unit may be organised but understrength, equipped but unserviceable, trained but fatigued, supplied but poorly coordinated, or tactically capable but unavailable for a specific mission.
9. **Commanders are agents within a system.** Officer Characters have capabilities, limitations, personalities, experience, traits, and relationships. Their decisions should be contextual rather than scripted as universal bonuses.
10. **Joint operations require coordination.** Shared objectives do not automatically create interoperable communications, compatible procedures, common situational awareness, or synchronised timing.
11. **Outcomes emerge from interacting systems.** Avoid a universal combat or operational-strength multiplier that replaces the underlying simulation.
12. **Narrative presentation is not a separate truth.** The Narrative Battle System may present events as a readable account, but it must be derived from the underlying simulation and must not invent outcomes.
13. **Historical grounding and counterfactual freedom coexist.** Doctrine, command systems, logistics, and technologies reflect the 1955 baseline and evolve through research, institutions, experience, and policy.
14. **The Player receives actionable explanations.** Delays, failures, shortages, and deviations should have discoverable causes, even when the exact enemy situation remains uncertain.

## 3. Levels of military activity

The simulation distinguishes three interacting levels. They are analytical layers, not necessarily fixed unit sizes or separate game modes.

### 3.1 Strategic level

The strategic level concerns national or coalition objectives, theatres, resource allocation, mobilisation, force posture, campaign priorities, political constraints, and the relationship between military activity and national strategy.

Strategic decisions may include:

- Defining war aims and political objectives.
- Opening, prioritising, or deprioritising theatres.
- Mobilising reserves and changing force readiness.
- Allocating resources, transport capacity, production, and strategic stockpiles.
- Setting national doctrine, escalation policy, and rules of engagement.
- Establishing alliances, basing rights, and coalition commitments.
- Accepting risk, prioritising objectives, or terminating a campaign.

Strategic objectives should be expressed in terms that can be assessed through world state and operational outcomes, rather than merely as a list of enemy units to destroy.

### 3.2 Operational level

The operational level connects strategic objectives to coordinated campaigns and major operations. It allocates forces, synchronises actions across time and space, sequences objectives, establishes priorities, and manages sustainment across a theatre.

Operational decisions may include:

- Creating and sequencing operations.
- Assigning formations and supporting units.
- Establishing main efforts, supporting efforts, reserves, and branches of a plan.
- Setting phase lines, boundaries, control measures, objectives, and timings.
- Coordinating manoeuvre, fires, air support, reconnaissance, engineering, air defence, and logistics.
- Managing operational reserves, rotations, pauses, and reinforcement.
- Assessing progress and adapting the plan.

The operational level is where the War Planner should help the Player understand feasible courses of action, dependencies, risk, and resource demand.

### 3.3 Tactical level

The tactical level concerns the immediate employment of units and subunits: movement, local positioning, observation, engagement requests, support, withdrawal, reorganisation, and reactions to nearby events.

The Player may issue detailed tactical orders directly or delegate execution to subordinate commanders. The level of automation should be configurable.

Tactical orders must still respect the actual capabilities of the commanded units, command relationships, rules of engagement, communications, and current information.

### 3.4 Inter-level connection

Strategic objectives should be decomposable into operational objectives and tactical tasks. Tactical outcomes feed back into operational assessments, which in turn influence strategic decisions.

This relationship is not always a clean hierarchy. Local events may create strategic consequences, and political decisions may impose immediate restrictions on tactical action. The system must support these feedback paths without collapsing all decisions into one command layer.

## 4. Command relationships and authority

### 4.1 Separate relationship types

The simulation must represent distinct relationships instead of one universal parent-child pointer:

- **Organic organisation:** the unit's permanent administrative and organisational home.
- **Operational command:** authority to direct a unit's mission and employment for a defined purpose.
- **Tactical control:** limited authority to direct specified actions within a mission or area.
- **Attachment or detachment:** a time-bounded reassignment that changes which formation employs a unit without erasing its organic identity.
- **Administrative authority:** responsibility for personnel administration, records, discipline, replacements, and other administrative matters.
- **Logistical support relationship:** responsibility for providing supply, maintenance, medical, transport, or other sustainment services.
- **Support relationship:** a defined obligation to support another unit or formation, such as direct support, general support, reinforcing support, or a scenario-specific equivalent.
- **Direct Player authority:** an explicit order issued by the Player at a level where the Player is permitted to intervene.

These relationships may overlap but are not interchangeable. A unit can be operationally attached to a formation while receiving administration from its parent and supply from a separate support organisation.

### 4.2 Authority boundaries

Every order should be checked against the issuer's authority over the recipient and the action requested. A headquarters may have authority to assign a mission but not to change a unit's permanent organisation, or may direct a supporting unit only within a specified scope.

Authority can be delegated, limited, suspended, or revoked. Changes should have effective times and clear records. An order that conflicts with an existing valid order should trigger a defined resolution process rather than silently overwriting the earlier order.

### 4.3 Organic familiarity

Permanent organisational relationships can support coordination through shared procedures, training, communications, and familiarity. Such benefits are not automatic or unconditional. They should arise from the unit's actual training, experience, personnel continuity, doctrine, and command arrangements.

A temporarily attached unit can learn to operate effectively with a receiving formation, but that familiarity may take time and depend on prior interoperability. Attachment should not instantly erase differences in procedures or staff practice.

### 4.4 Units without headquarters

A group or formation may exist without a dedicated HQ unit. It can still receive orders and act, but its planning, coordination, reporting, communications, and ability to manage multiple subordinate elements may be reduced according to the group's actual staff and command arrangements.

A headquarters is not a magical range-extension object. Its effects depend on staff capacity, communications, command authority, location, mobility, protection, equipment, and the tasks it must coordinate.

## 5. Headquarters and command capacity

### 5.1 HQ function

A headquarters coordinates forces, converts intent into plans and orders, maintains situational understanding, manages reports, allocates tasks, resolves conflicts, and coordinates sustainment and support.

HQ capability depends on more than the existence of an HQ unit. Relevant factors include:

- Authorised and present staff.
- Staff training, experience, and specialisation.
- Commander competence and decision-making.
- Communications equipment, network access, and interoperability.
- Planning tools, maps, intelligence, and current information.
- Command post mobility, survivability, and power.
- Workload, number and complexity of subordinate elements, and operational tempo.
- Continuity of staff and established procedures.
- Fatigue, disruption, casualties, and loss of communications.

### 5.2 Workload and capacity

A headquarters has finite capacity to plan, coordinate, process reports, issue orders, and manage simultaneous tasks. Workload is influenced by subordinate count, geographic spread, task complexity, operational tempo, unfamiliar attachments, coordination demands, and the quality of staff procedures.

Exceeding capacity may increase delays, omissions, stale assessments, coordination failures, or the need to simplify plans. It should not merely apply a flat penalty to every subordinate unit.

### 5.3 Command succession and continuity

Commanders and key staff may become unavailable due to casualties, illness, reassignment, capture, communications loss, or other events. The system should define succession arrangements, acting appointments, and the time needed to restore effective command.

A successor's experience, authority, staff familiarity, and relationship with subordinate commanders affect continuity. The loss of a commander should not automatically make a unit incapable of action; standing orders, subordinate initiative, and delegation may allow continued operations.

### 5.4 Command posts and displacement

HQ locations may be fixed or mobile. Moving a command post can improve survivability or communications access but may temporarily disrupt planning, reporting, and coordination. A headquarters may operate while moving only to the extent its equipment, doctrine, staff procedures, and communications support it.

Command posts can be affected by terrain, weather, infrastructure, enemy action, electronic warfare, and physical disruption.

## 6. Commanders, staff, and decision-making

### 6.1 Officer Characters

Command appointments are held by Officer Characters as defined by the character and military organisation systems. A commander's effects should arise from relevant attributes, experience, personality, traits, training, staff support, relationships, and the circumstances of the decision.

Possible characteristics include:

- Professional competence and relevant command experience.
- Planning, organisation, judgement, and adaptability.
- Initiative, caution, aggressiveness, and tolerance for risk.
- Integrity, discipline, empathy, and concern for personnel.
- Political outlook, loyalty, institutional relationships, and personal ambition.
- Familiarity with the unit, theatre, adversary, doctrine, and mission.
- Ability to communicate intent and delegate authority.

The exact attribute schema should be shared with the character system and should avoid redundant statistics that encode the same behaviour.

### 6.2 Commander decision quality

Commanders assess the situation using information available to them, their own experience, staff support, doctrine, orders, and perceived risk. They may make sound decisions, delay, misinterpret information, or make mistakes.

Decision quality should be contextual. A commander may be excellent at defensive operations but less experienced in amphibious planning; a bold commander may exploit an opportunity but incur avoidable risk under different conditions.

Do not use a single leadership score as a universal multiplier for movement, firepower, morale, logistics, and every other unit attribute.

### 6.3 Staff processes

Staffs transform information and intent into feasible plans, coordinate subordinate actions, maintain records, and assess progress. Staff specialisation can matter where modelled, including operations, intelligence, logistics, communications, engineering, air operations, and personnel.

Staff work should be limited by available information, time, capacity, training, tools, and coordination. A sophisticated plan may take longer to develop and may be more vulnerable to disruption or information becoming stale.

### 6.4 Delegation and initiative

A commander may be given a mission and freedom to determine how to achieve it, or may receive detailed control measures and specific tasks. The degree of initiative permitted depends on doctrine, orders, commander characteristics, communications, training, and political constraints.

Subordinate initiative should not mean disobedience without cause. It means acting within delegated intent and authority when circumstances change or communication is unavailable. The system should distinguish prudent adaptation, reasonable interpretation, failure to understand, and deliberate refusal.

### 6.5 Unit culture and command climate

Unit culture develops through repeated leadership, training, personnel continuity, shared experience, discipline, institutional norms, and operational history. It should evolve over time rather than change instantly when a commander is appointed or a policy is announced.

Culture may influence cohesion, trust, initiative, reporting candour, discipline, risk-taking, and treatment of civilians or prisoners, but these effects must interact with actual incentives, oversight, orders, and enforcement. No personality or national culture should deterministically cause unlawful behaviour.

## 7. Doctrine and standard operating procedures

### 7.1 Definition

Doctrine expresses principles and established methods for organising and employing forces. Standard operating procedures (SOPs) define repeatable actions and processes under known conditions. Both guide behaviour; neither guarantees identical outcomes.

Doctrine may exist at national, service, theatre, formation, unit, or mission level. It can be formal, locally adapted, experimental, or improvised.

### 7.2 Doctrine dimensions

Relevant doctrine domains may include:

- Command philosophy and delegation.
- Manoeuvre and force concentration.
- Defensive posture and counterattack.
- Fire support and coordination.
- Reconnaissance, screening, and security.
- Air superiority, interception, strike, and close air support.
- Naval surface, undersea, amphibious, and convoy operations.
- Air defence and protection of critical assets.
- Electronic warfare and emission control.
- Logistics, maintenance, medical evacuation, and recovery.
- Joint operations and interoperability.
- Strategic deterrence and escalation management.
- Civilian protection, detention, discipline, and rules of engagement.

These domains should be extensible and applicable where relevant, rather than hard-coded into a single land-combat framework.

### 7.3 Doctrine is not a universal bonus

Doctrine affects decisions and processes: formation, spacing, task allocation, fire coordination, reporting, movement, reserves, emission policy, use of support, and risk acceptance. Its results depend on training, equipment, staff capability, experience, enemy behaviour, terrain, weather, and the actual mission.

A doctrine can be well suited to one situation and poorly suited to another. An outdated or mismatched doctrine may cause unnecessary losses, coordination friction, inefficient use of equipment, or poor adaptation.

### 7.4 Adoption, training, and institutional change

A doctrine change may require policy decisions, revised manuals, new courses, staff procedures, exercises, equipment compatibility, and leadership support. Adoption takes time and can be uneven across the armed forces.

Existing units should not instantly gain proficiency when a doctrine is researched or promulgated. The training and organisation systems determine how doctrine is taught, practised, and retained.

### 7.5 Local SOPs and mission-specific procedures

Formations may establish SOPs that refine doctrine for their equipment, theatre, or operational experience. Mission-specific procedures may cover communications schedules, identification standards, reporting, movement control, support requests, and emergency actions.

Local procedures should not contradict higher authority without a defined waiver or conflict-resolution process. Where units use different SOPs, interoperability may suffer until procedures are reconciled or liaison arrangements are established.

## 8. Orders and command workflow

### 8.1 Order structure

An order should be a structured, auditable object, not a free-text instruction alone. Depending on type, it may contain:

- Issuer and intended recipients.
- Authority basis and command relationship.
- Mission or task and desired end state.
- Purpose, priority, and commander's intent.
- Assigned objectives and tasks.
- Area of responsibility, boundaries, routes, or control measures.
- Start time, deadlines, phases, and timing dependencies.
- Movement, posture, formation, and engagement constraints.
- Rules of engagement and escalation limits.
- Support, supply, transport, and coordination requirements.
- Reporting, communications, and information requirements.
- Contingencies, triggers, branches, and decision points.
- Validity period, supersession rules, and cancellation conditions.

Not every order requires every field. The schema should support order templates and task-specific fields.

### 8.2 Order lifecycle

An order may progress through states such as:

1. Drafted.
2. Planned or coordinated.
3. Authorised.
4. Transmitted.
5. Received.
6. Acknowledged or queried, if required.
7. Understood and prepared.
8. Executing.
9. Completed, failed, superseded, expired, or cancelled.

States may be combined where a particular command arrangement does not require a separate step. The important principle is that issuance, receipt, comprehension, and execution are not always simultaneous.

### 8.3 Transmission and command latency

Order latency is the time between relevant stages of the command process. It can be affected by:

- Distance and communications method.
- Available communications infrastructure and equipment.
- Signal quality, bandwidth, congestion, and encryption or processing demands.
- Enemy jamming, interception, deception, or physical disruption.
- HQ workload, staff capacity, and current operational tempo.
- Commander's decision time and staff planning requirements.
- Recipient availability, training, and clarity of the order.
- Whether the unit is moving, dispersed, concealed, or operating under emission control.

Different eras and forces should exhibit different command characteristics. A 1955 force should not inherit modern networked command capabilities simply because later technology exists in the simulation.

### 8.4 Communications failure

When communications are unavailable, units may continue under standing orders, execute pre-planned branches, act within delegated intent, wait for contact, or attempt alternate communications. Behaviour depends on doctrine, mission urgency, commander's judgement, and the consequences of acting without updated information.

The simulation should distinguish an order that was never delivered from an order delivered but misunderstood, and from an order understood but not executed.

### 8.5 Order conflicts and amendments

A new order may amend, supersede, or cancel an existing order. Conflict resolution must consider authority, issue time, effective time, scope, and whether the recipient has received the change.

Units should not instantly obey an amendment they have not received. A previously valid order may continue until the new instruction arrives or another defined condition applies. Emergency actions and standing safety rules may take precedence where explicitly defined.

### 8.6 Direct Player intervention

The Player may issue orders directly to units or formations where the game rules permit. Direct control should be represented as a valid command path with its own communications, authority, and latency assumptions, rather than a magical exception to the simulation.

The interface may allow the Player to override subordinate plans, but the consequences should be visible: conflicting tasks, delayed execution, disrupted coordination, or increased command workload where applicable.

## 9. Operational planning and the War Planner

### 9.1 Purpose

The War Planner supports the Player in translating strategic intent into feasible operations. It should be a planning and decision-support tool, not an oracle that knows the enemy's actual state or guarantees success.

It should help the Player define objectives, assemble task forces, sequence actions, assess routes and timing, estimate supply and support requirements, identify dependencies, and compare courses of action.

### 9.2 Planning inputs

The planner may use:

- Friendly force organisation, equipment, condition, readiness, and training.
- Known or estimated enemy dispositions and capabilities.
- Terrain, movement networks, weather, and infrastructure.
- Mission objectives, boundaries, deadlines, and political constraints.
- Logistics stocks, delivery capacity, routes, depots, and consumption estimates.
- Sensor coverage, intelligence confidence, reconnaissance plans, and reporting delays.
- Air, naval, artillery, engineering, medical, communications, and other support.
- Headquarters capacity, command relationships, and interoperability.
- Civilian presence, protected sites, escalation risks, and rules of engagement.

The planner must use the information available to the planning authority, not the hidden world state, unless the Player explicitly chooses a scenario editor or other omniscient mode.

### 9.3 Courses of action

The War Planner may propose or let the Player construct multiple courses of action. Each should state its assumptions, required forces, timing, dependencies, expected resource demand, key risks, and criteria for success or revision.

A course of action can be infeasible, uncertain, or feasible only with significant risk. The planner should identify why: insufficient transport, inadequate fuel, unsupported flanks, poor communications, missing engineering capability, unrealistic timing, or insufficient information.

Recommendations must remain explainable and editable. The planner must not silently change the Player's orders or commit resources.

### 9.4 Synchronisation

Plans may coordinate manoeuvre, fires, air support, reconnaissance, electronic warfare, logistics, engineering, medical support, and reserve commitment. Synchronisation relies on compatible timing, shared references, communications, liaison, and clear authority.

The system should represent planned timing separately from actual execution. Delays, weather, enemy action, equipment failure, or command latency can cause events to desynchronise.

### 9.5 Branches, sequels, and decision points

A plan may include contingencies, alternative routes, follow-on operations, reserve release conditions, and decision points. A branch is a planned response to a condition that arises during the current operation; a sequel is a subsequent operation prepared to follow a likely outcome.

Conditions may include confirmed enemy movement, loss of a crossing, a supply threshold, an objective being secured, unacceptable losses, or a change in political authority. Triggers should be based on the deciding commander's available information, not automatically on omniscient world truth.

### 9.6 Feasibility versus prediction

The planner may estimate movement times, consumption, readiness, coordination risk, and likely opposition, but estimates should carry assumptions and uncertainty. A plan labelled feasible means its known requirements appear supportable, not that it is guaranteed to succeed.

## 10. Operations, missions, tasks, and objectives

### 10.1 Operation

An operation is a coordinated set of tasks intended to achieve one or more operational objectives within a defined time and space. It has a purpose, command authority, participating forces, phases or stages where appropriate, support arrangements, constraints, and assessment criteria.

Operations may be offensive, defensive, stabilisation, security, reconnaissance, reinforcement, evacuation, interdiction, transport, humanitarian support, or other scenario-appropriate types. These categories should remain extensible.

### 10.2 Mission and task

A mission describes the purpose and expected result assigned to a force. A task describes a more specific action or responsibility that contributes to the mission.

The same unit may have a primary mission and supporting tasks, but incompatible requirements should be surfaced. For example, a unit cannot simultaneously be expected to maintain a position, move rapidly to another area, and remain available as an uncommitted reserve.

### 10.3 Objectives and conditions

Objectives should define the desired condition, location, effect, or protection outcome. They may be geographic, force-related, logistical, informational, temporal, or political.

Examples include securing a crossing, maintaining an evacuation route, preventing enemy reinforcement, protecting a port, locating an enemy formation, keeping a supply corridor open, or holding an area until a specified time.

An objective is not automatically complete merely because an assigned unit reaches a map coordinate. Completion depends on the stated conditions and evidence available to the authority assessing it.

### 10.4 Phases and control measures

Plans may use phases, boundaries, routes, phase lines, checkpoints, assembly areas, restricted areas, engagement areas, support areas, and other control measures. These are coordination tools whose meaning depends on the order and doctrine.

Control measures should be spatially and temporally scoped where appropriate. A boundary may restrict movement or assign responsibility without necessarily preventing all fires across it; the exact rule must be explicit.

### 10.5 Task organisation

Task organisation assigns units and support relationships for a mission. It can include temporary attachments, detachment, supporting relationships, operational reserves, and mission-specific groupings.

Task organisation must respect unit capabilities, equipment, personnel, readiness, command authority, transport, and sustainment. Assigning a unit to a task does not create missing capability or erase its support needs.

## 11. Execution, adaptation, and initiative

### 11.1 Execution is stateful

Once an order is received and acted upon, a unit's state changes over time: movement, posture, task progress, readiness, ammunition and fuel, fatigue, cohesion, and exposure to threats. The operational system coordinates these changes but should not duplicate the authoritative state managed by other systems.

### 11.2 Replanning

Operations can be revised when objectives change, intelligence updates, supply falls short, forces become unavailable, or the enemy acts unexpectedly. Replanning requires decisions, communications, and time. Units may continue executing previous instructions while a revised plan is being prepared and transmitted.

### 11.3 Initiative within intent

Subordinate commanders may adapt within assigned intent and authority. Examples include selecting a covered route, adjusting local formation, occupying a nearby suitable position, or pausing when an unexpected obstacle makes continued movement unsafe.

Initiative is bounded by mission purpose, rules of engagement, political constraints, and command relationships. It should not authorise a subordinate to pursue an incompatible objective or disregard explicit restrictions without a defined emergency or override rule.

### 11.4 Failure and deviation

Units may fail to complete tasks because of enemy action, terrain, weather, loss of communications, inadequate support, poor planning, equipment failure, fatigue, or misinterpretation. Deviations should be recorded with causes where known and reported according to available communications and doctrine.

Not every failure is incompetence. The system should distinguish impossible tasks, reasonable risk, adverse conditions, execution errors, and deliberate non-compliance.

### 11.5 Withdrawal, disengagement, and reorganisation

Operations must support withdrawal, retreat, disengagement, relief in place, rotation, regrouping, and reorganisation. These actions have distinct purposes and constraints. They may require covering forces, routes, transport, communications, and a plan to preserve equipment and personnel.

A unit that receives a withdrawal order may need time to disengage and move. The order does not erase ongoing threats or instantly restore cohesion, ammunition, or readiness.

## 12. Rules of engagement and use-of-force constraints

### 12.1 Purpose

Rules of engagement (ROE) define the circumstances and limits under which force may be used. They are distinct from weapon capability and from a unit's general tactical doctrine.

ROE can be established at national, theatre, operation, or mission level. More restrictive orders may apply to a particular force or area. The system should define how rules at different levels interact.

### 12.2 Possible constraints

Depending on scenario and legal model, ROE may specify:

- Self-defence and defence of designated forces or persons.
- Positive identification requirements.
- Hostile act or hostile intent thresholds.
- Geographic or temporal limits on engagement.
- Protected persons, sites, vessels, aircraft, and infrastructure.
- Restrictions on weapon types, target classes, or collateral risk.
- Warning, escalation, and authorisation procedures.
- Detention, treatment of prisoners, and handling of civilians.
- Authority required for certain strikes or escalation levels.

These are configurable rule categories, not a claim that one universal ROE template applies to every country or historical period.

### 12.3 Target identification and uncertainty

ROE may require a level of confidence or positive identification before an engagement can be authorised. The command system must use the target information available to the decision-maker, including its uncertainty and age.

The detailed sensor classification and combat engagement systems determine what is detected or tracked. The command system determines whether the available evidence and authority satisfy the applicable rules.

### 12.4 Compliance, oversight, and accountability

Orders, authorisations, reports, and incidents should be auditable. Units may comply, request clarification, delay action, or violate rules due to misunderstanding, poor training, communication failure, unlawful orders, indiscipline, or deliberate misconduct.

Consequences should depend on the actual circumstances, evidence, leadership, oversight, institutional incentives, and political context. Aggression or other personality traits must not automatically cause war crimes, and the simulation must not treat unlawful conduct as a generic tactical bonus.

### 12.5 Civilian harm and operational consequences

Civilian harm, damage to protected infrastructure, and other incidents can affect legitimacy, local cooperation, intelligence access, morale, diplomatic relations, escalation, recruitment, and political objectives. These effects should be contextual and traceable, not a universal arbitrary penalty.

## 13. Intelligence, reconnaissance, and situational awareness

### 13.1 Separate world truth from knowledge

The simulation should maintain separate concepts for:

1. **Actual state:** the real positions, conditions, capabilities, intentions, and actions represented in the world simulation.
2. **Observation:** information obtained by a sensor, patrol, report, source, or other collection method.
3. **Assessment:** an interpretation or estimate based on available observations and prior knowledge.
4. **Report:** information transmitted to a command authority, potentially with delay, loss, distortion, or missing context.
5. **Operational picture:** the set of information currently available to a particular headquarters, unit, or Player interface.

A force must not automatically know the exact location, strength, equipment, readiness, or intention of every enemy unit.

### 13.2 Intelligence products

Intelligence products may include enemy order of battle estimates, strength assessments, likely courses of action, readiness estimates, terrain analysis, indications and warning, threat assessments, and reports about political or civilian conditions.

Each product should have a source or basis, confidence, time of observation, time of issue, area and subject scope, and known limitations where modelled.

### 13.3 Reconnaissance and surveillance

Reconnaissance is collection intended to answer specific questions or establish information about an area, route, object, or force. Surveillance is observation over time. They may use ground patrols, aircraft, ships, submarines, satellites, sensors, electronic support, human sources, or other capabilities available in the historical period.

Collection tasks need objectives, assigned assets, routes or coverage areas, timing, reporting requirements, and risk limits. Collection effectiveness depends on platform capability, sensor capability, environmental conditions, enemy countermeasures, crew proficiency, and task design.

### 13.4 Intelligence collection is not automatic knowledge

A sensor detection, reconnaissance observation, human report, and assessed intelligence product are not interchangeable. Information can be ambiguous, stale, contradictory, deceptive, incomplete, or incorrectly interpreted.

The Intelligence system should track uncertainty and provenance. This specification defines how command authorities use that information; the Sensors & Detection and Electronic Warfare systems govern relevant collection and interference mechanics.

### 13.5 Reporting and fusion

Reports pass through command and communications structures. A report may be delayed, summarised, duplicated, misrouted, or lost. Headquarters can fuse multiple reports into an assessment, but fusion quality depends on timeliness, source reliability, staff expertise, and the consistency of evidence.

Conflicting reports should not be silently converted into certainty. The interface should show confidence, competing assessments, and the evidence or assumptions that materially affect a decision where practical.

### 13.6 Deception and counterintelligence

Forces may conceal, deceive, spoof, misdirect, or protect information. Deception may create plausible but false assessments; counterintelligence and corroboration can reduce some risks. The model should distinguish deliberate deception from ordinary sensor error or stale reporting.

### 13.7 Intelligence requirements

A commander may issue priority intelligence requirements and other information requirements tied to a decision point. Collection should be prioritised according to operational need, available assets, time, and risk. Intelligence assets assigned to one question may be unavailable for other missions.

## 14. Logistics, supply, and sustainment

### 14.1 Scope

Logistics enables forces to move, operate, repair, recover, and remain effective over time. It includes supply, transport, distribution, maintenance, medical support, recovery, personnel support, and the infrastructure required to connect industrial production to operational units.

This specification defines operational logistics behaviour and its relationship to command and planning. Detailed inventories, goods, production, transport networks, infrastructure, and equipment condition remain authoritative in their dedicated systems.

### 14.2 Supply categories

Depending on the equipment and historical era, supply demand may include:

- Ammunition and weapon-specific consumables.
- Fuel, lubricants, and other operating fluids.
- Food, potable water, and field supplies.
- Spare parts, repair materials, and maintenance consumables.
- Medical supplies and evacuation capacity.
- Engineering materials and construction supplies.
- Batteries, electrical power, and specialist equipment consumables.
- Clothing, personal equipment, and replacement items.
- Mission-specific stores and strategic materials.

The system should be data-driven. Not every unit consumes every category, and consumption rates depend on its equipment, tasks, activity, environment, and operating tempo.

### 14.3 Demand and consumption

Units generate supply demand from their establishment, actual equipment, personnel, condition, operational activity, and orders. Consumption varies with movement, engine operation, firing, flight hours, sailing, weather, maintenance, and other relevant activities.

Planning estimates should distinguish routine consumption, expected mission demand, reserve requirements, and emergency demand. Estimates should be revised when actual conditions differ from assumptions.

### 14.4 Supply states and shortages

A unit may have adequate, constrained, critical, or exhausted supplies for a given category. Supply status is category-specific: a unit may have fuel but lack ammunition, or ammunition but lack the spare parts needed to keep weapons operational.

Shortages affect the activities that depend on the missing supplies. The consequences should be explicit and equipment-specific rather than a generic reduction to all unit statistics. For example, fuel shortage can constrain movement and engine operation; ammunition shortage limits relevant firing; spare-parts shortage can prolong equipment unserviceability.

### 14.5 Supply chain and distribution

Supply moves through sources, depots, distribution points, transport assets, routes, and receiving units. Each stage has capacity, time, handling requirements, and vulnerability. The chain can be disrupted by enemy action, weather, infrastructure damage, congestion, political restrictions, labour shortages, or insufficient transport.

A stockpile at a national depot is not the same as stock available to a front-line unit. Planning must account for the time and capacity required to deliver it.

### 14.6 Supply priorities and allocation

Commanders and logistics authorities may prioritise units, missions, or supply categories when demand exceeds availability. Priorities can be based on operational importance, urgency, risk, readiness, or national policy.

Allocation decisions should be auditable and may create downstream consequences. Prioritising one operation can leave another force short. The system must not create supplies merely because a commander assigns a high priority.

### 14.7 Transport and movement

Transport requirements depend on cargo volume, mass, handling, route capacity, vehicle or vessel availability, loading time, travel time, and destination access. Strategic movement, theatre distribution, and tactical resupply may use different transport systems and authorities.

The logistics planner should identify route constraints, transshipment needs, port and airfield capacity, bridge or road limitations, rail gauge or network constraints where modelled, and exposure to interdiction. Transport assets assigned to one task are not simultaneously available for incompatible tasks.

### 14.8 Forward supply and resupply

Units may receive supply through scheduled delivery, demand-based replenishment, local distribution, emergency resupply, or mission-specific supply packages. The appropriate method depends on doctrine, infrastructure, transport, and the operational environment.

Resupply can be delayed, incomplete, damaged, lost, or delivered to the wrong location. Units may need to move to a supply point or wait for a delivery. A successful delivery updates actual inventory; it does not necessarily restore readiness immediately.

### 14.9 Maintenance and repair

Maintenance requirements originate from the Equipment & Materiel System and the relevant equipment designs. Operational plans should account for maintenance downtime, repair capacity, recovery assets, spare parts, workshop access, and evacuation to suitable facilities.

A damaged or unserviceable item may require repair, replacement, recovery, or cannibalisation. Field repair capability differs from depot-level overhaul. Maintenance backlogs can grow when operational tempo exceeds available staff, facilities, or parts.

### 14.10 Medical support and personnel recovery

Where modelled, medical support includes casualty collection, stabilisation, evacuation, treatment capacity, return to duty, and replacement or recovery of personnel. The system should distinguish personnel losses, temporary unavailability, evacuation, and treatment rather than treating all casualties as one irreversible state.

Medical movement and facilities consume resources and depend on routes, security, transport, and capacity. Medical and humanitarian constraints may also interact with ROE and political objectives.

### 14.11 Operational reach and culmination

Operational reach is constrained by the ability to sustain forces as they move away from bases, depots, ports, airfields, and other support. A force may continue advancing while its supply position becomes increasingly fragile.

The War Planner should help identify likely culmination points: circumstances in which fuel, ammunition, fatigue, maintenance, transport, or other constraints make continued operations infeasible or excessively risky. These are estimates, not guaranteed prediction.

### 14.12 Strategic and industrial connection

National industry produces equipment, ammunition, spare parts, fuel products, and other supplies through the economic and production systems. Logistics moves these resources to operational forces. War demand competes with civilian consumption, exports, other military programmes, and industrial constraints.

The operational system must not independently generate unlimited supplies or duplicate national inventory. It should express demand, priorities, and movement requirements through authoritative economic, inventory, and logistics records.

## 15. Readiness and force availability

### 15.1 Readiness is multidimensional

Readiness describes a unit's ability to perform a specified task within a specified timeframe. It should not be reduced to one universal percentage without an explanation of the underlying state.

Relevant dimensions include:

- Personnel strength and availability.
- Equipment holdings, serviceability, and configuration.
- Ammunition, fuel, spares, and other supply.
- Training and qualification for the intended mission.
- Morale, cohesion, fatigue, suppression, and other unit condition.
- Command and communications capability.
- Transport and deployment capability.
- Headquarters, support, and specialist enablers.
- Time required to assemble, prepare, move, or recover.
- Environmental and infrastructure constraints.

The authoritative unit-condition and equipment-condition systems retain their own data; readiness aggregates those inputs for planning.

### 15.2 Readiness states and thresholds

The simulation may expose readiness categories or estimated mission availability, but the underlying reasons should remain inspectable. Thresholds should be configurable by doctrine, service, mission, and scenario where appropriate.

A unit can be ready for a defensive mission but not for a long-range offensive operation; a unit can be combat-capable but unable to deploy overseas due to transport or support limitations.

### 15.3 Mobilisation and preparation

Mobilisation can require recalling personnel, forming units, issuing equipment, completing training, assembling transport, establishing command links, and building supply stocks. These steps consume time and resources and may compete with ongoing commitments.

A unit should not become fully ready merely because it has been activated in an interface. Readiness is produced by the completion of relevant preparation activities.

### 15.4 Rotation and recovery

Operational plans should account for fatigue, maintenance, supply replenishment, personnel recovery, reorganisation, and training. Rotating a unit out of the line can preserve long-term capability but creates temporary gaps that must be filled by reserves or other forces.

## 16. Joint and combined operations

### 16.1 Joint operations

Joint operations coordinate two or more military branches or domains. Examples include land forces supported by air interdiction, naval forces providing air defence, amphibious forces relying on naval lift and air support, or intelligence assets supporting a theatre operation.

Joint action requires clear tasking, compatible communications, timing, liaison, identification procedures, support priorities, and deconfliction. A joint headquarters may coordinate assets it does not organically own, subject to the authority granted.

### 16.2 Combined and coalition operations

Combined operations involve forces from different countries or political authorities. They may differ in doctrine, language, equipment, communications, security restrictions, logistics, and ROE.

Coalition participation may require national caveats, shared plans, liaison officers, compatible data exchange, host-nation support, and agreements governing command authority. A coalition commander cannot automatically override national restrictions unless the scenario's legal and political arrangements explicitly permit it.

### 16.3 Interoperability

Interoperability depends on equipment compatibility, common procedures, training, communications, data formats, identification standards, staff experience, and shared understanding. It may improve through joint exercises, liaison, common doctrine, and common equipment, but takes time and resources.

### 16.4 Support requests and allocation

A supported commander may request air support, artillery, naval gunfire, engineering, intelligence, transport, electronic warfare, or other assets. The supporting authority evaluates availability, priority, mission compatibility, ROE, and competing demands.

Support requests can be approved, denied, delayed, modified, or only partially fulfilled. The interface should show the reason and expected effect of the decision where known.

### 16.5 Deconfliction

Joint operations may require separation in time, altitude, area, spectrum, routes, and weapon employment. Deconfliction reduces friendly interference and fratricide risk but consumes planning effort and can restrict flexibility.

The system should support explicit control measures and identify conflicts before execution where feasible. It must also allow unplanned interactions to emerge when forces act on incomplete or stale information.

## 17. Terrain, weather, and infrastructure in planning

The operational system consumes the authoritative geographic, terrain, weather, and infrastructure state. It should not create a separate competing map model.

Plans should account for:

- Terrain shape, slope, cover, concealment, and observation.
- Roads, railways, ports, airfields, bridges, crossings, and other transport infrastructure.
- Movement and load limits, route capacity, and choke points.
- Weather effects on mobility, visibility, flight, sensors, supply, and personnel.
- Terrain-state changes such as mud, ice, flooding, fire, cratering, damage, destruction, repair, and obstruction.
- Civilian population, protected locations, urban density, and other mission-relevant geographic features.

The continuous geographic surface and 100-metre movement grid remain distinct representations: the grid supports movement and routing calculations, while underlying geography and Terrain Objects represent physical features and spatial effects. Operational plans should use the appropriate representation without treating grid cells as the complete truth of the world.

Routes and estimates can become invalid when terrain or infrastructure changes. Planners should update routes and movement estimates as information and world state change.

## 18. Simulation timing and resolution

### 18.1 Shared time model

Orders, planning, communications, movement, logistics, intelligence reporting, and combat must use a consistent simulation time model. Each subsystem may have different event frequencies or resolution needs, but their state changes must be ordered and reconciled consistently.

### 18.2 Event-driven and periodic processes

Some events occur at specific times: order arrival, a convoy reaching a waypoint, an objective being reported secured, or a resupply delivery. Other processes accumulate over time: fuel consumption, fatigue, maintenance, staff workload, and supply demand.

Implementation may use event-driven updates, fixed-step simulation, or a hybrid approach. The authoritative technical architecture should decide the exact scheduling strategy while preserving the behaviour defined here.

### 18.3 Command time and execution time

The time an order is drafted, issued, transmitted, received, understood, and executed should be distinguishable where those distinctions matter. Planning interfaces should not imply that a planned action begins at the moment the Player clicks the order button if the model requires preparation or transmission.

### 18.4 Simulation speed and pause

The Player should be able to pause and change simulation speed as supported by the game architecture. Pausing or accelerating time must not change the logical sequence or validity of events, except where the interface explicitly defines real-time external processes.

### 18.5 Determinism, auditability, and replay

Where practical, the simulation should produce reproducible outcomes from the same initial state, orders, model parameters, and random seed. Command decisions, order transitions, supply allocations, and significant operational events should be logged so the Player can understand what happened and why.

## 19. Operational assessment and after-action analysis

### 19.1 Assessment

An operation should have assessment criteria tied to its objectives, desired end state, time limits, and acceptable cost or risk. Assessment may consider territory, force preservation, enemy capability, freedom of movement, supply access, intelligence gained, civilian outcomes, political effects, and other mission-specific factors.

Objective completion and operational success are not always identical. A force may capture an objective but fail to hold it, suffer unacceptable losses, lose a critical supply route, or create political consequences that undermine the strategic aim.

### 19.2 Assessment under uncertainty

Commanders assess progress using reports and observations available at the time. Their assessment may differ from the actual state. Later intelligence or after-action analysis can revise the understanding of what occurred.

The interface should distinguish what a commander believed during execution from what is established after the operation.

### 19.3 After-action review

After-action review may reconstruct orders, reports, movement, logistics, engagement outcomes, delays, and decision points. It should help the Player identify causes and alternatives without implying that every outcome was foreseeable at the time.

### 19.4 Narrative Battle System

The Narrative Battle System is a presentation layer over the underlying simulation. It may turn significant events into a coherent timeline, battle account, command report, or after-action narrative.

Narrative outputs must be grounded in actual simulated events and recorded evidence. They should distinguish confirmed facts from contemporary assessments and later interpretations. Narrative presentation must not independently create hits, casualties, discoveries, orders, or outcomes that the simulation did not produce.

### 19.5 Strategic consequences

Operational outcomes can affect strategic resources, force availability, political support, alliances, escalation, public opinion, industrial demand, territory, infrastructure, and future campaign options. These consequences should be resolved by their authoritative systems and remain traceable to the events that caused them.

## 20. User interface requirements

### 20.1 Command map and order interface

The Player should be able to inspect command relationships, issue and amend orders, view order state, set objectives and constraints, assign support, and understand the difference between planned, transmitted, received, and executing actions.

### 20.2 Order of battle and command view

The command interface should show organic relationships, current operational command, attachments, administrative authority, support relationships, HQ assignments, and relevant command limitations. Temporary changes should have clear start and end conditions.

### 20.3 War Planner

The War Planner should support objective definition, force allocation, route and timing analysis, task organisation, support coordination, logistics estimates, course-of-action comparison, contingency planning, and plan validation.

### 20.4 Intelligence picture

The intelligence interface should distinguish observed, reported, assessed, and unknown information. It should show relevant timestamps, confidence, source or basis where available, and known contradictions without exposing hidden enemy truth by default.

### 20.5 Logistics and readiness view

The Player should be able to inspect supply by category, consumption, delivery estimates, route constraints, maintenance backlog, readiness dimensions, deployment preparation, and likely operational reach.

### 20.6 ROE and authority interface

The interface should show which rules apply to each force or mission, who has authority to approve restricted actions, and whether a proposed action is compliant, requires authorisation, or conflicts with an existing constraint.

### 20.7 Alerts and decision support

Alerts should prioritise decisions that materially affect the operation: delayed orders, lost communications, critical shortages, conflicting tasks, unsatisfied support requests, changing intelligence, deteriorating readiness, and objectives at risk.

Recommendations must be explainable and must not silently alter orders, doctrine, allocations, or ROE.

## 21. Conceptual data model

The following entities are conceptual and may be divided or combined during implementation. They must reference authoritative entities from other specifications rather than duplicate them.

- **DoctrineDefinition:** scope, principles, procedures, prerequisites, version, and applicability.
- **SOPDefinition:** repeatable procedures, conditions, authority, and version.
- **DoctrineAdoptionRecord:** organisation, adoption status, training state, and effective date.
- **CommandAppointment:** character, position, authority, tenure, and succession rules.
- **CommandRelationship:** issuer, recipient, relationship type, scope, and validity period.
- **HeadquartersCapability:** staff, equipment, communications, workload, and planning capacity.
- **Order:** issuer, recipients, authority, mission, constraints, timing, and state.
- **OrderEvent:** transmission, receipt, acknowledgement, amendment, cancellation, and execution history.
- **MissionDefinition:** purpose, tasks, constraints, and desired end state.
- **Objective:** desired condition, scope, assessment criteria, and dependencies.
- **Operation:** objectives, phases, participating forces, command authority, and assessment.
- **TaskAssignment:** unit, task, priority, time window, area, and support relationships.
- **CourseOfAction:** assumptions, force allocation, sequence, resource estimates, and risks.
- **PlanDependency:** trigger, prerequisite, timing relationship, and response.
- **RulesOfEngagementProfile:** applicable constraints, authorities, and effective scope.
- **IntelligenceRequirement:** question, priority, decision linkage, and required confidence or timeliness.
- **IntelligenceObservation:** source, observed subject, time, location, confidence, and limitations.
- **IntelligenceAssessment:** interpretation, evidence, uncertainty, and validity.
- **IntelligenceReport:** origin, recipients, transmission history, content, and timestamps.
- **LogisticsDemand:** unit or operation, supply category, quantity, priority, and required time.
- **SupplyMovement:** origin, destination, cargo, route, transport, schedule, and state.
- **ReadinessAssessment:** task, assessment time, dimension values, limiting factors, and estimate.
- **SupportRequest:** requester, support type, purpose, priority, authority, and response.
- **OperationalAssessment:** objectives, observed progress, confidence, risks, and outcome.
- **AfterActionRecord:** events, decisions, reports, assessments, and lessons.

## 22. System interfaces

### 22.1 Military Branches & Unit Organisation

Consumes authoritative unit structures, unit classes, officer appointments, organic relationships, and attachment mechanisms. Adds operational command relationships and task assignments without rewriting permanent organisation.

### 22.2 Unit condition, training, and courses

Consumes training, experience, morale, cohesion, fatigue, suppression, fitness, and relevant persistent attributes from their authoritative systems. Issues training, preparation, rest, and readiness requirements through defined workflows.

### 22.3 Equipment & Materiel System

Consumes equipment identity, quantities, configuration, condition, serviceability, ammunition, spares, and compatibility. Operational orders cannot grant a unit equipment it does not possess or make unserviceable equipment functional.

### 22.4 Equipment Design, Research & Production

Provides requirements and receives approved designs and manufactured equipment through the production and delivery workflow. Operational urgency may influence procurement priorities but cannot bypass physical manufacturing and delivery constraints without explicit scenario rules.

### 22.5 Sensors & Detection

Consumes detection, classification, identification, tracking, and information-quality outputs as appropriate. Command and intelligence systems use those outputs to form reports and assessments; they do not duplicate sensor calculations.

### 22.6 Electronic Warfare

Consumes communications, sensor, and electronic-warfare effects from the authoritative EW system. Jamming, deception, interference, and emission control can affect command and intelligence processes according to the applicable equipment and doctrine.

### 22.7 Combat & Engagement Systems

Receives orders, target information, ROE, support assignments, and command constraints. It resolves the detailed engagement mechanics. This system consumes engagement outcomes for operational assessment and follow-on decisions.

### 22.8 World, geography, terrain, and weather

Consumes geographic, terrain, infrastructure, and environmental state for route planning, movement estimates, visibility assumptions, logistics, and operational feasibility.

### 22.9 Economy, industry, inventory, and transport

Issues demand and priorities and consumes authoritative stock, production, company, transport, and infrastructure capacity. It must not independently duplicate or generate resources.

### 22.10 Intelligence and strategic systems

Consumes available reports and assessments and provides intelligence requirements, political constraints, strategic objectives, and information relevant to operations. Strategic outcomes are returned to the systems responsible for national policy, diplomacy, economy, and campaign state.

## 23. Simulation integrity and implementation rules

- Keep actual world state, observations, reports, assessments, and Player knowledge separate.
- Keep organic organisation, operational command, tactical control, administrative authority, logistics support, and direct Player orders distinct.
- Track order issuance, transmission, receipt, interpretation, and execution as separate events where relevant.
- Do not allow units to act on amended orders before receiving them unless standing orders or explicit rules justify the action.
- Do not let command authority exceed the issuer's actual scope.
- Model doctrine as a set of procedures and decision influences, not as a universal performance bonus.
- Use actual personnel, equipment, supply, training, communications, and readiness states to determine operational feasibility.
- Do not allow the War Planner to use hidden enemy truth unless an explicit omniscient planning mode is active.
- Keep planned events distinct from actual events and allow delays, disruption, and replanning.
- Do not create supplies, equipment, transport capacity, or specialist capabilities solely to satisfy a plan.
- Do not assume joint or coalition forces are automatically interoperable.
- Preserve the basis, timestamp, and uncertainty of intelligence where practical.
- Keep ROE, legal authority, and political restrictions explicit and auditable.
- Ensure narrative battle accounts are generated from simulated events and do not create independent outcomes.
- Use stable identifiers and explicit timestamps for orders, relationships, plans, assessments, and supply movements.
- Resolve cross-system state changes through documented interfaces and authoritative data ownership.
- Prefer reproducible calculations and event histories so that outcomes can be investigated and replayed.
- Keep scenario data and historical doctrine separate from core simulation rules.
- Ensure the framework supports land, air, naval, strategic, and joint operations without forcing every branch into identical procedures.

## 24. Acceptance criteria

The specification is satisfied when the system can support the following outcomes:

1. The Player can define a strategic objective and translate it into operational objectives, missions, and subordinate tasks.
2. The Player can create an operation with participating forces, command authority, timing, support, constraints, and assessment criteria.
3. Organic organisation remains intact when units are temporarily attached or assigned operationally elsewhere.
4. Command, tactical control, administrative authority, logistics support, and direct Player intervention are represented separately.
5. Headquarters capability depends on staff, equipment, communications, workload, and command arrangements rather than existence alone.
6. Orders pass through appropriate authorisation, transmission, receipt, and execution stages, with realistic delays where relevant.
7. Units can continue under standing orders or delegated intent when communications are lost, within the limits of doctrine and authority.
8. The Player can amend or cancel orders without retroactively changing what recipients knew or were doing before receiving the update.
9. Doctrine affects procedures and decisions, while proficiency depends on actual adoption, training, experience, equipment, and circumstances.
10. Commander attributes affect contextual decision-making without becoming universal bonuses or deterministic personality scripts.
11. The War Planner can compare courses of action, expose dependencies and logistics demand, and flag infeasible or high-risk plans.
12. Planning uses the information available to the relevant authority and does not reveal hidden enemy truth by default.
13. Intelligence observations, reports, and assessments retain meaningful distinctions and can be delayed, uncertain, contradictory, or wrong.
14. Reconnaissance and intelligence collection require assets, time, tasking, and risk, and compete with other uses of those assets.
15. ROE and command authority constrain actions and produce auditable authorisation and compliance records.
16. Supply demand derives from actual forces, equipment, and activity; shortages constrain the relevant operations and can be traced to their causes.
17. Transport, infrastructure, routes, depots, and delivery times constrain supply availability at the unit.
18. Readiness can be assessed for a particular mission and timeframe, with its limiting factors visible.
19. Joint and coalition operations can coordinate support while representing command boundaries, national caveats, and interoperability limits.
20. Terrain, weather, and infrastructure changes can invalidate plans and trigger route reassessment or replanning.
21. Planned timing is distinct from actual execution timing, and simulation speed does not invalidate the logical sequence of events.
22. After-action analysis can reconstruct key decisions, orders, reports, supply states, and outcomes.
23. Narrative Battle System outputs are grounded in actual simulation events and distinguish contemporary assessments from later-established facts.
24. Operational outcomes affect strategic state through explicit system interfaces rather than duplicated ad hoc rules.
25. The same framework supports historically appropriate command and operational practices from the 1955 baseline through at least 2026.

## 25. Open design decisions

The following should be resolved during detailed design and implementation:

- The exact command latency model and which communications technologies require explicit simulation.
- The degree of command and staff workload detail exposed to the Player.
- The balance between direct Player control, subordinate autonomy, and automated execution.
- How doctrine is represented: structured doctrine packages, individual procedures, policy parameters, or a combination.
- How much planning is performed by the War Planner versus the Player and subordinate HQs.
- The specific data model for operation phases, branches, sequels, control measures, and trigger conditions.
- The granularity of logistics categories, transport scheduling, stock allocation, and maintenance support.
- The boundary between operational logistics and the broader Economy, Production, Inventory, and Infrastructure systems.
- How medical support, personnel replacement, prisoner handling, and civilian effects are modelled across historical periods.
- How coalition authority, national caveats, and ROE conflicts are adjudicated.
- The extent to which commanders can independently initiate or modify missions within delegated authority.
- The simulation tick/event model and the resolution of simultaneous orders, movement, supply, and engagement events.
- How intelligence source reliability, deception, and uncertainty are represented without making the interface unmanageably complex.

These decisions should be informed by implementation constraints and the Combat & Engagement Systems specification. They should not be used to collapse the foundational distinctions established in this document.

## 26. Summary

Doctrine, Command & Operations connects military organisation to purposeful action. It defines how intent becomes orders, how headquarters coordinate forces, how commanders act under uncertainty, how operations are planned and adapted, and how logistics and readiness constrain what can be achieved. It preserves the distinction between world truth and information, between permanent organisation and temporary command, and between planned action and actual outcome. Combat resolution remains the responsibility of the Combat & Engagement Systems, while the Narrative Battle System presents the resulting events without inventing them.
