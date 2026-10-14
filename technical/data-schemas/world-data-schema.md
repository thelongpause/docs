# Project Sovereign: World Data Schema

**Canonical Path:** `technical/data-schemas/world-data-schema.md`  
**Document Type:** Logical Data Schema Specification  
**Status:** Foundational Schema Draft  
**Target Branch:** `implementation-data-schema-foundation`

## 1. Purpose

This document defines the logical data model for authored world geography and the persistent spatial state that the simulation places on that geography. It covers world geometry, terrain objects, the Movement Grid as a derived analysis layer, political boundaries, territorial control, population-centre references, installations, and facilities.

This is a logical schema, not a final SQL migration or engine-specific class layout. It defines entities, relationships, ownership boundaries, invariants, and query requirements while leaving storage choices open where performance testing is required.

## 2. Core Principles

1. **Geometry and meaning are separate.** World Map owns coordinates and geometry. Domain systems own political, terrain, population, infrastructure, and operational meaning.
2. **The world is continuous.** Units retain continuous geographic positions. The 100 m Movement Grid supports spatial analysis and is not a cell-by-cell movement model.
3. **Vector geometry is authoritative.** Grid representations, render caches, simplified geometries, and indexes are derived data.
4. **Spatial features overlap.** Terrain Areas, Lines, and Points may overlap unless a specific domain invariant prohibits it.
5. **Political and military state are distinct.** Sovereignty, claims, effective control, frontline, and occupation must not be collapsed into a single owner field.
6. **Facilities are composable.** One installation may contain multiple facility types and sub-features.
7. **Presentation is not state.** Textures, flags, labels, symbols, and rendered frontlines cannot create or change simulation truth.
8. **Historical changes are traceable.** State transitions need effective simulation time and provenance.
9. **Avoid one row per world cell unless proven necessary.** Grid data should use a tested tiled, sparse, packed, or generated representation rather than defaulting to a billion independent records.

## 3. Domain Ownership

| Domain | Authoritative data |
|---|---|
| World Map | World coordinate system, geometry, elevation references, spatial indexes, geometry versioning |
| Terrain Objects | Terrain feature definitions, terrain attributes, supported terrain states |
| National Territory and Control | Nation-to-territory political relationships, boundary status, claims, effective controller, occupation, control assessments |
| Population | Population-centre identity and population/demographic state |
| Installations and Facilities | Installation composition, facility capabilities, capacities, condition, ownership/operator references |
| Military Systems | Military unit identity, organisation, equipment and personnel state, orders, readiness, and subsystem-specific operational state. Each military subsystem must have an explicit authority boundary. |
| Movement | Authoritative continuous unit/platform position, route traversal, movement execution state, and movement-cost evaluation |
| Economy and corporate domains | Legal economic ownership, company/operator state, financial records |
| Inventory | Physical stock, lots, reservations, and movements |
| Logistics | Routes, shipments, transport activity, loading, transit, and delivery |
| MapMaker/Renderer | Asset assignments, visual styling, label placement, zoom thresholds, derived presentation caches |

Cross-domain references use stable identifiers. A domain may cache foreign values for performance only if the cache is rebuildable and never becomes an alternative authority.

## 4. Identifier and Time Conventions

### 4.1 Identifiers

All persistent entities require stable identifiers independent of display names and file paths. The concrete identifier representation remains an implementation decision. Geometry records, domain records, and asset definitions should not rely on array order or editor-local indexes as identity.

### 4.2 Simulation Time

State that can change during play must identify its effective simulation time. The game's canonical date model is defined by the simulation-time specification. Use explicit valid-from/valid-to or event history where a relationship must be historically queryable. Do not store timestamps only as display strings.

### 4.3 Versioning

Authored scenario data and geometry need a world-data version. Changes to authored geometry should be distinguishable from runtime state changes. Runtime damage, flooding, control changes, or facility condition must not overwrite the authored baseline.

## 5. World Geometry Model

### 5.1 World Definition

