# Equipment & Materiel System

**Project:** Project Sovereign  
**System:** Military Simulation  
**Legacy document ID (retired):** MIL-EQM-001 (reassigned to `equipment-and-material-system.md`)  
**Status:** System specification, draft 1  
**Historical baseline:** 1 January 1955  
**Technology horizon:** At least 2026

> **Legacy / superseded specification:** This concise draft is retained for historical comparison and is not the implementation authority. Use [`equipment-and-material-system.md`](equipment-and-material-system.md) as the canonical specification. Any unique requirements in this file must be reconciled into the canonical document before this file is archived or removed. See [`canonical-specification-register.md`](../../technical/architecture/canonical-specification-register.md).

## 1. Purpose and scope

The Equipment & Materiel System defines military equipment, equipment designs, physical items and stockpiles, inventories, allocations, compatibility, serviceability, and the materiel requirements of military organisations.

Its central principle is:

> **Equipment defines potential capability; condition, availability, personnel, ammunition, training, and circumstances determine realised capability.**

A tank is not an abstract attack value. It is an equipment design with characteristics, weapons, protection, mobility, crew requirements, compatible ammunition, maintenance needs, fuel consumption, reliability, and a physical service history. The same principle applies to small arms, aircraft, ships, sensors, communications equipment, engineering equipment, personal equipment, and consumables.

This system does not independently determine combat outcomes, research progress, production scheduling, or logistics routing. It provides authoritative equipment and materiel records to those systems.

## 2. Design principles

1. **Design and physical stock are separate.** A researched design is not a manufactured item.
2. **Inventory and availability are separate.** A unit may own equipment that is unserviceable, uncrewed, out of ammunition, or otherwise unavailable.
3. **Use the right tracking granularity.** Track individual identities where meaningful; aggregate interchangeable consumables and common items where appropriate.
4. **Equipment characteristics are type-specific.** Do not force a rifle, ship, radar, and uniform into a single universal statistics block.
5. **Compatibility is explicit.** Weapons, ammunition, platforms, modules, power systems, and support equipment must declare relevant constraints.
6. **No automatic technology upgrades.** New research does not replace equipment already fielded.
7. **Physical condition matters.** Wear, damage, maintenance, age, and environmental exposure affect availability and performance through the relevant systems.
8. **Connected systems own their domains.** Production creates stock, logistics moves and sustains it, personnel supply operators, and combat systems evaluate operational effects.

## 3. Equipment classification

The classification system must be extensible and data-driven. Initial categories include:

| Category | Examples |
|---|---|
| Individual weapons | Rifles, pistols, machine guns, anti-tank weapons |
| Crew-served weapons | Heavy machine guns, mortars, anti-tank guns |
| Armoured vehicles | Tanks, infantry fighting vehicles, armoured personnel carriers |
| Other vehicles | Trucks, utility vehicles, engineering vehicles |
| Artillery | Towed guns, self-propelled artillery, rocket artillery |
| Aircraft | Fighters, bombers, transports, helicopters |
| Naval materiel | Surface vessels, submarines, shipboard systems |
| Sensors | Radar, sonar, optical and electronic detection systems |
| Communications | Radios, field telephones, data links |
| Electronic warfare | Interception, jamming, electronic protection systems |
| Engineering equipment | Bridging, earthmoving, mine-clearing systems |
| Personal equipment | Uniforms, body armour, helmets, load-bearing equipment |
| Support equipment | Generators, workshop, medical and test equipment |
| Ammunition and consumables | Small-arms ammunition, shells, missiles, torpedoes, fuel |
| Installation components | Fixed radar arrays, power systems, antennas, facility equipment |

Categories support discovery, validation, reporting, and default behaviours. They must not prevent a new equipment class from being defined when a nation's historical or technological requirements call for it.

## 4. Design, item, lot, and allocation

### 4.1 Equipment Design

An Equipment Design defines a type or model of equipment. It contains its identity, design lineage, configuration, characteristics, requirements, compatibility rules, and relevant production and service information.

A design may have variants, blocks, national adaptations, upgrade packages, or successive generations. A variant must explicitly identify which characteristics and components differ from its parent design.

