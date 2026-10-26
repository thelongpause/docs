# Equipment and Material System

**Canonical Path:** \`systems/military/equipment-and-material-system.md\`  
**System:** Military Systems  
**Document Type:** Behavioural Specification  
**Document ID:** MIL-EQM-001  
**Status:** Canonical behavioural specification; detailed reference catalogues, numerical parameters, cross-system interfaces, and logical schema remain subject to review  
**Starting World Date:** 1 January 1955

---

## 1. Purpose

The Equipment and Material System defines the identity, technical characteristics, configuration, compatibility, operational capability, and lifecycle of military equipment and materiel.

It supports individual weapons, ammunition, protective equipment, uniforms, communications equipment, sensors, vehicles, aircraft, ships, engineering equipment, tools, spare parts, fuel-related equipment, and other military items. It also defines the relationships between major platforms, installed systems, components, weapon mounts, stores, loadouts, and the personnel qualifications required to operate them.

Equipment must not be reduced to a generic attack, defence, or hit-point value. Its performance emerges from technical design, configuration, ammunition or material compatibility, condition, crew and operator proficiency, training, technology, environment, and the relevant tactical or operational context. Specialist systems calculate actual combat, detection, movement, electronic warfare, logistics, and other effects using equipment data and current state.

This specification defines behavioural contracts and domain boundaries. It does not yet freeze the full equipment catalogue, all technical equations, every historical variant, every ammunition type, or the final relational schema.

## 2. Design Principles

1. **Canonical identity.** Every catalogue-defined item resolves to a stable canonical Catalogue identity. Equipment-specific data extends that identity; it does not create an unrelated duplicate product identity.
2. **Definition is not instance.** An equipment definition describes a type or variant. An equipment instance is a particular physical platform or serialised asset with its own condition, location, configuration, and history.
3. **Technical properties are not combat outcomes.** Equipment provides inputs and constraints. Combat, Detection, Movement, Electronic Warfare, and other specialist systems calculate their own effects.
4. **Design matters.** Weapon, ammunition, armour, sensor, propulsion, communications, and platform characteristics must be represented at the level needed by the simulation, rather than hidden behind a single arbitrary equipment score.
5. **Compatibility is explicit.** A weapon does not accept every ammunition type; a radio does not communicate with every network; a platform does not accept every mount, store, or component. Compatibility must be declared and validated.
6. **Configuration is distinct from availability.** A valid equipment design may exist in the catalogue but be unavailable to a nation because of date, research, production, export restrictions, procurement, stock, or scenario rules.
7. **Technology is dated and contextual.** Equipment, components, ammunition, software or control systems, and specialist capabilities must be available only under applicable scenario and research rules.
8. **Material identity is distinct from stock.** This system defines what an item is and how it behaves. Inventory owns physical stock, lot quantities, reservations, custody, transfers, and consumption.
9. **Unit assignment is distinct from ownership and inventory.** Assigning an item to a unit or platform does not create stock or duplicate an asset.
10. **Condition affects performance.** Damage, wear, maintenance state, reliability, and configuration can change an item's effective capability.
11. **Crew and training matter.** A technically capable item may be unusable or less effective without the required trained personnel, qualifications, communications, and supporting systems.
12. **Historical interpretation is reproducible.** Equipment definitions and configuration versions used in a saved world or committed event remain identifiable even if the catalogue is later revised.
13. **No double-counted supply abstraction.** Abstract logistics demand and exact physical item quantities may coexist only when their accounting relationship is explicit. A generic stock unit must not be treated as additional physical ammunition or material on top of the exact items it represents.
14. **Examples are not canonical data.** Illustrative loadouts and historical examples are design aids until validated against the scenario's date, national availability, and source data.

## 3. Scope

### 3.1 Included

- Military equipment and materiel definitions and variants.
- Equipment-specific extensions to canonical Catalogue entries.
- Major platforms and serialised equipment instances.
- Component, subsystem, mount, store, and installed-equipment relationships.
- Weapon, ammunition, armour, sensor, communications, mobility, propulsion, and platform technical characteristics.
- Compatibility and configuration validation.
- Equipment sets, unit establishment requirements, platform loadouts, and reusable configurations.
- Crew/operator qualification requirements and interfaces to Military Personnel and Training.
- Technology, date, research, and nation-specific availability.
- Serviceability, condition, damage, wear, reliability, and maintenance requirements at the equipment level.
- Equipment issue, assignment, installation, removal, replacement, and return interfaces.
- Historical configuration versioning and provenance.
- Interface contracts with Inventory, Production, Research, Military Units, Combat, Detection, Electronic Warfare, Movement, Logistics, Installations and Facilities, and the Good and Service Catalogue.

### 3.2 Excluded or owned elsewhere

- Canonical goods/services identity and units of measure: Good and Service Catalogue.
- Physical stock, lot quantities, reservations, custody, transfers, and consumption: Inventory.
- Production recipes, processes, production orders, and actual output: Production and Production Processes and Recipes.
- Industry capabilities, economic state, company accounts, and financial transactions: Economy and Industry Data.
- Research progression and discoveries: Research.
- Military unit identity, organisation, command relationships, training frameworks, and unit readiness: Military Branches and Unit Organisation.
- Individual character identity, biography, and general attributes: Character System.
- Military service records, rank, personnel assignments, qualifications, and crew availability: Military Personnel domain or explicit Military Systems contract.
- Movement, route traversal, terrain sampling, and authoritative continuous platform position: Movement and World Map.
- Weapon firing resolution, hit probability, damage, blast, penetration resolution, casualties, and combat-induced suppression: Combat and Weapons.
- Sensor detection processing, track confidence, signature comparison, and information dissemination: Detection and Intelligence.
- Electronic attack, jamming, emissions, and countermeasure resolution: Electronic Warfare.
- Facility identity, construction, shared condition, and generic capacity: Installations and Facilities.
- Weather and environmental state: Weather and Environment.
- Exact database schema and implementation technology.

## 4. Core Concepts

### 4.1 Catalogue Entry

A Catalogue Entry is the canonical identity of a good or service in the Good and Service Catalogue. Equipment and physical materiel that are traded, produced, stocked, issued, or consumed must reference the appropriate canonical Catalogue ID.

Equipment-specific technical data extends the canonical entry through an explicit relationship. It must not create a competing identity for the same good merely because Military Systems requires additional technical fields.

A service such as maintenance, training, or technical inspection may be relevant to equipment operation but is not itself a physical equipment item.

### 4.2 Equipment Definition

An Equipment Definition describes a type, model, or variant of an equipment item. It references its canonical Catalogue identity and provides technical properties, compatibility, configuration options, availability rules, maintenance requirements, and other applicable data.

Variants must be represented explicitly when their differences affect simulation outcomes. A variant may differ in dimensions, mass, armour, propulsion, weapon fit, sensor fit, communications, reliability, ammunition compatibility, fuel consumption, crew requirement, or other relevant characteristics.

A variant must not be created merely to represent a different name if no meaningful difference is modelled, unless historical or procurement identity requires a separate definition.

### 4.3 Equipment Instance

An Equipment Instance represents a particular physical asset where individual identity matters. Examples include a specific aircraft, ship, tank, vehicle, radar set, or other serialised platform.

An instance may have:
- Stable equipment-instance identity and serial or tail number where applicable.
- Equipment Definition and variant.
- Owner or controlling organisation where modelled.
- Current location and assigned unit or facility.
- Current configuration and installed components.
- Condition, damage, serviceability, wear, and maintenance state.
- Crew or operator assignment references.
- Fuel, ammunition, stores, or other carried quantities through Inventory and loadout interfaces.
- Production, delivery, assignment, maintenance, damage, and retirement history.

Not every rifle, uniform, consumable, or generic spare part requires an individual instance. The model must allow quantity-based stock for non-serialised items and individually tracked instances for assets whose identity and condition matter.

### 4.4 Component and Subsystem

A Component is a separately defined item or subsystem that may be installed in, mounted on, carried by, or integrated into a larger platform. Examples include engines, radios, radar sets, fire-control systems, armour packages, weapon mounts, navigation systems, and other replaceable components.

Components may have their own Catalogue identity, equipment definition, variant, compatibility requirements, condition, and history. The implementation must distinguish physically separate components from abstract capabilities that do not require their own physical stock identity.

The system must support different levels of detail by equipment class. It must not require every bolt or circuit board to be individually modelled unless the game design explicitly demands it.

### 4.5 Platform

A Platform is a major equipment assembly whose movement, operation, damage, or configuration is simulated as a coherent asset. Examples include vehicles, aircraft, ships, and other configured mobile or fixed platforms.

A platform definition specifies the supported architecture and configurable slots or interfaces for components, crew, mounts, weapons, sensors, communications, stores, fuel, and other systems. Platform-specific behaviour is consumed by Movement, Combat, Detection, Electronic Warfare, Logistics, and other domains.

A platform's physical characteristics are not automatically equivalent to the sum of its component properties. The platform definition must specify integration rules, limits, dependencies, and any calculated characteristics.

### 4.6 Weapon System and Ammunition

A Weapon System is an equipment definition or configured assembly that can deliver a defined effect through Combat and Weapons. It may be an individual weapon, a mounted weapon, an artillery system, a missile launcher, a shipboard mount, or an integrated platform weapon.

Ammunition is a distinct physical good with its own canonical Catalogue identity and ammunition-specific technical definition. Ammunition variants may differ in projectile or warhead design, mass, dimensions, propellant, fuse, guidance, range envelope, penetration, fragmentation, blast, incendiary or other effects, storage constraints, shelf life, and compatibility.

A weapon's technical capability and an ammunition item's effect characteristics are separate inputs. Combat and Weapons resolves the actual effect using the applicable weapon, ammunition, target, range, environment, crew, fire-control, and tactical conditions.

### 4.7 Equipment Set and Loadout

An Equipment Set is a reusable proposed configuration of equipment and quantities for a unit, subunit, role, or establishment. It may include weapons, ammunition scales, protective clothing, load-carrying equipment, communications, sensors, tools, spares, and other required items.

A Loadout is a specific configuration assigned to a platform or unit for a particular role, mission, or period. Aircraft loadouts may include weapon stores, fuel tanks, sensors, and mission equipment. Naval configurations may include weapons, magazines, aircraft, mission systems, and other installed or carried assets.

A set or loadout defines intended composition; it does not itself create physical stock, establish ownership, or guarantee that the configuration is available or serviceable.

### 4.8 Material

Material includes physical goods that support equipment operation, maintenance, deployment, or mission execution but may not be a major equipment asset. Examples include ammunition, fuel, food and water where represented as goods, spare parts, lubricants, engineering supplies, medical supplies, batteries, clothing, and repair materials.

Each physical material must use canonical Catalogue identity and the appropriate unit of measure. Inventory remains authoritative for quantity, lot, location, custody, reservation, transfer, and consumption.

A broad category such as "Basics" may be used by a logistics abstraction only if its meaning, accounting basis, and relationship to actual physical goods are explicitly defined. It must not be treated as a physical item with an undefined quantity unit.

### 4.9 Equipment Lot or Batch

An Equipment Lot or Batch groups multiple materially equivalent items that share relevant provenance, production or acquisition history, configuration/version, condition history, or other accounting attributes. Lot identity is useful for ammunition, spare parts, consumables, and equipment categories where individual tracking is unnecessary or too expensive.

A lot is not itself a design, an individual equipment instance, or a stock quantity. Equipment definitions describe what an item is; equipment instances represent individually tracked assets where required; lot records group items for tracking; Inventory owns physical quantity, location, custody, reservations, transfers, and stock-changing transactions. A lot may be split, merged, reclassified, transferred, consumed, captured, or disposed of only through explicit, auditable state transitions that preserve provenance and quantity conservation.

The chosen tracking granularity is category-specific and data-driven. It must not imply that all members of a lot have identical operational condition if the model permits within-lot variation. When condition or provenance diverges beyond the defined threshold, the system may split the affected quantity into a new lot or individually tracked instances.

## 5. Equipment Categories and Data Profiles

The following categories organise technical data. They are not mutually exclusive taxonomic labels and do not determine how detailed each item must be.

### 5.1 Individual Weapons

Definitions may include weapon type, calibre, dimensions, mass, operating mechanism, rate-of-fire envelope, feed system, magazine or belt capacity, barrel characteristics, sights, mounting options, reliability, service life, maintenance requirements, and ammunition compatibility.

Actual accuracy, hit probability, suppression, rate of effective fire, and damage are calculated by Combat and Weapons using these properties plus shooter skill, training, posture, range, visibility, fatigue, suppression, target, and other relevant conditions.

The system should support small arms, crew-served weapons, anti-tank weapons, mortars, artillery, naval weapons, aircraft guns, missile systems, and other configured categories without requiring them to share an identical property set.

### 5.2 Ammunition and Stores

Ammunition definitions may include calibre or interface standard, projectile/warhead type, mass, dimensions, propellant or energy source, fuze, guidance, range-related parameters, penetration or terminal-effect inputs, safety and storage constraints, shelf life, handling requirements, and compatible weapon systems.

The required detail depends on the ammunition class and the effects the simulation intends to resolve. Missing or unknown technical values must be explicitly marked as unknown, estimated, or not modelled; they must not be silently replaced with arbitrary values.

Ammunition expenditure is a physical inventory event when the simulation models exact stock. Combat resolution may request consumption, but Inventory owns the stock mutation and resulting lot balance.

### 5.3 Personal Protective Equipment and Clothing

Definitions may include item type, protection class, coverage, mass, environmental suitability, durability, fit or size categories where modelled, and compatibility with other equipment.

Protective effects must be resolved by the relevant Combat and Weapons or medical/casualty system. A clothing or equipment label must not imply protection against threats for which no protection parameter or rule exists.

Uniforms, helmets, boots, body armour, load-bearing equipment, camouflage, cold-weather clothing, and similar items are physical goods with canonical Catalogue identities. Their effects may depend on fit, issue, condition, training, environment, and compatibility.

### 5.4 Communications and Command Equipment

Communications equipment may define supported bands or channels, frequency ranges, output power, antenna requirements, range conditions, encryption or security capabilities, waveform or protocol compatibility, power supply, portability, weight, reliability, and operator requirements.

Possession of a radio does not guarantee communication. Effective communication depends on compatible equipment and configuration, operator qualifications, terrain, distance, antenna placement, interference, jamming, emissions control, network availability, and other conditions owned by the relevant systems.

Specific communication capabilities, such as ground-to-air coordination or ship-to-unit support requests, require an explicit interface contract and appropriate training, procedures, equipment compatibility, and current operational conditions. Equipment alone does not grant the full capability.

### 5.5 Sensors and Detection Equipment

Sensor equipment may define sensor modality, operating band, aperture or relevant physical parameters, power, resolution, search and tracking modes, supported target classes, field of regard, scan characteristics, processing capability, environmental constraints, emissions, mobility restrictions, and operator requirements.

The technical definition supplies inputs to Detection and Sensors. It does not itself establish that a target has been detected or identified. Actual detection depends on target signatures, geometry, distance, environment, clutter, emissions, operator proficiency, processing, electronic warfare, and sensor employment.

Fixed installations and mobile platforms may use related sensor definitions while differing in mounting, power, integration, staffing, field of view, and operational constraints.

### 5.6 Electronic Warfare and Countermeasures Equipment

Equipment may support electronic support, electronic attack, electronic protection, jamming, deception, emission analysis, warning, or countermeasure functions.

Definitions may specify supported bands, power, bandwidth, antenna or directional characteristics, processing capability, duty cycle, signature, detection sensitivity, countermeasure capacity, and integration requirements. These are technical inputs; Electronic Warfare owns the resolution of actual effects and interactions.

An item must not be assigned EW capability merely because it is categorised as electronic equipment. Capabilities and operating modes require explicit definitions and compatibility.

### 5.7 Vehicles and Ground Platforms

Ground-platform definitions may include dimensions, mass, crew, passenger capacity, cargo capacity, engine and propulsion, speed envelope, fuel type and consumption, range, road and cross-country performance inputs, terrain limits, protection, signature, suspension, reliability, maintenance demand, weapon mounts, sensors, communications, and supported components.

Movement calculates actual travel and terrain response from the platform's characteristics, unit movement state, terrain, route, orders, weather, and other conditions. Equipment data supplies capability and constraint inputs; it does not define an independent cell-to-cell movement model.

Vehicle protection, weapon effects, visibility, signatures, and fuel consumption are resolved by their owning specialist systems using the platform's configuration and state.

### 5.8 Aircraft

Aircraft definitions may include dimensions, mass limits, crew positions, engine/propulsion, speed and altitude envelopes, range, endurance, climb and manoeuvre inputs, fuel capacity and consumption, payload, hardpoints, stores interfaces, sensors, communications, signatures, reliability, maintenance, and operating requirements.

Aircraft loadouts must validate station compatibility, mass and balance limits, electrical or data interfaces where modelled, weapon integration, fuel-tank configuration, and relevant technology. A weapon that physically fits a station is not necessarily certified, integrated, or operationally compatible.

Aircraft and crew are distinct entities. A valid aircraft instance does not imply that qualified pilots, other crew, maintenance staff, or mission operators are available. Crew qualification and current assignment are validated through Military Personnel and Training contracts.

### 5.9 Naval Platforms

Ship definitions may include length, beam, draught, displacement, crew requirements, accommodation, cargo and mission capacity, propulsion, speed and endurance, fuel consumption, sensors, EW, communications, weapon mounts, firing arcs, magazines, reload or replenishment constraints, aviation facilities, signature, damage zones, flooding or stability-related characteristics where modelled, and maintenance requirements.

Naval configurations may include onboard medical, maintenance, supply, command, sensor, aviation, and other capabilities. A ship may provide a support capability to other units only when the capability, capacity, personnel, inventory, access, and operational conditions are valid.

The detailed damage, weapon, sensor, movement, logistics, and facility behaviour of ships belongs to the respective specialist systems. This system defines technical inputs, configuration, component relationships, and capability constraints.

### 5.10 Engineering, Maintenance, Medical, and Support Equipment

Equipment definitions may cover engineering tools, bridging assets, recovery vehicles, construction equipment, medical devices, workshops, diagnostic systems, spare-part kits, and other support items.

Their capabilities must be defined in terms that the relevant receiving system can validate. For example, a maintenance vehicle may provide recovery or repair capability only if its equipment, trained staff, spares, tools, capacity, and access conditions support the task.

A medical device does not itself create a medical unit or qualified clinician. A bridge-laying vehicle does not itself create a completed crossing. Equipment supports actions; the owning system resolves the action and its outcome.

### 5.11 Consumables and General Materiel

Consumable goods may include ammunition, fuel, batteries, food, water, medical consumables, lubricants, spare parts, and other materials. The exact canonical categories and their unit conventions are defined by the Good and Service Catalogue and relevant domain reference data.

Consumable definitions may include handling, storage, compatibility, hazard, shelf-life, environmental limits, and intended use. Quantity, lot condition, stock location, and consumption remain Inventory facts.

## 6. Compatibility and Configuration

### 6.1 Compatibility Rules

Compatibility is a declared relationship, not a guess based on names, calibre labels, or broad category. Rules may cover:
- Weapon and ammunition.
- Platform and weapon mount.
- Platform and sensor or communications component.
- Aircraft station and store.
- Ship mount and ammunition.
- Power, data, mounting, physical dimension, mass, and interface requirements.
- Fuel type and propulsion system.
- Tool, spare part, component, and supported equipment.
- Crew qualification and operating role.
- Equipment and training or doctrine prerequisite.
- Technology, version, and scenario availability.

Compatibility records must identify the applicable equipment definitions or interface standards, validity/version, and any restrictions. If conversion or adaptation is allowed, it must be represented as an explicit supported configuration or modification, not as a silent exception.

### 6.2 Configurable Versus Fixed Equipment

Some items have fixed designs; others support selectable components, modules, weapon fits, stores, or configuration options. Each Equipment Definition must state which configuration dimensions are permitted.

A configuration may be valid, invalid, incomplete, or conditionally valid. The validator must report the missing or incompatible component and the rule that failed. The simulation must not automatically substitute a different weapon, ammunition type, or component unless an explicit substitution rule permits it.

### 6.3 Configuration Versioning

Equipment definitions, compatibility rules, platform configurations, equipment sets, and loadouts must be versioned where changes could alter interpretation or simulation outcomes.

An instance or committed event references the applicable definition/configuration version. A catalogue update must not retroactively alter the historical interpretation of an earlier mission or save unless an explicit migration or balance-update policy requires it.

### 6.4 Capability Derivation

A platform or equipment configuration may expose derived capabilities based on its validated components. These are calculated from canonical definitions, installed components, integration rules, and current condition.

Derived capability must be traceable to its source properties and version. It must not become a second independently edited source of truth. A cached result is permitted only if it can be invalidated or reproduced when its dependencies change.

## 7. Unit Equipment Sets and Platform Loadouts

### 7.1 Unit Equipment Set

A Unit Equipment Set specifies the expected equipment and material composition for a unit type, role, establishment, or mission. It may specify exact items, permitted alternatives, quantities, quantity units, personnel roles, and conditions.

A set may distinguish:
- Required establishment equipment.
- Optional or mission-specific equipment.
- Personal issue and protective clothing.
- Weapons and ammunition.
- Communications and sensors.
- Tools, spares, and maintenance materials.
- Training and qualification requirements.
- Basic operational consumables.
- Replenishment and reserve expectations.

The set must state whether each quantity is per person, crew, platform, subunit, unit, day, sortie, or another declared basis. The quantity basis must use a canonical unit or explicit reference-data convention. A number without a unit and denominator is invalid.

### 7.2 Equipment Allocation

The unit organisation and personnel systems reference which equipment definitions, quantities, instances, and roles are assigned to a unit. Inventory validates physical availability and performs stock and custody changes where required.

An assignment must distinguish authorised establishment, currently assigned quantity, serviceable quantity, unavailable or damaged quantity, and outstanding requirement where those concepts are modelled. A unit must not be considered fully equipped merely because a template lists an item.

Serialised assets require an unambiguous current assignment or custody state. Quantity-based items may be assigned in aggregate, but the stock movement and unit-held quantity must remain reconcilable with Inventory.

### 7.3 Platform Loadout

A platform loadout defines the actual intended installed or carried configuration for a specific platform or platform class. It may include weapons, ammunition/stores, fuel tanks, sensors, communications, pods, mission equipment, and other supported modules.

Loadout validation checks physical and interface compatibility, mass and balance constraints where modelled, station or mount limits, ammunition and store compatibility, crew requirements, technology availability, and mission restrictions.

A loadout may be saved as a reusable template, but assigning it to a physical platform requires the relevant equipment and material to be available and the platform to be in a condition that permits configuration.

### 7.4 Equipment Changes in Service

Re-equipment, refit, weapon replacement, component upgrade, and loadout changes require explicit actions and may consume time, stock, facilities, personnel, and funding. Changes must update the configuration version and preserve history.

A component swap does not silently create or destroy the removed component. The removed item returns to Inventory or another explicitly modelled custody state, and its condition and provenance are preserved where applicable.

## 8. Technology, Date, and National Availability

### 8.1 Historical Availability

Each Equipment Definition and relevant configuration must declare its availability conditions. These may include earliest and latest valid dates, technology prerequisites, research outcomes, national adoption, licence or import rules, production capability, and scenario-specific exceptions.

The starting world date is 1 January 1955, but the simulation may develop later technologies over time. Equipment examples from later eras must not appear as valid starting-world equipment unless the scenario explicitly includes them as anachronistic, fictional, captured, or otherwise authorised content.

Historical availability is not identical to physical stock. An item may be known but not produced, produced but not adopted, adopted but not available in sufficient quantity, or available only through import or captured stock.

### 8.2 Research and Technology

Research owns technological discovery and progress. Equipment and Material references the relevant technology definitions and evaluates the equipment's eligibility under current research and scenario rules.

Researching a technology does not automatically create equipment, production capacity, trained crews, inventory, or operational readiness. It changes eligibility or enables configured production, integration, training, or procurement paths.

### 8.3 National Adoption and Configuration

Nations may use different variants, licensed builds, local modifications, naming conventions, or operating standards. These must reference canonical definitions and explicit configuration differences rather than create untracked duplicates.

National adoption, export controls, procurement policy, and legal restrictions are owned by the relevant Government, Economy, Research, and procurement systems. Equipment availability consumes those outcomes through explicit interfaces.

### 8.4 Captured, Damaged, or Improvised Equipment

Captured or improvised equipment may be usable only where explicit rules permit it. Usability may depend on condition, ammunition supply, spare parts, trained personnel, compatible communications, technical documentation, and available maintenance support.

Captured equipment must retain its canonical technical identity and provenance. A new national variant should be created only when an actual modification or distinct configuration is modelled.

## 9. Condition, Reliability, and Maintenance

### 9.1 Equipment Condition

Equipment condition describes the physical or operational state of an equipment instance or quantity-based stock where condition is tracked. Relevant states may include serviceable, degraded, damaged, under repair, awaiting parts, unsafe, destroyed, or retired. The exact state catalogue may vary by equipment type.

Condition is distinct from the equipment definition's baseline reliability and from unit-level readiness. A unit may have high training but damaged equipment, or serviceable equipment but unqualified operators.

### 9.2 Reliability and Wear

Equipment definitions may specify reliability parameters, expected wear, operating limits, service intervals, and failure characteristics. Actual failure or degradation must be resolved through explicit rules using usage, environment, condition, maintenance, production quality, and other relevant inputs.

Reliability values are not a direct promise that an item will fail at a fixed time. The simulation must define whether a parameter represents deterministic wear, probability, expected rate, or another specific quantity.

### 9.3 Damage and Repair

Combat and Weapons, Movement, Environment, or other systems may report damage or degraded condition through explicit events. Equipment and Material updates or references the resulting equipment state according to its ownership contract.

Repair requires a valid repair action, available personnel, tools, parts, facilities, time, and applicable technical capability. Maintenance and Inventory own execution and material consumption as defined by their contracts. A repair order does not itself restore condition.

Destroyed equipment may be irreparable, salvageable, or recoverable under explicit rules. Destruction, write-off, salvage, and replacement must preserve provenance and avoid duplicate stock or instances.

### 9.4 Maintenance Planning

Equipment definitions may declare preventive maintenance requirements, inspection intervals, required skills, tools, spare parts, consumables, and estimated work. Maintenance scheduling and execution are owned by the relevant Maintenance/Logistics system.

Equipment and Material supplies the technical requirements and acceptance criteria for returning an item to service. It must not maintain a duplicate stock ledger or claim that required maintenance occurred merely because time elapsed.

## 10. Crew, Operator, and Qualification Requirements

### 10.1 Crew Definition

Equipment may define minimum and recommended crew positions, roles, qualifications, and proficiency requirements. These may include pilot, vehicle commander, driver, gunner, loader, radio operator, sensor operator, engineering specialist, or shipboard roles.

The exact personnel assignment and service record are owned by Military Personnel and the relevant personnel contracts. Equipment definitions declare requirements; they do not create personnel or prove that a crew is assigned.

### 10.2 Crew Completeness and Role Coverage

A platform or equipment instance may be structurally complete but unable to operate because required crew roles are vacant or unqualified. Validation should distinguish minimum safe operation, normal operation, and optional specialist capabilities where the equipment supports those distinctions.

Where a role can be filled by a qualified alternate, the substitution must be explicitly permitted. One person cannot be counted simultaneously in incompatible assignments unless the simulation explicitly models the time and duty conditions that allow it.

### 10.3 Training and Operational Capability

Crew qualifications and unit courses may enable use of equipment modes or specialist capabilities. Examples include night operation, advanced sensor interpretation, specialist communications, aircraft control, or specific weapon systems.

Equipment, Training, and Military Personnel must share an explicit qualification contract. Equipment possession alone does not grant capability; course completion alone does not supply the equipment; research alone does not supply trained operators.

## 11. Inventory and Logistics Contracts

### 11.1 Physical Stock Authority

Inventory is authoritative for physical stock quantities, lot identities, locations, reservations, custody, transfers, issue, and consumption. Equipment and Material owns definitions and technical properties, not a parallel quantity ledger.

Every physical good involved in equipment issue, loadout, maintenance, ammunition expenditure, fuel consumption, or replenishment must resolve to the canonical Catalogue identity and unit of measure.

### 11.2 Equipment Issue and Assignment

A request to equip a unit or platform must identify the target unit or instance, required canonical items, quantities or asset identities, compatible configuration, effective time, and applicable authorisation.

Inventory validates availability and commits the stock/custody transition. Military Systems records the resulting assignment or configuration only after the transaction is committed or through an explicit recoverable transaction contract. Retries must not issue the same physical item twice.

### 11.3 Ammunition and Fuel Consumption

When a unit or platform expends ammunition or fuel, the relevant operational system calculates the requested consumption based on actual activity and configuration. Inventory commits the corresponding stock mutation under the agreed transaction contract.

The technical definition supplies consumption parameters and compatibility. It does not directly subtract stock. If exact physical quantities are modelled, the simulation must not also consume an unlinked generic stock quantity representing the same ammunition or fuel.

### 11.4 Generic Sustainment Abstractions

A logistics model may use abstract demand or sustainment units to simplify planning, capacity estimation, or high-level reporting. Such values are not physical Catalogue goods unless they have an explicit canonical definition and stock semantics.

If an abstract demand unit is converted into exact goods, the conversion must define the applicable equipment set, standard quantities, time basis, allocation rule, and accounting treatment. Conversion must not create additional supply or double-count the exact goods already held by the unit.

### 11.5 Replenishment and Substitution

Substitution between equipment, ammunition, fuel, or material types is permitted only through explicit compatibility and operational rules. A nominally similar item is not automatically interchangeable.

Substitution rules must identify the allowed source and destination items, conversion or quantity basis, capability restrictions, required approvals, and any effects on performance, training, safety, or maintenance. Inventory records the actual item transferred or consumed.

## 12. Equipment Definitions, Provenance, and Data Quality

### 12.1 Required Metadata

Each canonical equipment definition should identify its source and confidence where known, applicable date/version, variant relationships, technical units, known limitations, and status of the data.

Technical values must declare their units and interpretation. The same parameter name must not be used for incompatible meanings across equipment categories. Where exact historical data is unavailable, estimates must be explicitly labelled and should include provenance and confidence.

### 12.2 Data Completeness

An equipment definition may be incomplete during authoring, but it must not be treated as operationally valid for a calculation that requires missing fields.

Validation must distinguish:
- Required and missing.
- Optional and absent.
- Not applicable.
- Unknown from source.
- Estimated.
- Not modelled by design.

The system must not silently convert an unknown value to zero, unlimited capacity, perfect reliability, or another favourable default.

### 12.3 Canonical Variant Relationships

Variants may share a parent family while preserving separate identities where technical or historical differences matter. Shared attributes may be inherited through an explicit versioned relationship, but overrides must be visible and deterministic.

A variant update must not silently alter the historical technical definition of existing instances or committed events. Migrations must specify whether saved instances retain their prior version, are explicitly upgraded, or require a compatibility conversion.

### 12.4 Names and Localisation

Display names, national designations, nicknames, serial numbers, and translated labels are not canonical identity. Multiple names may resolve to the same canonical definition where they are genuinely the same item; distinct variants require distinct identities where their differences matter.

## 13. Player and AI Configuration Tools

### 13.1 Equipment Browser

The player should be able to inspect equipment by category, role, branch, era, nation, technology, compatibility, cost where available, availability, and technical properties. Filters must distinguish known, available, producible, purchasable, assigned, and serviceable states.

The browser must not imply that a technically compatible item is available in stock or authorised for use.

### 13.2 Compatibility and Loadout Validation

The configuration interface should identify invalid combinations, missing components, incompatible ammunition, exceeded mass or mount limits, absent crew qualifications, unavailable technology, maintenance restrictions, and other unmet requirements.

Each warning should identify the failed rule, affected capability, and remediation where known. The system must not silently substitute a different component or ignore an incompatibility.

### 13.3 AI Equipment Recommendations

Officer Characters, unit commanders, maintenance staff, and procurement advisers may recommend equipment or configuration changes based on mission, doctrine, threat, training, logistics, available stock, cost, maintenance burden, and intelligence.

Recommendations must use information actually available to the character or AI and should expose important assumptions. They must not be treated as an order, procurement transaction, or physical re-equipment unless authorised and committed through the relevant systems.

## 14. Cross-System Interfaces and Ownership

| Domain | Authoritative responsibility | Equipment and Material interface |
|---|---|---|
| Good and Service Catalogue | Canonical identity, product definition, units of measure | Equipment definitions extend canonical entries and use the canonical Catalogue ID |
| Inventory | Physical stock, lots, reservations, custody, transfer, issue and consumption | Validates availability and commits physical stock mutations |
| Production | Production capability, orders, execution and actual output | Consumes canonical product definitions and produces catalogue-defined goods through Production contracts |
| Production Processes and Recipes | Input/output transformations and recipe versions | Equipment production references applicable processes, recipes, and configuration versions |
| Economy | Economic state, costs, Company accounting and financial transactions | Supplies authorised cost and procurement inputs; Equipment does not maintain financial accounts |
| Research | Technology and discovery | Supplies technology prerequisites, discovery status, and applicable availability |
| Government / Procurement | Legal authority, defence policy, procurement and export/import constraints where modelled | Supplies authorised national adoption and procurement decisions |
| Military Branches and Unit Organisation | Unit identity, organisation, roles, command and unit-level state | References equipment sets, loadouts, assignments, and resulting capability requirements |
| Military Personnel and Training | Personnel identity in service, qualifications, crew assignment and course completion | Supplies operator/crew availability and qualification state |
| Combat and Weapons | Firing, hit/damage, penetration, casualties, suppression and combat outcomes | Consumes weapon, ammunition, armour, and platform technical parameters |
| Detection and Sensors | Sensor processing, detection, track and confidence | Consumes sensor characteristics, platform configuration, signatures, and operating modes |
| Electronic Warfare | EW resolution, jamming, countermeasures and emissions effects | Consumes EW equipment characteristics and platform integration |
| Movement | Continuous position, routes, movement execution and terrain response | Consumes platform movement, dimensions, mass, propulsion and other relevant technical inputs |
| Logistics | Supply planning, transport, replenishment and delivery | Uses equipment/material demand, cargo characteristics, fuel, maintenance and compatibility requirements |
| Maintenance | Maintenance planning and execution, repair operations | Uses service intervals, work requirements, compatible spares, tools and repair acceptance criteria |
| Installations and Facilities | Facility identity, condition, construction and generic capacity | Supplies canonical facility and site references; may host installed equipment or maintenance capability |
| World Map | Geographic identity and spatial model | Supplies canonical locations and geographic references |
| Weather and Environment | Environmental conditions | Supplies operating conditions for performance, reliability, signatures, and maintenance |
| Intelligence | Information quality, source and dissemination | Supplies information available for equipment selection and threat assessment |
| Simulation Scheduler | Deterministic timing, update order and replay | Coordinates time-based availability, maintenance, and state transitions |

The table defines behavioural boundaries only. Final transaction design and detailed ownership among Military Systems subsystems will be resolved during the architecture/domain-map and logical-schema stages.

## 15. Determinism, History, and Save/Load

Persistent equipment definitions, variants, physical instances, components, loadouts, compatibility rules, and configuration templates require stable identities and relevant version history.

Authoritative events may include:
- Equipment manufacture or delivery.
- National adoption or availability change.
- Issue, assignment, transfer, installation, removal, and return.
- Loadout configuration and reconfiguration.
- Damage, degradation, inspection, maintenance, repair, salvage, and retirement.
- Ammunition, fuel, and material consumption.
- Crew assignment or qualification change where linked to equipment eligibility.
- Technology or configuration upgrade.

Events must reference canonical equipment and Catalogue identities, applicable definition/configuration versions, effective simulation time, source, and idempotency scope. Retrying an event must not duplicate stock, assets, component installations, or consumption.

Saved worlds must preserve the equipment instances, configuration versions, condition, assignments, and history required to resume deterministic simulation. Current availability and compatibility must be evaluated under the applicable scenario and versioning rules, not inferred from display names.

## 16. Open Decisions

1. Canonical Equipment and Material taxonomy and the division between catalogue reference data and Equipment-specific definitions.
2. Final relationship between canonical Catalogue entries, Equipment Definitions, platform definitions, component definitions, and serialised instances.
3. Which item classes require individual instances versus quantity-based stock.
4. Exact platform, component, mount, station, and interface model.
5. Required technical fields by equipment category and the level of detail needed for simulation.
6. The boundary between Equipment and Material and the specialist Combat, Detection, EW, Movement, Maintenance, and Logistics calculations.
7. Weapon and ammunition parameter definitions, units, and combat-resolution contracts.
8. Armour and protection models and their relationship to damage and casualty calculations.
9. Sensor parameters, signatures, and the Detection system's required inputs.
10. Communications compatibility, network configuration, range and interference contracts.
11. Platform propulsion, fuel consumption, range, endurance, movement, and environmental dependencies.
12. Component damage, platform damage, repair, replacement, and salvage ownership.
13. Crew and operator requirement vocabulary and Military Personnel interface.
14. Equipment-set and loadout schemas, quantity bases, alternatives, and validation levels.
15. Exact item-level ammunition/fuel accounting versus abstract sustainment-demand units.
16. Inventory transaction guarantees for equipment issue, component installation, and consumption.
17. Technology, date, national adoption, procurement, licensing, and captured-equipment rules.
18. Historical reference-data sources, confidence labels, and validation policy.
19. Reliability, wear, service intervals, and maintenance requirement semantics.
20. Unit and platform configuration versioning and saved-world migration policy.
21. Which technical values are authoritative reference data versus derived/cacheable values.
22. How AI recommendations evaluate capability, cost, availability, training, maintenance, and uncertainty.
23. Numerical calibration and player-facing presentation.

## 17. Acceptance Criteria

This specification is ready to inform architecture and schema design when:

- [ ] Every equipment and physical material item references a canonical Good and Service Catalogue identity.
- [ ] Equipment definitions, variants, physical instances, components, platforms, equipment sets, and loadouts are distinct concepts.
- [ ] Quantity-based stock and serialised equipment instances are both supported without duplicate identity or stock.
- [ ] Equipment categories can define relevant technical properties without forcing all categories into one identical schema.
- [ ] Weapon and ammunition technical properties are separate and consumed by Combat and Weapons.
- [ ] Sensor definitions supply inputs to Detection without asserting that detection has occurred.
- [ ] EW equipment definitions supply technical inputs without duplicating EW resolution.
- [ ] Platform movement characteristics are consumed by Movement without creating a grid-cell movement model.
- [ ] Compatibility is explicit, versioned, validated, and never inferred solely from display names.
- [ ] Loadouts validate physical, technical, crew, technology, and configuration constraints where applicable.
- [ ] Equipment sets declare quantity units and denominators.
- [ ] Equipment possession does not create personnel, qualifications, inventory, facilities, or operational readiness.
- [ ] Inventory owns physical stock, custody, reservations, transfers, issue, and consumption.
- [ ] Exact ammunition/fuel consumption and abstract logistics demand cannot double-count the same physical goods.
- [ ] Research and historical availability are separate from physical stock and procurement.
- [ ] Equipment condition, reliability, damage, wear, and maintenance are distinct concepts with explicit ownership.
- [ ] Repair does not restore equipment without a valid action and required resources.
- [ ] Crew and operator requirements are explicit and validated against personnel and training contracts.
- [ ] Historical definitions and committed configurations remain reproducible after catalogue updates.
- [ ] Unknown and estimated technical values are identified and are not silently replaced by favourable defaults.
- [ ] Illustrative loadouts are not treated as validated starting-world data.
- [ ] Cross-system transactions are deterministic, idempotent, and auditable.
- [ ] Open decisions remain explicit until resolved in the architecture and logical-schema workflow.

## 18. Related Specifications

- [Military Branches and Unit Organisation System](military-branches-and-unit-organisation.md)
- [Good and Service Catalogue](../../reference/economy/good-and-service-catalogue.md)
- [Production System](../production/production-system.md)
- [Production Processes and Recipes](../production/production-processes-and-recipes.md)
- [Inventory System](../inventory/inventory-system.md)
- [Installations and Facilities System](../infrastructure/installations-and-facilities-system.md)
- [World Map System](../world/world-map-system.md)
- [World Simulation Dependency Map](../../technical/architecture/world-simulation-dependencies.md)