A world definition identifies the coordinate reference, units, map extent, supported vertical datum or altitude convention, geometry version, and the scenario's authored dataset references.

The coordinate system must provide stable continuous X/Y positions and a defined Z/elevation interpretation. Slope and other sampled properties may be derived from elevation and terrain data rather than redundantly authored at every point.

### 5.2 Spatial Feature

A spatial feature is a canonical geographic object with a stable identifier, geometry type, geometry reference, and owning domain reference or classification. Spatial geometry supports:

- **Area:** polygon or multi-polygon, such as forests, lakes, population-centre extents, and installation footprints.
- **Line:** polyline or multi-line, such as roads, railways, rivers, and political boundary geometry.
- **Point:** discrete coordinate, such as a crossing, resource point, named point of interest, or facility anchor.

Crossings are Point primitives only. Visual bridge artwork may span a road and river, but does not convert the authoritative crossing feature into a Line.

Geometry storage must support spatial queries, intersections, containment, distance, nearest-feature, and feature-to-feature relationships. Topology validation should detect invalid polygons, broken lines, duplicate IDs, invalid coordinates, and prohibited self-intersections.

### 5.3 Geometry and Domain Record Separation

Domain records reference geometry by stable ID. Geometry does not own population, legal sovereignty, company ownership, military control, facility capacity, inventory, or readiness.

A named city may have a geographic anchor and polygon footprint, a Population Centre record, a political-control assessment, and one or more installation references. These records are related but not interchangeable.

## 6. Terrain Objects and Runtime Terrain State

A Terrain Object definition describes a reusable type and its baseline simulation attributes. An authored terrain feature instantiates that type against geometry. Multiple terrain features may overlap spatially and their effects are resolved by the owning simulation systems.

The logical model must distinguish:

- Terrain type definition.
- Authored terrain feature instance.
- Feature geometry reference.
- Baseline physical attributes.
- Current terrain state and state-specific attributes.
- State transition history and provenance where required.
- Asset references for normal, mud, ice, and other visual variants.

Runtime states such as muddy, frozen, flooded, damaged, destroyed, or under repair must not overwrite the authored base definition. A visual asset reference cannot be used as a simulation state field.

## 7. Movement Grid and Derived Spatial Data

The 100 m Movement Grid is a spatial analysis layer, not the canonical world geometry and not the unit movement model.

The schema must permit grid data to be generated, cached, tiled, sparsely stored, or incrementally invalidated. Candidate derived values include terrain samples, movement-cost inputs, slope, visibility inputs, and local environment values. The final storage strategy should be selected through profiling on representative map sizes.

The grid must not require one database record per cell. Nor should a unit's authoritative position be stored only as a grid coordinate. Unit positions remain continuous geographic coordinates, while routing and local analysis may use grid samples, coarser graphs, road networks, vector geometry, or a hybrid.

Derived grid records must identify the source world-data version and any runtime-state revision needed to determine whether the cache is valid.

## 8. Political Geography Schema

### 8.1 Nation Reference

A Nation record is owned by Government/Political domains and exposes a stable identifier and references for national identity and visual assets. World Map must not duplicate the nation as an independently editable geographic entity.

### 8.2 Political Boundary

A political boundary record references one canonical Line geometry and stores boundary type, participating nation/authority IDs, legal or diplomatic status, effective simulation interval, and provenance. Supported types include recognised national border, disputed boundary, administrative boundary, armistice/ceasefire line, demarcation line, and historical/inactive boundary.

The same geographic line may be relevant to more than one political interpretation over time. Geometry and legal meaning must be stored separately.

### 8.3 Territorial Claim

A territorial claim references a claimant nation/authority and a spatial area or region definition. It records status and effective time. Claims may overlap. Claim records do not imply effective control.

### 8.4 Territorial Control Assessment

A territorial control record references a spatial region or partition and records, at minimum:

