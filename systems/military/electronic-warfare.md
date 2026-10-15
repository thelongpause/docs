# Electronic Warfare System

**Project:** Project Sovereign  
**System:** Military Simulation  
**Document ID:** MIL-EW-001  
**Status:** Formal system specification, draft 1.0  
**Canonical path:** `systems/military/electronic-warfare.md`  
**Starting historical date:** 1955  
**Initial technology horizon:** Researchable technologies through the 2026 state, subject to scenario and research rules  
**Dependencies:** Sensors and Detection, Equipment, Research, Unit State, World, Terrain, Communications  
**Related systems:** Combat, Command, Intelligence, Armaments Builder, Logistics

---

## 1. Purpose

The Electronic Warfare (EW) System models military actions that use, exploit, disrupt, deceive, or protect against electromagnetic emissions and electronic sensing.

It must create meaningful operational trade-offs between sensing, communication, concealment, jamming, and countermeasures. A radar that radiates can improve detection while revealing its presence. A jammer can degrade hostile sensing while making itself conspicuous to electronic support receivers. A self-protection countermeasure may defeat an incoming weapon in its terminal phase without suppressing every hostile sensor in the area.

EW is not one universal bonus or a single “jammed” status. It is a collection of related but distinct capabilities:

- Electronic Support Measures (ESM): receiving and interpreting emissions.
- Electronic Attack (EA): disrupting, denying, deceiving or exploiting hostile electronic systems.
- Electronic Protection (EP): protecting friendly systems against hostile electronic attack.
- Defensive Electronic Countermeasures (DECM): self-protection mechanisms that affect the terminal interaction between an incoming weapon seeker and its target, where supported by the weapon and countermeasure models.
- Offensive Electronic Countermeasures (OECM): active area or directional jamming intended to degrade opposing sensing and tracking.
- Emission control (EMCON): controlling when and how friendly systems radiate.
- Electronic deception and counter-countermeasures.
- Research and equipment design for emitters, receivers, jammers, decoys and protective systems.

The architecture must integrate with Sensors and Detection while keeping weapon endgame resolution in Combat.

## 2. Historical and Technological Scope

The initial historical setting is 1955, with long-running campaigns intended to research and field technology through at least the 2026 state.

Technology availability must be governed by research prerequisites, alternate-history outcomes, industrial capacity, equipment design and operational fielding. A 2026-capability jammer is not automatically available in 1955; it must be reached through the campaign's technology and production systems.

EW capabilities must be represented by explicit technical properties and compatibility, not by a single universal generation number. Technology generations may be used within a domain when their meaning is defined, but frequency coverage, receiver design, signal processing, power, antenna, geometry and waveform compatibility remain relevant.

## 3. Design Principles

1. **Separate attack effects.** Area jamming, self-protection countermeasures, deception, emission detection and communications disruption are distinct mechanics.
2. **Emissions create trade-offs.** Active systems may improve friendly capability while increasing the risk of detection or geolocation.
3. **Geometry matters.** Range alone is insufficient; line of sight, propagation, orientation and relative position can determine effectiveness.
4. **Compatibility matters.** A jammer cannot affect every receiver. Band coverage, signal type, system design and technology determine which effects are possible.
5. **Effects are probabilistic where appropriate.** EW degrades, confuses or defeats systems according to their capabilities and circumstances, not by an automatic universal shutoff.
6. **No omniscient target selection.** Area jammers affect eligible emitters according to their actual coverage and capacity. They do not receive hidden perfect knowledge of enemy identities.
7. **The player controls doctrine, not every waveform.** Provide understandable operating modes and mission settings, while resolving technical interactions automatically.
8. **Research and custom design matter.** Equipment characteristics must come from the Armaments Builder and research catalogue.
9. **Preserve system ownership.** Sensors and Detection owns observations and contact tracks; Combat owns weapon endgame; Communications owns message delivery; EW supplies the relevant electronic effects.

