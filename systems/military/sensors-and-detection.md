# Sensors and Detection System

**Project:** Project Sovereign  
**System:** Military Simulation  
**Document ID:** MIL-SENS-001  
**Status:** Formal system specification, draft 1.1  
**Canonical path:** `systems/military/sensors-and-detection.md`  
**Starting historical date:** 1955  
**Initial technology horizon:** Researchable technologies through the 2026 state, subject to the technology model and scenario rules  
**Dependencies:** World, Terrain, Weather and Environment, Time, Unit State, Equipment, Research, Communications  
**Related systems:** Electronic Warfare, Intelligence, Command, Combat, Logistics, Armaments Builder

---

## 1. Purpose

The Sensors and Detection System governs how military forces observe the World, detect potential targets, classify and identify them, maintain tracks, and distribute imperfect intelligence.

The authoritative simulation state and a side's knowledge of that state are separate. Every entity has a real position, state, equipment and activity in the simulation, but opposing sides do not automatically know these facts. Information becomes available through sensors, observation, reporting, intelligence, and interpretation.

The system must support:

- Visual observation and line of sight.
- Optical instruments and rangefinding.
- Radar detection and tracking.
- Acoustic observation, listening systems and sonar.
- Electronic Support Measures (ESM) and emission detection.
- Reconnaissance and other sensor methods added later.
- Target signatures, camouflage, concealment, movement and posture.
- Detection, classification, identification and tracking as distinct outcomes.
- Position uncertainty, contradictory reports, false contacts and stale intelligence.
- Local awareness, communications delays and side-level intelligence pictures.
- Sensor design, installation, research, maintenance, damage and operating modes.
- Integration with Electronic Warfare, the Armaments Builder, Research, Combat and Command.

The desired gameplay combines the operational uncertainty of Command Ops 2 with the sensor diversity and equipment detail of Command: Modern Operations and the long-term technological progression of Aurora 4X.

**Core principle:** The World knows what is true. Sensors observe evidence. Intelligence systems interpret evidence. Commanders and players act on the information available to them.

## 2. Historical and Technological Scope

### 2.1 Starting date

The initial historical setting is **1955**. Initial equipment, doctrine, industry, research availability and scenario content must reflect the selected 1955 baseline.

The date is not a limit on the length of a campaign. Project Sovereign is intended to support long-running games in which nations research, design, manufacture, field, maintain and replace equipment over time.

### 2.2 Research horizon

For the current development plan, the technology catalogue should include a researchable horizon reaching at least the **2026 state of technology**. This is a planning horizon, not a promise that every technology will be available in every scenario or nation.

The technology system must distinguish:

- **Historical availability:** technology known or fielded by the selected date in the reference history.
- **Research availability:** technologies a nation can discover or develop under its actual in-game research path.
- **Design availability:** capabilities and components a player can legally use in a new design.
- **Production availability:** whether the required facilities, materials, skills and industrial capacity exist to manufacture it.
- **Operational availability:** whether a completed design has been produced, delivered, crewed, supplied and maintained.

Research must not automatically grant equipment to existing units. A newly researched sensor becomes usable in a design only when prerequisites and design constraints are satisfied; that design must then be produced, installed or refitted before the operational capability is fielded.

### 2.3 Avoid hard-coded calendar assumptions

Technology definitions should use explicit prerequisites, capability properties and availability rules. A technology's historical introduction date may inform default scenarios, but should not by itself determine whether it is researchable in every alternate-history campaign.

The architecture must permit research beyond 2026 in future expansions without requiring a redesign of the detection system.

## 3. Design Principles

### 3.1 No universal visibility flag

Do not represent all knowledge using a single `visible` boolean. A target can be visually hidden but detected by radar, acoustically detected without being located, or known from an old report whose position is no longer reliable.

### 3.2 Separate information stages

The following are distinct:

1. **Observation:** A sensor produces evidence.
2. **Detection:** The evidence supports the conclusion that an object or event may be present.
3. **Classification:** The object is assigned to a broad or specific category.
4. **Identification:** Evidence supports a more specific type or identity.
5. **Location estimation:** The system estimates where the target is.
6. **Tracking:** Observations are associated over time to maintain an estimate.
7. **Reporting:** Information is made available to an authorised recipient.

