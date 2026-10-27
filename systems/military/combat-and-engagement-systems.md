# Combat & Engagement Systems

**Project:** Project Sovereign  
**System:** Military Simulation  
**Document ID:** MIL-CBT-001  
**Status:** System specification, draft 1  
**Historical baseline:** 1 January 1955  
**Technology horizon:** At least 2026, extensible beyond  
**Canonical path:** `systems/military/combat-and-engagement-systems.md`

## 1. Purpose and design references

This specification defines how hostile forces detect, assess, engage, damage, disable, capture, or destroy one another, and how those outcomes emerge from equipment, information, command, doctrine, terrain, environment, and time.

The intended player experience is informed by three complementary design references:

- **Command Ops 2:** operational command as a continuous process; command hierarchy and order delay; time and space without a hex-and-turn abstraction; subordinate execution; planning, assessing, ordering, and reacting; cohesion, fatigue, morale, and the consequences of command friction.
- **Command: Modern Operations (CMO):** detailed individual platforms and systems; sensor and weapon interactions; missions and tasking; doctrine and rules of engagement; weapon allocation; magazines and mounts; joint all-domain operations; uncertainty, target tracks, and equipment-specific capabilities.
- **Aurora 4X:** a player-driven, data-rich design and technology model in which component designs, research, engineering constraints, production, and integrated systems are meaningful rather than represented by a small set of abstract upgrade levels.

These are references for design principles and player experience, not instructions to reproduce another game's proprietary implementation or exact formulas. Project Sovereign must retain its own architecture, data model, mechanics, UI, and historical-economic simulation.

The desired result combines **Command Ops 2's operational command friction** with **CMO's equipment-level engagement detail**, within a persistent strategic world where equipment can be designed, manufactured, fielded, sustained, lost, repaired, captured, and replaced over decades.

## 2. Scope and boundaries

This specification covers:

- The engagement lifecycle from information available to a decision-maker through authorisation, weapon assignment, attack execution, and assessment.
- Weapon employment, launch and release conditions, fire control, guidance, interception, hit assessment, and terminal effects.
- Damage, component failure, mission kill, mobility kill, suppression, incapacitation, destruction, abandonment, capture, and recovery.
- Ground, air, naval surface, subsurface, amphibious, air-defence, strategic strike, and joint engagements.
- Fire support, indirect fire, area effects, counter-battery action, mine warfare, and other domain-specific mechanics.
- Combat doctrine, ROE, target identification, weapon allocation, automation, and Player intervention.
- Combat-time resolution, simulation scheduling, battle logs, after-action analysis, and integration with the Narrative Battle System.
- Interactions with Sensors & Detection, Electronic Warfare, Command & Operations, Equipment & Materiel, Research & Production, Logistics, Terrain, Weather, Characters, and strategic systems.

This document does **not** redefine the authoritative mechanics for:

- Sensor detection, classification, identification, and track quality.
- Electronic warfare effects and emitter detection.
- Equipment design, component specifications, production, or physical inventory.
- Unit organisation, training, doctrine adoption, command latency, or operational planning.
- National economic production, transport networks, and logistics inventory.

Those systems provide inputs and receive consequences through explicit interfaces. This document governs the combat-resolution layer and the engagement decisions that connect them.

## 3. Foundational principles

1. **A target must be known sufficiently to be engaged by the chosen method.** Detection, classification, identification, tracking, targeting-quality data, and weapon guidance are different concepts.
2. **The world state and the combatant's knowledge are separate.** The simulation knows the actual state; each side acts on observations, reports, tracks, assessments, and orders available to it.
3. **No universal attack or defence score.** Outcomes emerge from the relevant weapon, target, sensors, geometry, range, environment, crew, condition, countermeasures, and tactics.
4. **Weapon employment is a workflow.** A weapon must be available, compatible, authorised, assigned, prepared, launched or fired, and able to reach and affect the target.
5. **A shot is not a hit, and a hit is not necessarily a kill.** Guidance, target motion, countermeasures, fuzing, impact geometry, armour, structure, damage control, and component placement affect the result.
6. **Misses and uncertainty have causes.** Resolution should be explainable through contributing factors, without requiring the UI to expose every internal calculation in real time.
7. **Doctrine and automation govern decisions, not physical truth.** A unit's policy may determine what it attempts; equipment and circumstances determine what is physically possible and what happens.
8. **Player control ranges from direct allocation to mission-level delegation.** Both are first-class modes, with explicit authority, ROE, and automation behaviour.
9. **Command latency matters.** Orders, target updates, abort instructions, support requests, and changes in ROE may arrive too late to affect an action already underway.
10. **Combat has physical and organisational consequences.** Ammunition, fuel, wear, fatigue, suppression, casualties, damage, morale, cohesion, and logistics evolve through the engagement.
11. **The same framework supports multiple domains without forcing them to behave identically.** A rifle engagement, tank duel, air intercept, submarine attack, naval missile exchange, and strategic strike share core concepts but require domain-specific mechanics.
12. **Historical technology is real.** Capabilities become available through the technology and equipment systems. Later sensors, weapons, datalinks, precision, and countermeasures must not appear in a 1955 force without an explicit scenario or technology basis.
13. **Combat is continuous in space and time.** The interface may use pauses, speed controls, planning overlays, and event logs, but must not depend on a conventional turn structure as the only way to resolve action.
14. **The simulation is authoritative; narrative is presentation.** Combat reports and narratives must be generated from actual events and evidence, never create independent casualties or outcomes.

## 4. Combat model overview

### 4.1 Engagement lifecycle

An engagement may pass through these conceptual stages:

1. **Observe:** sensors, reconnaissance, or other sources produce observations and contacts.
2. **Assess:** a force develops a classification, identification, track, confidence, and estimate of target state.
3. **Decide:** a Player, commander, mission AI, or local engagement controller determines whether to engage, continue tracking, hold fire, evade, or request authority.
4. **Authorise:** applicable command authority, ROE, mission rules, weapon-control status, and target restrictions are checked.
5. **Select:** the system identifies suitable weapons, mounts, launchers, fire-control channels, firing positions, and ammunition.
6. **Allocate:** the Player or authorised automation assigns a weapon or firing unit to the target or target area.
7. **Prepare:** the platform aligns, aims, obtains a firing solution, establishes illumination or datalink as required, loads, or otherwise prepares the weapon.
8. **Release:** the weapon is fired, launched, dropped, laid, or otherwise deployed.
9. **Transit:** the projectile or weapon travels, guided by its supported guidance method and available updates.
10. **Acquire or track:** a seeker or fire-control system may acquire, lose, reacquire, or fail to acquire the target.
11. **Terminal interaction:** the weapon approaches, is intercepted, evaded, decoyed, impacts, detonates, misses, or otherwise resolves its attack.
12. **Effects:** damage, casualties, suppression, fire, flooding, mission disruption, destruction, or no meaningful effect is applied.
13. **Assess:** observers and systems determine what happened, with confidence and reporting delays.
14. **React:** units may continue, re-engage, manoeuvre, defend, withdraw, report, or request support according to their orders and current information.

Not every weapon uses every stage. A direct-fire rifle round has a short chain; a long-range guided weapon may depend on targeting updates, navigation, seeker acquisition, electronic counter-countermeasures, and a lengthy flight.

### 4.2 State transitions, not scripted outcomes

An engagement should be represented as a sequence of state transitions and events. It must be possible for a target to move, become obscured, lose a component, change posture, receive support, or be destroyed while a weapon is in flight.

A weapon's outcome must be based on the state and information relevant at each stage, not simply a one-time attack-versus-defence roll at order issuance.

### 4.3 Simultaneity and mutual destruction

Combatants may act concurrently. An aircraft may release a weapon and then be destroyed; a ship may launch missiles shortly before sinking; infantry may fire while being suppressed; an attacking vehicle may be disabled after its shot has been committed.

The event scheduler must represent actions already physically committed even when the launching unit is subsequently damaged or destroyed, subject to the weapon's actual dependencies. A weapon requiring continued guidance or illumination may lose effectiveness if that support is lost.

## 5. Engagement entities and terminology

The following are conceptual entities; implementation may split or combine them.