## 4. Terminology and System Boundaries

### 4.1 Electronic Support Measures (ESM)

ESM is passive reception, detection, measurement and interpretation of electromagnetic emissions. ESM may detect that a transmitter is active, estimate its bearing or location, classify the emitter, or identify a particular system depending on receiver capability and evidence.

ESM is not equivalent to radar detection. It may detect an emitter without directly detecting the platform's physical presence through radar or visual means.

### 4.2 Electronic Attack (EA)

EA covers actions intended to deny, degrade, disrupt, deceive or exploit hostile electronic systems. This includes jamming and supported forms of electronic deception.

### 4.3 Electronic Protection (EP)

EP includes system characteristics and procedures that improve operation under hostile electronic attack, such as relevant frequency agility, filtering, signal processing, directional reception, shielding, emission discipline or other researched capabilities.

EP is not a single immunity flag. Each protective capability must define the effects it mitigates.

### 4.4 DECM: Defensive self-protection

DECM represents a target platform's defensive electronic countermeasure that interacts with a compatible incoming weapon seeker during the weapon's terminal phase.

DECM is evaluated by the Combat weapon-endgame process, which queries the relevant countermeasure and seeker characteristics. DECM does not automatically create area jamming, conceal the platform from all sensors, or make the platform invisible to ESM.

Its effect depends on compatibility, technology, geometry where relevant, operating condition, countermeasure capacity and the weapon's seeker and counter-countermeasure capabilities.

### 4.5 OECM: Offensive or area jamming

OECM represents an active jammer projecting interference to degrade eligible hostile sensors or electronic systems over an area or direction.

OECM operates upstream of the weapon endgame. It may reduce detection probability, degrade track quality, interrupt illumination or guidance support, or prevent a sufficiently accurate firing solution where the affected system depends on the disrupted sensor or track.

OECM does not directly modify a weapon's terminal seeker-versus-target calculation unless a separately defined mechanic explicitly connects the effects. Its principal effect is on the hostile sensing, tracking and engagement-information chain.

### 4.6 Emission control (EMCON)

EMCON controls whether emitters operate, which modes are permitted, and when emissions are authorised. EMCON is a policy and operational state, not a physical sensor or jammer.

### 4.7 Ownership boundary

| System | Responsibility |
|---|---|
| Sensors and Detection | Observation results, contact tracks, sensor detection and track quality |
| Electronic Warfare | Jamming, deception, electronic protection, emission-related effects and EW interaction rules |
| Combat | Weapon seeker endgame and weapon-hit resolution |
| Communications | Message delivery, communication availability and communication disruption outcomes |
| Equipment / Armaments Builder | Emitter, receiver, jammer and countermeasure design properties |
| Research | Technology prerequisites and capability improvements |
| World / Terrain / Environment | Position, geometry, terrain and environmental inputs |
| Command | EMCON orders, mission policy, priorities and delegation |

## 5. Core Entities

### 5.1 EmitterDefinition

Defines an equipment design that produces an electromagnetic emission.

Conceptual fields:

- `emitter_definition_id`
- `name`
- `emitter_type`
- `operating_bands[]`
- `emission_characteristics`
- `power_requirements`
- `antenna_or_aperture_properties`
- `operating_modes[]`
- `technology_prerequisites[]`
- `design_version`

An emitter may be a radar, radio, jammer or other supported transmitter. Not every emitter is a jammer.

### 5.2 ReceiverDefinition

Defines an ESM receiver or other equipment that can detect electromagnetic emissions.

Conceptual fields:

- `receiver_definition_id`
- `name`
- `supported_bands[]`
- `sensitivity_characteristics`
- `direction_finding_capabilities`
- `signal_analysis_capabilities`
- `processing_capabilities`
- `technology_prerequisites[]`
- `design_version`

### 5.3 JammerDefinition

Defines an electronic attack device.

