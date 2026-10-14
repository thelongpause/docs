# Installations and Facilities System

**Canonical Path:** `systems/infrastructure/installations-and-facilities-system.md`  
**System:** Installations and Facilities  
**Document Type:** Systems Specification  
**Status:** Foundational Specification  
**Simulation Start:** 1 January 1955

## 1. Purpose

This system represents fixed sites that provide physical, logistical, military, industrial, administrative, or commercial capabilities. It establishes a common model for military bases, airfields, naval bases, ports, depots, headquarters, factories, workshops, and other facilities.

A facility is not merely a map symbol. It has a location, capabilities, finite capacity, physical condition, access constraints, and relationships to organisations that own, operate, or control it.

## 2. Ownership Boundaries

This system owns installation identity and composition; facility types and declared capabilities; shared capacity definitions; physical condition and availability; references to location geometry and sub-features; and distinct ownership, operator, and effective-controller relationships.

It does not own canonical coordinates or spatial indexing, national sovereignty or occupation, unit readiness or loadout rules, physical stock balances, route planning or delivery, aircraft/ship/weapon performance, combat resolution, or financial accounting. These remain with World Map, National Territory and Control, military operations, Inventory, Logistics, equipment catalogues, Combat, and Economy.

## 3. Core Model

### 3.1 Installation

An Installation is a persistent site grouping one or more related facilities and spatial features. An airbase may contain runways, taxiways, hangars, parking aprons, fuel and ammunition storage, workshops, and headquarters. A naval base may contain piers, berths, anchorages, dry docks, repair facilities, and cargo-handling areas.

An installation may combine capabilities usually associated with several site types. It is not limited to exactly one airfield, port, depot, or military base.

### 3.2 Facility

A Facility is a distinct functional element within an installation or a standalone site. Examples include runways, aircraft parking aprons, hangars, maintenance workshops, naval berths, anchorages, dry docks, cargo-loading areas, fuel systems, ammunition stores, vehicle parks, depots, repair workshops, medical facilities, headquarters, and production facilities.

A facility may reference one or more physical map features. A visible symbol or texture is not itself a facility.

### 3.3 Capability and Capacity

A capability identifies an activity a facility supports, such as aircraft take-off and landing, vessel berthing, cargo handling, refuelling, rearming, repair, personnel recovery, equipment replacement, storage, or production. Capabilities must be explicit data, not inferred solely from a display name.

Capacity is the finite amount of an activity a facility can support at a time or over a defined interval. It may describe simultaneous accommodation, throughput, storage, repair workload, cargo handling, or service rate. These measures are not interchangeable: aircraft parking capacity is not runway throughput, and warehouse capacity is not current stock. The model must identify the capability, capacity measure, unit, time basis, and constraints. The domain using the capacity owns its precise calculation.

## 4. Ownership, Operation, and Control

The model must distinguish:

- **Legal owner:** the organisation that legally owns the asset.
- **Operator:** the organisation responsible for running the facility.
- **Effective controller:** the authority currently exercising practical control over the location.
- **Territorial sovereign:** the nation legally sovereign over the surrounding territory.
- **Access authority:** the entity or rules determining whether a unit, organisation, or shipment may use the facility.

These may differ. A privately owned port may be operated by a contractor, occupied by an enemy, and physically damaged. Company control of an economic facility must be distinct from national sovereignty, military control, and occupation. Military capture does not automatically transfer legal ownership.

## 5. Condition and Availability

The shared model must support operational, degraded, damaged, disabled, destroyed where destructible, under repair, closed/restricted by order, and temporarily unavailable due to environmental or access conditions.

Condition and availability differ. A runway may be physically intact but closed by order; a depot may be undamaged but unusable because it has no stock or access route. State changes record cause and effective simulation time. Damage may originate from combat, accidents, weather, wear, sabotage, or other systems. Repair consumes time, labour, materials, and suitable capacity under the responsible system.

## 6. Military Unit Support

Facilities may provide accommodation, personnel recovery, refuelling, rearming, repair, maintenance, replenishment, loadout configuration, crew replacement where modelled, mission preparation, deployment, launch, recovery, or embarkation.

A unit's physical location, assigned base, readiness, and eligibility to use a facility are distinct. Support is not instantaneous: duration and availability depend on capability, capacity, serviceable equipment, inventory, staff, permissions, unit requirements, competing work, and operational rules.

At a suitable facility, a returning unit may recover personnel, repair damage, refuel, rearm, replenish, replace equipment, or change authorised loadouts. These activities may run in parallel only where capacity and rules permit. Before deployment, a unit may require preparation time for checks, loading, crew preparation, mission configuration, and weapons installation. A loadout change requiring physical work is not an instantaneous menu change.

Installations and Facilities supplies capabilities and capacity. Military operations determines unit requirements, eligibility, preparation sequence, readiness, and release-to-deploy condition.

## 7. Airfields and Runways

An airfield is a composition of facilities and may include runways, taxiways, parking aprons, dispersal areas, hangars, maintenance, fuel, ammunition, control, and cargo-handling facilities.

Each runway must support data needed to evaluate compatibility and throughput, including length, width, surface type and condition, operational status and damage, available directions of use where modelled, relevant slope/elevation constraints, links to taxiways and aprons, and restrictions from weather, obstacles, repairs, or command decisions.