- **Combatant:** a unit, platform, installation, formation, or person capable of participating in combat.
- **Contact:** a detected or reported object as represented by a side's Sensors & Detection system.
- **Track:** a maintained estimate of a contact's state, uncertainty, identity, and history.
- **Targeting Solution:** the quality and freshness of information required for a particular weapon or fire-control method.
- **Engagement:** a coordinated attempt to apply force against a target or target area.
- **Engagement Order:** the command or automated decision authorising an engagement within a defined scope.
- **Weapon System:** a weapon design and the launch, aiming, guidance, control, and support components required to employ it.
- **Weapon Instance:** a physical weapon or launcher, with its own condition, ammunition state, and availability.
- **Mount / Launcher:** the installed interface that carries, aims, launches, or fires a weapon.
- **Magazine:** the physical or logical storage and ready-use arrangement from which weapons or ammunition are drawn.
- **Fire-Control System:** equipment and processes used to calculate, refine, distribute, or maintain a firing solution.
- **Firing Solution:** the estimated conditions needed to deliver a weapon to a target or target area, including geometry, timing, and relevant target motion.
- **Projectile / Weapon Flight:** the physical or abstracted transit of a fired or launched munition.
- **Intercept:** an attempt by a defensive weapon or effect to prevent an incoming weapon from reaching its intended target.
- **Countermeasure:** an action, device, or effect intended to defeat or reduce the effectiveness of detection, tracking, guidance, or impact.
- **Damage Event:** a resolved physical or functional effect on a target or its components.
- **Damage State:** the current condition of a platform, structure, component, crew, or unit.
- **Combat Assessment:** an estimate of damage, destruction, mission effect, or target survival based on available evidence.
- **Battle Event:** a timestamped record of an action or state change used by logs, analysis, and narrative presentation.

Use these terms consistently across code, data, UI, and documentation. Avoid using “target” to mean both an actual world entity and an uncertain contact unless the context is explicit.

## 6. Engagement control and Player experience

### 6.1 Three complementary control modes

The Player must be able to engage in combat in ways inspired by both Command Ops 2 and CMO. The system should support three complementary modes:

**A. Operational command**

The Player sets objectives, missions, boundaries, priorities, ROE, posture, and desired effects. Subordinate commanders and mission systems decide how to engage within their authority. This mode is essential for commanding large forces without click-heavy micromanagement and draws on Command Ops 2's emphasis on command decisions, planning, order delay, and subordinate execution.

**B. Tactical direction**

The Player selects units or formations, assigns targets or target areas, sets engagement priorities, designates support, controls posture, and chooses how much initiative to permit. Units still require time, information, ammunition, suitable weapons, and valid authority.

**C. Direct engagement control**

The Player can inspect a platform's weapons, sensors, mounts, launchers, magazines, available ammunition, target track quality, weapon suitability, and likely constraints; then allocate weapons or firing units to a target or target area. This mode draws on CMO's detailed platform and weapon allocation experience.

These modes are not mutually exclusive. The Player can operate at operational level on one front while directly managing a critical engagement elsewhere. Orders and authority remain coherent when the Player changes control level.

### 6.2 Avoiding click fatigue

The interface must not require the Player to allocate every individual round or micromanage every low-value exchange. Provide orders, missions, engagement policies, salvos, firing plans, target priorities, fire discipline, and automated weapon allocation.

Manual control should remain available when it creates meaningful value, such as conserving scarce missiles, coordinating a salvo, engaging a high-value target, using an uncertain contact, selecting a firing arc, or preventing a subordinate from wasting weapons.

### 6.3 Player choice and informed automation

Automation should be explicit and inspectable. For a given unit or mission, the Player can choose whether the unit:

- Holds fire unless specifically ordered.
- Engages only positively identified hostile targets.
- Engages confirmed hostile targets within delegated authority.
- May engage targets of opportunity within mission constraints.
- Uses specified weapon categories or preserves them for higher-priority targets.
- Requires approval for selected target classes, weapon types, or escalation levels.
- Continues, aborts, or reassesses an attack when target quality changes.
- Uses manual, semi-automatic, or automatic weapon allocation.

Exact policy names may vary by historical doctrine and branch. The model should be expressive rather than limited to these labels.

### 6.4 Orders versus physical control

An engagement order is not a guarantee that a weapon will fire. The platform may lack a suitable weapon, have no ammunition, be unable to track the target, lack a required firing solution, be unable to turn or align in time, be outside effective range, have a damaged subsystem, or be prohibited from firing by ROE.

The interface must distinguish order accepted, engagement authorised, weapon allocated, firing solution ready, weapon released, and effect assessed.

### 6.5 Command Ops 2-inspired operational combat

Combat at formation level should emerge from the orders and decisions of subordinate units, not from the Player manually moving each unit every moment. Commanders assess threats, establish positions, request support, move within their orders, fire when appropriate, report results, and react to changing circumstances.

Command delay, staff workload, cohesion, fatigue, morale, training, doctrine, and communication state should affect how quickly and effectively forces act. These factors influence the decision and execution processes; they do not directly replace physical weapon and damage calculations.

### 6.6 CMO-inspired platform combat

At platform level, the Player should be able to inspect actual installed systems and manage missions, targets, weapon allocations, fire-control constraints, emissions, magazines, and rules of engagement. The platform should not gain a capability merely because its broad unit type suggests it.

The design must support mixed-era equipment, platform-specific sensor and weapon fits, different magazines and launchers, embarked units, formation support, and weapon-specific guidance dependencies.

## 7. Target knowledge and targeting quality

### 7.1 Sensors own contact truth as known by a side

The Sensors & Detection system owns detection, classification, identification, tracking, uncertainty, contact age, sensor provenance, and information sharing. Combat consumes those records and must not secretly substitute the actual world target position for a stale or uncertain track.

### 7.2 Targeting quality is weapon-dependent

A contact may be adequate for one weapon and inadequate for another. Examples:

- A broad area bombardment may only require an approximate target area.
- Direct-fire weapons may require line of sight and a sufficiently precise target position.
- A semi-active guided weapon may require compatible illumination or support through part of its flight.
- An active radar seeker may need a suitable initial target solution and then acquire the target independently.
- A command-guided weapon may require a valid communications link and continuous tracking.
- A torpedo may require a search area or launch solution and then rely on its own seeker.
- A long-range strike weapon may accept pre-planned coordinates but have limited ability to adapt to moving targets.

The weapon definition must declare its required targeting inputs and acceptable uncertainty. The combat system evaluates the available targeting solution against those requirements.

### 7.3 Contact uncertainty and stale data

A track includes uncertainty and age. As time passes, target motion and missing observations can expand uncertainty. Updates, sensor fusion, or new observations may reduce it. The exact model belongs to Sensors & Detection.

The combat system should evaluate whether the uncertainty is compatible with the weapon's guidance, acquisition area, seeker behaviour, and intended effect. It must not silently collapse uncertainty to the target's true location.

### 7.4 Identification and target selection

Classification and identification are separate from track precision. A force may know a precise position but not whether the object is hostile; it may identify a hostile force but have only an approximate location.

ROE and mission policy determine what degree of identification is required. Weapon guidance and physical delivery determine what degree of positional information is required. These are separate checks.

### 7.5 Target areas and coordinate attacks

The system must support engagements against a defined area or coordinate when the weapon and doctrine permit. Area attacks may affect multiple entities, empty ground, decoys, or unintended objects. Coordinate attacks against moving targets may fail if the target has moved or the weapon cannot update its aimpoint.

The Player must be shown when an attack is against an identified entity, an uncertain contact, an estimated position, or a coordinate/area rather than a continuously tracked target.

## 8. Weapon definitions and physical employment

### 8.1 Weapon designs are authoritative

A weapon's performance derives from its researched and designed definition, including its propulsion, mass, dimensions, seeker, guidance, warhead, fuze, range, launch constraints, flight profile, countermeasure susceptibility, reliability, and other applicable parameters.

The combat system should not maintain a separate hand-authored combat value that contradicts the equipment design. Derived values may be cached for performance but must be traceable to their source parameters and version.

### 8.2 Weapon categories

The data-driven model should support, where applicable:

- Small arms, machine guns, automatic cannons, and direct-fire infantry weapons.
- Tank and anti-tank guns, recoilless weapons, guided anti-armour missiles, and anti-tank mines.
- Mortars, howitzers, field guns, naval guns, rockets, and guided artillery munitions.
- Unguided bombs, guided bombs, glide weapons, and air-to-ground missiles.
- Air-to-air guns and missiles.
- Surface-to-air and missile-defence systems.
- Anti-ship missiles, naval guns, torpedoes, depth charges, and anti-submarine weapons.
- Ballistic, cruise, and other strategic weapons where enabled by technology and scenario.
- Mines, demolition charges, obstacles, incendiaries, and other area or infrastructure effects.
- Non-kinetic effects that belong to electronic warfare, cyber, psychological, or other dedicated systems, interfacing with combat only where an explicit physical or operational effect is defined.

This list is extensible and should not imply every weapon category is available in every era.

### 8.3 Launchers, mounts, and magazines

A weapon must be compatible with the actual launcher, mount, hardpoint, tube, gun, or release mechanism. The platform must possess the required magazine, ammunition, power, cooling, fire-control, and support systems.

The system should distinguish:
- The weapon design and its variants.
- The launcher or mount design.
- The platform's installed configuration.
- The magazine and available rounds.
- Ready-use weapons versus stored weapons.
- Weapon instance condition and serviceability.
- Loading, reloading, rearming, and maintenance time.

A ship with missiles in storage may not be able to fire them if the relevant launcher is damaged, unavailable, incompatible, or unable to obtain a solution. An aircraft may carry only approved stores and loadouts within its hardpoint, mass, drag, clearance, and mission constraints.