### 4.2 Equipment Instance

An Equipment Instance represents an individually tracked physical item. This is appropriate for major platforms and assets whose identity, location, service history, damage, or loss matters individually, such as a ship, aircraft, tank, or strategic sensor installation component.

An instance has a persistent identifier independent of its current name, assigned unit, location, and condition.

### 4.3 Equipment Lot or Batch

A lot represents a quantity of equivalent or sufficiently interchangeable materiel. It is appropriate for ammunition, common spares, uniforms, and other stock where item-by-item tracking adds little strategic value.

Lots may record design/type, quantity, production batch, location, ownership, condition, expiry or shelf-life information where relevant, and provenance. Transfers, expenditure, losses, and degradation change the lot's quantity or condition.

### 4.4 Equipment Allocation

An Equipment Allocation records the assignment of equipment or materiel to a unit, installation, depot, warehouse, vehicle, platform, or other authorised holder. Allocation does not guarantee operational availability.

An allocation should identify the holder, equipment design or lot, quantity or instance IDs, effective status, and any relevant restrictions. Equipment may be authorised for a unit but not yet delivered; the establishment and physical allocation must remain distinct.

### 4.5 Inventory and custody

Inventory records physical holdings and their location or custody. It must support national stockpiles, depots, production sites, units, installations, transport, repair facilities, captured stock, and other relevant holders.

Ownership, custody, location, allocation, and operational control may differ and should not be collapsed into one field where the distinction matters.

## 5. Equipment characteristics

Each design exposes the characteristics relevant to its type. Applicable properties may include:

- Mass, dimensions, volume, and transport footprint.
- Crew, operator, and maintainer requirements.
- Mobility, speed, range, endurance, and terrain restrictions.
- Protection, structural integrity, and vulnerability.
- Weapons, firing mechanisms, and compatible ammunition.
- Sensor, communications, navigation, and processing capabilities.
- Power generation and consumption.
- Fuel, lubricants, and other operating consumables.
- Reliability, expected service life, and maintenance intervals.
- Environmental and operating limits.
- Training, certification, and support requirements.
- Spare parts, tools, facilities, and repair requirements.
- Production cost, production time, material inputs, and industrial prerequisites.
- Research prerequisites and design lineage.
- Decommissioning, salvage, and disposal requirements where modelled.

Properties should be type-specific. A rifle needs weapon and ammunition properties; a radar needs detection-related characteristics and operating requirements; a ship needs platform, crew, propulsion, endurance, and subsystem properties. Missing or inapplicable fields should not be represented as misleading zero values.

## 6. Establishment, inventory, and operational availability

The system must distinguish the following concepts:

- **Authorised establishment:** what a unit is intended or authorised to hold.
- **Physical holdings:** what equipment exists in the unit's custody.
- **Allocated quantity:** what has been assigned to the unit or subunit.
- **Serviceable quantity:** what is physically fit for service under the relevant maintenance rules.
- **Operationally available quantity:** what can be used for the specified task, given serviceability and other requirements.
- **Unserviceable quantity:** equipment awaiting inspection, repair, or recovery.
- **Unavailable quantity:** equipment temporarily inaccessible, awaiting delivery, missing required components, or otherwise unusable.
- **Lost or removed quantity:** equipment destroyed, abandoned, captured, written off, transferred, or decommissioned.

For example, a battalion authorised 40 tanks may physically hold 34. If 5 are undergoing maintenance and 3 lack available crews, the number operationally available for a particular task may be lower still. Exact availability is derived from equipment condition, personnel, supplies, and task requirements; it must not be guessed from physical holdings alone.

Operational availability is task-dependent. A vehicle may be able to move but not fight, a radar may be powered but lack communications, or an aircraft may be serviceable but lack a qualified crew or required weapon.

## 7. Ammunition and consumables

Ammunition is physical materiel with defined types, compatibility, quantity, location, and condition. Weapon performance and target effects are evaluated by the relevant combat system, which consumes ammunition data and availability from this system.

