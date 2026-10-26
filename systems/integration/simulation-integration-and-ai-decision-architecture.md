# Simulation Integration & AI Decision Architecture

**Project:** Project Sovereign  
**System:** Cross-System Integration  
**Document ID:** INT-AI-001  
**Status:** Integration and gap analysis, draft 1  
**Historical baseline:** 1 January 1955  
**Technology horizon:** At least 2026, extensible beyond  
**Canonical path:** `systems/integration/simulation-integration-and-ai-decision-architecture.md`

## 1. Purpose

This document audits the interfaces between Project Sovereign's military specifications and defines a common decision architecture for the Player, AI-controlled nations, military headquarters, formations, and Officer Characters.

It follows the specifications for:

- Military Branches & Unit Organisation.
- Equipment & Materiel System.
- Equipment Design, Research & Production.
- Doctrine, Command & Operations.
- Sensors and Detection.
- Electronic Warfare.
- Combat & Engagement Systems.

It also establishes the integration requirements that these systems must expose to wider national simulation, including government, diplomacy, economy, industry, research, logistics, intelligence, and war.

The aim is not to introduce a separate autonomous-agent game, nor to require machine learning. The aim is to ensure every decision-maker uses an explicit, testable interface to the same authoritative simulation and that the simulation, rather than an AI narrative, determines what actually happens.

## 2. Design references and what Project Sovereign takes from them

### 2.1 Command Ops 2: command as a process

Command Ops 2 informs the operational command experience:

- The Player expresses intent through orders rather than directly puppeteering every subordinate action.
- Orders pass through command relationships and can incur delay, interpretation, planning, and execution time.
- Subordinate formations act within their capabilities, doctrine, information, and local circumstances.
- Time continues to pass while units move, prepare, fight, recover, and receive revised orders.
- The Player assesses, plans, orders, and reacts at operational scale.

Project Sovereign should preserve meaningful direct control where appropriate, especially in detailed engagements, but must not erase command friction merely because an order was issued by the Player.

### 2.2 Command: Modern Operations: detailed, player-directed engagements

CMO informs the granularity of engagement simulation:

- Platforms are composed of systems, sensors, mounts, weapons, magazines, communications, engines, and other components.
- Detection, classification, tracking, weapon assignment, launch, interception, damage, and mission effect are separate stages.
- The Player can establish missions, doctrine, rules of engagement, weapon release conditions, and detailed engagement intent.
- Joint and multi-domain operations interact through shared time, geometry, information, and effects.

Project Sovereign must retain the underlying detailed simulation even when the Player or an AI gives a higher-level order. Higher-level control should generate and supervise lower-level activity, not replace it with abstract guaranteed outcomes.

### 2.3 pyCMO: a useful decision-interface reference

pyCMO demonstrates a useful separation between a simulation and the software deciding what action to take. It packages structured scenario state into observations, defines an action interface, applies actions to the simulation, and exposes subsequent observations. Its side-specific feature representation also highlights that an agent's useful information is not identical to the complete simulation state.

Project Sovereign should adopt these **design principles**, not pyCMO's CMO-specific protocol, action set, reinforcement-learning assumptions, or software architecture.

Applicable principles:

1. **Explicit observations:** each decision-maker receives a defined view of relevant state.
2. **Explicit action vocabulary:** decisions are expressed through valid, typed commands rather than arbitrary state mutation.
3. **Action validation:** a command can be rejected, constrained, delayed, or fail during execution.
4. **State-transition accountability:** the simulation resolves consequences and returns updated state and events.
5. **Information boundaries:** decision-makers cannot act on hidden facts unless those facts are obtained through a valid information channel.
6. **Composable actions:** complex plans can be built from smaller orders, tasks, and actions.
7. **Scenario-robust schemas:** missing or inapplicable data must be represented explicitly, not handled through assumptions that every unit or scenario has every possible field.
8. **Evaluation:** decisions and outcomes should be measurable for regression tests, AI evaluation, and debugging.

pyCMO is a reference for structuring the interface between decision-making and simulation, not evidence that reinforcement learning is necessary or sufficient for Project Sovereign.

Reference: https://minhhua.com/pycmo/  
Reference implementation: https://github.com/duyminh1998/pycmo

## 3. Governing architectural rule

**All actors decide; the authoritative simulation resolves.**

The Player, AI nations, governments, Officer Characters, headquarters, and units may propose or issue decisions within their authority. They do not directly edit authoritative world state to manufacture outcomes.

For example:

- A government can authorise mobilisation. The simulation checks law, resources, manpower, industry, time, and other relevant constraints.
- A national research authority can fund an engine programme. Research progresses according to capability, funding, personnel, facilities, uncertainty, and time.
- A design authority can approve an aircraft design. The simulation checks component compatibility, prototype results, production qualification, and industrial feasibility.
- A commander can order an attack. The command system processes the order; subordinate units plan and execute; detection and engagement systems determine what each force knows and can do; combat resolves the resulting interactions.
- An Officer Character can decide to delay, reinterpret, or execute an order according to defined character, command, information, and situation factors. That decision does not grant the character impossible knowledge or override hard constraints.
- An AI nation can decide to invade. The simulation still determines mobilisation, diplomatic consequences, force readiness, access, movement, logistics, detection, combat, losses, and political outcomes.

No Player-only shortcuts are permitted in authoritative simulation rules. Difficulty settings may alter AI decision quality, planning depth, reaction time, or strategic risk tolerance, but must not silently grant AI nations free resources, hidden information, instant production, or superior combat physics.

## 4. The common decision cycle

Every autonomous decision process must follow a common conceptual cycle. Systems may implement it at different frequencies and with different levels of sophistication.