- Legal sovereign ID, where established.
- Effective controller ID, if any.
- Control status: controlled, contested, uncontrolled, or unresolved.
- Occupation status: not occupied, occupied, contested occupation, or unresolved.
- Optional civil-administration status.
- Effective simulation time and provenance event.
- Observation/freshness metadata only in the player-knowledge model, not as a substitute for authoritative state.

The representation must allow local territorial changes without changing an entire nation or province. Storage may use polygons, spatial partitions, tiled overlays, or a hybrid. It must not require territorial state to align to the 100 m Movement Grid.

### 8.5 Frontline

Frontlines are derived operational geometry based on military positions and control assessments. A frontline geometry record must include an effective simulation time and source revision so it can be rebuilt. Smoothing or simplification is a presentation operation and must not modify territorial control.

Frontline geometry must not be the sole authoritative record of control, occupation, or sovereignty. It may cross national borders and may be fragmented or uncertain.

### 8.6 Political Invariants

- A recognised national border and a frontline are separate records and may occupy different geometries.
- Occupation does not automatically transfer legal sovereignty.
- Military presence does not automatically establish occupation.
- A territorial-control change does not automatically transfer company ownership or inventory.
- A treaty or political decision may change sovereignty without a frontline movement.
- Player knowledge of territory is separate from authoritative territorial state.

## 9. Population-Centre References

A Population Centre record references canonical geographic feature geometry and is owned by Population. It may carry a centre point, boundary geometry reference, stable name keys, and classification such as village, town, city, or capital city.

Population counts, age cohorts, labour, culture, religion, political interest groups, needs, education, employment, wealth, and living standards belong to the Population system. They must not be stored in the base geometry record.

The city name and label position are presentation metadata; the geographic identity and demographic state remain separate.

## 10. Installation and Facility Schema

### 10.1 Installation

An Installation record contains a stable identifier, one or more geometry references, installation classification, and references to the owning, operating, controlling, and access-authority domains. It may reference a footprint Area, a location Point, or multiple sub-features.

### 10.2 Facility

A Facility record references its parent installation where applicable, facility type, supported capabilities, capacity definitions, condition, availability state, effective time, and geometry references. Standalone facilities are permitted.

A single installation may combine airfield, depot, headquarters, maintenance, fuel, ammunition, or other facilities. Do not encode the assumption that one installation has one type as a schema constraint.

### 10.3 Facility Sub-Features

Where required, physical sub-features have their own records and geometry. Examples include runways, taxiways, parking aprons, hangars, berths, piers, dry docks, loading areas, and storage buildings.

A runway record must expose geometry and physical attributes needed by airfield operations, including length/width or values derived from validated geometry, surface, and physical condition. Physical condition and generic facility restrictions must remain distinct from operational availability, aircraft compatibility, and throughput. Airfield and Runway Operations evaluates those operational outcomes using runway attributes, aircraft requirements, weather, shared infrastructure, damage effects, and other current constraints. Runway throughput is not a static property inferred only from runway count.

### 10.4 Ownership and State Separation

Legal owner, operator, territorial sovereign, effective controller, and access authority are separate relationships. Current inventory, unit readiness, cargo, financial value, repair-task progress, and derived operational eligibility remain owned by their respective domains. Facility records may reference these states but must not duplicate them as independent truth. Installation and Facilities owns generic facility identity, declared capabilities, capacity definitions, physical condition, and generic availability restrictions; the consuming operational system determines whether a particular activity can be performed under current conditions.

Capture of a facility may change its effective controller without automatically changing ownership, stock, condition, or operational availability.

## 11. Visual Asset References

A visual asset assignment references an asset ID and role, such as terrain pattern, terrain-state variant, terrain-key swatch, national flag, installation symbol, or unit icon. It may include zoom constraints and renderer parameters.

Visual assets must not be stored as the source of simulation attributes. Alpha may be embedded in the asset or represented by a separate mask; the schema must not mandate a Command Ops 2-style three-file format. Asset provenance, version, and licence metadata should be retained.

Label placement, strategic zoom thresholds, symbol clustering, and layer styling are presentation configuration, not world simulation state.