Aircraft compatibility is separate from throughput. Compatibility uses aircraft performance and operating requirements, not a generic “airbase” label.

### 7.1 Runway Throughput

Runway count, layout, orientation, condition, and independence affect take-off and landing rates. Two runways must not automatically be treated as exactly twice the throughput of one. Intersections, shared taxiways, traffic sequencing, weather, ground crews, parking, command restrictions, and simultaneous-use rules may constrain operations. Airfield operations calculates usable throughput from runway configuration and current conditions.

### 7.2 Runway Damage and Repair

Runways may be damaged by enemy attack, including anti-runway weapons. Damage may reduce usable length, impose restrictions, close a runway, or make it unavailable. Effects depend on damage location, severity, geometry, and repair state.

Combat and Weapons resolve attacks and damage. Infrastructure and airfield operations determine runway serviceability and operational consequences. Engineering or repair systems determine labour, materials, time, and equipment required to restore service. A damaged runway must not be represented only by a texture or health bar; its operational restrictions must be queryable.

## 8. Naval Bases and Ports

A naval base or port may include berths, piers, anchorages, dry docks, repair facilities, fuel systems, ammunition and torpedo storage, cargo-handling areas, warehouses, and supporting services.

Facility data should support vessel compatibility, dimensions, draught, berth capacity, handling requirements, repair capability, and access constraints. A ship's presence does not guarantee that the port can berth, refuel, rearm, repair, or replenish it.

Cargo ships must depart from a suitable port or explicitly modelled loading facility, with a valid transport assignment, cargo availability, loading capacity, and route feasibility.

## 9. Depots and Support Facilities

Military bases, garrisons, depots, and support sites may provide accommodation, storage, maintenance, medical support, vehicle parking, command functions, or other declared capabilities.

Declared storage capacity is not current stock. Inventory owns stock quantities, lots, reservations, and movements. A depot must not supply equipment or ammunition unavailable in inventory or through a valid replenishment process. Storage eligibility, safety constraints, and capacity must be explicit.

## 10. Economic Facilities and Trade

Factories, mines, farms, warehouses, ports, rail yards, and other economic sites may use the common installation/facility foundation. Economy, Industry Data, Production, and Inventory remain authoritative for establishment, capacity, inputs, output, labour, costs, and stock.

The map should display the company or organisation that owns or operates an economic facility through an economic layer. This remains distinct from territorial sovereignty and military control.

A trade agreement does not itself create a shipment or complete delivery. Logistics coordinates origin and destination facilities, cargo identity and quantity, suitable transport, loading, departure, transit, unloading, and receipt. Economy handles financial obligations and settlement. Cargo aircraft may depart from suitable airports or airbases; cargo ships require suitable ports or explicitly modelled loading locations. Physical delivery, inventory transfer, and financial settlement are distinct events. In-transit cargo must not simultaneously be available at origin and destination.

## 11. Map Geometry

World Map owns canonical spatial geometry. An installation may have a point, an area footprint, or multiple features. Runways, piers, hangars, storage areas, and taxiways may have their own geometry where required for spatial interaction or visual representation.

The map may simplify an installation to a symbol at strategic zoom and expose its internal layout at operational zoom. Visual simplification must not discard geometry needed for movement, combat, aircraft compatibility, damage, or logistics. MapMaker must support stable references and validate dangling or contradictory geometry references.

## 12. Integration Contracts

- **World Map:** location, geometry, elevation, spatial queries.
- **National Territory and Control:** sovereignty, territorial controller, occupation, contested status.
- **Military Operations:** unit assignment, readiness, preparation, loadouts, deployment eligibility.
- **Aircraft and Naval Operations:** compatibility, take-off/landing, berthing, throughput.
- **Inventory:** physical stock, reservations, stock movements.
- **Logistics:** routes, cargo, loading, transit, unloading, delivery.
- **Economy:** legal ownership, operating costs, purchases, settlement, corporate control.
- **Production and Industry Data:** establishment and production capabilities.
- **Combat and Weapons:** attacks and damage events.
- **Engineering and Repair:** repair tasks and restoration.
- **MapMaker and Renderer:** facility footprints, condition, ownership, operator, operational state.

Consumers should query the owning system rather than copy authoritative stock, control, readiness, or financial values into facility records.

## 13. Acceptance Criteria

- One installation can combine multiple facility types.
- Capabilities and capacities are explicit.
- Legal owner, operator, territorial sovereign, and effective controller can differ.
- A unit cannot use services without capability, capacity, access, and required stock.
- Recovery, rearming, repair, replenishment, and preparation consume appropriate time and resources.
- Aircraft compatibility is evaluated separately from runway throughput.
- Two runways do not automatically double throughput.
- Runway damage affects operations and can be repaired through a defined process.
- Cargo transport cannot depart without a suitable loading facility, valid cargo, transport capability, and feasible movement.
- Company ownership does not imply national sovereignty or military control.
- Rendering does not create facility capability, stock, readiness, ownership, or damage state.

## Governing Principle

**An installation is a physical site; a facility provides a defined capability; capacity limits what it can do; and the relevant domain system determines whether a particular activity can actually occur.**
