# National Territory and Control System

**Canonical Path:** `systems/world/national-territory-and-control-system.md`  
**System:** National Territory and Control  
**Document Type:** Systems Specification  
**Status:** Foundational Specification  
**Simulation Start:** 1 January 1955

## 1. Purpose

The National Territory and Control System represents the political geography of the world and the changing relationship between legal sovereignty, national claims, military presence, effective territorial control, frontlines, and occupation.

It provides the authoritative interpretation of political boundaries and territorial control for diplomacy, government, military operations, logistics, population, economic activity, and map presentation.

The central rule is that **legal sovereignty, military control, occupation, and frontline position are separate facts**. They may correspond, but must never be treated as interchangeable.

## 2. Scope and Ownership

This system owns:

- National territorial claims and recognised sovereignty relationships.
- Political boundary geometry references and boundary status.
- The effective territorial-control state of spatial regions.
- Contested territory and frontline derivation.
- Military occupation status and occupation administration state.
- Time-stamped territorial changes and their causes.
- Queries that determine which nation or authority controls a location for a particular purpose.

This system does not own:

- Canonical coordinates, vector geometry storage, elevation, or spatial indexing. These belong to the World Map System.
- Authoritative continuous unit/platform positions, route traversal, and movement execution state. These belong to Movement.
- Unit identity, organisation, formations, orders, and military operational state. These belong to the relevant Military Systems.
- National legal recognition policy or diplomatic decisions beyond their resulting sovereignty and claims state. These belong to Government and Diplomacy.
- Population, tax, industry, or inventory state.
- Rendering, flag artwork, line colour, or map label placement.

## 3. Core Concepts

### 3.1 Nation

A Nation is a political entity defined by the Government and Diplomacy domains. It has a stable identifier, a national identity, and references to its flag and other visual identifiers.

The map may display a flag at strategic zoom or use national identifiers in borders, labels, and information panels. Flag assets are presentation references and do not determine national identity or territorial status.

### 3.2 Legal Sovereignty

Legal sovereignty identifies the nation recognised by the simulation's political state as sovereign over a territory. Sovereignty may be disputed, contested diplomatically, transferred by treaty, or changed through an explicitly recorded political outcome.

Military occupation does not automatically transfer sovereignty. A ceasefire, frontline shift, or temporary military presence does not itself transfer sovereignty.

### 3.3 Territorial Claim

A territorial claim records that a nation asserts a claim over a defined area. A claim may overlap another nation's claim or recognised territory. Claims may have a status such as recognised, disputed, dormant, withdrawn, or superseded, according to the political rules established by Government and Diplomacy.

Claims are not equivalent to effective control.

### 3.4 Effective Territorial Control

Effective territorial control identifies which force or authority can exercise practical control over an area at a given simulation time. It is derived from military and security presence, the opposing force's presence, access, isolation, local resistance, and other rules that the game later formalises.

The system must be able to represent control by a nation or an explicitly identified controlling authority. It must not assume that the legal sovereign is also the effective controller.

### 3.5 Military Occupation

Occupation records that a foreign military authority exercises effective control over territory without a corresponding transfer of legal sovereignty. Occupation may be partial, contested, provisional, or consolidated.

Occupation is more than the presence of a unit. A passing patrol, a raid, a temporary position, or a unit moving through a settlement must not automatically establish occupation. The occupation rules must consider sustained control and the ability to exercise authority, with any required thresholds defined as configurable simulation rules rather than hard-coded assumptions.

### 3.6 Frontline

A frontline is a derived operational representation of the contact boundary between opposing forces or their controlled areas. It is not a national border and does not, by itself, determine sovereignty or occupation.

A frontline may cross pre-war national borders, pass through a nation, or remain far from an internationally recognised boundary. It may be irregular, fragmented, fluid, or uncertain.

## 4. Political Boundary Model

A political boundary references canonical spatial geometry maintained by the World Map System and has its own political metadata.

A boundary record should support:

- Stable boundary identifier.
- Geometry reference.
- Boundary type.
- Participating nation or authority identifiers.
- Legal or diplomatic status.
- Effective simulation-time interval.
- Source or cause of the boundary state, such as scenario definition, treaty, diplomatic decision, or territorial settlement.
- Optional notes or references for scenario authors.

Boundary types should include at least:

- Internationally recognised national border.
- Disputed or claimed border.
- Administrative boundary, where the simulation uses subnational political divisions.
- Armistice or ceasefire line.
- Demarcation line established by agreement.
- Historical or inactive boundary retained for historical records.

A boundary may be legally recognised while nearby territory is occupied by another nation. An armistice line may constrain military activity without changing sovereignty. The system must represent these conditions independently.

The system must not infer borders automatically from terrain features, population-centre outlines, military unit positions, or a colour fill.

## 5. Spatial Territorial State

Territorial state is associated with spatial regions, not only with whole nations, cities, or provinces. The representation must support local changes without requiring an entire nation or administrative region to change status at once.

The initial logical model must distinguish these fields:

- **Legal sovereign:** the nation with recognised sovereignty under current political state.
- **Claimants:** zero or more nations asserting claims.
- **Effective controller:** the nation or authority currently exercising practical control, if any.
- **Authoritative control status:** controlled, contested, uncontrolled, or unresolved. This describes the simulation's current assessment, not what a player knows.
- **Player knowledge:** a separate observer-specific assessment that may be known, uncertain, stale, or unknown under fog-of-war rules. It must not overwrite authoritative territorial state.
- **Occupation status:** not occupied, occupied, contested occupation, or occupation status unresolved.
- **Administrative status:** whether an occupying or local authority is exercising civil administration, if this is modelled.
- **Effective time:** when the state applies in simulation time.
- **Provenance:** the event or rule that created the state.

These are logical fields. The final storage representation may use spatial partitions, polygons, tiled overlays, references to authored regions, or a hybrid. It must not force all territorial state to align with the 100 m Movement Grid.

### 5.1 No Automatic Whole-Region Capture

A military unit entering a nation, province, city, or map tile does not automatically transfer control of the entire enclosing geography. Control changes must be evaluated at an appropriate spatial granularity using explicit rules.

Population centres, facilities, transport nodes, and strategic terrain may have their own local control state. Their capture can affect nearby control and operational access, but the relationship must be defined by simulation rules rather than assumed from visual overlap.

### 5.2 Contested Areas

An area may be contested when opposing forces have overlapping influence, when effective control cannot be established, or when a control assessment is unresolved. The system must not force every location into a binary friendly/enemy state.

Contested status can coexist with a known legal sovereign. A location can, for example, remain legally sovereign to Nation A while being contested by Nation B's forces.

## 6. Frontline Derivation

The frontline is a derived operational layer, calculated from current military spatial state and control assessments. It must not be manually edited as the sole authoritative record of control.

The derivation process should:

1. Obtain authoritative continuous positions and movement state from Movement, and force identity, formations, orders, operational presence, and relevant control assessments from the appropriate Military Systems.
2. Identify areas controlled or contested by opposing sides.
3. Determine spatial contact or near-contact boundaries according to configurable operational rules.
4. Apply smoothing and simplification for legible display without altering the underlying control state.
5. Retain discontinuities, pockets, encirclements, salient shapes, and isolated forces where the simulation supports them.
6. Recalculate when relevant positions, control state, visibility, or operational conditions change.
7. Expose an effective simulation time and, where appropriate, confidence or uncertainty.

The frontline algorithm must account for the fact that units are continuously positioned in geographic space. It must not simply mark every 100 m cell containing a unit as the frontline, nor may it treat each occupied grid cell as a discrete territorial transfer.

The exact tactical-to-operational algorithm, control radii, influence rules, and refresh cadence remain implementation decisions. They must be documented and tested before being treated as authoritative.

## 7. Capture and Occupation Lifecycle

Territorial control changes should be represented as state transitions with a cause and timestamp. A conceptual lifecycle is:

1. **Presence:** a force is present in or near an area.
2. **Contested:** opposing forces or unresolved control prevent a reliable controller assignment.
3. **Controlled:** one authority has established effective control under the applicable rules.
4. **Occupied:** the controlling authority is exercising control over territory whose legal sovereign remains another nation.
5. **Consolidated occupation:** where modelled, the authority has established sustained security and administration.
6. **Liberated, retaken, or transferred:** control changes again, while sovereignty changes only if a separate legal or political event establishes it.

These labels describe concepts, not a mandatory linear state machine. A raid may begin and end without occupation; control may change without civil administration; and a ceasefire may freeze a frontline while legal sovereignty remains unresolved.