### 8.4 Weapon availability and preparation

Before firing, the system evaluates weapon availability, serviceability, ammunition state, crew, platform state, launch geometry, target suitability, and relevant preparation time. Some weapons are ready immediately; others require loading, alignment, warm-up, seeker preparation, target illumination, or crew actions.

Preparation and reload durations should derive from equipment and operational state. A unit should not fire faster than its actual mount, crew, magazine, and weapon system can support.

### 8.5 Range and engagement envelopes

Weapons have valid and effective employment envelopes, not one universally meaningful range. Depending on weapon type, relevant limits may include minimum range, maximum kinematic range, effective range, altitude, depth, bearing, aspect, target speed, target altitude, launch speed, turn limits, seeker acquisition limits, and fuze constraints.

The UI should distinguish a theoretical envelope from a favourable engagement region and from the range at which a specific target and situation are realistically engageable. Range is not a guarantee of probability of hit.

### 8.6 Rate of fire and salvo behaviour

Guns, automatic weapons, launchers, and missile systems have rates of fire, burst sizes, reload cycles, cooling limits, and ammunition constraints. A salvo is a coordinated group of releases whose timing and spacing can affect saturation, target channel capacity, interception opportunities, and resource expenditure.

The system should support burst, single shot, salvo, ripple, scheduled fire, and area fire where appropriate. These are not all applicable to every weapon.

## 9. Fire control and firing solutions

### 9.1 Fire control as a system

A firing solution depends on the weapon, target state, relative geometry, platform motion, sensor or rangefinder inputs, fire-control equipment, crew skill, environmental conditions, and time available. Different eras may rely on visual estimation, optical sights, rangefinders, mechanical computers, radar directors, sonar, inertial navigation, satellite navigation, datalinks, or advanced sensor fusion.

### 9.2 Solution quality

A solution may be unavailable, preliminary, valid, degraded, stale, or lost. The exact states should be data-driven and suited to the weapon type.

Solution quality may depend on:
- Target position, velocity, heading, and uncertainty.
- Own-platform position, velocity, heading, and stability.
- Range, bearing, elevation, and aspect.
- Sensor and fire-control accuracy.
- Track continuity and target manoeuvre.
- Weather, visibility, sea state, terrain, and atmospheric effects.
- Crew proficiency and equipment condition.
- Guidance support and communication links.
- Time between measurement, calculation, and weapon release.

### 9.3 Direct fire

Direct-fire engagements require a valid line of fire or other explicitly modelled firing path, suitable range and geometry, and a weapon able to affect the target. Line of sight is not equivalent to line of fire: intervening terrain, structures, friendly units, obstructions, and weapon trajectory can prevent a shot or alter its effect.

Direct-fire accuracy may be affected by sight quality, stabilisation, range estimation, target movement, shooter movement, suppression, fatigue, crew training, weapon condition, and visibility.

### 9.4 Indirect fire

Indirect-fire systems use firing data and a ballistic or guided trajectory to deliver rounds to a target area. Relevant factors include weapon and ammunition type, range, elevation, charge, weather, meteorological data, barrel condition, fire-control quality, survey accuracy, observer corrections, and target-location error.

Indirect fire may be observed, unobserved, pre-planned, counter-battery, suppression, interdiction, or emergency fire. Observed fire may be corrected over time; unobserved fire relies more heavily on available coordinates and estimates.

The system should model dispersion and area effects without implying every round lands at the aimpoint. Ammunition type and fuze behaviour determine whether effects are fragmentation, blast, penetration, smoke, illumination, incendiary, or another supported effect.

### 9.5 Naval and air fire control

Naval gunfire must account for platform motion, range, target motion, fire-control directors, radar or optical inputs, salvo spotting, and the relevant historical fire-control capability.

Air-delivered weapons must account for aircraft state, speed, altitude, attitude, release envelope, navigation, aiming system, weapon type, and target movement. A guided weapon may correct some release errors but is still constrained by its design and guidance conditions.

### 9.6 Fire-control degradation

Damage, jamming, loss of sensors, loss of datalink, smoke, obscuration, shock, power failure, and crew casualties can degrade or disable a firing solution. Effects should be applied to the actual affected systems rather than automatically disabling all weapons on a platform.

## 10. Attack resolution and probability

### 10.1 No single universal hit formula

There should be no single universal attack-versus-defence formula applied identically to a rifle, a tank shell, a guided missile, a torpedo, and a strategic weapon. Each weapon family has different physical mechanisms and information dependencies.

A shared resolution framework may orchestrate common stages, but detailed models must be selected according to weapon type and available data. Implementation should prefer transparent, testable models with clearly defined inputs over one opaque combat score.

### 10.2 Conceptual factors

Depending on the engagement, relevant factors may include:

- Weapon type, design version, condition, reliability, and performance envelope.
- Launch platform and mount condition.
- Target type, size, signature, speed, aspect, manoeuvre, protection, and state.
- Track accuracy, target age, fire-control solution, and guidance support.
- Range, relative geometry, launch conditions, and time of flight.
- Terrain, line of sight, sea state, weather, visibility, and atmospheric conditions.
- Crew proficiency, training, fatigue, suppression, morale, and unit experience where relevant.
- Countermeasures, decoys, electronic interference, evasive manoeuvre, and active defence.
- Weapon reliability, seeker acquisition, fuze behaviour, and terminal geometry.
- Target component layout, armour, structural integrity, redundancy, and damage-control capability.

Not every factor applies to every weapon. The implementation must avoid adding generic modifiers merely because they exist elsewhere in the simulation.

### 10.3 Probabilistic resolution

Combat involves uncertainty, including unresolved physical variation and incomplete knowledge. Where stochastic resolution is used, probabilities should be tied to defensible physical or empirical models and calibrated data. Randomness should represent uncertainty, not compensate for poorly specified mechanics.

Use a reproducible random stream or equivalent deterministic event record so an engagement can be investigated and replayed from the same initial state, orders, and random seed. Do not reroll an already resolved event merely because the Player reloads a UI panel or changes simulation speed.

### 10.4 Salvos and repeated attacks

Repeated attacks are not necessarily independent. A second shot may benefit from corrected range estimates, or suffer from obscuration, target movement, loss of track, ammunition constraints, or a changed firing solution. A salvo may saturate defensive channels or create opportunities for mutual interference, but should not receive a universal saturation bonus.

### 10.5 Misses and near misses

A miss may still have effects if the weapon has an area effect, proximity fuze, fragmentation radius, blast, suppression, or environmental interaction. Conversely, a near miss may have no meaningful effect on a hardened or protected target. The damage model must consume the actual miss distance, geometry, fuze, and warhead effects where those values are available.

## 11. Guidance, seekers, and weapon transit

### 11.1 Guidance families

The system should support different guidance families, including:
- Unguided ballistic or aerodynamic delivery.
- Command guidance.
- Beam-riding or other line-of-sight guidance.
- Semi-active homing requiring an external illuminator or designator.
- Active homing.
- Passive homing, including supported radar, infrared, optical, acoustic, or electromagnetic methods.
- Inertial navigation and pre-programmed routes.
- Satellite-assisted or other external navigation when available in the era and scenario.
- Terrain- or scene-referenced guidance where supported.
- Hybrid guidance with multiple phases or hand-offs.
- Wire-guided, tethered, or other guidance links where applicable.

Each weapon definition declares its guidance phases, required support, update options, acquisition conditions, limitations, and likely failure modes.

### 11.2 Weapon transit

A weapon in flight or transit may change state due to movement, manoeuvre, fuel, propulsion, navigation error, guidance, environmental effects, countermeasures, or loss of support. Travel time matters because the target and environment can change before arrival.

A weapon should not instantly resolve against a distant target unless the scenario uses an explicitly abstracted resolution mode. Even when a detailed physical trajectory is not simulated, time of flight and intermediate state changes should remain meaningful.

### 11.3 Seeker acquisition and reacquisition

A seeker may have to search a volume or area, acquire a target, reject clutter or decoys, maintain lock, or reacquire after losing it. Acquisition depends on seeker capability, target signature, geometry, background, weather, countermeasures, and the quality of initial cueing.

The weapon must not gain access to target truth merely because its target entity was selected by the Player. Its seeker and guidance system operate according to their own design and available inputs.

### 11.4 Guidance updates and loss of support

Some weapons accept mid-course updates; others cannot. Some require continuous illumination or tracking; others can continue autonomously after launch. If an external support platform is destroyed, jammed, forced to manoeuvre, or loses the target, the effect depends on the weapon's guidance design.

### 11.5 Abort, self-destruct, and safety states

Where supported, a weapon may be aborted, diverted, commanded to self-destruct, or rendered safe. These are not universally available and may fail or arrive too late. An abort order cannot retroactively stop a weapon that has already impacted.

## 12. Interception, evasion, and countermeasures

### 12.1 Active defence

