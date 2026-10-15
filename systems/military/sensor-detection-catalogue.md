# Sensor and Detection Data Catalogue

**Project:** Project Sovereign  
**System:** Military Simulation  
**Document ID:** MIL-SENS-DATA-001  
**Status:** Initial catalogue specification, draft 1.0  
**Canonical path:** `systems/military/sensor-detection-catalogue.md`  
**Starting historical date:** 1955  
**Research horizon:** Through the 2026 technology state for the current development plan  
**Depends on:** `sensors-and-detection.md`, `electronic-warfare.md`, Equipment, Research, World, Terrain, Weather and Environment

---

## 1. Purpose and Scope

This document defines the initial data catalogue structure and the first sensor families needed to make Sensors and Detection playable. It is a data specification, not an exhaustive list of every historical sensor model.

The catalogue must support equipment available in a 1955 starting scenario, subsequent technological development, alternate-history research and custom designs created through the Armaments Builder.

It must distinguish:

1. **Sensor family:** A functional class, such as optical observation, air-search radar, passive sonar or ESM receiver.
2. **Sensor definition:** A reusable equipment design with specified technical properties.
3. **Sensor instance:** An installed or deployed copy of a design, with its own condition and operating state.
4. **Research capability:** A technology that unlocks or improves specific design properties.
5. **Detection model:** The domain-specific rules that calculate observations from a sensor, target, environment and geometry.
6. **Target signature profile:** The domain-specific observability of a target in its current configuration and state.

A family is not a technology level, and a technology level is not a guaranteed detection result.

## 2. Catalogue Rules

### 2.1 Starting scenario and progression

The default campaign starts in 1955. Initial designs and available research should be consistent with that scenario's chosen historical baseline. The game must support researchable technology through at least the 2026 state for the current development plan.

The catalogue must not assume that a technology becomes available automatically on a particular calendar date. Availability is determined by scenario rules, research prerequisites, national capabilities, design requirements, production, and operational fielding.

### 2.2 Data-driven design

Adding a new sensor model should normally require data, a compatible detection model and validation tests, not new bespoke logic tied to a particular military unit class.

Sensor families should be reusable across branches. A fixed radar station, a shipboard radar and a vehicle-mounted radar may share a detection family while differing in design, installation, geometry, power, mobility, environmental limits and operational state.

### 2.3 Units and measurement

All numerical properties require explicit units and documented ranges. Use canonical World distance and time units in simulation interfaces. Recommended conventions:

- Distance: metres internally.
- Time: simulation time units with explicit conversion rules.
- Frequency: hertz internally or a canonical scaled unit with explicit conversion.
- Power: watts internally or a canonical scaled unit with explicit conversion.
- Mass: kilograms internally.
- Angle: a single documented canonical unit.
- Confidence: a documented normalized scale, such as 0.0 to 1.0.
- Signature values: domain-specific scales, never interchangeable across domains.

Human-readable displays may use kilometres, kilometres per hour, MHz, GHz or other convenient units, but conversion must occur at the presentation boundary.

### 2.4 Avoid false precision

Initial catalogue values should be calibrated for playable relative behaviour, not presented as exact real-world performance unless supported by the project's reference data.

Every provisional value must be marked as provisional in data metadata. Use test fixtures and balance scenarios to tune detection behaviour. Do not silently interpret a nominal maximum range as guaranteed detection range.

### 2.5 Design values versus operational state

A definition stores design capabilities. An instance stores condition and current operating state. The runtime model then evaluates actual performance from the design, instance, target, geometry, environment and opposing effects.

Do not duplicate mutable operational values in both the definition and instance.

## 3. Canonical Catalogue Schema

The following is the minimum conceptual schema for each sensor definition.

```yaml
sensor_definition_id: sensor.example_family.design_001
name: Example Sensor
sensor_family_id: sensor_family.example
sensor_domain: visual
sensor_method: optical_observation
technology_prerequisites: []
historical_reference_date: null
scenario_availability_tags: []
research_unlocks: []
supported_modes:
  - passive
supported_signature_domains:
  - visual
detection_model_id: detection_model.visual_baseline
classification_capabilities:
  maximum_classification_level: general_category
tracking_capabilities:
  bearing_only: false
  position_estimate: true
operating_requirements:
  power_w: null
  crew_required: true
  minimum_operator_effectiveness: null
physical_properties:
  mass_kg: null
  volume_m3: null
  installation_constraints: []
emission_profile:
  intentionally_emits: false
  emission_types: []
environmental_susceptibility: {}
reliability:
  base_availability: null
design_version: 1
data_status: provisional
```