Conceptual fields:

- `jammer_definition_id`
- `name`
- `supported_bands[]`
- `jamming_methods[]`
- `effective_output_characteristics`
- `antenna_and_directionality`
- `coverage_geometry`
- `maximum_simultaneous_targets`
- `supported_target_systems[]`
- `power_requirements`
- `emission_profile`
- `technology_prerequisites[]`
- `design_version`

A jammer's design capacity is an equipment property. Its realised effect depends on actual geometry, compatible targets, operating state and opposing protection.

### 5.4 CountermeasureDefinition

Defines a defensive countermeasure and the interactions it supports.

Conceptual fields:

- `countermeasure_definition_id`
- `name`
- `countermeasure_type`
- `compatible_seeker_types[]`
- `supported_bands[]`, where relevant
- `effect_model_id`
- `technology_prerequisites[]`
- `capacity_and_resource_requirements`
- `design_version`

### 5.5 EWInstallation

Represents a specific installed emitter, receiver, jammer or countermeasure.

Conceptual fields:

- `ew_instance_id`
- `equipment_definition_id`
- `host_entity_id`
- `operational_status`
- `damage_state`
- `power_state`
- `current_mode`
- `emcon_state`
- `available_capacity`
- `resource_state`

### 5.6 EWEffect

Represents a resolved electronic effect for a defined simulation interval or event.

Conceptual fields:

- `ew_effect_id`
- `source_instance_id`
- `affected_sensor_or_system_id`, if known to the simulation
- `affected_domain`
- `effect_type`
- `start_time`
- `end_time`, if applicable
- `effect_strength`
- `geometry_context`
- `resolution_provenance`

Internal affected-system references must not disclose hidden enemy identities to the player unless independently detected.

## 6. Electronic Warfare Operating Model

Each EW installation has a technical capability and an operational state. The capability determines what the equipment could do; the state determines what it is currently doing.

Common conceptual states include:

- **Off:** not operating.
- **Standby:** available but not actively transmitting or applying the effect.
- **Passive:** receiving or analysing emissions without intentional transmission.
- **Active:** transmitting or applying an active EW effect.
- **Degraded:** operating with reduced capability.
- **Disabled:** unable to operate.

Valid states depend on equipment type. For example, a passive receiver and an active noise jammer have different valid mode sets.

Operating state may be controlled by direct orders, EMCON policy, mission doctrine, automatic protection logic or scenario rules. The source of each state change should be recorded for debugging and replay.

## 7. Emissions and Detection of Emitters

### 7.1 Emission production

When an emitter operates, it creates an emission according to its equipment definition, operating mode and current state.

The emission model should expose relevant characteristics to compatible opposing receivers without automatically revealing the emitter's exact position to every enemy.

### 7.2 ESM detection

ESM detection uses the Sensors and Detection architecture. The receiver evaluates whether the emission is within its capability, whether propagation and geometry permit reception, and whether signal strength and processing are sufficient.

Possible outcomes include:

- Emission detected, type unknown.
- Emitter class estimated.
- Emission bearing estimated.
- Location estimated.
- Emitter type or identity assessed.
- Existing emitter track updated.

These are distinct outcomes. An ESM receiver may detect a radar transmission without identifying the platform carrying it.

### 7.3 Geolocation

Geolocation can be based on direction finding, repeated observations, multiple receivers, movement or other supported methods. The result should carry uncertainty and observation time.

One receiver's detection must not automatically produce a perfect position. Multiple receivers only improve an estimate when their observations and geometry support it.

### 7.4 Passive is not invisible

A platform operating a passive receiver may still be detected by other methods or revealed by its other emitters. Emission control reduces selected emissions; it does not make the physical platform universally undetectable.

## 8. OECM: Area and Directional Jamming

### 8.1 Purpose

OECM projects interference intended to degrade compatible hostile sensors or electronic systems. It is normally resolved automatically once the jammer is operating and its mission and EMCON settings permit transmission.