Targets may defend against incoming attacks through manoeuvre, cover, concealment, armour, decoys, electronic countermeasures, point-defence weapons, interceptor aircraft, air-defence systems, anti-torpedo measures, smoke, or other capabilities available to them.

Defensive action consumes time, ammunition, energy, fuel, attention, and sometimes mission effectiveness. A platform cannot use every defensive system at full effectiveness simultaneously unless its actual configuration and doctrine support it.

### 12.2 Interception as an engagement

An interceptor or point-defence weapon has its own detection, tracking, targeting, launch, transit, and terminal stages. It must have sufficient warning, a valid solution, compatible geometry, available ammunition, and a weapon capable of reaching the incoming threat.

An interception attempt may succeed, fail, arrive too late, or damage rather than destroy the incoming weapon. A weapon that is damaged may still continue if its design and physical state permit it.

### 12.3 Electronic warfare

EW effects are provided by the Electronic Warfare System. Combat consumes effects such as degraded track quality, seeker disruption, communications loss, deception, decoy response, or altered probability of acquisition where explicitly represented.

Area jamming, self-protection countermeasures, deception, emission detection, and physical interception remain distinct mechanisms. EW should not be modelled as a generic percentage reduction to all enemy weapons.

### 12.4 Decoys and false targets

Decoys may create, reinforce, or redirect detections and target selections if the relevant sensor and weapon systems can be deceived. Decoy performance depends on its signature, timing, geometry, environment, and the opposing system's discrimination capability.

A weapon may select a decoy, reject it, or be confused without necessarily losing every other target. Decoy effects must be based on sensor/seeker interactions and available evidence.

### 12.5 Evasion and manoeuvre

Evasion can change target geometry, reduce weapon energy, defeat a firing solution, move behind cover, or increase the chance of a miss. It may also expose the target, reduce its ability to perform its mission, consume fuel, break formation, or interfere with its own weapon employment.

The AI should evaluate evasion according to threat, platform capability, mission, doctrine, and command authority. It should not evade every possible threat without cost or awareness.

## 13. Damage, penetration, and physical effects

### 13.1 Damage is not one hit-point bar

Platforms and installations should have physical structures and functional components appropriate to their type and simulation scale. Damage may affect crew, mobility, sensors, weapons, power, communications, fire control, propulsion, protection, cargo, magazines, fuel systems, flight control, hull integrity, buoyancy, or other subsystems.

The level of internal detail may vary by entity class. The architecture should permit component-level damage for complex platforms while using appropriate aggregate models for small units, mass formations, or low-value entities.

### 13.2 Warhead and impact mechanisms

Effects depend on the weapon's actual mechanism, including kinetic penetration, fragmentation, blast, shaped charge, incendiary, thermal effects, underwater shock, flooding, fragmentation, proximity detonation, or other supported physics.

The weapon definition supplies relevant properties; target design supplies material, protection, layout, compartmentation, and vulnerability data. Avoid assigning the same damage behaviour to all weapons of a category.

### 13.3 Penetration and armour

Where applicable, penetration depends on the projectile or warhead, impact velocity, angle, armour type and thickness, material properties, spacing, obliquity, and target geometry. Armour can be penetrated, defeated, spalled, damaged, or left intact while external components are damaged.

Do not treat armour as a universal percentage reduction independent of attack direction and weapon mechanism. Where a full physical model is impractical, use a documented approximation calibrated against relevant data.

### 13.4 Component placement and hit location

The impact point and target's internal or external layout affect which components are damaged. A hit to a tank's track, turret, optics, ammunition storage, or crew compartment can produce different outcomes. A ship's propulsion, fire-control, magazine, compartments, and superstructure have different operational consequences. An aircraft's engine, control surfaces, sensors, fuel system, or structure can fail in different ways.

Where exact placement is not modelled, use a platform-specific or class-specific distribution based on known layout and attack geometry. Do not distribute damage uniformly across unrelated components.

### 13.5 Crew and personnel effects

Personnel may be killed, wounded, suppressed, incapacitated, evacuated, or otherwise unavailable. Crew losses can degrade specific functions or force abandonment if minimum crew requirements are no longer met. The effect depends on crew role, redundancy, automation, training, protection, and damage location.

Personnel outcomes should feed the Personnel, Unit Condition, Medical, and Organisation systems. Avoid double-counting the same casualty in both component damage and unit-level casualty totals.

### 13.6 Fires, flooding, and secondary damage

Fire, flooding, explosions, structural failure, ammunition cook-off, toxic exposure, and other secondary effects may evolve after the initial hit. Damage-control actions, firefighting equipment, compartmentation, crew availability, training, and external support affect the outcome.

Secondary damage is time-dependent. A platform may remain operational immediately after a hit but deteriorate later; a prompt damage-control response may contain the effect. These mechanics should be supported where meaningful for the platform and scale.

### 13.7 Damage propagation

Damage to a component can affect connected systems. Loss of electrical power may disable sensors and weapons; propulsion damage may reduce speed or manoeuvrability; fire-control damage may limit engagements; a destroyed track may immobilise a vehicle; flooding may impair stability and eventually cause sinking.

Propagation must follow explicit dependencies in equipment designs or platform configurations. Do not use arbitrary cascades that destroy unrelated systems without a causal path.

### 13.8 Mission kill, mobility kill, and destruction

Distinguish at least:
- **No significant damage:** the target remains operational within normal or near-normal capability.
- **Degraded:** one or more functions are impaired.
- **Mission kill:** the target can no longer perform the relevant mission, though it may survive and retain other functions.
- **Mobility kill:** the target cannot move under its own power or cannot move as required.
- **Firepower kill:** its relevant offensive capability is disabled.
- **Sensor or command kill:** its relevant sensing, coordination, or command function is disabled.
- **Incapacitated:** the entity cannot continue effective action for a defined reason.
- **Abandoned:** personnel have left or ceased operating the platform.
- **Destroyed:** the entity is no longer a functioning combatant under the model's destruction criteria.
- **Sunk / crashed / catastrophically lost:** domain-specific terminal outcomes where appropriate.
- **Captured or recovered:** ownership or control changes without necessarily implying destruction.

A single entity can have multiple simultaneous damage states. A mission kill is contextual: a damaged aircraft unable to intercept may still be able to return to base; a ship that cannot launch aircraft may remain capable of escort or gunfire.

### 13.9 Repair, recovery, and reconstitution

Combat outcomes connect to Equipment & Materiel, Logistics, and Unit Organisation. Repair may restore some functions over time if personnel, parts, facilities, and safety conditions permit. Recovery can tow, evacuate, salvage, or retrieve damaged equipment. Some damage is irreparable; some platforms can be cannibalised or returned to service after substantial overhaul.

Destroyed equipment must be removed from operational availability and inventory according to the authoritative equipment model. A damaged item must not be counted as fully serviceable until repairs are completed.

## 14. Ground combat

### 14.1 Scale and representation

Ground combat must support individual platforms, crew-served weapons, infantry elements, armoured vehicles, artillery, air defence, engineers, headquarters, installations, and formations. The simulation should select an appropriate level of detail for each entity and situation.

Command Ops 2 provides the intended reference for operational ground warfare: continuous movement, command hierarchy, order delay, tactical execution by subordinate units, terrain-aware movement, cohesion, fatigue, and operational decisions. CMO provides a complementary reference for detailed weapon and platform capability where individual-system detail is appropriate.

### 14.2 Infantry fire

Infantry fire depends on weapon type, range, line of sight, target exposure, posture, movement, suppression, training, ammunition, visibility, cover, and firing conditions. Individual shots need not always be simulated as discrete entities, but the aggregate model must preserve rate of fire, ammunition use, dispersion, hit likelihood, and effects.

Fire volume, accuracy, suppression, and casualty production are related but distinct. A unit can be pinned down by fire without suffering proportionate casualties, and a brief exposure to effective fire can produce severe casualties.

### 14.3 Small arms and crew-served weapons

Weapon characteristics include effective range, rate of fire, burst or sustained-fire limits, ammunition type, penetration, crew requirements, setup time, and portability. Machine guns, mortars, anti-tank weapons, recoilless rifles, and other crew-served weapons may require emplacement, observation, ammunition handling, and a crew able to operate them.

### 14.4 Armoured combat

Armoured engagements depend on target and weapon geometry, armour layout, ammunition type, optics, fire-control systems, crew training, stabilisation, range, terrain, concealment, movement, and firing conditions.

The system should distinguish a vehicle's ability to detect a target, obtain a firing solution, fire, hit, penetrate, damage components, and remain operational. A technologically superior vehicle may have advantages without being invulnerable; tactical position, visibility, ammunition, reliability, and crew performance remain important.

### 14.5 Suppression, morale, and cohesion

Suppression represents temporary disruption caused by threat or incoming fire. Morale represents willingness and confidence; cohesion represents the ability to remain organised and coordinate. Fatigue and casualties affect the unit over longer periods.