Fields with `null` values must be completed before a design is eligible for final production balancing. A prototype record may remain incomplete during content development, but runtime systems must not guess missing units or treat missing data as zero.

### 3.1 Required identifiers

Use stable, namespaced IDs. Examples:

- `sensor_family.visual_binoculars`
- `sensor_family.radar_air_search`
- `sensor_family.esm_receiver`
- `sensor_definition.radar_air_search.design_001`
- `detection_model.visual_baseline`
- `signature_domain.radar`

Names are presentation data; IDs are references. Renaming a sensor must not break installed equipment, research prerequisites, saves or scenario files.

### 3.2 Detection model contract

Every sensor definition must reference a detection model compatible with its domain. The model should declare:

- Candidate eligibility rules.
- Required geometry and propagation inputs.
- Supported signature domains.
- Relevant environmental inputs.
- Detection probability calculation.
- Classification and location outputs.
- Supported interference or EW inputs.
- Update interval requirements.
- Calibration parameters and test fixtures.

A detection model must not silently read or expose hidden target data beyond the inputs permitted by the simulation architecture.

## 4. Sensor Families

The first playable catalogue should cover these families.

| Family ID | Family | Primary use | Initial priority |
|---|---|---|---|
| `sensor_family.visual_observation` | Unaided visual observation | Close and medium-range observation in suitable conditions | Phase 1 |
| `sensor_family.optical_instruments` | Binoculars and optical instruments | Improved observation and classification | Phase 1 |
| `sensor_family.optical_rangefinding` | Optical rangefinding | Range estimation and improved location | Phase 1/2 |
| `sensor_family.air_search_radar` | Air-search radar | Detecting airborne targets | Phase 3 |
| `sensor_family.surface_search_radar` | Surface-search radar | Detecting surface targets | Phase 3 |
| `sensor_family.fire_control_radar` | Fire-control radar | Precise track/illumination support where applicable | Phase 3 |
| `sensor_family.ground_surveillance_radar` | Ground-surveillance radar | Detecting moving ground targets where supported | Phase 3/4 |
| `sensor_family.passive_acoustic` | Passive acoustic observation | Detecting acoustic sources | Phase 3 |
| `sensor_family.passive_sonar` | Passive sonar / hydrophones | Underwater acoustic detection | Phase 3 |
| `sensor_family.active_sonar` | Active sonar | Active underwater search | Phase 3 |
| `sensor_family.esm_receiver` | ESM receiver | Detecting and analysing electromagnetic emissions | Phase 3 |
| `sensor_family.radio_direction_finding` | Radio direction finding | Estimating bearings to transmissions | Phase 3/4 |
| `sensor_family.infrared` | Infrared sensing | Detecting thermal contrast | Later |
| `sensor_family.image_intensification` | Image intensification | Improved low-light visual observation | Later |
| `sensor_family.electro_optical_tracking` | Electro-optical tracking | Enhanced observation and tracking | Later |
| `sensor_family.multisensor_fusion` | Sensor fusion capability | Combining evidence from multiple sources | Phase 2 onward |

Family priority is an implementation order, not a claim that the technology was first invented in that phase or that all equipment of a family is available in 1955.

## 5. Initial 1955 Catalogue Baseline

The 1955 starting catalogue should be built from the following functional equipment groups. Exact named models and performance values should be added only after the reference catalogue and historical sources have been audited.

### 5.1 Human visual observation

**Family:** `sensor_family.visual_observation`  
**Method:** Direct visual observation  
**Mode:** Passive  
**Primary domains:** Visual  
**Typical outputs:** Detection, approximate bearing, broad classification, estimated location when geometry and observer capability support it.

Inputs include distance, target visual signature, terrain obstruction, vegetation, built-up areas, weather, lighting, target posture and movement, camouflage, observer condition and sighting duration.