These stages can succeed independently. A radar can detect an aircraft without identifying its model; ESM can detect a transmission without identifying the platform that emitted it.

### 3.3 Detection is asymmetric

Detection is not automatically reciprocal. An observer can detect a target without being detected in return. Each direction must be resolved using the applicable sensor, signatures, geometry, environment and operational state.

### 3.4 Preserve uncertainty and provenance

Observations and reports must retain their source, timestamp, quality and uncertainty. Multiple reports may improve an estimate, but duplicated copies of the same report are not independent evidence.

### 3.5 Data-driven equipment

Sensors must be defined through reusable equipment records. The same detection architecture should support binoculars, radar sets, hydrophones, sonar, ESM receivers and future sensor types without bespoke logic for every platform.

### 3.6 Rich simulation, manageable commands

Detection calculations should be automatic. The player makes meaningful decisions about sensor design, placement, operating modes, emissions, reconnaissance priorities, communications and how to respond to uncertain intelligence.

## 4. System Ownership

| System | Owns |
|---|---|
| World | Authoritative coordinates, spatial relationships and world geometry |
| Terrain | Elevation, slope, obstruction, vegetation, buildings, cover and terrain effects |
| Weather and Environment | Visibility, precipitation, fog, ambient conditions and domain-specific propagation inputs |
| Time | Simulation time, daylight, twilight and darkness |
| Unit State | Authoritative position, movement, posture, activity, condition and damage |
| Equipment and Armaments Builder | Sensor designs, installed equipment and physical integration constraints |
| Research | Technology prerequisites and unlocked capabilities |
| Sensors and Detection | Sensor evaluation, observations, detection outcomes, contact tracks and contact uncertainty |
| Electronic Warfare | Jamming, electronic deception, countermeasures and emission-related effects |
| Intelligence | Strategic intelligence products, assessments and source evaluation beyond local sensing |
| Communications | Transmission, recipients, delay, bandwidth, connectivity and disruption |
| Command | Orders, reporting priorities and delegated authority |
| Combat | Engagement decisions and combat resolution |
| Logistics | Power, fuel, spares, batteries, maintenance and other support resources |

The Sensors and Detection System must read authoritative information from the owning system and must not duplicate or silently override it.

## 5. Core Entities

Names below are canonical conceptual entities. Implementation must align field naming and ID conventions with the project's shared schema rules.

### 5.1 SensorDefinition

A reusable technical design.

Required concepts:

- `sensor_definition_id`
- `name`
- `sensor_domain`
- `sensor_method`
- `technology_prerequisites[]`
- `detection_model_id`
- `supported_modes[]`
- `supported_signature_domains[]`
- `sensor_capabilities[]`
- `sensor_geometry_and_coverage`
- `search_and_track_precision_model_id`
- `operating_requirements`
- `emission_profile`
- `classification_capabilities`
- `tracking_capabilities`
- `design_version`

### 5.2 SensorInstance

A specific installation on a platform, carried by a unit, or deployed as a fixed sensor.

Required concepts:

- `sensor_instance_id`
- `sensor_definition_id`
- `host_entity_id`
- `operational_status`
- `damage_state`
- `calibration_state`
- `operator_effectiveness`, where applicable
- `power_state`
- `current_mode`
- `current_emission_state`
- `maintenance_state`

### 5.3 TargetSignatureProfile

Describes how an entity may be observed in a particular signature domain.

Required concepts:

- `signature_profile_id`
- `target_entity_id`
- `signature_domain`
- `baseline_signature`
- `aspect_modifiers`
- `operating_state_modifiers`
- `movement_modifiers`
- `concealment_modifiers`
- `environmental_modifiers`
- `signature_version`

Signatures must be domain-specific. Radar cross-section, acoustic source level, visual conspicuity and electromagnetic emissions are not interchangeable units.

### 5.4 Observation

An individual sensor result.

Required concepts:

- `observation_id`
- `source_sensor_id`
- `source_entity_id`
- `observing_side_id`
- `observation_time`
- `sensor_domain`
- `observation_result`
- `estimated_position`, if available
- `position_uncertainty`, if available
- `detection_confidence`
- `classification_estimate`, if available
- `classification_confidence`, if available
- `source_quality`
- `provenance`

An internal association to the true entity may be retained for simulation logic where necessary, but must not be exposed to the player-facing intelligence view.

### 5.5 ContactTrack

A side-specific estimate maintained from one or more observations.

Required concepts:

- `contact_track_id`
- `owning_side_id`
- `contact_status`
- `first_observed_time`
- `last_observed_time`
- `last_updated_time`
- `estimated_position`, if available
- `position_uncertainty`
- `estimated_movement`, if supported
- `estimated_target_class`
- `estimated_identity`, if supported
- `classification_confidence`
- `identification_confidence`
- `track_quality`
- `contributing_observation_ids[]`

A contact track is an intelligence estimate, never the target itself.

### 5.6 IntelligenceReport

A report derived from observations or tracks and delivered to an authorised recipient.

Required concepts:

- `report_id`
- `recipient_side_id`
- `recipient_entity_id`, if applicable
- `report_time`
- `source_time`
- `report_type`
- `reported_position`, if available
- `reported_uncertainty`, if available
- `reported_classification`, if available
- `reliability`
- `source_provenance`
- `delivery_status`

A report's delivery time must not replace its original observation time.

## 6. Sensor Domains

The architecture must support these initial domains through a common interface while preserving their distinct physical rules.

### 6.1 Visual and optical

Includes unaided vision, binoculars, optical rangefinders and observation instruments. Relevant factors include range, target size and contrast, observer and target elevation, terrain obstruction, vegetation, built-up areas, weather, lighting, movement, posture, camouflage, observer condition and optical quality.

Magnification can improve observation and classification within optical and atmospheric limits. It cannot bypass opaque terrain obstruction.

### 6.2 Radar

Radar emits electromagnetic energy and processes returns. Relevant factors include operating frequency and band, transmitter and receiver characteristics, antenna gain and orientation, target radar signature and aspect, range, propagation, clutter, terrain masking, interference, processing, technology and electronic warfare.

Radar definitions must explicitly model applicable horizontal and vertical coverage limits, sensor height, minimum and maximum range, target-altitude envelope, scan or revisit interval, and whether the sensor is searching, maintaining a track, or illuminating a target. Mechanically scanned systems may need to trade search coverage against sustained tracking or illumination; a continuous-wave illumination channel can be occupied by an engagement. Track-while-scan, moving-target indication, look-down/shoot-down, frequency agility, non-cooperative target recognition, and phased-array properties are distinct capabilities, not universal properties of all radars.

Search detection and cued tracking may have different performance envelopes. A sensor that can maintain a track after another source provides a cue must not automatically receive that same range when searching an un-cued volume. Radar altitude limits, antenna field of view, land/sea clutter, target motion, frequency band and sensor geometry may impose hard limits or calibrated modifiers.

Detection, track formation, fire-control-quality data and identification are separate outcomes. A radar return must not automatically reveal exact platform identity, and a contact that is detected is not necessarily precise enough to support a weapon engagement. Phased arrays and modern processing may improve tracking or resistance to particular jamming methods, but must not be treated as universally immune to clutter, deception or EW.

### 6.3 Acoustic

Includes human hearing, listening posts and acoustic equipment. Relevant factors include source intensity, distance, bearing, background noise, terrain, weather, target activity, sensor placement and operator effectiveness.

An acoustic observation may provide a bearing or area without a precise position.

### 6.4 Sonar and underwater detection

Underwater detection requires domain-specific acoustic propagation. Relevant factors may include active or passive mode, source and target signatures, water depth, seabed, water conditions, ambient noise, relative motion and sensor capability.

Active sonar can reveal the source to an opposing receiver. Passive sonar receives without intentionally transmitting the sensing signal. The model must not treat terrestrial line of sight as a substitute for underwater propagation.

### 6.5 Electronic Support Measures

ESM detects, intercepts or analyses electromagnetic emissions, including radar and active jamming emissions. Relevant factors include whether the target emits, frequency coverage, signal characteristics, receiver sensitivity, geometry, terrain and propagation, signal processing, interference and technology.