These states interact but are not interchangeable. Suppression may cause personnel to take cover or reduce effective fire temporarily; severe casualties and fatigue can damage cohesion; morale may influence whether a unit holds, withdraws, or routs under pressure. These effects are owned by the Unit Condition model and should not be duplicated as a second combat-only morale system.

### 14.6 Cover, concealment, and fortification

Cover reduces exposure or the effects of incoming attacks; concealment reduces observation and targeting quality. They are distinct and depend on weapon effects, sensor methods, terrain, building construction, posture, and geometry.

Prepared positions, trenches, bunkers, fighting positions, obstacles, minefields, and fortifications require construction time, material, engineering capability, and location-specific effects. They should not grant a blanket invulnerability bonus.

### 14.7 Indirect fire and suppression missions

Indirect fire may be used to destroy targets, suppress positions, disrupt movement, interdict routes, screen areas, illuminate, or create smoke. Effects depend on ammunition, fuze, accuracy, target protection, terrain, warning, and duration.

A suppression mission may be successful without destroying targets. The system should distinguish fire delivered, area covered, target exposure, suppression achieved, casualties, and physical destruction.

### 14.8 Urban and close terrain

Urban combat must account for buildings, streets, interior positions where modelled, line-of-sight breaks, short engagement distances, concealment, verticality, structural damage, civilian presence, and restricted movement. A building is not simply a generic terrain multiplier; its construction, damage state, access, and occupants affect the result.

The level of building-interior detail may vary by scale and scenario, but the abstraction must preserve the major tactical consequences without making all urban terrain identical.

### 14.9 Retreat, surrender, and capture

Units may withdraw, rout, surrender, become isolated, or be captured based on command orders, morale, cohesion, casualties, supply, encirclement, available routes, leadership, and perceived prospects. Surrender should not be triggered by a single universal casualty threshold.

Capture changes control, personnel status, equipment ownership, and possibly intelligence availability. Treatment of prisoners and compliance with relevant rules belongs to the personnel, political, and ROE systems.

## 15. Air combat

### 15.1 Aircraft as systems

Aircraft combat depends on airframe performance, engines, fuel, altitude, speed, acceleration, manoeuvrability, payload, sensors, communications, weapons, crew, and damage state. Aircraft designs and installed components are authoritative; combat must use actual capabilities rather than generic aircraft classes.

### 15.2 Detection and interception

Air combat begins with detection and identification, followed by tasking, vectoring, intercept geometry, weapon selection, firing solution, release, and assessment. A force may detect an aircraft without being able to intercept it in time, and may intercept without obtaining a valid weapon solution.

Interception depends on readiness, launch delay, airbase or carrier availability, climb and transit time, fuel, command latency, communications, weather, and the target's route and speed.

### 15.3 Beyond-visual-range and within-visual-range combat

Different combat regimes have different requirements. Beyond-visual-range engagements depend heavily on sensors, target information, weapon envelope, guidance, datalinks, electronic warfare, and identification policy. Within-visual-range engagements depend on visual/infrared sensing, geometry, manoeuvre, energy state, weapon seeker limits, and pilot decisions.

The model should not make one regime universally superior. The outcome depends on era, platform, weapons, sensors, rules of engagement, geometry, training, and countermeasures.

### 15.4 Aircraft weapons

Aircraft may employ guns, unguided rockets, bombs, guided bombs, air-to-air missiles, air-to-ground missiles, anti-ship weapons, torpedoes, and other stores supported by their design and era. Release envelopes, loadouts, hardpoints, drag, weight, and weapon compatibility are defined by equipment data.

### 15.5 Close air support and strike

Close air support requires target location, friendly-force awareness, coordination, identification, deconfliction, and compatible weapons. Strike missions may target infrastructure, installations, ships, mobile forces, or other objectives, depending on weapon and intelligence quality.

A planned strike does not guarantee the target is present at the time of attack. Target movement, stale intelligence, weather, air defence, mission aborts, and weapon error may alter the outcome.

### 15.6 Air defence and interception

Ground-based, shipborne, and airborne air defence systems use their actual sensors, fire-control channels, launchers, weapon inventories, engagement envelopes, and doctrine. Multiple defensive systems may coordinate or compete for the same target. Fire-control channel capacity, reload time, geometry, and ammunition availability can constrain simultaneous engagements.

### 15.7 Sorties and turnaround

Aircraft availability is constrained by crew readiness, maintenance, damage, fuel, weapons loading, inspection, base capacity, runway or deck operations, and turnaround time. A returning aircraft is not instantly ready for another sortie.

Sortie generation and logistics remain operational constraints rather than a simple counter that resets automatically after each mission.

## 16. Naval surface combat

### 16.1 Surface platforms

Naval engagements use ship and boat designs, installed sensors, fire-control systems, guns, missiles, torpedoes where applicable, decoys, electronic warfare, propulsion, damage-control systems, magazines, and crew. Platform speed, manoeuvrability, sea state, formation, and operating condition affect engagements.

### 16.2 Surface detection and targeting

A vessel may be detected without being classified, identified, or tracked accurately enough for a specific weapon. Radar horizon, line of sight, sea clutter, weather, emissions, target signature, reconnaissance, and external cueing are handled by the appropriate sensor systems.

### 16.3 Naval gunfire

Naval gunfire depends on mount arcs, rate of fire, fire-control, range, platform motion, ammunition, target motion, and observation or correction. Shore bombardment and naval surface engagements may use different target-location and damage models.

### 16.4 Anti-ship missiles

Anti-ship engagements may involve external targeting, launch platforms, weapon transit, mid-course guidance, seeker acquisition, target discrimination, electronic warfare, decoys, terminal manoeuvres, and defensive interceptors. The weapon must use the actual target information and guidance capabilities available to it.

### 16.5 Formation and task-force combat

Task forces coordinate escorts, sensors, air defence, anti-submarine warfare, strike assets, emissions policy, and weapon allocation. Formation membership does not automatically share every sensor track or grant every platform access to every weapon; information sharing and command relationships must permit it.

### 16.6 Damage control and sinking

Ship damage may cause flooding, fire, power loss, propulsion loss, magazine danger, reduced stability, loss of weapons, and progressive deterioration. Damage control depends on compartmentation, pumps, firefighting, crew, training, damage location, and available support.

A ship may be mission-killed without sinking, or continue to fight after serious damage. Sinking should follow the physical and damage state, not a single hit-point threshold alone.

## 17. Subsurface and anti-submarine warfare

### 17.1 Submarine operations

Subsurface engagements depend on acoustic and other relevant sensors, target classification, track uncertainty, depth, speed, noise, water conditions, terrain, thermal structure where modelled, and the platform's own signatures and equipment.

A submarine may detect a contact without knowing its exact identity or position. Acoustic tracking and target motion analysis may require time and manoeuvre. The combat system must not provide exact target truth because a submarine has an active engagement order.

### 17.2 Torpedoes and underwater weapons

Torpedoes and other underwater weapons have propulsion, speed, endurance, depth limits, seeker characteristics, search patterns, guidance, warhead, and countermeasure susceptibility. Launch solutions may be uncertain, and target motion during weapon transit can be decisive.

### 17.3 Anti-submarine warfare

ASW integrates sensors, patrol aircraft, helicopters, ships, sonobuoys, dipping sonar, active and passive methods, weapons, and coordination. Detection, classification, localisation, prosecution, attack, and post-attack assessment are distinct stages.

An ASW attack may disrupt or force a submarine to manoeuvre without destroying it. Search, reacquisition, and prosecution may consume significant time and resources.

### 17.4 Mines and mine countermeasures

Mine warfare includes mine laying, detection, classification, avoidance, clearance, sweeping, hunting, and the effects of mines on ships, submarines, landing craft, and infrastructure. Mines may remain dangerous after the laying unit has left the area.

Mine effects depend on mine design, target signature, trigger mechanism, depth, placement, countermeasures, and the affected vessel's structure. Minefields are not merely invisible damage zones with uniform effects.

## 18. Amphibious, airborne, and cross-domain engagements

### 18.1 Amphibious operations

Amphibious operations combine sea movement, landing craft, shore access, naval and air support, engineering, reconnaissance, beach or port conditions, and sustainment ashore. Landing forces may be vulnerable during embarkation, transit, landing, and initial consolidation.

Combat resolution must account for the capabilities and constraints of actual landing craft, ships, aircraft, and units. Landing on a map location does not guarantee the force can unload, assemble, or sustain itself there.

### 18.2 Airborne and air assault

Airborne operations require suitable aircraft, lift capacity, trained personnel, drop or landing conditions, navigation, weather, air defence risk, and follow-on support. Units may be dispersed, delayed, injured, lose equipment, or land outside the intended area.

A successful insertion is not the same as a successful mission. Isolated forces may need to secure objectives, establish communications, link up with other forces, or be resupplied before their mission can be sustained.

### 18.3 Cross-domain support

Air, land, naval, space, and other domain capabilities may support one another where technology and scenario rules permit. The system must represent support requests, coordination, timing, deconfliction, command authority, target data quality, and competing demands.