The player does not ordinarily issue a hidden-target-specific order such as “jam this exact enemy radar” when the jammer cannot legitimately identify that radar. Instead, the player configures the jammer's operating state, mission, direction or coverage where supported, and rules of engagement/EMCON policy. The simulation determines the eligible affected set from actual conditions.

A scenario or advanced equipment may support explicit target assignment where the target is known and the equipment supports that function, but this is a separate capability, not the universal default.

### 8.2 Eligibility gates

For each candidate hostile system, evaluate:

1. **Operational state:** The jammer is operational, powered, and authorised to transmit.
2. **Domain compatibility:** The target system belongs to a domain the jammer is designed to affect.
3. **Band or signal compatibility:** The jammer's supported band and method can interfere with the target system.
4. **Geometry:** Range, terrain, line of sight or applicable propagation and directional coverage permit an effect.
5. **Technical contest:** Jammer characteristics are evaluated against receiver, antenna, processing and protection characteristics.
6. **Capacity:** The jammer has capacity available for the eligible target.
7. **Operational constraints:** Mission settings, EMCON, resource limits and damage do not prevent the effect.

A candidate failing a hard eligibility gate is not affected by that jammer for the evaluation interval.

### 8.3 Effect resolution

For an eligible target, calculate an effect strength from equipment characteristics, geometry, signal compatibility, target-system resistance, processing and technology. The technical contest must account for the relative sophistication of the jammer and the specific receiver/radar, not only a single generic EW rating. Relevant properties include frequency-band compatibility, distance and positional geometry, antenna/beam characteristics, sidelobes, pulse-repetition and emission characteristics where modelled, receiver processing, and applicable electronic-protection capabilities.

Phased-array radars may receive explicit resistance benefits where supported by their modeled properties. AESA systems can be particularly resistant to some OECM techniques because of low sidelobes, signal-to-noise performance and beamforming. These are method-specific advantages, not blanket immunity; a newer radar is not automatically unaffected by every jammer, and a jammer must not receive a universal bonus simply because it is newer.

The initial model may use a calibrated domain-specific function:

`J_effect = f(jammer capability, receiver capability, band compatibility, geometry, power, processing, protection, environment)`

The resulting effect should be translated into explicit downstream consequences, such as:

- Reduced detection probability.
- Reduced observation quality.
- Reduced track quality.
- Increased uncertainty.
- Increased time to establish a track.
- Disruption of a supported illumination or guidance-support function.

Do not apply every consequence to every sensor. A given jammer method must declare which target-system behaviours it can affect.

### 8.4 Capacity and target selection

A jammer may be limited in how many eligible emitters it can affect simultaneously. Its capacity must be explicit in its definition and modified only by supported equipment state or research.

When eligible candidates exceed capacity, selection must use a documented, deterministic priority policy. A recommended initial policy is:

1. Exclude ineligible candidates.
2. Apply explicit mission or direction constraints.
3. Rank candidates by configured priority class.
4. Within equal priority, use a stable tie-breaker based on tactical relevance and distance, followed by a stable entity identifier.
5. Preserve the selected set for the resolution interval unless a material event requires recalculation.

The exact priority policy must be visible in system documentation and testable. Do not leave target selection as undocumented engine behaviour. The default must not grant the jammer perfect knowledge of hidden target identity or threat level; use only information legitimately available to the jammer's control system.

### 8.5 Geometry and placement

Jamming effectiveness depends on the relationship between jammer, target receiver, propagation path and relevant geometry. Range alone is insufficient. Stand-off distance, relative bearing and placement within a supported strike formation can materially change the result; a jammer supporting strikers may need to remain close enough to the threat axis to be effective while balancing exposure and survivability.

The model must allow a jammer to be effective against some targets but ineffective against others due to terrain masking, band mismatch, directionality, receiver protection or other constraints. OECM is area/directional interference, not a guarantee that every eligible radar in a region is suppressed equally.