1. **Observe:** obtain the decision-maker's permitted view of current state, reports, history, and relevant forecasts.
2. **Interpret:** estimate the current situation, confidence, unknowns, and likely explanations.
3. **Set or update objectives:** select priorities within the actor's role, authority, doctrine, and current commitments.
4. **Generate candidate actions:** consider feasible orders, plans, policies, or investments.
5. **Evaluate:** compare expected effects, costs, risks, timing, uncertainty, opportunity costs, and possible reactions.
6. **Select and authorise:** choose an action within the actor's authority and resource limits.
7. **Submit:** create a typed command or order through the relevant system interface.
8. **Validate and schedule:** check legality, prerequisites, authority, resources, dependencies, and execution timing.
9. **Resolve:** the authoritative simulation processes the action and its consequences.
10. **Assess:** compare actual outcomes with expected outcomes; update beliefs, priorities, plans, and lessons.
11. **Report:** communicate decisions, failures, changes, and relevant results to authorised recipients.

This is a logical cycle, not a requirement that every actor run every step on every simulation tick. A tactical unit may react at short intervals; a formation headquarters may reconsider a plan less frequently; a government may review strategic priorities over days, months, or years. Urgent events can trigger earlier reconsideration.

## 5. Decision-makers and levels of autonomy

Project Sovereign must not model all AI as one omniscient, all-purpose agent. Decision-making is distributed by role and authority.

### 5.1 National government and strategic leadership

Responsible for national objectives and policy, including:

- Diplomatic posture, alliances, treaties, sanctions, and foreign policy.
- Defence priorities, mobilisation policy, force structure, and military expenditure.
- Industrial strategy, research priorities, procurement policy, and resource allocation.
- War initiation, escalation, termination, and acceptable political costs.
- Domestic stability, public support, political legitimacy, and the consequences of prolonged conflict.
- Strategic assessment of rivals, allies, threats, opportunities, and uncertainty.

Strategic leadership should not directly control every tactical engagement unless the political and command structure explicitly grants that authority.

### 5.2 National institutions and functional authorities

Examples include defence ministries, general staffs, procurement agencies, intelligence services, research institutions, industrial ministries, and service branches.

They convert national policy into plans and programmes, compete for resources, coordinate with other institutions, and report feasibility, progress, risk, and failure. Their authority and effectiveness depend on the country's institutions, expertise, organisation, and resources.

### 5.3 Military high command and theatre commands

Responsible for translating strategic objectives into campaigns, theatre priorities, force allocation, operational plans, sustainment priorities, and command relationships.

They must account for the difference between forces that exist on paper and forces that are ready, supplied, informed, and able to reach the required theatre.

### 5.4 Formation and unit headquarters

Responsible for planning and executing assigned missions within their orders, command capacity, doctrine, staff, communications, and local understanding.

They may create subordinate tasks and adjust execution to local circumstances where permitted. They must not silently abandon the higher commander's intent without a reason grounded in doctrine, authority, loss of communication, emergency, or character-specific behaviour.

### 5.5 Officer Characters

Officer Characters are people embedded in the chain of command, not interchangeable stat multipliers and not omnipotent tactical agents.

Their decisions may be influenced by:

- Competence and relevant training.
- Experience and lessons from previous operations.
- Personality, traits, temperament, and risk tolerance.
- Loyalty, ambition, ideology, professional ethos, and political beliefs where relevant to the setting.
- Trust in superiors and subordinates.
- Fatigue, stress, morale, health, and personal circumstances where modelled.
- Information available, its freshness, source, and credibility.
- Understanding of doctrine and the commander's intent.
- Organisational culture and familiarity with subordinate formations.
- Communication quality, workload, time pressure, and operational uncertainty.

These factors influence judgement and execution through explicit rules. They should not be combined into a universal opaque “AI score” that overrides the underlying circumstances.

Character effects should be bounded and explainable. A brilliant commander cannot create ammunition, eliminate order delay, see through fog of war, or make incompatible equipment work. An incompetent commander can make errors, but should not be randomly incompetent merely to create drama.

### 5.6 Local units and platform behaviour

Units and platforms handle bounded execution tasks: navigation, movement, formation keeping, sensor operation, emission control, threat response, weapon employment under doctrine and authorisation, damage response, and survival actions.

Their autonomy is conditional on command policy, equipment, training, crew, doctrine, communications, and immediate circumstances. A unit may continue standing orders or pre-authorised defensive behaviour during communications loss, but it should not acquire unrestricted independent strategic authority.

### 5.7 Player control

The Player is one actor within the simulation, not a privileged exception to its rules.

The Player may receive richer controls and information displays for usability, but commands remain subject to the same physical, organisational, informational, and temporal constraints as comparable AI commands.

Where the Player has direct tactical authority, the simulation should expose detailed controls comparable in spirit to CMO. Where the Player issues operational intent, the simulation should preserve subordinate planning, command delay, and execution behaviour comparable in spirit to Command Ops 2.

## 6. Officer Character decision model

Officer Character AI should be integrated with command and unit systems rather than built as an isolated dialogue or personality subsystem.

### 6.1 Inputs

A character's decision context should include only information legitimately available to that character:

- Orders received and their provenance.
- Superior's stated intent and known priorities.
- Relevant subordinate reports and observed unit state.
- Current contacts, intelligence assessments, and uncertainty.
- Command authority, rules of engagement, doctrine, and standing orders.
- Available resources, unit readiness, logistics, and time.
- Character profile, skills, experience, traits, and relevant history.
- Recent outcomes and lessons the character could have learned.

### 6.2 Decision outputs

Depending on role and authority, a character may:

- Accept, acknowledge, or clarify an order.
- Plan execution and issue subordinate tasks.
- Recommend an alternative course of action.
- Request resources, reinforcement, permission, or clarification.
- Delay or prioritise actions due to workload, readiness, risk, or conflicting orders.
- Adapt execution within delegated authority.
- Report unexpected circumstances or inability to comply.
- Commit an error, misinterpretation, or inappropriate decision when justified by the model.
- Escalate a decision to a superior when authority is insufficient.