A support asset is not instantly available just because it is in range. It may be tasked elsewhere, lack a suitable weapon or sensor, require transit or preparation time, be constrained by ROE, or be unable to identify the target.

## 19. Strategic weapons and high-consequence effects

### 19.1 Scope

Strategic weapons and other high-consequence effects must use the same general principles of real equipment, targeting information, command authority, delivery constraints, and post-event consequences, while employing domain-appropriate resolution models.

The specification must support conventional long-range strike and, if enabled by scenario and policy, nuclear, chemical, biological, or other strategic weapons. The existence of a category in the data model does not imply universal availability or unrestricted use.

### 19.2 Authorisation and command integrity

Strategic weapon release may require special command chains, authentication, political authority, multiple-person procedures, or other scenario-specific controls. These requirements are separate from technical launch capability.

The system must distinguish a weapon that is physically available from a weapon that is authorised for use. Lost communications, failed authentication, damaged command systems, or conflicting orders may prevent release or create uncertainty according to the defined command model.

### 19.3 Effects and consequences

High-consequence weapons may produce blast, thermal, radiation, contamination, infrastructure destruction, casualties, fires, persistent environmental effects, escalation, retaliation, diplomatic consequences, and long-term economic damage where relevant to the game's scope.

The model should use explicit effect categories and world-system interfaces. It must not reduce such events to a generic large damage number when persistent environmental, political, or strategic consequences are part of the simulation.

## 20. Combat AI and tactical behaviour

### 20.1 Decision hierarchy

Combat decisions can be made by the Player, headquarters, formation commander, unit commander, platform mission controller, or local weapon system. Each has defined authority and information. The system should not let lower-level automation silently override higher-level orders or ROE.

### 20.2 Tactical decision inputs

AI decisions may consider:
- Current mission and commander's intent.
- Known targets, track quality, threat assessments, and uncertainty.
- Available weapons, ammunition, fire-control channels, and support.
- ROE, target restrictions, weapon allocation policies, and escalation.
- Range, geometry, terrain, environment, and target movement.
- Own platform condition, crew, readiness, fuel, and escape options.
- Threat priority, mission priority, time sensitivity, and opportunity cost.
- Communications, command latency, and whether approval can be obtained in time.
- Formation, coordination, friendly-force locations, and fratricide risk.

### 20.3 Weapon allocation

Automated allocation should select suitable weapons according to the weapon's design, target class, range, targeting solution, expected effect, availability, doctrine, mission policy, and resource priorities.

The AI should avoid wasteful overkill where a smaller engagement is likely sufficient, but must also account for uncertainty, target resilience, salvo coordination, and the cost of failure. It should not always choose the weapon with the highest nominal performance if that weapon is scarce or needed for a more important threat.

### 20.4 Target priority and opportunity

Target priorities may include mission-critical targets, imminent threats, high-value assets, weapons platforms, logistics nodes, air-defence systems, command facilities, or other scenario-defined objectives. Target priority is not identical to weapon suitability or ROE permission.

Engaging a target of opportunity must be governed by explicit mission policy. A unit may be ordered to hold fire, preserve ammunition, remain concealed, avoid revealing its position, or focus on a designated target set.

### 20.5 Local autonomy and command friction

When communication is delayed or lost, a unit acts under its standing orders, delegated authority, local information, doctrine, and commander's judgement. It may continue an attack, defend itself, disengage, or hold position depending on those instructions.

The AI must not use global hidden information unavailable to the local commander. It should be possible for subordinate decisions to be reasonable given their information yet poor given the full world state.

### 20.6 Explainable decisions

For significant actions, the simulation should be able to record a concise rationale: target quality was sufficient, a weapon was available, ROE permitted the engagement, a threat was imminent, a commander chose to preserve a scarce weapon, or an order had not yet arrived.

The Player need not see all calculations continuously, but should be able to inspect why an engagement occurred, did not occur, or used a particular weapon.

## 21. Friendly fire, fratricide, and non-combatants

### 21.1 Identification is fallible

Friendly, neutral, civilian, unknown, and hostile status should remain distinct from sensor classification and target position. Identification can be incorrect, stale, or unavailable. Friendly-fire risk depends on actual information, procedures, weapon effects, and geometry.

### 21.2 Deconfliction and safety constraints

Orders may establish no-fire areas, restricted-fire areas, protected sites, weapons-control status, altitude or route separation, and other control measures. These constraints should be explicit, time-bounded where needed, and checked at the appropriate stage of engagement.

### 21.3 Area effects and collateral effects

Area weapons, fragmentation, blast, fires, infrastructure collapse, and other secondary effects may affect nearby entities beyond the intended target. Effects depend on the physical mechanism, terrain, structures, distance, protection, and actual target locations.

The simulation should record incidental effects and provide evidence for later assessment. It must not treat civilian harm as a generic fixed penalty unrelated to the weapon and circumstances.

### 21.4 Orders, compliance, and accountability

A unit may refuse, question, delay, or violate an order according to authority, command relationships, training, discipline, morale, culture, and circumstances. These behaviours connect to the Command & Operations and character systems.

The combat system records the action and its effects; legal assessment, discipline, political response, and strategic consequences are resolved through the relevant systems. Personality traits must not deterministically cause unlawful conduct.

## 22. Ammunition, wear, and combat sustainability

### 22.1 Ammunition expenditure

Every resolved shot, launch, burst, or munition deployment must consume the appropriate ammunition from the authoritative inventory. A weapon cannot fire unavailable ammunition. Some systems may have ready-use ammunition distinct from reserve storage and may require reload or transfer time.

### 22.2 Fire discipline and expenditure policy

The Player and commanders should be able to set fire discipline and expenditure policies appropriate to weapon type, such as single shot, controlled burst, sustained fire, salvo size, missile preservation, or emergency expenditure. Policies affect availability and timing, not the physical truth of the shot once fired.

### 22.3 Wear and reliability

Combat activity may increase barrel wear, engine wear, launch-system wear, thermal stress, maintenance demand, or failure risk. The Equipment & Materiel and Logistics systems own condition and repair records; combat sends usage and damage events to them.

Reliability must derive from equipment design, condition, operating environment, maintenance, and known failure models. Avoid random failures that have no connection to equipment or operating conditions.

### 22.4 Fatigue and crew effectiveness

Combat activity, alertness demands, movement, noise, heat, stress, and prolonged operations can affect crew and unit condition. The Unit Condition system owns fatigue, suppression, morale, and cohesion. Combat consumes and updates these states through explicit interfaces rather than implementing competing versions.

### 22.5 Replenishment and rearming

Rearming requires suitable stocks, access, transport or support assets, and time. Aircraft may need to land and turn around; ships may need to return to port or receive at-sea replenishment where capable; ground units may require resupply vehicles or access to a depot.

Ammunition consumed in combat cannot be replenished by a UI reset or the start of a new engagement. Supply and production constraints remain in force.

## 23. Time, resolution, and event scheduling

### 23.1 Continuous-time experience

The game should provide a continuous-time simulation experience with pause and adjustable speed, informed by the real-time-pausable experience of CMO and the continuous-time operational command model of Command Ops 2.

The internal architecture may use a hybrid of event-driven resolution and periodic updates. It must preserve consistent event ordering, avoid unnecessary per-frame work, and support engagements at different time scales.

### 23.2 Multiple resolution scales

Not every domain requires the same update frequency. A fast missile intercept may need fine-grained trajectory or terminal updates; a prolonged artillery mission may be represented through scheduled firing events; an operational headquarters may update its planning state less frequently.

Resolution should become more detailed when it materially changes outcomes. Any abstraction must preserve the relevant timing, uncertainty, resource consumption, and causal relationships.

### 23.3 Event ordering

When events occur at the same nominal time, the scheduler must apply a deterministic tie-breaking or simultaneous-resolution policy. For example, a launch and a platform destruction at the same time may require checking whether launch preparation had completed and whether the weapon had physically left the platform.

Event order must not be an accidental consequence of UI selection order or file iteration order.

### 23.4 Pause and speed changes

Pause and speed changes affect how quickly the simulation is presented and advanced, not the underlying mechanics. The same initial state, orders, model configuration, and random seed should yield the same results irrespective of how often the Player pauses, opens panels, or changes speed.

### 23.5 Simulation fidelity settings

The architecture may support performance settings or scenario-defined fidelity levels. These can change update granularity or aggregate detail, but must not silently give one side an advantage or change core rules in ways that undermine reproducibility. The Player should be informed when a setting changes simulation fidelity.

## 24. Battle log, combat assessment, and Narrative Battle System

### 24.1 Event log

Important combat events should be recorded with timestamps, entities, locations or areas, event types, relevant orders, weapons, ammunition, observed results, and source confidence where applicable.

Events may include detection updates, target assignment, order receipt, weapon allocation, release, intercept attempt, impact, damage, component failure, casualty, withdrawal, surrender, capture, and repair.

### 24.2 Assessment is not ground truth