## 12. Historical State and Provenance

Authored baseline geometry, runtime state, and presentation cache versions must be distinguishable.

For state transitions that affect territory, facility condition, terrain state, or other historically relevant world properties, retain enough information to identify prior state, new state, effective time, cause, and source system. Full event-sourcing architecture is not mandated by this logical schema, but silent overwriting of consequential history is not acceptable.

## 13. Spatial Indexes and Performance

Spatial indexes are derived and rebuildable. Their selection is an implementation decision based on engine and database profiling. The data architecture should support:

- Bounding-box and intersection queries.
- Point-in-polygon and containment.
- Nearest-feature and distance queries.
- Geometry-to-feature relationship queries.
- Local terrain and elevation sampling.
- Fast retrieval of map features within the current viewport.
- Incremental invalidation of grid and rendering caches after geometry or state changes.

Large world maps must not require loading every feature, label, unit, and cell into active memory simultaneously. Tiling and streaming should be considered for authored geometry and derived caches.

## 14. Validation Rules

MapMaker and schema validation should reject or flag:

- Duplicate or missing stable identifiers.
- Invalid or empty geometry where geometry is required.
- References to missing terrain, nation, asset, installation, or facility definitions.
- Incompatible geometry type for a feature, such as a Crossing represented as a Line.
- Conflicting time intervals for a relationship that is required to be unique.
- Invalid facility parent-child references.
- Runways with invalid dimensions or no valid geometry where required.
- Asset variants that refer to missing or incompatible files.
- Runtime state incorrectly written into authored baseline data.
- Territorial records that conflate sovereignty, effective control, occupation, and frontline.
- Grid caches that cannot be associated with the world-data version from which they were generated.

Some conditions are warnings rather than hard errors, such as overlapping terrain Areas, overlapping territorial claims, or multiple facility capabilities at one installation. Those overlaps are valid unless the owning system defines a specific conflict.

## 15. Implementation Sequence

1. Confirm identifier, simulation-time, and unit conventions from shared data-model foundations.
2. Define world and geometry records, geometry versioning, and validation.
3. Define terrain type, feature instance, state, and asset-reference relationships.
4. Define nation boundary, claim, territorial-control, and frontline records.
5. Define population-centre references without duplicating Population state.
6. Define installation, facility, sub-feature, capability, capacity, and ownership relationships.
7. Implement spatial queries and representative test maps.
8. Profile candidate grid/tile/cache storage with realistic geography.
9. Implement derived caches and invalidation.
10. Add runtime-state history and save/load compatibility tests.

## 16. Open Decisions

Do not guess the following in production migrations:

- Concrete database technology and spatial extension.
- Identifier representation and global-ID strategy.
- Coordinate projection and unit encoding.
- Geometry storage format and precision.
- Exact tile size and Movement Grid storage strategy.
- Temporal-history implementation.
- Whether some runtime spatial state is stored in a database, packed files, or engine-native resources.
- Frontline derivation algorithm and territorial partition granularity.
- Exact facility capacity schema and how capability types are normalised.
- Asset packaging and runtime texture formats.

These decisions must be made with representative map data, profiling, and explicit system contracts.

## 17. Acceptance Criteria

- Continuous positions and vector geometry remain authoritative.
- The 100 m grid is a derived analysis layer, not a cell movement model.
- Political sovereignty, claims, control, occupation, and frontlines remain distinct.
- Installation and facility records support multiple facility types per installation.
- Runway geometry and condition can be queried by airfield operations.
- Company ownership and operator state are distinct from territorial control.
- Population state is not duplicated into geography records.
- Visual assets and rendering caches are rebuildable and non-authoritative.
- Spatial queries and validation work on representative map data.
- Schema design avoids requiring one row per 100 m cell without demonstrated need.

## Governing Principle

**Persist the authoritative geography and domain state; derive the grids, indexes, frontlines, labels, and visual caches that make the world fast to query and clear to read.**