Refusal, disobedience, or insubordination must be explicit, relatively rare unless the character and context support it, and traceable to defined causes. A character should not arbitrarily refuse a lawful order merely because a random trait roll failed.

### 6.3 Avoid personality as a hidden combat bonus

Traits should change decision tendencies and behaviour, not directly grant universal damage, accuracy, speed, or morale multipliers.

Examples:

- A cautious commander may require stronger evidence before committing reserves, but may also preserve forces from avoidable losses.
- An aggressive commander may accept higher casualties for tempo, but should still recognise clear logistical or operational impossibility.
- A methodical commander may plan more carefully while taking longer to issue an order.
- A trusted commander may receive more candid reports or coordinate more effectively where the organisation supports that relationship.
- An inexperienced commander may misjudge risk or fail to anticipate a second-order consequence more often, but should still be constrained by the actual information and rules.

### 6.4 Learning and memory

Characters and organisations may learn from experience. Learning must be grounded in events they experienced, reports they received, and lessons they could plausibly infer.

The system should distinguish:

- **Personal experience:** events the character directly observed or participated in.
- **Reported experience:** events known through subordinate, superior, intelligence, or after-action reports.
- **Institutional lessons:** formal doctrine, training, manuals, and organisational learning.
- **Uncertain inference:** conclusions that may be wrong and should retain confidence.

Do not give every character perfect recall of every world event. Do not allow an AI nation to instantly convert a battle it could not observe into perfect tactical knowledge.

## 7. National AI: a continuously reacting nation

Each AI-controlled nation must be capable of pursuing objectives across the full simulation, not merely reacting to a battle currently visible on the map.

### 7.1 Strategic state

A nation's decision context may include, subject to its legitimate knowledge:

- National objectives and policy.
- Government stability, political constraints, and leadership.
- Population, manpower, labour, skills, and demographic change.
- Treasury, debt, taxation, trade, resources, industrial capacity, and production bottlenecks.
- Research capability, technological knowledge, and active programmes.
- Military branches, order of battle, readiness, training, equipment, and supply.
- Intelligence assessments, known foreign capabilities, and uncertainty.
- Alliances, treaties, access rights, diplomatic relations, and international obligations.
- Wars, fronts, threats, commitments, and likely adversary reactions.
- Domestic opinion, elite interests, economic pressures, and public costs where these systems are modelled.

The nation must not receive hidden enemy design data, exact reserves, production totals, orders, or battle plans unless it has a valid means of learning them.

### 7.2 Strategy and planning horizons

National AI should use layered planning:

- **Long-term:** industrial development, education, research portfolios, force design, infrastructure, doctrine, and national strategic positioning.
- **Medium-term:** procurement, production schedules, mobilisation preparations, force deployment, alliance management, and campaign preparation.
- **Short-term:** diplomatic responses, immediate production reallocations, readiness changes, operational reactions, and crisis management.
- **Event-driven:** invasion warnings, declaration of war, mobilisation, major intelligence revelations, equipment failures, supply disruptions, leadership changes, and other high-impact events.

Plans are provisional. New information, cost changes, unexpected losses, political constraints, or adversary actions can invalidate them. Replanning should account for sunk costs, production already underway, existing commitments, and the time required to reverse decisions.

### 7.3 A common planning loop, not a single monolithic brain

The national AI coordinates specialised functional decision-makers. These may include diplomacy, economy, industry, research, procurement, intelligence, and military planning. A national coordinator resolves conflicts and sets priorities when institutions compete for finite resources.

This does not require separate software agents or large language models. It is a conceptual separation of responsibilities, authority, and evaluation.

Examples of competing priorities that must be represented:

- Expand civilian industry or accelerate military production.
- Fund basic research or develop a near-term weapon.
- Build a new platform or increase ammunition and spare-part output.
- Mobilise now at an economic cost or wait for stronger evidence.
- Reinforce a threatened theatre or preserve a strategic reserve.
- Continue an offensive or consolidate because fuel, ammunition, or replacement equipment is insufficient.
- Accept diplomatic costs for a military opportunity or preserve an alliance.

The AI should produce a coherent decision that reflects its national objectives, constraints, risk preferences, and beliefs, rather than maximising a single variable such as GDP, territory, or enemy losses.

### 7.4 National identity and strategic variation

AI nations should differ through explicit state and policy:

- Strategic objectives and perceived threats.
- Political institutions and decision authority.
- Military traditions and doctrine.
- Industrial and research strengths.
- Risk tolerance, time horizon, and casualty tolerance.
- Alliance commitments and diplomatic preferences.
- Economic model and domestic political constraints.
- Leadership personalities and institutional competence.
- Historical experience and accumulated lessons.

Variation must not depend entirely on random personality labels. National behaviour should emerge from the interaction of capabilities, objectives, beliefs, institutions, and constraints.

### 7.5 War decisions and escalation

The AI must consider whether it can achieve political objectives, not only whether it can win a local battle.

Before initiating or escalating war, it should assess available evidence on:

- Military balance and readiness.
- Likely campaign duration and sustainment.
- Industrial replacement capacity and stockpiles.
- Diplomatic alignment and possible third-party intervention.
- Economic disruption, trade exposure, and domestic stability.
- Geographic access, terrain, infrastructure, and mobilisation time.
- Uncertainty and plausible adverse outcomes.

This assessment can be wrong. AI nations must make decisions using beliefs and available information, and the simulation must resolve the consequences.

War termination is also a decision process. A nation may seek a negotiated settlement, limited objectives, withdrawal, armistice, or continued war based on military position, political objectives, domestic pressure, and expectations of future outcomes.

## 8. Common observation and action contracts

The game should expose a stable conceptual contract between decision-makers and the authoritative simulation. This is an internal architecture requirement, not a requirement to expose a public API to players.

### 8.1 Observation contract