Ammunition definitions may include:
- Calibre, dimensions, or other compatibility properties.
- Compatible weapon designs and platforms.
- Type-specific effects and operating constraints.
- Storage, handling, and transport requirements.
- Shelf life, degradation, and inspection rules where applicable.
- Packaging and quantity units.
- Production and replenishment references.

The system must support small-arms ammunition, artillery shells, rockets, missiles, torpedoes, mines, bombs, depth charges, and future or historical equivalents without assuming a single common consumption model.

Ammunition can be held by individual units, platforms, installations, or depots. Expenditure, replenishment, transfer, spoilage, loss, capture, and destruction must update authoritative stock records. Specialised ammunition may enable particular effects, but possession of a weapon does not imply that all ammunition types are available.

Fuel, lubricants, batteries, medical consumables, and other materiel should use compatible inventory and replenishment principles, while retaining their own type-specific constraints.

## 8. Compatibility and configuration

Compatibility rules describe which components and materiel can operate together. Examples include weapon-to-ammunition compatibility, vehicle-to-module compatibility, platform-to-munition compatibility, power and connector requirements, sensor interfaces, and equipment-to-operator qualification requirements.

Compatibility must be explicit and validated. The system should distinguish:
- Physically incompatible combinations.
- Compatible combinations requiring an adapter or modification.
- Supported configurations.
- Configurations that are possible but unvalidated or unreliable.
- Configurations prohibited by design or doctrine where such restrictions are authoritative.

A unit should not receive a capability simply because a compatible design exists in the database. The actual item must be present, installed or allocated appropriately, serviceable, supported, and operated by qualified personnel where required.

## 9. Equipment condition, damage, and maintenance

Equipment condition is persistent and may change through use, age, wear, environmental exposure, damage, storage, poor handling, or other events.

The condition model should support relevant states such as:
- New or serviceable.
- In use and serviceable.
- Degraded but usable under restrictions.
- Awaiting inspection.
- Awaiting maintenance or repair.
- Under maintenance or repair.
- Damaged and unavailable.
- Cannibalised or held for parts.
- Written off, destroyed, or otherwise removed from service.

These are conceptual states; the precise lifecycle may differ by equipment type. A ship, rifle, radar component, and ammunition lot do not require identical condition fields.

Maintenance and repair consume the relevant staff time, parts, tools, facilities, and other resources. The Equipment & Materiel System records condition and requirements; Military Logistics and Maintenance systems schedule and execute work according to their own rules.

Equipment should not recover automatically merely because time passes. Maintenance, repair, replacement, inspection, or other defined processes must cause the appropriate state change. Damage should persist until repaired or the item is written off.

## 10. Personnel and equipment interaction

Equipment capability depends on the personnel who operate, maintain, and support it. The system should expose crew requirements, operator qualifications, staffing levels, and maintenance demands to the Personnel, Training, and Organisation systems.

It should not duplicate personnel records or independently determine a character's qualifications. It consumes authoritative references to those records.

Examples:
- A tank may be physically serviceable but unavailable because a required crew position is vacant.
- A radar may be operational but less effective when qualified operators are unavailable.
- A specialised system may be unusable until the assigned personnel complete the required training.
- A vehicle fleet may become progressively less available when maintenance personnel or spare parts are insufficient.

Crew quality and proficiency affect realised performance through the appropriate systems; they do not change the underlying equipment design.

## 11. Production, research, procurement, and introduction into service

The Equipment & Materiel System consumes designs and manufactured output from Equipment Design, Research & Production. It does not itself conduct research or schedule industrial production.

The lifecycle may include:
1. Research or acquisition of design knowledge.
2. Design development and validation.
3. Authorisation or selection for procurement.
4. Production, purchase, or transfer.
5. Acceptance, testing, and inventory entry.
6. Allocation and delivery.
7. Crew/operator training and support preparation.
8. Introduction into service.
9. Maintenance, upgrades, replacement, and eventual retirement.

Not every design requires every stage, but the stages must remain conceptually distinct.