### 8.6 Emission risk

An active jammer emits a detectable signal where opposing receivers have compatible capability and suitable geometry. The source's location may be estimated from one or more ESM observations.

Detection of the jammer is not guaranteed, and geolocation accuracy depends on the opposing receiver and evidence. The jammer's emission-detection range is not necessarily equal to its effective jamming range.

### 8.7 Effects on engagement

OECM may degrade the track or sensing chain needed to support an engagement. If a fire-control radar loses a required track or illumination, the Combat system may determine that the associated weapon or engagement procedure can no longer proceed normally.

The Sensors and Detection System must communicate the degraded observation or track. Combat decides the consequences for engagement eligibility and weapon resolution. OECM does not automatically alter terminal weapon seeker calculations.

## 9. DECM: Defensive Self-Protection

### 9.1 Purpose

DECM is a platform-level countermeasure evaluated against a compatible incoming weapon during the appropriate endgame phase.

DECM does not project general area noise and does not protect other units merely because they are nearby.

### 9.2 Eligibility

A DECM attempt is permitted only when:

- The platform has a compatible, installed and operational countermeasure.
- The incoming weapon's seeker or guidance method is within the countermeasure's supported interaction.
- The countermeasure is powered, available and in an appropriate operating state.
- Required resources or capacity are available.
- Any relevant geometry, timing and operational constraints are satisfied.

The Combat system must supply the incoming weapon and seeker context at the appropriate stage.

### 9.3 Resolution

Combat resolves the terminal interaction using the countermeasure's effect model and the weapon seeker's capabilities. The reference model is probabilistic: success depends on chance and the relative generations/sophistication of the defensive countermeasure and the attacking weapon's seeker. An older DECM system should generally be less effective against a substantially more modern seeker, and a modern seeker should not be assumed automatically defeated by the presence of DECM.

Where generation levels are used, the comparison must be calibrated and domain-specific. Compatibility with the seeker type, counter-countermeasures, operating state and any supported resource/capacity constraints must also be evaluated. A numerical generation label must not be treated as the sole physical determinant of success.

DECM is a platform-level self-protection effect resolved as part of the weapon endgame. It is not area jamming and does not directly reduce hostile search-radar detection of unrelated units. DECM success may reduce the probability of successful acquisition, tracking or terminal engagement according to the relevant seeker model. The precise outcomes belong to the Combat weapon-endgame contract.

### 9.4 Information and observability

DECM operation does not automatically reveal the platform to ESM. If a particular countermeasure emits a detectable signal, that emission must be explicitly defined in its equipment data. The default must not assume all defensive countermeasures are either detectable or undetectable.

## 10. Electronic Protection and Counter-Countermeasures

Electronic Protection represents capabilities that preserve system function under hostile EW. Possible examples include frequency agility, filtering, directional antennas, waveform changes, signal processing, sidelobe control, emission discipline and other domain-appropriate techniques.

The system must define what each capability mitigates. For example, a capability may reduce the effect of a certain jamming method without making the receiver immune to all interference.

Counter-countermeasures may improve the chance of maintaining a track, identifying a deceptive signal, rejecting interference or operating through a disrupted environment. They must be compatible with the relevant system and should consume appropriate design, research, power, mass, complexity or cost.

Avoid a single “ECM resistance” score that silently counters all EW methods. A summary rating may be displayed to the player, but the underlying simulation must retain method-specific interactions.

## 11. Electronic Deception

Electronic deception creates or alters electronic evidence to mislead an opposing system. Supported implementations may include false emissions, misleading bearings, deceptive signals, or decoy-related electronic signatures.

The architecture must distinguish:

- A genuine emitter.
- An emission falsely suggesting an emitter or platform.
- A real target whose observed track has been deceived.
- A sensor observation corrupted by interference.
- A report that has been incorrectly interpreted.