The baseline should support ordinary observers, lookout posts and observers operating from elevated positions. Operator quality and posture should affect performance without turning every soldier into a separate expensive sensor entity if the game's unit abstraction does not require it.

### 5.2 Optical instruments

**Family:** `sensor_family.optical_instruments`  
**Method:** Magnified optical observation  
**Mode:** Passive  
**Primary domains:** Visual  
**Typical outputs:** Improved detection or classification under suitable conditions.

Binoculars and similar instruments should improve useful observation in their supported conditions. Magnification does not remove terrain obstruction, darkness, fog or atmospheric limitations.

The initial catalogue may represent common field glasses as a family-level definition and allow the Armaments Builder to differentiate optical quality, magnification, field of view, mass, durability and handling.

### 5.3 Optical rangefinding

**Family:** `sensor_family.optical_rangefinding`  
**Method:** Optical range estimation  
**Mode:** Passive  
**Typical outputs:** Improved range estimate to a visible target.

Rangefinding requires an observable target and suitable operating conditions. It improves location estimation but does not independently make an otherwise undetectable target visible.

### 5.4 Air-search radar

**Family:** `sensor_family.air_search_radar`  
**Method:** Active radar  
**Mode:** Active, with standby/off states where supported  
**Primary domains:** Radar  
**Typical outputs:** Detection, estimated range/bearing, track updates according to equipment capability.

The initial baseline should include historically appropriate air-search radar capability for fixed installations and relevant naval or other platforms where scenario content supports them. Exact platform compatibility belongs to equipment and installation data.

Air-search definitions must specify supported frequency bands, transmitter/receiver properties, antenna coverage, rotation or update behaviour where relevant, target signature sensitivity, power demand, installation constraints, classification capability and emission profile.

### 5.5 Surface-search radar

**Family:** `sensor_family.surface_search_radar`  
**Method:** Active radar  
**Primary domains:** Radar  
**Typical outputs:** Detection and tracking of supported surface targets.

Surface-search equipment requires its own target and clutter calibration. It must not inherit air-search behaviour unchanged. Horizon, sea or land clutter, target aspect and installation geometry may materially affect outcomes.

### 5.6 Fire-control radar

**Family:** `sensor_family.fire_control_radar`  
**Method:** Active radar or another explicitly supported tracking method  
**Primary domains:** Radar  
**Typical outputs:** Higher-quality track and support for compatible engagement procedures.

A fire-control radar is not simply a long-range search radar with a larger number. It has distinct tracking, update, precision and engagement-support requirements. Combat decides whether the resulting track is sufficient for a particular weapon or engagement.

### 5.7 Passive acoustic observation

**Family:** `sensor_family.passive_acoustic`  
**Method:** Passive acoustic reception  
**Mode:** Passive  
**Primary domains:** Acoustic  
**Typical outputs:** Acoustic event detection, bearing or approximate source area, source classification where supported.

The initial implementation can use a simplified environmental propagation model, but it must include ambient noise, source signature, range, terrain or medium, observer/equipment quality and target activity as relevant.

### 5.8 Passive sonar and hydrophones

**Family:** `sensor_family.passive_sonar`  
**Method:** Passive underwater acoustic reception  
**Mode:** Passive  
**Primary domains:** Underwater acoustic  
**Typical outputs:** Detection, bearing, approximate track and classification depending on equipment and observations.

The model must distinguish bearing-only detection from a well-localised track. Water and seabed conditions, source noise, target speed, sensor placement and geometry may affect results.

### 5.9 Active sonar

**Family:** `sensor_family.active_sonar`  
**Method:** Active underwater acoustic transmission and reception  
**Mode:** Active  
**Primary domains:** Underwater acoustic  
**Typical outputs:** Detection and tracking under supported conditions.

Active sonar creates an acoustic emission that can itself be observed by suitable opposing sensors. Its detection behaviour must use the underwater model, not terrestrial visual LOS.

### 5.10 ESM receiver

**Family:** `sensor_family.esm_receiver`  
**Method:** Passive electromagnetic reception and analysis  
**Mode:** Passive  
**Primary domains:** Electromagnetic emission  
**Typical outputs:** Emission detection, emitter classification, bearing or estimated location according to capability.

The initial catalogue should support receiver band coverage, sensitivity, signal analysis, bearing accuracy, processing capability and operating restrictions. ESM observations must feed the shared observation/contact model.