The system must distinguish temporary tactical possession from durable territorial control. Time, supply access, local security, force presence, enemy resistance, and the ability to maintain authority may contribute to the determination. Exact thresholds belong in configurable rules.

### 7.1 Facilities and Population Centres

Military bases, airfields, ports, depots, cities, and other population centres can be captured independently of surrounding territory. Their controller and operating state may change even while the broader area remains contested.

Capture must not automatically transfer company ownership of a facility, erase its inventory, or make it immediately usable. Military control, legal ownership, operator identity, physical condition, inventory, and operational availability remain separate state.

### 7.2 Infrastructure and Access

Control over a road, bridge, port, airfield, or other transport node can affect movement and logistics. A route is not necessarily usable merely because its territory is friendly-controlled: infrastructure condition, enemy interdiction, permissions, capacity, and physical access remain relevant.

## 8. Fog of War and Knowledge

Authoritative territorial state and a player's knowledge of that state are separate.

The simulation may know that an area is controlled or contested while a player sees an outdated, approximate, or unknown assessment. The presentation layer must not reveal hidden frontline detail, unit presence, occupation status, or control confidence unless permitted by the player's information sources.

Observed territorial status should carry an observation time or freshness state where useful. A stale frontline display must not overwrite current authoritative control.

## 9. Integration Contracts

- **World Map:** provides geometry, coordinates, spatial queries, and boundary geometry storage.
- **Government and Diplomacy:** provide nation identities, claims, recognition, treaties, sovereignty-transfer decisions, and relevant political permissions.
- **Movement:** provides authoritative continuous positions, route traversal, and movement execution state.
- **Military Systems:** provide unit identity, force identity, formations, orders, and military operational state relevant to control assessments.
- **Combat:** provides combat outcomes that can change unit presence and local control conditions.
- **Population:** provides population-centre geography and, where modelled, local administration or resistance inputs.
- **Installations and Facilities:** provides facility locations and facility identity; the territorial system supplies the current controlling authority separately.
- **Logistics:** uses territorial control and access rules when evaluating routes and delivery feasibility.
- **MapMaker and Runtime Renderer:** display boundaries, flags, frontlines, contested areas, occupied areas, and uncertainty from authoritative data.

No consumer may infer legal sovereignty solely from the effective controller or infer military occupation solely from a national border.

## 10. Map Presentation Requirements

At strategic zoom, the map should communicate national identity and broad territorial change through national flags or identifiers, readable borders, frontlines, contested areas, and occupation overlays. At operational zoom, these should resolve into more detailed control boundaries and unit context without overwhelming terrain and route readability.

The renderer must use separate visual semantics for:

- Recognised borders.
- Disputed boundaries.
- Armistice or ceasefire lines.
- Frontlines.
- Contested territory.
- Occupied territory.
- Legal sovereignty and effective control when they differ.

Colour alone must not convey every distinction. Line style, pattern, iconography, labels, and optional layer toggles should reinforce meaning. The MapMaker may author the visual style, but visual choices do not change territorial state.

## 11. Historical Record and Events

Territorial state changes must be traceable to simulation events. Records should preserve the prior state, new state, effective time, cause, and relevant authority or force. Historical records must not be rewritten when the current frontline moves.

A scenario may define initial boundaries and control state. At runtime, the simulation changes effective control through military and political rules. Scenario-authored legal boundaries should not be silently replaced by the frontline or occupation overlay.

## 12. Acceptance Criteria

The system is not ready for implementation until the following behaviours can be tested:

- A nation can retain legal sovereignty over territory controlled by another nation.
- A frontline can cross an internationally recognised border without rewriting that border.
- A city or airfield can change effective controller independently of its surrounding region.
- A contested area can exist without forcing a binary controller assignment.
- A unit's temporary presence does not automatically create occupation.
- An occupation can end without a legal sovereignty transfer.
- A treaty or political decision can change sovereignty without requiring a corresponding frontline movement.
- The player can receive incomplete or stale frontline information under fog of war.
- Rendering changes cannot alter territorial state.
- Control changes preserve a timestamped historical record.

## Governing Principle

**A border describes political status; a frontline describes military contact; control describes who can exercise power on the ground; occupation describes control over territory without an equivalent transfer of sovereignty.** The simulation must represent these independently.