A decision observation should be assembled for a particular actor, role, and point in simulation time. It should include relevant state and knowledge without exposing unauthorised hidden state.

Conceptual fields:

- `observer_id`: actor or decision-making entity.
- `observer_role`: national leadership, institution, headquarters, officer, unit, or platform.
- `simulation_time`: authoritative world time.
- `known_state`: current facts the observer is permitted to know.
- `reports`: received reports with source, timestamp, reliability, and confidence.
- `uncertainties`: unknown, stale, contradictory, or low-confidence facts.
- `objectives`: current assigned and self-maintained objectives.
- `orders`: active orders, authority, constraints, status, and provenance.
- `capabilities`: relevant available capabilities and limitations.
- `resources`: resources accessible to this decision-maker, not global totals by default.
- `constraints`: legal, political, doctrinal, technical, organisational, and temporal restrictions.
- `recent_events`: relevant events known to this actor.
- `schema_version`: observation contract version.

Not every observation contains every field. Domain-specific optional data should be typed and documented. Absence of a field must not be interpreted as a value of zero, proof of nonexistence, or permission to assume a favourable condition.

### 8.2 Action contract

A decision is submitted as a typed intent or command, not as an arbitrary patch to simulation state.

Conceptual fields:

- `action_id`: unique identifier.
- `actor_id`: issuer.
- `action_type`: typed action or order.
- `target`: unit, formation, institution, programme, location, nation, contact, or other relevant entity.
- `parameters`: action-specific options and constraints.
- `authority_basis`: the actor's command or institutional authority.
- `priority`: relative priority where the receiving system supports it.
- `validity`: time window, expiry, or conditions for cancellation.
- `preconditions`: requirements that must be true for acceptance or execution.
- `provenance`: parent order, plan, or policy that generated the action.
- `schema_version`: action contract version.

The receiving system returns an explicit status, such as accepted, rejected, queued, awaiting authority, awaiting resources, executing, completed, failed, cancelled, or superseded. Exact enumerations belong in implementation schemas.

### 8.3 Separate acceptance from success

Three states must never be conflated:

1. **Accepted:** the receiving system accepts the command as valid and authorised.
2. **Executed:** the action or its relevant attempt actually occurs.
3. **Achieved:** the intended effect is realised.

An accepted movement order does not mean a unit has arrived. An accepted attack order does not mean a target was detected or fired upon. A completed research programme does not mean a design is production-qualified. A signed procurement contract does not mean equipment has been manufactured, delivered, accepted, and fielded.

### 8.4 Events and reports

Authoritative state changes should emit structured events. Examples include:

- Order received, delayed, amended, rejected, or completed.
- Unit detects, loses, or reclassifies a contact.
- Weapon assigned, fired, intercepted, misses, hits, or causes damage.
- Unit becomes suppressed, retreats, loses cohesion, or is destroyed.
- Production begins, encounters a bottleneck, completes, or fails quality acceptance.
- Research milestone reached or programme delayed.
- Mobilisation authorised, delayed, or completed.
- Intelligence assessment changes confidence.
- Treaty proposed, signed, violated, or terminated.

Events are not all automatically visible to all actors. The information/reporting system determines who receives which facts, when, and with what fidelity.

## 9. Integration audit: system boundaries

This section defines the key hand-offs and identifies what must be verified or implemented before the simulation can be considered integrated. It does not assume that all supporting systems have already reached implementation-ready schemas.

### 9.1 Military organisation and Officer Characters

**Owns:** organisational hierarchy, organic command relationships, attachments, unit state, training, appointments, Officer Character profiles, and unit lifecycle.

**Consumes:** orders, personnel availability, equipment and materiel state, intelligence reports, doctrine, logistics, terrain, and combat outcomes.

**Provides:** command hierarchy, command capacity, unit composition, readiness, cohesion, morale, fatigue, suppression, training, leadership assignments, and authority context.

**Integration requirements:**

- Organic command, operational control, temporary attachment, administrative authority, and direct Player orders must remain distinct relationships.
- Every order must resolve its issuer, recipient, authority, parent order, timing, and current status.
- A unit without an assigned headquarters remains possible, but its command and planning limitations must be explicit.
- Character decisions must use the information available at the relevant time.
- Combat results update unit and character state through explicit events; no duplicate casualty or morale application is permitted.
- A commander's replacement, casualty, reassignment, or loss of communications must update the command system and affected orders coherently.

**Gap status:** conceptual ownership is established in the existing specifications; exact shared data contracts, event ownership, and cross-system invariants require consolidation into canonical implementation schemas.

### 9.2 Doctrine, Command and Operations

**Owns:** doctrine, standing orders, mission intent, command authority, order lifecycle, command latency, planning, and operational execution.

**Consumes:** organisation, command capacity, unit state, intelligence, terrain, logistics, equipment capability, and combat events.

**Provides:** orders, tasks, mission policies, delegated authority, execution priorities, and operational plans.

**Integration requirements:**

- Orders are durable entities with lifecycle and provenance, not one-time text instructions.
- Order processing, acknowledgement, planning, movement, and execution may consume time.
- Subordinates may adapt within delegated authority, but must preserve command intent unless circumstances or authority justify a change.
- Player and AI orders use the same command validation and execution rules.
- Conflicting, obsolete, unreachable, or impossible orders must produce explicit status and reports.
- Higher-level plans decompose into lower-level tasks without skipping simulation prerequisites.

**Gap status:** the command concepts are specified; integration testing must prove that order lifecycle and command delays affect real simulation behaviour rather than remaining descriptive fields.

### 9.3 Sensors and Detection

**Owns:** sensor definitions and instances, signatures, observations, contact tracks, classification, identification, fusion, uncertainty, and information ageing.

**Consumes:** sensor and target equipment, platform condition, environment, terrain, emissions, movement, and electronic warfare effects.