### 5.11 Radio direction finding

**Family:** `sensor_family.radio_direction_finding`  
**Method:** Passive direction finding  
**Mode:** Passive  
**Primary domains:** Electromagnetic emission  
**Typical outputs:** Bearing to an eligible transmitter.

A bearing is not a precise position. Position estimation requires suitable geometry, repeated observations, multiple receivers or other evidence.

## 6. Technology Progression: 1955 to 2026

The catalogue should model technological progress through explicit capability families. The list below is a research-planning structure, not a rigid calendar-based tech tree.

### 6.1 Optical and visual observation

Potential progression:

- Improved optics, coatings and range estimation.
- Better low-light observation.
- Image intensification.
- Thermal and infrared imaging.
- Stabilised observation and tracking.
- Digital image processing and assisted classification.
- Integrated multisensor electro-optical systems.

### 6.2 Radar

Potential progression:

- Improved transmitters, receivers and antennas.
- Better clutter rejection and signal processing.
- Improved tracking and target discrimination.
- Frequency agility and other electronic protection capabilities.
- Phased-array architectures, where researched and supported.
- Digital beamforming and advanced signal processing.
- Improved low-observable target detection under specific conditions.
- Networked and cooperative sensor processing.

These are capability categories, not universal superiority guarantees. Newer radar technologies may improve particular functions while carrying cost, power, cooling, size, complexity or other trade-offs.

### 6.3 Acoustic and sonar

Potential progression:

- Improved hydrophones and acoustic transducers.
- Better signal processing and noise rejection.
- Improved bearing estimation and track maintenance.
- Towed or distributed acoustic arrays where supported by the relevant platform.
- Improved environmental modelling.
- Digital classification and multisensor fusion.

### 6.4 ESM and electronic sensing

Potential progression:

- Wider or more sensitive reception.
- Improved signal analysis and emitter classification.
- Better direction finding and geolocation.
- Improved processing of complex or agile emissions.
- Improved integration with shared intelligence and tracking.
- Advanced counter-countermeasure and signal discrimination capabilities.

### 6.5 Sensor fusion

Potential progression:

- Manual or local report correlation.
- Automated multi-source track association.
- Improved uncertainty estimation.
- Networked track sharing.
- Advanced fusion and assisted classification.
- Better handling of contradictory, deceptive or low-confidence evidence.

Research should unlock capabilities and properties, not simply assign a later calendar year to a larger universal detection bonus.

## 7. Target Signature Catalogue

Sensor definitions are only half of the model. Detection also requires target signature profiles.

The initial signature catalogue should support the following domains:

| Signature domain | Examples of target properties | Main consumers |
|---|---|---|
| Visual | Contrast, apparent size, camouflage, aspect, posture | Visual and optical sensors |
| Radar | Radar signature by aspect/configuration and relevant operating state | Radar sensors |
| Acoustic | Source level and acoustic characteristics by activity | Acoustic sensors and sonar |
| Infrared | Thermal contrast and operating-state effects | Infrared sensors |
| Electromagnetic emissions | Active emitters, frequency and emission characteristics | ESM receivers |
| Other future domains | Domain-specific observability | Compatible future sensors |

A target may have multiple profiles, and a profile may vary with equipment configuration, movement, damage, operating mode, environment or aspect.

### 7.1 Signature state changes

The system must allow target signatures to change when:

- Equipment is switched on or off.
- A vehicle or platform changes operating state.
- A target moves or performs an observable activity.
- Camouflage or concealment changes.
- A platform is damaged or changes configuration.
- Environmental conditions materially alter observability.

Changes must be sourced from the owning system. Sensors and Detection must not independently invent a target's physical state.

## 8. Calibration Parameters and Runtime Behaviour

Every detection model must expose a small, documented set of tunable parameters. Avoid burying balance constants in source code.

Example parameter groups:

- Baseline detection curve.
- Range response.
- Signature response.
- Geometry and obstruction response.
- Environmental modifiers.
- Posture and activity modifiers.
- Sensor condition and operator effectiveness.
- Technology effects.
- False-positive rate where applicable.
- Classification and identification thresholds.
- Track update and ageing parameters.