Researching a new design does not create physical inventory. Manufacturing a design does not automatically assign it to units. A new design must not automatically upgrade equipment already in service. Re-equipping a formation may require conversion planning, retraining, maintenance arrangements, new ammunition, revised establishments, and supply changes.

Foreign procurement, licensed production, captured equipment, transfers, and emergency conversions should be represented through explicit acquisition or configuration paths appropriate to the wider economic and diplomatic systems.

## 12. Variants, upgrades, and service life

Equipment designs should support a lineage of variants and upgrades. An upgrade may alter selected components or characteristics without necessarily creating an entirely new platform identity. The system must distinguish a design-level upgrade from a physical modification performed on an existing item.

A physical upgrade consumes time, parts, labour, facilities, and potentially money. It may require the item to be unavailable during conversion and may require operator retraining. Not every older platform is upgradeable to every new configuration.

Service life and retirement should be modelled where relevant. Items may become uneconomical to maintain, obsolete for a particular mission, unsafe, or incompatible with available support infrastructure. Obsolescence should not automatically destroy an item; it changes its suitability, supportability, and potential usefulness.

## 13. Capture, loss, transfer, and disposal

Equipment can change custody through transfer, sale, aid, capture, abandonment, recovery, or other events. These transitions must preserve item identity or lot provenance where practical.

Captured equipment should not instantly become fully usable by the capturing force. Its condition, compatibility, ammunition, technical documentation, operator training, maintenance support, and security requirements affect whether it can be exploited, repaired, or integrated.

Losses must update inventory and allocation records. Destroyed equipment cannot remain operationally allocated. Abandoned or damaged equipment may be recoverable if the relevant systems and circumstances permit. Salvage and cannibalisation may return components or materials to stock according to defined rules.

## 14. Fixed installations and embedded equipment

Installations may contain fixed equipment such as radar arrays, antennas, power systems, processing equipment, and communications infrastructure. The Installation entity and its facility composition are defined by Military Branches & Unit Organisation and Military Construction; this system provides the equipment definitions, physical condition, component allocations, and serviceability.

An installation's facility capabilities and staff capabilities remain separate. A facility may be physically intact but non-operational because power, communications, spare parts, or qualified staff are missing. Damage to a specific component should affect the capabilities that depend on that component rather than applying an arbitrary global penalty to the entire installation.

## 15. Conceptual data model

| Entity | Responsibility |
|---|---|
| `EquipmentCategory` | Extensible classification and default validation rules |
| `EquipmentDesign` | Design identity, characteristics, compatibility, requirements, lineage |
| `EquipmentVariant` | Explicit changes from a parent design |
| `EquipmentInstance` | Individually tracked physical item |
| `EquipmentLot` | Aggregated quantity of equivalent materiel |
| `EquipmentAllocation` | Assignment of item or quantity to a holder |
| `InventoryRecord` | Physical stock, location, custody, and quantity |
| `EquipmentCondition` | Serviceability, damage, wear, and lifecycle state |
| `CompatibilityRule` | Valid and restricted combinations of designs or components |
| `AmmunitionType` | Ammunition characteristics, compatibility, and constraints |
| `ConsumableType` | Fuel, lubricants, batteries, and other consumed materiel |
| `EquipmentComponent` | Subsystems and replaceable components where modelled |
| `MaintenanceRequirement` | Maintenance intervals, parts, skills, and facility needs |
| `ModificationRecord` | Physical upgrades, conversions, and configuration changes |
| `EquipmentTransfer` | Movement or change of custody between holders |
| `EquipmentLossRecord` | Loss, destruction, capture, write-off, or disposal |
| `DesignLineage` | Relationships among design generations and variants |

These are conceptual entities. Implementation may combine or split records where appropriate, provided the domain boundaries and invariants remain intact.

### 15.1 Persistent identity

Individually tracked equipment has a persistent identifier independent of name, unit, location, and condition. Lots have stable identifiers and track quantity changes. Design identifiers must remain distinct from instance identifiers.

### 15.2 Authoritative records

There must be one authoritative source for each physical item's identity, current custody, condition, and allocation status. Other systems may cache or derive views, but must not maintain competing truth.

### 15.3 Integrity rules