**Provides:** side-specific observations, contacts, classifications, track confidence, estimated position and movement, and intelligence reports.

**Integration requirements:**

- Combat AI must select targets from contacts and tracks available to the relevant actor, not directly from hidden enemy units.
- A track must preserve source, time, confidence, classification, and uncertainty.
- Track quality and age affect weapon eligibility and attack decisions where the weapon system requires it.
- Sensor fusion must not leak information across nations or unrelated command recipients.
- The authoritative simulation may know exact positions; decision-makers must only receive the information permitted by their sensors and reporting channels.

**Gap status:** strong conceptual contracts are present in the Sensors and Detection specification. The combat and AI consumers must be validated against these contracts through end-to-end tests.

### 9.4 Electronic Warfare

**Owns:** electronic support, electronic attack, electronic protection, defensive countermeasures, jamming, deception, emission control, and EW effects.

**Consumes:** emitter and receiver definitions, platform and installation state, geometry, emissions, communications, sensor operation, and adversary capabilities.

**Provides:** effects on detection, tracking, communications, guidance, survivability, and confidence; observations of emitters and possible deception.

**Integration requirements:**

- EW changes the relevant observation, communication, or engagement conditions; it does not directly set a universal “jammed” result independent of geometry and equipment.
- ESM detections are observations with uncertainty and provenance, not automatic exact identification.
- Emission control is an order/policy that changes operational trade-offs and may reduce own detection risk while limiting own sensor or communication use.
- Electronic protection and counter-countermeasures depend on equipment, capability, doctrine, and conditions.
- EW events must be available to the appropriate sensor, command, and combat systems without double-applying effects.

**Gap status:** system boundaries are described; integration must test combined EW, sensor, communication, and weapon-guidance cases under time progression.

### 9.5 Combat and Engagement

**Owns:** engagement eligibility, target/weapon matching, weapon assignment, firing, flight or travel, interception, hit resolution, damage, mission effects, and combat events.

**Consumes:** orders and ROE, contacts and tracks, weapon and platform state, ammunition, component condition, doctrine, terrain, environment, EW, and target characteristics.

**Provides:** engagement outcomes, losses, damage, ammunition expenditure, mission kills, suppression, destruction, reports, and lessons for subsequent decisions.

**Integration requirements:**

- Order to attack, weapon assignment, firing, hit, damage, and destruction remain distinct.
- Weapon eligibility must depend on actual weapon capabilities, condition, ammunition, geometry, target information, and applicable rules.
- A platform may act autonomously only within its configured mission, doctrine, ROE, and delegated authority.
- A contact that is lost or degraded can invalidate or change an engagement.
- Weapon allocation, salvo size, defensive response, and abort/re-engagement decisions must use current permitted information and actual available weapons.
- Combat outcomes update authoritative unit, equipment, personnel, supply, and intelligence state exactly once.

**Gap status:** broad combat behaviour is specified. The key risk is not absence of combat concepts but failure to connect them to command, sensors, EW, equipment, logistics, and national decision-making through common contracts.

### 9.6 Equipment design, research, production, and materiel

**Owns:** technological knowledge, component designs, complete equipment designs, compatibility, prototypes, tests, qualification, industrial manufacturing, production orders, equipment lots, inventory, service introduction, upgrades, and obsolescence.

**Consumes:** research investment, scientific and engineering capacity, company capability, resources, industrial capacity, manufacturing processes, procurement authority, and operational feedback.

**Provides:** available designs and versions, validated performance, production availability, delivery forecasts, costs, compatibility, equipment instances, condition, and supply demands.

**Integration requirements:**

- Research unlocks possibilities; it does not automatically redesign, produce, install, or upgrade fielded equipment.
- A design may exist without being tested, qualified, affordable, or in production.
- An item may be ordered but not delivered; delivered but not accepted; accepted but not installed; installed but not serviceable.
- AI nations must use the same component compatibility, design, production, and introduction-to-service constraints as the Player.
- Combat and maintenance results may inform future design decisions, but only through recorded evidence, reports, and learning.
- AI procurement decisions must reconcile the total force package: platforms, weapons, ammunition, spares, training, support infrastructure, fuel, and production lead times.

**Gap status:** the lifecycle is specified. The principal integration need is an AI procurement/force-planning loop that evaluates real, design-specific performance and life-cycle support rather than selecting equipment from an abstract strength rating.

### 9.7 Logistics, industry, economy, and infrastructure

**Owns:** resource availability, transport, production inputs, supply distribution, fuel and ammunition delivery, maintenance, infrastructure, and economic consequences.

**Consumes:** force plans, production orders, mobilisation, movement, equipment state, battle expenditure, and national priorities.

**Provides:** actual availability, throughput, bottlenecks, delivery time, shortages, serviceability, costs, and sustainment limits.

**Integration requirements:**

- Operational plans must query feasible sustainment, not just nominal stockpiles.
- Production output must flow through delivery, acceptance, distribution, and installation before it changes field capability.
- A supply shortage must affect the relevant units and operations through defined rules and time, not through an arbitrary global combat penalty.
- Military expenditure and mobilisation must have economic opportunity costs.
- National AI must be able to recognise bottlenecks and choose between competing resource demands.

**Gap status:** this document defines the required interface but does not replace a full logistics/economy integration audit. Confirm canonical ownership of inventory, transport capacity, resource accounting, infrastructure capacity, and maintenance scheduling before implementation.

### 9.8 Intelligence, diplomacy, and national simulation

**Owns:** national beliefs and assessments, foreign relations, treaties, diplomatic commitments, intelligence sources, strategic objectives, and war status.

**Consumes:** reports and observations, diplomatic actions, military events, economic conditions, leadership decisions, and public/institutional feedback.

**Provides:** national objectives, authorised policies, intelligence assessments, diplomatic constraints, known threats, and political consequences.

**Integration requirements:**