An ESM result may range from a warning that an emitter is present, through emitter classification and a bearing, to a position estimate. Detection does not guarantee emitter or platform identification. A single bearing is not a point location; geolocation may require multiple suitably placed receivers, intersecting bearings, repeated observations or other independent evidence. Even a well-classified emitter may have poorer location precision than an active sensor tracking the platform directly.

Passive coherent location is a distinct passive method: it relies on suitable external illuminators and receiver geometry, and an initial detection should not automatically be treated as a fire-control-quality track. Magnetic anomaly detection is also a specialised case: a detection can indicate a nearby submarine without providing a useful range estimate.

ESM detection belongs to this system. Jamming, electronic deception and countermeasures belong to Electronic Warfare and must use explicit interfaces.

### 6.6 Future domains

The system must permit infrared and other specialised methods to be added through sensor-specific detection models, signature domains, operating requirements and observation outputs.

## 7. Detection Resolution Pipeline

Each sensing evaluation follows this logical pipeline:

1. **Eligibility:** Is the sensor operational and permitted to attempt this observation?
2. **Candidate selection:** Is the target within a plausible spatial and domain envelope?
3. **Geometry:** Evaluate range, obstruction, line of sight or domain-specific propagation.
4. **Signature assessment:** Evaluate target observability in the relevant domain.
5. **Environmental assessment:** Apply applicable weather, lighting, clutter and other conditions.
6. **Detection resolution:** Resolve whether a meaningful observation occurs.
7. **Classification and identification:** Determine what the evidence supports.
8. **Location and tracking:** Estimate location and movement where possible.
9. **Reporting:** Update local knowledge and submit reports to authorised recipients.
10. **Ageing:** Reduce confidence as evidence becomes stale or contradicted.

Not every observation must complete every stage. A bearing-only ESM observation, for example, may be useful without producing a precise location or target identity.

### 7.1 Eligibility checks

A sensor cannot attempt detection if its operating mode, power, damage, domain compatibility, frequency coverage, operating limits or required geometry make the attempt invalid.

### 7.2 Geometry

Use canonical World and Terrain services. Do not create a contradictory terrain model inside Sensors and Detection.

For visual, infrared and radar sensing, evaluate the sensor's own elevation and local skyline, target-side land cover, and relevant obstruction along the observation path. Terrain and land cover can reduce effective range even where a simple endpoint-to-endpoint line appears open. Sensor height and target altitude must be evaluated separately from a weapon's engagement envelope. Radar, ESM and optical/infrared propagation may use different horizon models; do not apply one universal horizon multiplier to all sensor families.

For radar, geometry also includes antenna field of view, vertical coverage, scan/revisit timing, target altitude, range limits and applicable clutter or look-down effects. Moving ground targets may be easier to detect than halted targets when the radar relies on Doppler discrimination; specialised MTI/GMTI and frequency-band capabilities must be represented explicitly. For optical/infrared sensors, account for limited field of view and background clutter, and allow a detection to have meaningful range ambiguity when the sensor cannot estimate distance precisely. Other domains must apply their appropriate propagation rules. A straight-line geometry test is a useful baseline, not a universal physical model.

### 7.3 Initial detection model

A generic initial probability model is:

`P(detection) = clamp(P_base × M_range × M_geometry × M_signature × M_environment × M_posture × M_sensor_state × M_technology × M_EW, 0, P_max)`

This is a common framework for tunable factors, not a claim that all sensors share the same physical equation. Each sensor domain must define its own range response, geometry handling, environmental modifiers and calibration.

Modifiers require documented units and valid ranges. Apply hard constraints before probability resolution. Avoid arbitrary bonuses without calibration.

### 7.4 Reproducibility

Use the simulation's controlled random-number system. A given saved state, event sequence and random seed should reproduce outcomes under the project's determinism rules. Opening a panel, moving the camera or refreshing the UI must not reroll detection.

## 8. Signatures, Movement and Concealment

### 8.1 Domain-specific signatures

One target may be visually difficult to observe but acoustically conspicuous. Another may be visually obvious but have a relatively small radar signature. Do not collapse these into one universal stealth value.