A firing unit may report a hit without confirming destruction. A target may disappear from sensors without being destroyed. A platform may be damaged and continue operating. Combat assessment must use available evidence and retain uncertainty.

The simulation maintains actual damage state internally; each side's intelligence picture receives only what its sensors, reports, and assessment methods support.

### 24.3 Battle reports

Reports may summarise shots fired, ammunition spent, targets engaged, confirmed effects, probable effects, unknown outcomes, own losses, support activity, and notable tactical decisions. A report should distinguish observed facts from estimates.

### 24.4 Narrative Battle System

The Narrative Battle System may transform the event log into readable battle accounts, timelines, command summaries, or historical narratives. It may explain how a commander received a delayed report, committed a reserve, or lost an engagement due to poor information or supply.

The narrative layer must not invent events, casualties, hits, weapon characteristics, or motives not supported by simulation state or recorded evidence. Where motives are inferred, they should be framed as assessments, not facts.

### 24.5 After-action review

The Player should be able to review the engagement timeline, orders and delays, target knowledge at each decision point, weapon allocation, firing solutions, attacks, damage events, resource expenditure, and outcome. It should be possible to distinguish what was known at the time from what became known later.

### 24.6 Strategic consequences

Combat results feed into unit readiness, equipment inventory, personnel status, morale, cohesion, supply, territory, infrastructure, political outcomes, escalation, and campaign assessment. These effects must pass through the relevant authoritative systems rather than be duplicated in the combat resolver.

## 25. User interface requirements

### 25.1 Common combat picture

The map and unit interface should show friendly units, known contacts, uncertain tracks, threat zones, engagement status, orders, support relationships, and relevant control measures. It must not display hidden enemy state by default.

### 25.2 Engagement panel

For a selected unit or platform, the Player should be able to inspect:
- Actual installed weapons and launchers.
- Available and ready ammunition.
- Sensor and fire-control availability.
- Known targets and track quality.
- Suitable weapons and constraints for each target.
- ROE and authority restrictions.
- Estimated engagement envelopes and important uncertainty.
- Current orders, allocated weapons, and pending actions.
- Reasons a weapon or engagement is unavailable.

### 25.3 Manual weapon allocation

The Player should be able to select one or more targets or target areas, choose eligible weapons, set quantity or salvo policy, inspect constraints, and commit the allocation. The interface should clearly distinguish a proposed allocation from an accepted allocation and from an actual release.

A weapon allocation window inspired by CMO should be powerful but legible: explain compatibility, target uncertainty, range, required support, ammunition, and likely resource cost. It must not promise a hit probability more precise than the underlying model can support.

### 25.4 Operational engagement controls

At formation level, the Player should be able to set mission-level target priorities, weapon-use policies, fire discipline, ROE, support requests, engagement restrictions, and initiative. This should enable operational command without forcing individual weapon allocation for every unit.

### 25.5 Command and order status

Combat UI must reflect the Command & Operations model. Orders can be drafted, transmitted, pending, received, understood, executing, superseded, or completed. The Player should see when a unit is still acting on an earlier order or cannot yet respond to a new one.

### 25.6 Damage and system status

Damage presentation should show the effects relevant to the entity: mobility, weapons, sensors, communications, propulsion, power, crew, fires, flooding, structural state, and mission capability. More detailed internal component views may be available for complex platforms.

### 25.7 Combat log and replay

The Player should be able to inspect combat events in time order and filter by unit, target, weapon, mission, or event type. Replay should show what the relevant side knew at the time and what was only established later.

### 25.8 Alerts and feedback

Alerts should focus on material decisions and changes: a weapon cannot engage, a target track is stale, a launch is delayed, a unit is low on ammunition, a critical component is damaged, a mission is no longer feasible, a target may have escaped, or ROE blocks an action.

Feedback should explain causes and offer choices without silently changing orders or policy.

## 26. Conceptual data model

The following are conceptual and may be split or combined during implementation. They should reference authoritative records from other systems.

- **WeaponDesignReference:** design ID/version and relevant combat parameters from Equipment Design.
- **WeaponInstanceState:** physical identity, condition, availability, and configuration from Equipment & Materiel.
- **WeaponMountState:** mount, launcher, magazine, arcs, channels, and availability.
- **EngagementOrder:** issuer, recipients, target or area, authority, constraints, policy, and validity.
- **EngagementInstance:** participating entities, mission context, start time, state, and resolution events.
- **TargetingSolution:** target reference, information source, quality, timestamp, uncertainty, and weapon-specific suitability.
- **WeaponAllocation:** weapon/launcher, target, quantity, priority, and status.
- **FireControlState:** solution quality, source inputs, tracking state, and availability.
- **WeaponFlightState:** launch time, trajectory or abstract transit state, guidance phase, support dependencies, and terminal status.
- **InterceptionAttempt:** incoming weapon, defender, intercept weapon, solution, timing, and result.
- **DamageEvent:** source, target, affected component or aggregate, effect mechanism, severity, and timestamp.
- **DamageState:** current component and platform state, including degradation and terminal status.
- **CasualtyEvent:** affected personnel or aggregate, status, time, cause, and confidence.
- **CombatEvent:** timestamped state transition and supporting evidence.
- **CombatAssessment:** estimated effect, confidence, source, and assessment time.
- **AmmunitionUseEvent:** weapon, ammunition type, quantity, and inventory reference.
- **CombatUsageEvent:** rounds, flight time, firing cycles, wear, and maintenance-relevant activity.
- **EngagementPolicy:** ROE, target restrictions, weapon-use policy, fire discipline, and automation settings.
- **CombatReplayRecord:** ordered events and the information available to relevant actors at each time.

## 27. System interfaces and data ownership

### 27.1 Sensors & Detection

Consumes contacts, tracks, target identification, confidence, uncertainty, sensor observations, and information-sharing state. Combat must not reimplement detection or replace track uncertainty with actual target state.

### 27.2 Electronic Warfare

Consumes relevant jamming, deception, seeker interference, emissions, decoy, and communications effects. EW owns its effect model; combat resolves weapon and target consequences where those effects interact with an engagement.

### 27.3 Doctrine, Command & Operations

Consumes orders, mission tasking, command relationships, authority, ROE, targeting restrictions, command latency, support requests, and the information available to each decision-maker. Returns engagement events, ammunition expenditure, casualties, damage, and operational effects.

### 27.4 Military Branches & Unit Organisation

Consumes unit hierarchy, unit class, personnel and crew roles, command appointments, and relevant unit condition. Combat returns casualties, suppression-related events, equipment losses, and changes that affect operational readiness.

### 27.5 Equipment & Materiel

Reads actual equipment designs, installed systems, magazines, mounts, ammunition, serviceability, and physical inventory. Returns weapon usage, ammunition expenditure, damage, component loss, destruction, capture, and repair requirements.

### 27.6 Equipment Design, Research & Production

Uses researched and approved designs, component parameters, production variants, and historical availability. Combat does not create or alter equipment design parameters. Combat outcomes can inform testing, doctrine, design assessment, or research only through explicit systems.

### 27.7 Logistics and maintenance

Consumes ammunition, fuel, spares, maintenance demand, damaged equipment, recovery requirements, and resupply priorities. Combat sends consumption and damage events; Logistics determines how and when supplies or repairs are delivered.

### 27.8 Terrain, geography, weather, and environment

Consumes terrain, line-of-sight inputs, cover, concealment, infrastructure, weather, sea state, atmosphere, and relevant environmental conditions. Combat should use shared world geometry and state, not create a duplicate terrain truth.

### 27.9 Characters, training, and unit condition

Consumes crew roles, qualifications, experience, training, fatigue, suppression, morale, cohesion, and relevant traits. These affect applicable decision and performance processes; they should not become a universal multiplier.

### 27.10 Strategic, political, and narrative systems

Returns losses, infrastructure effects, escalation-relevant events, civilian harm, captured equipment, and operational results. The strategic and political systems determine broader consequences. Narrative systems present recorded events and assessments without generating combat truth.

## 28. Historical progression and scenario rules

The simulation begins on 1 January 1955. Every weapon, sensor, fire-control system, guidance method, countermeasure, and combat procedure must be available only when justified by the country's technology, design, production, procurement, and fielding state.

A technology becoming researchable does not automatically equip existing forces. A design being completed does not mean it has passed testing, entered production, been delivered, or been integrated into a platform. A produced weapon does not become available to a unit until it is delivered, allocated, installed or loaded, and supported.

The same combat framework must support different levels of technological maturity. A 1955 force may rely on visual observation, optical rangefinding, early radar, manual command procedures, and unguided weapons. A later force may use advanced radar, datalinks, precision-guided weapons, multisensor fusion, sophisticated countermeasures, and networked command, subject to actual design and readiness.

Historical doctrine, national procedures, data, and scenarios should be separate from core resolution code. Counterfactual technologies can be supported through the research and design systems without changing the baseline rules.

## 29. Audit of CMO feature concepts and glossary