- AI nations reason from beliefs and reports, not omniscient world state.
- Diplomatic commitments, access rights, alliance obligations, and war status must be authoritative state, not merely narrative.
- Military actions can have diplomatic, economic, and domestic consequences.
- Intelligence may be false, stale, incomplete, or deliberately deceived.
- The AI should distinguish facts, estimates, assumptions, and confidence.
- National objectives must be reconciled with available capability and the likely cost of pursuing them.

**Gap status:** the military specifications identify these dependencies, but this document does not claim that a complete national strategic AI or intelligence model has already been specified. These require their own authoritative specifications and integration contracts.

## 10. Cross-system interaction sequence: a worked example

The following example demonstrates the intended causal chain. It is illustrative, not a fixed scenario or a hard-coded event script.

An AI nation identifies a strategic opportunity and considers a limited offensive.

1. **National assessment:** its leadership receives intelligence reports suggesting an adversary's forward position is weak. The reports are incomplete and have confidence values.
2. **Strategic decision:** the government weighs objectives, diplomatic risk, economic cost, readiness, likely adversary reactions, and the possibility of intervention by other nations.
3. **Authorisation:** if it chooses to proceed, the government authorises a military objective and the required mobilisation or operational policy.
4. **Force planning:** military high command checks available formations, command structure, equipment, training, readiness, and likely opposition.
5. **Sustainment planning:** logistics assesses fuel, ammunition, transport, maintenance, infrastructure, and replacement capacity. The plan may be delayed, narrowed, or rejected if support is insufficient.
6. **Order issuance:** command creates orders with authority, priorities, validity, intent, and dependencies.
7. **Command processing:** headquarters receive and interpret orders. Staff capacity, communication quality, workload, character judgement, and delay affect when subordinate plans become executable.
8. **Movement and observation:** formations move through the world. Terrain, infrastructure, weather, time, and supply affect movement. Sensors and intelligence create observations and contacts according to actual conditions.
9. **Adversary response:** the opposing AI receives only the information its side obtains. It may reinforce, conceal, reposition, mobilise, seek allies, change ROE, or prepare a counterattack.
10. **Engagement:** the attacker may receive an order to engage a contact. Combat checks identification, weapon eligibility, authority, ammunition, range, geometry, and current conditions before resolving fire.
11. **Effects:** losses, damage, suppression, expenditure, movement disruption, and information changes are applied to authoritative state.
12. **Assessment:** both sides receive reports of varying quality. They may disagree about losses or intentions and update their plans at different times.
13. **Strategic consequences:** the war affects production, trade, public support, diplomacy, research priorities, leadership reputation, and future force planning.
14. **Learning:** lessons enter personal or institutional memory only where supported by observed events and available reports.

At no point does the AI simply write “victory” into the world. The outcome emerges from the same simulation rules that govern the Player.

## 11. Gap register and priority

The following register distinguishes concepts already present in the specifications from integration work that remains to be verified. “Gap” does not necessarily mean the underlying concept is absent; it may mean there is not yet a single authoritative contract or end-to-end test.

| ID | Gap or risk | Priority | Required action |
|---|---|---|---|
| INT-01 | No single cross-system decision/observation/action contract is established across all AI roles. | Critical | Define canonical schemas, versions, authority checks, and result statuses. |
| INT-02 | Information boundaries may differ between national AI, headquarters, Officer Characters, and platform AI. | Critical | Define observer-scoped knowledge, report provenance, and hidden-state access rules. |
| INT-03 | Order acceptance, execution, and achievement may be conflated across systems. | Critical | Standardise lifecycle states and prove them in integration tests. |
| INT-04 | Combat decisions may bypass command, sensor, EW, or equipment constraints if consumers query authoritative state directly. | Critical | Enforce system-owned interfaces and end-to-end engagement tests. |
| INT-05 | AI nations need a unified loop across diplomacy, research, industry, production, logistics, and war. | Critical | Specify national strategic AI and functional planning responsibilities. |
| INT-06 | Officer Character traits risk becoming opaque modifiers instead of explainable decision tendencies. | High | Define character decision inputs, outputs, authority, and explainable causal factors. |
| INT-07 | AI procurement may optimise platform statistics without ensuring supportability. | High | Evaluate whole-life force packages, including ammunition, spares, training, fuel, and delivery. |
| INT-08 | Learning and adaptation may accidentally leak hidden information or create instantaneous knowledge. | High | Record experience provenance and distinguish personal, reported, and institutional learning. |
| INT-09 | Cross-system events may duplicate or omit damage, losses, supply expenditure, or readiness changes. | High | Define event ownership, idempotency, and authoritative state transitions. |
| INT-10 | Different decision loops may operate at inconsistent frequencies or react to the same event repeatedly. | High | Define scheduling, triggers, priorities, cooldowns, and safe replanning rules. |
| INT-11 | Schema fields may be assumed to exist for every unit, domain, scenario, or era. | High | Use versioned, typed, optional domain data and robust validation. |
| INT-12 | Deterministic simulation and reproducible AI testing are not yet consolidated across all systems. | High | Separate simulation randomness from AI choice and preserve replayable seeds and decisions. |
| INT-13 | National AI strategic goals and operational military objectives may conflict without a defined arbitration process. | High | Define objective priority, escalation, resource allocation, and conflict resolution. |
| INT-14 | Integration with population, domestic politics, economy, and diplomacy remains broader than the current military audit. | Medium/High | Conduct a cross-domain audit after their canonical specifications are identified. |
| INT-15 | Player-facing combat detail may conflict with manageable operational command if every decision requires manual control. | Medium/High | Define layered control, delegation, mission policies, and optional detail without reducing simulation fidelity. |

## 12. Reference architecture and ownership

The intended dependency direction is:

- **World simulation** owns authoritative time, geography, environment, movement constraints, and physical state.
- **Domain systems** own their rules and state transitions: sensors, EW, combat, equipment, production, logistics, diplomacy, economy, and research.
- **Command and authority systems** determine who may issue which orders, how they are transmitted, and how they are executed.
- **Decision processes** consume actor-specific observations and submit typed intents or orders.
- **Reporting and presentation** decide which results are communicated to which actors and how the Player sees them.

Decision processes must not duplicate authoritative domain calculations. For example, a combat AI can evaluate whether to launch a missile, but the weapon system resolves eligibility, launch, flight, interception, hit, and damage. A procurement planner can choose a design and quantity, but the production system resolves inputs, capacity, lead times, quality, and delivery.

### 12.1 Suggested logical modules

These are conceptual boundaries, not a mandate to use separate executables or microservices:

1. Observation Builder.
2. Decision Scheduler.
3. Objective and Policy Manager.
4. Candidate Action Generator.
5. Action Evaluator / Planner.
6. Authority and Constraint Validator.
7. Order and Action Lifecycle Manager.
8. Authoritative Simulation Systems.
9. Event and Report Distribution.
10. Decision and Outcome Recorder.
11. Evaluation and Regression Harness.

### 12.2 Determinism and reproducibility

The simulation should be reproducible from a known initial state, simulation version, input actions, and recorded random seeds wherever technically feasible.

AI decision-making may use probabilistic choices, but tests must be able to capture the chosen action and its inputs. For a replay, the system should either replay recorded decisions or reproduce their selection from the same inputs and seed.

Do not allow a nondeterministic language model or external service to become an unrecorded authority over physical simulation outcomes. If language models are used for planning or explanation in future, their outputs must be constrained to valid action schemas, validated, logged, and separated from authoritative state mutation.

### 12.3 Performance and scheduling

The full strategic AI must not recompute every national decision on every simulation tick.

Use event-triggered reconsideration, scheduled planning windows, priority queues, cached evaluations, and bounded planning depth. Urgent threats can trigger immediate assessment; long-horizon research or industrial plans may be reviewed at longer intervals.

Performance optimisation must not alter outcomes silently. Any approximation or reduced decision cadence should be documented and testable.

## 13. Testing and observability requirements

Integration is not complete because documents reference each other. It is complete when the causal chain works in a running simulation and failures can be diagnosed.

### 13.1 Decision trace

For any significant autonomous action, the debugging interface should be able to show:

- Decision-maker and role.
- Simulation time and triggering event.
- Observation or report set used.
- Relevant uncertainties and assumptions.
- Active objective and parent order or plan.
- Candidate actions considered, where practical.
- Chosen action and primary reasons.
- Validation result and any rejected constraints.
- Order lifecycle and execution timestamps.
- Actual result and resulting events.
- Whether and how the outcome changed the next decision.

This is primarily a developer/debugging requirement. It does not mean the Player should see an enemy nation's private deliberations during gameplay.

### 13.2 Required integration tests

1. **Information asymmetry:** an AI nation cannot target an undetected enemy unit solely because the simulation knows its exact position.
2. **Report delay:** a headquarters cannot react to a report before the information reaches it.
3. **Order lifecycle:** an accepted order can remain queued or fail, and acceptance alone never moves a unit or causes an attack.
4. **Command loss:** units follow their defined standing orders and delegated autonomy when communications are interrupted.
5. **Officer judgement:** character traits influence defined decisions without overriding authority, physical limits, or information boundaries.
6. **Weapon eligibility:** an attack order cannot fire an incompatible, unavailable, unserviceable, or unauthorised weapon.
7. **EW interaction:** jamming or deception changes the relevant detection, communication, or engagement process according to the EW system, without a global omniscient effect.
8. **Ammunition accounting:** a weapon firing consumes the correct ammunition exactly once.
9. **Combat-state propagation:** losses and damage update equipment, unit strength, personnel, morale/cohesion/suppression, supply, and reports through defined ownership.
10. **Research-to-fielding:** a technology discovery does not automatically upgrade existing equipment or formations.
11. **AI procurement:** an AI cannot field a newly designed platform before its design, qualification, production, delivery, and introduction prerequisites are met.
12. **Industrial bottleneck:** competing production orders cannot exceed available industrial capacity and inputs.
13. **Strategic trade-off:** the AI can choose between competing priorities rather than always maximising one score.
14. **War and diplomacy:** military action produces the defined diplomatic and political consequences and respects treaties, access, and war status.
15. **Replanning:** an AI can revise a plan when supply, intelligence, readiness, or adversary actions change.
16. **No hidden-state learning:** an AI learns only from direct experience, received reports, or legitimate institutional channels.
17. **Deterministic replay:** the same recorded inputs and simulation version reproduce the same authoritative state transitions within the defined determinism envelope.
18. **Multi-nation interaction:** several AI nations can simultaneously research, produce, mobilise, form alliances, respond to threats, and pursue conflicting objectives without bypassing shared constraints.
19. **Player parity:** comparable Player and AI actions use the same validation, physical resolution, production, logistics, and information rules.
20. **Long-horizon stability:** AI plans remain coherent over extended simulation time and do not endlessly cancel and recreate the same orders or oscillate between priorities without new information.

### 13.3 Evaluation metrics

Measure more than victory rate. Useful measures include:

- Order completion and failure rates.
- Time from event to assessment, order, and execution.
- Resource and ammunition waste.
- Force readiness and sustainment adequacy.
- Procurement lead-time and supportability errors.
- Quality of intelligence calibration and confidence.
- Strategic objective progress and opportunity cost.
- Frequency of plan churn, deadlock, and contradictory orders.
- Compliance with authority and information boundaries.
- Robustness across eras, equipment mixes, map types, and scenario sizes.
- Explainability of major decisions and ability to reproduce failures.

Difficulty should primarily reflect decision quality, planning depth, responsiveness, uncertainty handling, and strategic competence. Avoid hidden combat bonuses as a substitute for intelligence.