### 8.2 Movement and activity

Movement can increase observability, but effects depend on the sensor. Vehicle operation may increase acoustic signatures; firing may create visual, acoustic or other observable events; radar detectability depends on radar-target interaction; radio and radar transmission can create electromagnetic emissions.

Do not apply a universal movement bonus to every domain.

### 8.3 Posture and concealment

The detection model may use relevant states such as moving, halted, deployed, entrenched, concealed, camouflaged or resting where those states exist in the owning systems.

Camouflage, concealment and physical obstruction are distinct. Camouflage changes observability; opaque terrain may prevent a direct sensing path.

### 8.4 Uniforms and seasonal camouflage

Uniform and camouflage designs must influence detection through context-sensitive modifiers. Snow camouflage may reduce visual conspicuity in snow-covered terrain while performing poorly in forests, urban areas or dry terrain.

The result depends on uniform design, environment, terrain, sensor domain, posture and sensor capability. Uniforms must not grant universal stealth or combat bonuses unrelated to their actual effects.

Environmental clothing and personnel endurance belong to the appropriate personnel and environment systems; Sensors and Detection consumes only the relevant signature or visibility effects.

## 9. Classification and Identification

### 9.1 Suggested classification ladder

1. Unclassified contact.
2. Broad domain, such as land, air, surface or subsurface.
3. General category, such as vehicle, aircraft, ship, formation or installation.
4. Functional class, where supported.
5. Specific type or model, where supported.
6. Specific identity, where sufficient evidence exists.

This ladder is extensible and not every sensor can reach every level.

### 9.2 Confidence

Classification confidence, identification confidence, position uncertainty and track quality are separate values. Low-quality evidence may result in plausible misclassification. Additional independent evidence may improve or correct the assessment.

Identification requires stronger evidence than generic classification. The system must support mistaken identification and later correction.

## 10. Contact Tracking and Sensor Fusion

### 10.1 Contact creation

A qualifying observation can create a contact track. Ambiguous evidence may remain an unconfirmed observation or transient contact. Thresholds are configurable by scenario and doctrine.

### 10.2 Track association

When a new observation arrives, assess whether it plausibly belongs to an existing track using time, spatial proximity, estimated movement, classification compatibility, domain, uncertainty and provenance.

If association is uncertain, preserve separate contacts or record the uncertainty rather than merging automatically.

### 10.3 Fusion

Multiple observations can improve a position, classification or movement estimate. Fusion must account for age, source reliability, uncertainty, sensor limitations, independence and contradictions.

Copies of the same underlying report must not count as independent evidence. The initial implementation may use a confidence-weighted update model; advanced tracking filters can be added without changing the public contact schema.

### 10.4 Track merge and split

The system must support controlled merging of contacts later established to represent the same target, and splitting a contact later assessed to represent multiple targets. Historical observations and provenance must be preserved.

### 10.5 Side-specific knowledge

Each side maintains its own intelligence picture. Friendly units may hold different local observations until communications distribute them. Opposing sides may have different estimates of the same target.

## 11. Position and Movement Uncertainty

A contact may report a point estimate, bearing, bounded area, uncertainty region or movement corridor, depending on the sensor and evidence.

The initial implementation may use a coordinate plus an uncertainty radius in metres. The schema should permit future extension to non-circular regions or covariance representations.

The uncertainty radius is not the sensor's maximum range. Movement estimates must be inferred from observations rather than copied from hidden target velocity. When observations become stale or contradictory, movement estimates degrade or become unavailable.

The observation schema must support bearing-only, imprecise-position and area-of-uncertainty results as first-class outcomes. A sensor-specific precision model should determine when a detection can provide a point estimate and when it must provide a bounded or ambiguous location. Examples include long-range visual/infrared detections with poor range estimation, ESM bearings without triangulation, and MAD detections without a usable range component. Do not convert these observations into artificially precise coordinates merely to fit a point-based UI.

## 12. Intelligence Ageing

Ageing is a simulation mechanic, not merely a UI effect.

Each track should retain the time of the most recent supporting observation, last track update, report delivery where applicable, and time since last confirmed observation.

Suggested display states:

| State | Meaning |
|---|---|
| Current | Fresh supporting evidence under the configured rules |
| Recent | Previously observed but no longer freshly confirmed |
| Stale | Location or classification should be treated cautiously |
| Expired | No longer eligible for the active intelligence picture |
| Archived | Historical record retained for analysis |

Thresholds depend on target mobility, sensor type, source reliability and scenario settings. Moving-target location estimates generally lose value faster than fixed-installation reports.

Position, classification, identity and movement confidence may decay at different rates. Expiry means the side no longer has sufficiently current information; it does not mean the target no longer exists. Retained historical reports remain available when history retention is enabled.

## 13. False Contacts and Contradictions

The system must support false positives and false negatives, with probabilities and behaviours appropriate to each sensor domain.

A false positive may create an unconfirmed contact; it must not automatically create a fully identified enemy. When observations conflict, the system should consider sensor error, target movement, misclassification, separate targets, interference, deception or unresolved uncertainty.

Electronic deception and decoys are supplied by the appropriate systems. Sensors and Detection resolves resulting observations and contact consequences; it must not independently invent the physical effect of a countermeasure.

## 14. Communications and Reporting

Local detection does not guarantee theatre-wide knowledge.

Distinguish:

- Local observation.
- Local contact update.
- Report generated.
- Report transmitted.
- Report received.
- Report incorporated into the recipient's intelligence picture.

The Communications System governs latency, bandwidth, connectivity, disruption and recipient eligibility. Reporting doctrine may prioritise new aircraft contacts, hostile forces near objectives or high-confidence reports without requiring player approval for every routine message.

The UI must display only information available to the relevant side. It must not reveal hidden authoritative enemy coordinates through markers, tooltips, logs or ordinary debugging interfaces.

## 15. Research Integration

Research unlocks and improves explicit capabilities rather than applying a universal detection bonus.

Possible research areas include optical quality, radar transmission and reception, signal processing, acoustic detection, sonar, ESM, classification, track fusion, calibration, environmental compensation and electronic counter-countermeasures.

Research effects may change defined properties such as detection performance under specified conditions, operating range, classification quality, tracking stability, interference resistance, processing speed, reliability, size, mass, power demand or maintenance burden.

Technology generations must be compared within relevant domains, not treated as universal scores across unrelated technologies.

A technology unlock does not automatically install new equipment on existing units. Design, production, delivery, installation or refit, and operational support remain separate steps.

## 16. Armaments Builder Integration

The Armaments Builder defines sensor designs and installation choices. Sensors and Detection evaluates the installed design under actual operational conditions.

Depending on sensor type, design parameters may include:

- Domain and operating method.
- Frequency or band.
- Transmitter and receiver characteristics.
- Antenna or transducer properties.
- Signal processing and classification capabilities.
- Operating modes and emission profile.
- Power, cooling, mass and volume.
- Reliability and maintenance burden.
- Technology prerequisites and manufacturing cost.

Installation must respect platform mass, volume, power, cooling, placement, crew, maintenance and interference constraints. A design that exceeds platform limits must be rejected or require a compatible platform modification.

Design performance is not guaranteed operational performance. Damage, calibration, power, maintenance, crew effectiveness, environment and EW may degrade it. The interface should expose meaningful trade-offs without requiring every player to optimise every engineering variable.

## 17. Operating Modes and Emissions

Sensor modes must be defined per sensor type. Common concepts include active, passive, standby and disabled; not every sensor supports every mode.

Active sensors transmit energy or a signal and may reveal their emissions to opposing sensors. The range at which an emission is detected is not necessarily the range at which the sensor detects a target.

Passive sensors receive without intentionally transmitting the sensing signal in question. Passive operation does not make the host platform invisible if it emits through radios, other sensors, propulsion or other observable activity.

EMCON policies govern which emitters operate and when. Sensors and Detection consumes operating states; Electronic Warfare and the emission-detection interface resolve applicable opposing observations and effects.

## 18. Scheduling and Performance

Use scheduled sensor updates combined with event-triggered recalculation. Relevant events include sensor mode changes, plausible target movement, material geometry changes, major environmental changes, damage, EW effects and new reports.