CMO's feature breadth is a useful reference for an audit checklist, particularly its treatment of individual platforms, sensors, weapons, mounts, magazines, missions, doctrine, ROE, weapon allocation, air operations, surface and subsurface warfare, mines, and all-domain coordination. Project Sovereign should maintain a **CMO Feature Coverage Audit** as a separate traceability artefact when the relevant glossary and reference files are available in the repository.

The audit should not blindly reproduce every CMO feature. For each term or feature, record:
- Source term and source location.
- Plain-language meaning.
- Relevant Project Sovereign system.
- Whether it is a core mechanic, data field, UI feature, scenario tool, or future extension.
- Whether an existing specification already covers it.
- Required interaction with other systems.
- Historical and technology applicability.
- Coverage status: mapped, partially mapped, deferred, intentionally excluded, or unresolved.
- Rationale and follow-up specification.

At minimum, audit these broad concept families:
- Sensor types, signatures, detection, tracking, identification, and sensor fusion.
- Weapon categories, mounts, magazines, launchers, loadouts, guidance, and weapon allocation.
- Fire control, target uncertainty, weapon envelopes, salvo behaviour, and interception.
- Missions, patrols, strike packages, support, formation behaviour, doctrine, and ROE.
- Emissions control, jamming, decoys, electronic support, and countermeasures.
- Air operations, sortie generation, landing and rearming.
- Surface, subsurface, amphibious, mine, and anti-submarine warfare.
- Platform damage, component failure, mission kill, destruction, recovery, and damage control.
- Communications, command delay, contact sharing, and targeting support.
- Scenario setup, event triggers, scripting interfaces, and combat logging where in scope.

The audit should use the repository's actual glossary and feature documentation when available. Do not claim a term has been audited if the source catalogue has not been retrieved and reviewed.

## 30. Performance, validation, and balance integrity

### 30.1 Performance

The system must scale from small tactical engagements to large operational theatres. Use appropriate update rates, event scheduling, aggregation, and caching. Detailed component resolution should be concentrated where it materially affects outcomes, while still allowing later inspection of the causal record.

Optimisation must not create inconsistent information advantage, ignore committed actions, or change outcomes based on UI activity.

### 30.2 Validation

Combat models should be validated against historical examples, known physical constraints, equipment documentation, and controlled test scenarios where reliable data is available. Historical outcomes are evidence, not hard-coded scripts: the simulation should reproduce plausible mechanisms, not force every alternate-history engagement to match the historical result.

### 30.3 Avoid hidden balance multipliers

Do not introduce universal bonuses to accuracy, defence, lethality, or survival simply to produce desired game balance. If an abstraction or tuning parameter is necessary, document what physical or behavioural effect it represents and which weapon families it affects.

### 30.4 Data quality and uncertainty

Historical equipment data is incomplete and sometimes disputed. Each design parameter should support source, confidence, effective date, and scenario overrides where the data architecture allows. Avoid presenting estimated values as exact fact.

### 30.5 Regression tests

Changes to weapon models, targeting, damage, or event scheduling should be tested against repeatable scenarios, including:
- Clear direct-fire engagement with a stable target.
- Moving target with stale targeting information.
- Area fire with uncertain target location.
- Guided weapon with and without required mid-course support.
- Active defence against an incoming weapon.
- Damage to a critical component with secondary effects.
- Loss of communications during an engagement.
- Ammunition shortage and reload constraints.
- ROE restriction and uncertain target identification.
- Multiple simultaneous engagements competing for a limited fire-control channel.
- A launch and launching-platform destruction occurring at nearly the same time.
- Pausing and changing simulation speed without changing the resulting event sequence.

## 31. Acceptance criteria

The system is considered adequately specified when implementation can demonstrate that:

1. Combat uses a distinct engagement lifecycle and does not resolve every attack as a single universal score comparison.
2. The Player can command operationally, direct tactically, or allocate weapons directly at platform level.
3. Automated and manual weapon allocation are both available and respect actual weapon, mount, magazine, platform, and target constraints.
4. Orders, target assignments, weapon allocations, launch events, impacts, damage, and assessments are distinguishable states.
5. Combat uses the information available to the relevant actor, preserving track uncertainty and age.
6. Target identification requirements and weapon targeting requirements are checked separately.
7. Weapons use their actual design, guidance, range, launcher, ammunition, and fire-control requirements.
8. Weapon transit, guidance, acquisition, interception, and terminal interaction can evolve over time.
9. Damage can affect relevant components and functions, with causal propagation and domain-appropriate terminal states.
10. Mission kill, mobility kill, firepower kill, sensor/command kill, destruction, abandonment, capture, and recovery are distinct where relevant.
11. Ground, air, naval surface, subsurface, amphibious, and strategic engagements share common orchestration but use appropriate domain-specific mechanics.
12. Combat doctrine and automation govern decisions without overriding physical capabilities or valid higher-level authority.
13. Command latency, lost communications, local initiative, morale, cohesion, fatigue, suppression, and training interact through their authoritative systems.
14. Ammunition, fuel, wear, crew state, and maintenance needs are updated and remain subject to logistics constraints.
15. Friendly fire, area effects, ROE, and target uncertainty can produce consequences grounded in the actual situation.
16. Combat events are timestamped and reproducible from the same initial state, model version, orders, and random seed where deterministic replay is supported.
17. Combat assessment distinguishes actual damage from what a side believes happened.
18. Narrative Battle System outputs are grounded in the event log and do not invent combat outcomes.
19. The system respects the 1955 starting state and introduces later capabilities only through valid historical, research, design, production, and fielding paths.
20. The CMO Feature Coverage Audit tracks source terms and maps them to Project Sovereign systems without claiming unreviewed coverage.
21. Large operations remain playable without mandatory micromanagement of every shot, while direct engagement detail remains available when desired.
22. The combat framework remains compatible with the equipment design, organisation, command, sensors, EW, logistics, and world specifications.

## 32. Open design decisions

The following require detailed design and implementation work:

- Exact resolution algorithms for each weapon family and how much physics is simulated directly.
- The scale at which infantry fire and casualties are aggregated versus individually resolved.
- The level of component detail for different platform classes.
- The representation of ballistics, penetration, blast, fragmentation, fires, flooding, and secondary damage.
- The amount of trajectory detail used for different guided weapons and defensive interceptors.
- Which combat decisions can be fully automated and which require Player approval by default.
- The exact schema for weapon allocation policies, target priorities, fire discipline, and mission inheritance.
- How fire-control channels, illumination, datalinks, and shared tracks are represented across equipment designs.
- How domain-specific damage and terminal states are calibrated against historical and technical data.
- How strategic weapon effects and persistent environmental consequences are represented in scenarios where enabled.
- Which CMO glossary and feature reference files are available in the repository and the complete audit scope.
- Performance budgets for large engagements and the rules governing fidelity or aggregation settings.

These choices should be resolved through subsystem design, test scenarios, and cross-system review. They must preserve the core principles of actual equipment, imperfect information, command friction, domain-specific resolution, and traceable outcomes.

## 33. Summary

Combat & Engagement Systems connects orders and imperfect information to the physical and operational consequences of force. It combines Command Ops 2-inspired operational command, command latency, and subordinate execution with CMO-inspired platform detail, sensors, weapon allocation, mission control, and cross-domain engagement. Weapon and platform performance derive from the designed equipment actually fielded and available. Combat outcomes arise through a time-dependent chain of detection, decision, authorisation, weapon employment, transit, terminal interaction, damage, assessment, and reaction. The resulting losses, expenditure, damage, and tactical events flow back into the wider simulation, while the Narrative Battle System explains what happened without becoming a second source of truth.

## Command Ops 2 Operational Integration Contract (2026-10-27)

Command owns authorised intent and delegated control; Combat owns physical engagement resolution. A fire-support order creates a task/request, not a guaranteed target, firing solution or effect.

- Combat validates target/contact quality, weapon/platform capability, range/geometry, ammunition, readiness, rules of engagement and relevant fire-control constraints through their authoritative systems.
- Direct fire, anti-personnel fire, anti-armour fire, area fire, bombardment, registration and airstrikes retain their own target/area semantics and constraints rather than collapsing into one generic attack result.
- Material effects such as suppression, casualties, morale/cohesion changes, mobility/firepower/sensor/command loss, ammunition depletion, withdrawal, rout or destruction may trigger command reassessment. Combat does not rewrite superior orders.
- Actual damage is distinct from battle-damage assessment. The firing side does not automatically know an enemy was destroyed.
- Expenditure updates Inventory/Logistics through the owning interface exactly once. Fatigue, cohesion, morale and communications effects are consumed from their authoritative systems, not duplicated as parallel modifiers.
- Communications failure affects remote coordination through Communications and Command, not physical combat resolution.

Required tests: stale target data constrains fire support; mobility kill updates movement and triggers reassessment; ammunition expenditure is idempotent; unobserved outcome remains uncertain; disconnected units continue valid standing orders and local combat.