Each parameter requires a definition, unit or scale, valid range, default, owning model and test coverage.

### 8.1 Provisional balance values

The catalogue may use provisional normalized values during early development, but every such value must be labelled provisional and stored in data rather than hard-coded into simulation logic.

The development team should create controlled test fixtures such as:

- A visible target at short range in open terrain.
- The same target partially concealed.
- The same target behind opaque terrain.
- A moving target under poor visibility.
- A radar-compatible target at increasing range.
- A radar target under supported interference.
- An emitting target observed by compatible and incompatible ESM receivers.
- A submarine or underwater target under several acoustic conditions.

Calibration should be based on relative expected behaviour and scenario playtests. Do not present provisional values as real-world equipment specifications.

## 9. Research and Design Data Contracts

A research definition should specify which capabilities it unlocks or modifies.

Conceptual research contract:

```yaml
research_id: research.example.sensor_processing
name: Example Sensor Processing Advance
prerequisites: []
unlocks_sensor_definitions: []
unlocks_capabilities:
  - capability.example.processing
modifies_properties: []
eligible_sensor_families:
  - sensor_family.example
effect_stacking_rule: explicit
historical_context:
  earliest_reference_year: null
data_status: provisional
```

The Armaments Builder must validate that the design's selected components and capabilities are unlocked and compatible. Production must validate industrial feasibility and available resources. The unit/platform system must validate installation limits. Logistics and maintenance determine continued operational availability.

Research, design, production, delivery, installation and operation are separate lifecycle stages.

## 10. Data Validation Rules

A catalogue entry must fail validation if:

- Its identifier is missing or duplicated.
- Its sensor family or detection model does not exist.
- It uses an undefined sensor domain or signature domain.
- A numerical field lacks a documented unit or scale.
- A supported mode is invalid for the sensor family.
- Required operating properties are missing for a production-eligible design.
- A research prerequisite references a nonexistent technology.
- An installation would violate declared physical constraints.
- A detection model requests data that its system contract does not permit.
- An emission profile contradicts the sensor's defined operating modes.
- A design's capabilities are automatically granted to an existing instance without an explicit upgrade or refit process.

Provisional content may be retained during development, but it must be distinguishable from validated production data.

## 11. Acceptance Criteria

- A 1955 scenario can load its initial sensor catalogue without exposing later technologies as already fielded.
- Later technologies can be researched through the planned 2026 horizon when their prerequisites are met.
- Technology availability is not determined solely by calendar year.
- A sensor definition can be instantiated on compatible platforms without bespoke unit-class code.
- Different sensor families produce distinct observation types and use appropriate detection models.
- The same target can have different observability across signature domains.
- A design's operational performance changes with condition, environment, geometry and relevant EW effects.
- Research unlocks explicit capabilities without automatically upgrading existing equipment.
- Missing or invalid catalogue values are caught by validation.
- Provisional calibration values are clearly identified and can be tuned without changing simulation code.
- Contact reports use the shared Sensors and Detection contract.
- New sensor families can be added without rewriting the ContactTrack schema.

## 12. Recommended Implementation Sequence

1. Establish canonical sensor IDs, signature domains, data validation and definition/instance separation.
2. Implement visual observation and optical instruments with a simple calibrated detection model.
3. Implement observations, contact tracks, uncertainty and ageing against the Sensors and Detection specification.
4. Add radar and ESM definitions with emissions interfaces.
5. Add acoustic and sonar families with domain-specific models.
6. Add research prerequisites and Armaments Builder integration.
7. Expand the catalogue with researched capabilities through 2026.
8. Add further sensor families and higher-fidelity physics only when supported by validated data and gameplay requirements.

## 13. Definition of Done

The catalogue is ready for initial content production when canonical IDs and validation rules are implemented; the first sensor families have complete production-eligible definitions; detection models declare their required inputs and outputs; the 1955 scenario baseline is separated from later research; technology progression can reach at least the 2026 planning horizon; and tests prove that sensor definitions, instances, research, target signatures and contact tracks remain correctly separated.

**Design objective:** The catalogue should let Project Sovereign grow from a 1955 military simulation into a long-horizon technological sandbox. Equipment can become more capable through research and design, but detection remains a contest between sensor capability, target signature, geometry, environment, operating condition and the quality of available evidence.