Update intervals must be configurable by sensor class and operating mode. Do not resolve every sensor every simulation frame by default.

Avoid testing every sensor against every entity. First filter candidates by domain, operating state, plausible range and spatial proximity. Apply more expensive geometry, signature and propagation calculations only to plausible candidates.

The World and Terrain systems should provide reusable spatial queries. The 100-metre movement grid does not imply that every detection calculation must be performed independently for every grid cell.

## 19. Initial Data Contracts

The following is a conceptual contract to align with the project's canonical schema conventions.

```text
SensorDefinition
    sensor_definition_id
    name
    sensor_domain
    sensor_method
    technology_prerequisites[]
    detection_model_id
    supported_modes[]
    supported_signature_domains[]
    operating_requirements
    emission_profile
    classification_capabilities
    tracking_capabilities
    design_version

SensorInstance
    sensor_instance_id
    sensor_definition_id
    host_entity_id
    operational_status
    damage_state
    calibration_state
    operator_effectiveness
    power_state
    current_mode
    current_emission_state
    maintenance_state

TargetSignatureProfile
    signature_profile_id
    target_entity_id
    signature_domain
    baseline_signature
    aspect_modifiers
    operating_state_modifiers
    movement_modifiers
    concealment_modifiers
    environmental_modifiers
    signature_version

Observation
    observation_id
    source_sensor_id
    source_entity_id
    observing_side_id
    observation_time
    sensor_domain
    observation_result
    estimated_position?
    position_uncertainty?
    detection_confidence
    classification_estimate?
    classification_confidence?
    source_quality
    provenance

ContactTrack
    contact_track_id
    owning_side_id
    contact_status
    first_observed_time
    last_observed_time
    last_updated_time
    estimated_position?
    position_uncertainty?
    estimated_movement?
    estimated_target_class?
    estimated_identity?
    classification_confidence
    identification_confidence
    track_quality
    contributing_observation_ids[]

IntelligenceReport
    report_id
    recipient_side_id
    recipient_entity_id?
    source_observation_ids[]
    source_track_id?
    report_time
    source_time
    reported_position?
    reported_uncertainty?
    reported_classification?
    reliability
    age_status
    delivery_status
```

### 19.1 Schema invariants

1. Every persistent entity has a stable identifier.
2. References resolve to canonical records.
3. Observations preserve source and observation time.
4. Contact tracks are not authoritative physical entities.
5. Position uncertainty, classification confidence, identification confidence and track quality remain distinct.
6. Unknown, zero, false and inapplicable values are not conflated.
7. Sensor-specific fields do not become mandatory for unrelated sensor types.
8. Track merges and splits preserve provenance.
9. The player-facing information model does not expose hidden authoritative target state.
10. Research and equipment effects are applied once, through defined interfaces.

## 20. Public Interfaces

### World and Terrain

Provide authoritative spatial data, elevation, spatial queries, obstruction and relevant terrain effects.

### Unit State

Provide position, movement, activity, posture, condition, damage and relevant target-state inputs.

### Equipment

Resolve sensor definitions and installations, operational characteristics, power, damage, calibration, maintenance and target signature profiles.

### Research

Resolve technology prerequisites and research-enabled capabilities without duplicating or stacking the same effect unintentionally.

### Electronic Warfare

Provide applicable interference, jamming, emission-detection and deception effects through explicit contracts. Preserve the distinction between detection degradation and weapon endgame countermeasures.

### Communications

Accept reports, resolve recipient eligibility and delivery, and return delay or failure outcomes while preserving source time and provenance.

### Combat

Provide authorised contact information and track quality to engagement logic. Combat decides whether the available information supports an engagement and resolves combat outcomes; Sensors and Detection does not decide weapon hits or damage.

## 21. Player Interface

The contact display should communicate estimated classification, location, confidence, freshness, source and uncertainty without implying unsupported precision.

Provide standard, advanced and engineering views:

- **Standard:** contact status, estimated type, freshness, confidence and sensor status.
- **Advanced:** sensor mode, range, geometry, environmental effects, uncertainty and report provenance.
- **Engineering:** detailed equipment specifications, signature models, processing parameters, research effects, power and diagnostics.