## 14. Recommended implementation sequence

### Phase A: contract consolidation

1. Establish canonical IDs and ownership for entities shared across military specifications.
2. Define observation, action/order, report, and event contracts.
3. Standardise order lifecycle states and action validation.
4. Define observer-scoped knowledge and information provenance.
5. Document authoritative state ownership to prevent duplicated rules.

### Phase B: military vertical slice

Build one complete, testable path:

1. A formation receives a mission order.
2. Command delay and hierarchy affect receipt and execution.
3. The formation moves through the authoritative world.
4. Sensors produce observations and contacts.
5. The unit or headquarters evaluates a contact using doctrine and ROE.
6. Combat resolves a weapon engagement from actual equipment and ammunition.
7. Results update unit, equipment, supply, and character state.
8. Reports reach the relevant decision-makers after the appropriate delay.
9. The commander reassesses and continues, modifies, or cancels the plan.

The vertical slice should work for the Player and AI using the same underlying simulation.

### Phase C: autonomous operational command

Add headquarters planning, subordinate task generation, delegated authority, standing orders, command-loss behaviour, operational adaptation, and Officer Character judgement.

### Phase D: national strategic AI

Add the national objective manager and functional planning loops for diplomacy, intelligence, research, industry, procurement, mobilisation, and military operations. Make resource conflicts and political constraints explicit.

### Phase E: long-horizon adaptation and evaluation

Add personal and institutional learning, after-action assessment, strategic memory, improved planning methods, and systematic scenario evaluation. Reinforcement learning may be explored as an optional technique, but it is not a prerequisite for the architecture.

## 15. Decisions and non-goals

### 15.1 Decisions established by this document

- All actors operate through a shared authoritative simulation.
- AI receives actor-scoped observations rather than unrestricted world state.
- Actions are typed intents or orders that pass validation and have explicit lifecycle states.
- Acceptance, execution, and achieved effects are separate concepts.
- AI decision-making is distributed by role and authority.
- Officer Character traits affect judgement and tendencies, not universal combat outcomes.
- National AI spans the whole nation, including diplomacy, economy, research, design, production, logistics, and war.
- Learning is bounded by experience, reports, and institutional knowledge.
- Determinism, logging, replay, and evaluation are first-class engineering requirements.
- Command Ops 2 and CMO are complementary references; pyCMO informs the decision interface, not the overall game design.

### 15.2 Non-goals

This document does not:

- Require reinforcement learning, neural networks, or large language models.
- Require an agent-per-unit or agent-per-nation software deployment model.
- Replace the existing command, combat, sensor, EW, equipment, research, production, or logistics specifications.
- Permit AI to bypass simulation constraints for convenience or difficulty.
- Require the Player to micro-manage every unit.
- Expose enemy AI deliberations or hidden world state during ordinary gameplay.
- Claim that all national, economic, diplomatic, and population systems are already fully specified.

## 16. Open decisions for subsequent specifications

These decisions should be resolved in the relevant architecture or system specifications rather than guessed inside individual AI implementations:

1. The exact canonical schema and versioning strategy for observations, orders, reports, and events.
2. The authoritative simulation scheduler and permitted update cadence for each system.
3. The detailed priority and conflict-resolution rules between national institutions.
4. The extent of autonomous authority granted to headquarters and platforms under each doctrine and ROE.
5. The formal model of uncertainty, belief revision, intelligence deception, and confidence calibration.
6. The division between fixed rule-based planning, search/planning algorithms, utility evaluation, and optional learned policies.
7. The scope of personal and institutional memory, including decay, revision, and after-action learning.
8. The canonical interface between strategic AI and population, economy, politics, diplomacy, and public opinion.
9. The precise event ownership and idempotency conventions for losses, damage, expenditure, production, and state changes.
10. The benchmark scenario suite and quantitative thresholds for AI competence, performance, and long-horizon stability.

## 17. Definition of done

This integration specification is ready to guide implementation when:

- Every participating system has an identified state owner and explicit inputs/outputs.
- Actor-specific information boundaries are documented and testable.
- Orders, actions, events, and reports have consistent lifecycle and provenance.
- A complete command-to-engagement-to-reporting vertical slice passes integration tests.
- Player and AI use the same authoritative simulation rules.
- Officer Character decisions are bounded, explainable, and connected to command authority.
- A national AI can make and revise strategic decisions across military and non-military domains without omniscience or free resources.
- AI procurement, research, design, production, fielding, and sustainment remain causally connected.
- Important decisions and outcomes can be replayed and diagnosed.
- The remaining cross-domain gaps are tracked against canonical specifications and acceptance tests.

---

## 18. Repository specification governance: audit result

The canonical register is now maintained at [`technical/architecture/canonical-specification-register.md`](../../technical/architecture/canonical-specification-register.md). It designates the authoritative Military specifications, normalises the organisation and equipment document IDs, records legacy duplicates, defines the system ownership matrix, and tracks missing dependencies.

The two shorter duplicate specifications have been marked as legacy/superseded, their retired document IDs reassigned to the canonical files, and their known unique requirements reconciled into the canonical specifications. They remain in the repository as read-only historical references until a separate housekeeping decision. Implementation agents should use only the canonical paths listed in the register.

The audit also identifies Military Logistics & Sustainment as the next missing Military system specification, and flags Character System, Communications, Intelligence, and Environment ownership for confirmation before the Military interface contract is frozen.

## References

- Command Ops 2: https://store.steampowered.com/app/521800/Command_Ops_2_Core_Game/
- Command: Modern Operations official site: https://www.matrixgames.com/game/command-modern-operations
- Command: Modern Operations manual: https://www.matrixgames.com/amazon/PDF/CMO/CMO_manual_EBOOK.pdf
- pyCMO design article: https://minhhua.com/pycmo/
- pyCMO repository: https://github.com/duyminh1998/pycmo