The appropriate equipment or EW method defines the deception. Sensors and Detection resolves resulting observations and contact estimates. The Intelligence system preserves provenance and uncertainty. Combat consumes the resulting track quality where relevant.

Deception must not directly rewrite the authoritative position or identity of the real target.

## 12. Communications and EW

Radio communications may be affected by compatible interference, jamming, or other EW effects. The Communications System owns message delivery and communication availability.

Electronic Warfare provides the interference input and its applicable strength, domain and duration. Communications determines whether a message is delayed, degraded or lost according to its own rules.

A communications disruption must not automatically erase information already received, and it must not affect unrelated communications unless the applicable system and geometry support that effect.

## 13. Research Integration

Research should unlock and improve explicit EW capabilities, including:

- ESM sensitivity and signal analysis.
- Direction finding and emitter geolocation.
- Jamming output, supported bands and methods.
- Antenna directionality and coverage.
- Signal processing and interference rejection.
- Frequency agility and other protective techniques.
- Counter-countermeasures.
- Deception and decoy capabilities.
- Reduced size, mass, power, maintenance or resource consumption.
- Improved reliability and operational availability.

Research changes capability definitions or unlocks new designs. It does not automatically install equipment or retrofit existing units.

Research prerequisites and effect stacking must be explicit. Two research advances that improve the same property must follow documented stacking rules.

## 14. Armaments Builder Integration

The Armaments Builder must support compatible emitter, receiver, jammer, and countermeasure components.

Relevant design properties may include:

- Operating bands and supported signal types.
- Effective output and receiver sensitivity.
- Antenna gain, directionality and coverage.
- Jamming method.
- Maximum simultaneous target capacity.
- Signal processing and protection capability.
- Power, cooling, mass and volume.
- Emission characteristics.
- Reliability, maintenance and resource demand.
- Technology prerequisites and manufacturing cost.

Platform integration must respect mass, space, power, cooling, antenna placement, interference with other equipment, crew requirements and maintenance.

The equipment design defines capability. Installed equipment state, geometry, opposing systems and environmental conditions determine actual effect.

## 15. Operational Control and Player Orders

The player should primarily control EW through understandable orders and policies:

- Enable, disable or place an installation on standby.
- Set active or passive mode where supported.
- Set EMCON restrictions and authorisation conditions.
- Define mission, coverage direction or area where supported.
- Set priority classes for eligible target categories where equipment and information support it.
- Set automatic defensive-countermeasure policy where the platform supports it.
- Allocate scarce power or other operational resources through the relevant systems.

The simulation resolves technical details automatically. It must not expose controls the equipment cannot physically perform.

Default policies should be sensible and scenario-configurable. The player should be able to automate routine self-protection without manually triggering every countermeasure event.

## 16. Data Contracts

Conceptual schema:

```text
EmitterDefinition
    emitter_definition_id
    emitter_type
    operating_bands[]
    emission_characteristics
    operating_modes[]
    power_requirements
    technology_prerequisites[]
    design_version

ReceiverDefinition
    receiver_definition_id
    supported_bands[]
    sensitivity_characteristics
    direction_finding_capabilities
    signal_analysis_capabilities
    processing_capabilities
    technology_prerequisites[]
    design_version

JammerDefinition
    jammer_definition_id
    supported_bands[]
    jamming_methods[]
    effective_output_characteristics
    antenna_and_directionality
    coverage_geometry
    maximum_simultaneous_targets
    supported_target_systems[]
    power_requirements
    emission_profile
    technology_prerequisites[]
    design_version

CountermeasureDefinition
    countermeasure_definition_id
    countermeasure_type
    compatible_seeker_types[]
    supported_bands[]
    effect_model_id
    capacity_and_resource_requirements
    technology_prerequisites[]
    design_version

EWInstallation
    ew_instance_id
    equipment_definition_id
    host_entity_id
    operational_status
    damage_state
    power_state
    current_mode
    emcon_state
    available_capacity
    resource_state

EWEffect
    ew_effect_id
    source_instance_id
    affected_system_id?
    affected_domain
    effect_type
    start_time
    end_time?
    effect_strength
    geometry_context
    resolution_provenance
```