When useful, show reasons for poor detection, such as range, obstruction, poor visibility, weak signature, inactive or damaged sensor, power shortage, incompatible band, interference or insufficient evidence. Explanations must not reveal hidden enemy state.

## 22. Acceptance Criteria

### Detection fundamentals

- A target outside the valid detection envelope is not detected unless an explicitly supported mechanism permits it.
- Nominal range does not guarantee detection.
- Opaque obstruction blocks direct visual observation.
- Partial concealment modifies detection when specified by the sensor model.
- Different sensors may produce different outcomes against the same target.
- Opening or refreshing UI does not reroll detection.

### Asymmetry and uncertainty

- A unit can detect an enemy without being detected in return.
- Detection of one observer does not automatically reveal all observers.
- Detection may occur without classification or identification.
- Position, classification and track confidence are independent.
- False contacts and plausible misclassifications can occur where enabled.

### Tracking and ageing

- Qualifying observations create or update tracks.
- Uncertain association does not force a merge.
- Duplicate reports do not count as independent evidence.
- Track merge and split preserve history.
- Ageing advances with simulation time.
- Moving-target position reports can lose value faster than fixed-location reports.
- Expiry removes a contact from the active picture without implying target destruction.

### Information security and communications

- Local observations do not automatically reach every friendly unit.
- Reports are shared only through permitted information paths.
- Report delivery preserves source time.
- The normal player interface never exposes hidden authoritative enemy coordinates.

### Research and equipment

- Installed capabilities follow the selected design and operational state.
- Research unlocks only explicitly defined capabilities.
- New research does not silently upgrade existing equipment.
- Damage and power loss affect sensors as specified.
- EW effects enter through the defined interface.

### Performance and determinism

- Spatial filtering prevents unnecessary all-to-all sensor evaluation.
- Outcomes are reproducible under project determinism rules.
- Sensor update intervals are configurable.
- Simulation performance is tested at representative operational scale.

## 23. Implementation Sequence

**Phase 1: Core visual detection.** Implement sensor definitions, installations, range, line of sight, terrain obstruction, basic concealment, probabilistic detection, observations, contact tracks, ageing and side-specific fog of war.

**Phase 2: Intelligence quality.** Add classification, identification, uncertainty, track association, provenance, stale and expired contacts, reporting and automated tests.

**Phase 3: Sensor diversity.** Add radar, acoustic detection, passive listening, ESM and sensor-specific signatures and modes.

**Phase 4: Research and Armaments Builder.** Add equipment designs, installation constraints, technology prerequisites, operational degradation and maintenance dependencies.

**Phase 5: Fusion and Electronic Warfare.** Add multi-source fusion, jamming, deception, emission geolocation and advanced processing.

**Phase 6: Advanced physical models.** Add higher-fidelity radar propagation, underwater acoustics and other advanced models where they materially improve gameplay.

## 24. Explicit Decisions

1. Authoritative World state and intelligence estimates are separate.
2. Detection, classification, identification, tracking and reporting are distinct.
3. Detection is asymmetric and probabilistic where appropriate.
4. Sensor domains use distinct detection models and signature units.
5. World and Terrain own geometry and obstruction.
6. Observations preserve source, time and uncertainty.
7. Contact ageing is a simulation mechanic.
8. Research improves explicit capabilities, not a universal sensor score.
9. The Armaments Builder defines designs; operational state determines actual performance.
10. Communications govern report distribution.
11. Electronic Warfare provides jamming, deception and emission effects through defined interfaces.
12. The initial playable version prioritises visual detection and imperfect intelligence.
13. The architecture must support research from the 1955 starting state through at least 2026 technologies, with future extension possible.
14. New military branches and unit classes must use the common sensor and contact architecture.

## 25. Definition of Done

The system is ready for integration when core entities and contracts are implemented; visual detection uses authoritative World and Terrain state; contact information is separate from hidden truth; classification, uncertainty and ageing work independently; reports use the Communications interface; automated tests cover core behaviour; the player can understand contact uncertainty; UI activity does not affect simulation outcomes; and additional sensor domains can be added without rewriting contact tracking.