Validation must prevent:
- Allocating the same exclusive instance to multiple holders simultaneously.
- Negative quantities or invalid unit conversions.
- Assigning nonexistent designs or lots.
- Incompatible ammunition or components unless an explicit exception applies.
- Equipment marked destroyed while still serviceable and available.
- A physical upgrade being applied without a valid modification process where one is required.
- Inventory transfers that duplicate stock rather than moving it.
- Operational availability exceeding physical holdings.

## 16. System interfaces

- **Military Branches & Unit Organisation:** authorised establishments, unit composition, installation components, and equipment allocations.
- **Equipment Design, Research & Production:** equipment designs, variants, manufacturing output, and production provenance.
- **Military Logistics:** inventory locations, transport, transfers, resupply, repair supply, and replenishment.
- **Personnel and Training:** crew/operator availability, qualifications, and maintenance skills.
- **Combat & Engagement:** weapon/platform characteristics, compatible ammunition, physical condition, and available quantities.
- **Sensors & Detection:** sensor designs, configurations, serviceability, and installation of sensor systems.
- **Electronic Warfare:** electronic equipment definitions, configuration, serviceability, and operational availability.
- **Military Construction:** facility infrastructure and construction/installation requirements.
- **Economy and Industry:** material inputs, costs, procurement, production capacity, and disposal/salvage where modelled.

Each connected system should consume the authoritative data it needs and return state changes through explicit interfaces. This document does not prescribe the full internal implementation of those systems.

## 17. User interface and player information

The Player should be able to inspect equipment at design, unit, installation, depot, and national inventory levels, subject to available intelligence and information access.

Relevant views include:
- Design specifications and variants.
- Authorised establishment versus actual holdings.
- Serviceable, damaged, and unavailable quantities.
- Ammunition and consumable stocks.
- Compatibility warnings.
- Maintenance and spare-parts demand.
- Unit and installation allocations.
- Equipment age and service history where tracked.
- Production, procurement, and delivery status.
- Upgrade and conversion requirements.
- Estimated task availability, clearly distinguished from raw inventory.

UI summaries should explain why equipment is unavailable. For example, “unavailable: awaiting repair,” “no qualified crew,” “ammunition not available,” or “required component missing” is more useful than a single unexplained readiness penalty.

## 18. Acceptance criteria

- Designs are distinct from manufactured items and stock quantities.
- Individual tracking and lot-based aggregation are both supported.
- Authorised establishment, physical holdings, allocation, serviceability, and task availability are distinct.
- Equipment characteristics are type-specific and extensible.
- Ammunition compatibility is explicit and inventory is consumed through authoritative records.
- Equipment wear and damage persist until a defined repair, replacement, or disposal event.
- Personnel and qualification dependencies are referenced rather than duplicated.
- Research does not automatically create inventory or upgrade existing equipment.
- Production, allocation, delivery, introduction into service, and upgrade are separate processes.
- Captured and transferred equipment preserves relevant provenance and requires appropriate integration.
- Fixed installations can depend on individually defined equipment components.
- Inventory and allocation integrity prevent duplicate exclusive items and impossible quantities.
- Connected systems use authoritative records and explicit interfaces.
- The system supports historically appropriate materiel from 1955 and technological progression through at least 2026.

## 19. Open design considerations

1. **Tracking granularity:** define which categories require individual item tracking and which use lots, with performance and gameplay value as the deciding factors.
2. **Condition scales:** define serviceability and degradation models by equipment type rather than assuming one universal condition formula.
3. **Reliability modelling:** decide how expected failure rates, usage, age, maintenance quality, and environment interact.
4. **Ammunition effects:** keep ballistic, blast, penetration, guidance, and target interaction models in Combat & Engagement while exposing required ammunition data here.
5. **Upgrade architecture:** distinguish modular component replacement, field modification, depot-level upgrade, and full platform replacement.
6. **Visibility and intelligence:** determine which inventory and condition details are available to a Player without direct access or adequate intelligence.
7. **Simulation performance:** use event-driven updates and aggregation where individual detail does not materially improve gameplay.