### 16.1 Invariants

1. EW equipment must reference canonical equipment definitions.
2. Capability and operating state are distinct.
3. Ineligible systems cannot be affected by a jammer without an explicitly modelled mechanism.
4. Jamming effects must be scoped by domain, method and applicable geometry.
5. Target selection over capacity uses a documented, reproducible policy.
6. Active emissions can be detected only through a compatible opposing capability and valid sensing conditions.
7. DECM is resolved in the Combat endgame, not as a general sensor penalty.
8. OECM affects supported sensing, tracking or electronic functions upstream of weapon endgame.
9. Research and equipment effects are not applied multiple times.
10. Hidden enemy identities are not revealed to the player by internal EW resolution.
11. Communications owns message delivery outcomes.
12. EW effects have explicit timing and cannot persist indefinitely by accident.

### 16.2 CMO reference-data mapping (import guidance)

The audited DB3K dashboard exposes CMO-specific sensor roles and fields. These are source-data mappings, not Project Sovereign runtime identifiers.

| CMO source field or role | Project Sovereign mapping | Import rule |
|---|---|---|
| Sensor role 4001 (OECM in this dashboard) | jamming_method / jammer_definition | Preserve the source role in provenance; validate the source enum before mapping |
| Sensor role 4011 (DECM in this dashboard) | countermeasure_type: defensive_self_protection | Resolve through Combat's weapon-endgame interface |
| ECMPeakPower | effective_output_characteristics | Preserve the source unit and model context; do not treat power alone as jamming effectiveness |
| ECMGain | antenna_and_directionality | Map with its units and any source assumptions |
| ECMBandwidth and frequency-band associations | supported_bands / band_compatibility | Use as a hard compatibility gate only where the source definition supports it |
| ECMNumberOfTargets | maximum_simultaneous_targets | Treat as a capacity limit; target selection beyond capacity must follow Project Sovereign's documented deterministic policy |
| ECMPoKReduction | Candidate input to a DECM effect model | Preserve as source-specific evidence; validate semantics before interpreting it as a probability or direct effect modifier |

The CMO manual describes DECM as a probabilistic weapon-endgame interaction whose success depends on random chance and the relative generations of countermeasure and seeker. The dashboard's simplified chart presents ECMPoKReduction as a flat effect modifier; that presentation must not override the manual's generation-dependent description or be copied as the entire Project Sovereign resolution model.

The manual describes OECM as noise jamming that can affect multiple units, can itself be detected by ESM, and depends materially on geometry, positioning and technology generation. It does not document the engine's target-priority order when eligible emitters exceed capacity. Project Sovereign therefore defines its own deterministic and testable priority policy rather than reproducing an undocumented internal behaviour.

Dashboard annotations marked inferred or speculative remain hypotheses until independently validated. In particular, do not infer universal frequency agility from a PESA/AESA label, universal immunity from OECM, or a mandatory hard line-of-sight rule for every jammer method without an explicit model definition.

## 17. Scheduling and Resolution

EW should be resolved on defined simulation intervals and relevant events, not every rendered frame.

Recalculate when material conditions change, including:

- An emitter or jammer changes operating state.
- A target or jammer moves materially.
- Geometry or propagation changes enough to affect eligibility.
- A sensor changes band or mode.
- Damage, power or capacity changes.
- A relevant EW effect starts or ends.
- A countermeasure becomes available or unavailable.

Area jamming should evaluate plausible candidate receivers through spatial and compatibility filtering. Avoid all-to-all checks across every emitter in the World.

The selected target set and resolved effects must be reproducible under the project's simulation determinism rules. UI interactions must not trigger new EW rolls.

## 18. Player Interface and Feedback

The UI should make trade-offs understandable without disclosing hidden enemy state.

Display as appropriate:

- Current operating and EMCON state.
- Equipment readiness, power and damage.
- Supported bands and target-system categories.
- Current coverage or mission.
- Jammer capacity and utilisation.
- Known or estimated hostile emissions.
- EW effects reported by friendly sensors.
- Confidence and age of emitter location estimates.
- Reasons a known target system is ineligible, when revealing that reason does not expose hidden information.

Do not show a perfect list of all affected enemy systems to a player if their side has no means to know which systems are present. Engineering diagnostics may expose the simulation's own equipment state, not omniscient enemy state.

## 19. Acceptance Criteria

### OECM

- An inactive or unpowered jammer produces no active jamming effect.
- A jammer cannot affect a receiver outside its supported band or method unless a defined mechanism permits it.
- Geometry and propagation affect eligibility or strength.
- Technical protection can reduce effects according to documented compatibility.
- A jammer cannot exceed its defined simultaneous-target capacity.
- Over-capacity selection follows a documented deterministic policy.
- Area jamming can affect multiple eligible systems without requiring direct manual target assignment.
- An active jammer may be detected by compatible ESM, but detection and geolocation are not guaranteed.
- Jamming degrades supported sensor or electronic functions and does not automatically alter every weapon endgame calculation.

### DECM

- DECM is evaluated only at the appropriate weapon endgame stage.
- An incompatible countermeasure cannot affect a seeker unless an explicit interaction is defined.
- Countermeasure performance depends on defined characteristics, operating state and seeker characteristics.
- DECM does not automatically protect nearby units or suppress area sensors.
- The effect is reproducible under simulation rules.

### ESM and emissions

- A compatible receiver can detect a valid emission under suitable conditions.
- An emission detection does not automatically identify its platform.
- Location estimates retain uncertainty and provenance.
- Passive operation does not imply universal invisibility.
- EW-generated observations use the Sensors and Detection contact architecture.

### Protection and deception

- Protection mitigates only the effects it is defined to mitigate.
- Deception changes observations or estimates, not authoritative target state.
- Repeated reports derived from one source do not count as independent evidence.
- Communications disruption is resolved by the Communications System.

### Research, equipment and performance

- Research unlocks explicit capabilities and does not silently refit existing units.
- Equipment installation respects platform constraints.
- EW effects are time-bounded and updated on defined intervals.
- Candidate filtering prevents unnecessary all-to-all evaluation.
- UI activity does not reroll EW outcomes.

## 20. Implementation Sequence

**Phase 1:** Operating states, emission records, basic ESM detection interface and EMCON.

**Phase 2:** OECM eligibility, compatibility, geometry, capacity, deterministic target selection and sensor/track degradation.

**Phase 3:** DECM-to-Combat endgame interface, with compatible seeker and countermeasure definitions.

**Phase 4:** Research and Armaments Builder integration for emitters, receivers, jammers and countermeasures.

**Phase 5:** Electronic protection, advanced processing, deception, geolocation and communications interference.

**Phase 6:** Advanced domain models and scenario-specific doctrines.

## 21. Definition of Done

The first playable EW implementation is complete when active emissions can be detected through Sensors and Detection; EMCON controls whether supported emitters operate; OECM affects only eligible systems according to documented compatibility, geometry and capacity; capacity selection is reproducible; DECM is resolved through Combat at the appropriate endgame stage; research and equipment definitions determine capability; Communications retains ownership of message delivery; and automated tests cover these boundaries.

**Design objective:** Electronic Warfare should create a contest of sensing, concealment, interference and adaptation. The player should gain real advantages from better equipment, research and positioning while accepting the operational risks of emitting, consuming resources and relying on systems vulnerable to opposing countermeasures.
