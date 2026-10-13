# Project Sovereign MapMaker

**Canonical Path:** `technical/map-authoring/mapmaker-specification.md`  
**Document Type:** Technical Architecture Specification  
**Status:** Foundational Architecture Decision  
**Runtime Engine:** Godot  
**World Model Owner:** World Map System

---

## 1. Purpose

MapMaker is the intended world-authoring environment for Project Sovereign. It provides a controlled workflow for creating, editing, validating, previewing and exporting the canonical World Definition consumed by the game runtime.

MapMaker exists because a continuous, vector-defined operational world with overlapping features, elevation, persistent infrastructure and a 100-metre Movement Grid requires more than painting a static map image. It needs inspectable spatial data, validation tools and the ability to preview the consequences of authored geography.

Godot remains the initial runtime and rendering engine. Selecting Godot does not require the project to abandon a dedicated authoring tool. Equally, the MapMaker ambition does not justify prematurely building an elaborate independent editor before the data model and authoring workflows have been tested.

## 2. Architecture Decision

**Decision:** Preserve MapMaker as a project capability and architectural goal. Prototype the earliest workflows using Godot where that is the fastest route, but keep canonical world data independent of the Godot scene tree, editor UI and rendering assets.

The architecture has three responsibilities:

1. **MapMaker:** authors and validates World Definition.
2. **Canonical World Data:** stores versioned geographic definitions and scenario initial conditions.
3. **Godot Runtime:** imports World Data, renders the map, provides player interaction and supplies geographic queries to simulation systems.

The runtime and authoring tool must consume the same canonical data model. Neither should maintain a competing set of terrain types, coordinate semantics or movement rules.

## 3. Authoritative Data Layers

### 3.1 World Definition

World Definition contains authored, relatively stable data, including:

- world coordinate reference and bounds;
- geographic geometry and elevation data;
- Terrain Object definitions and instances;
- Population Centre geometry and initial scenario values;
- roads, railways, rivers, crossings and other mapped features;
- locations and geographic references for facilities, resource deposits and installations;
- initial Terrain States;
- stable identifiers, provenance and schema versions.

Scenario data may override permitted initial conditions, but must identify the source definition and override explicitly.

### 3.2 World State

World State contains mutable values generated or changed during simulation, such as weather, flooding, mud, freezing, damage, destruction, repairs, occupation and population change.

The owning simulation system is authoritative for each state variable. MapMaker may inspect or preview state and may author scenario starting state where permitted, but it must not become the runtime authority for state transitions.

### 3.3 Derived Data

Derived Data contains caches and calculations generated from World Definition and current World State. Examples include Movement Grid samples, spatial indexes, movement previews, visibility acceleration structures, route connectivity, render meshes and level-of-detail geometry.

Derived Data must be tagged with the versions of its source inputs. It can be rebuilt or invalidated without changing authoritative data.

## 4. Core Authoring Workflows

MapMaker should support these workflows in dependency order.

### 4.1 World Setup

- establish world bounds and coordinate reference;
- set the geographic projection and precision according to the Technical Specification;
- configure the 100-metre Movement Grid origin, orientation and extent;
- load or import base geographic and elevation sources;
- validate coverage, alignment and source metadata.

### 4.2 Terrain and Geography

- create and edit Area, Line and Point geometry;
- place overlapping Terrain Objects without forcing mutually exclusive classifications;
- inspect altitude, height, slope and terrain composition;
- edit supported attributes and initial Terrain State;
- detect invalid geometry, duplicate IDs and unsupported type/primitive combinations.

### 4.3 Infrastructure and Crossings

- create roads, tracks, highways, railways, rivers and crossing features;
- associate bridges and fords with the relevant transport and water features;
- validate network intersections and geographic continuity;
- inspect crossing restrictions and linked feature identity;
- distinguish authored connectivity from Logistics and Movement rules.

### 4.4 Population Centres

- draw settlement Area geometry;
- assign settlement class and administrative references;
- author scenario-start demographic values through the Population data interface;
- inspect density or settlement zones where represented;
- link contained or adjacent infrastructure without merging its identity into the settlement.

### 4.5 Economic and Industrial Geography

- place or reference resource deposits, industrial sites, production facilities and other geographic installations where those systems support them;
- validate references to Industry, Production, Economy, Inventory and Logistics entities;
- ensure MapMaker records location and spatial relationships, not independent economic ledgers, output, inventory or workforce state.

### 4.6 Validation and Export

- run schema, geometry and reference validation;
- preview relevant derived layers;
- export versioned canonical data;
- generate a validation report and manifest;
- block export for fatal validation errors while allowing explicitly classified warnings.

## 5. Preview Layers

MapMaker should be able to display optional inspection overlays, including:

- Area, Line and Point feature geometry;
- Terrain Object type and state;
- altitude and slope;
- Movement Grid boundaries and cell identifiers;
- overlapping Terrain Objects;
- movement constraints or movement-rate previews supplied by the Movement System;
- visibility and Line of Sight previews supplied by Visibility and Detection;
- road, rail and crossing connectivity;
- Population Centre boundaries and density layers;
- invalid geometry, missing references and schema errors.

Previews are diagnostic. A movement-cost overlay or visibility preview must call or reproduce a versioned implementation of the owning system's rules; it must not create a second authoritative formula inside MapMaker.

## 6. Data and Identifier Requirements

Every authored feature must have a stable ID independent of its display name, geometry serialization order or current screen position.

The export format must include:

- schema version;
- world-definition version;
- coordinate reference and projection metadata;
- Movement Grid definition;
- feature IDs and canonical type references;
- geometry and relevant attributes;
- cross-feature references;
- initial state where supported;
- provenance for imported or generated data;
- validation report or report reference.

Geometry edits must preserve identity where the same feature remains conceptually the same. Split, merge, delete and replace operations must identify affected references and require explicit resolution when they would orphan other entities.

## 7. Runtime Integration

Godot imports canonical World Data through a dedicated import/build pipeline. The runtime may build render meshes, spatial indexes, cell samples and other derived structures, but these must remain traceable to the source data version.

The runtime provides geographic queries to simulation systems. Those queries must use the World Map System contract rather than reading arbitrary scene-node transforms as the canonical world coordinates.

Game saves store mutable World State and references to the relevant World Definition version. A save must not require MapMaker to be running. Updated world definitions require a migration or compatibility check when loaded against an existing save.

## 8. World Partitioning and Performance

The world may be partitioned into spatial chunks or tiles for storage, loading, editing and caching. Partitioning must not create separate maps or break continuity at boundaries.

Authoring and runtime should support:

- spatial indexing;
- lazy loading of non-active world data;
- cached and versioned Movement Grid derivations;
- independent rendering level of detail;
- chunk-aware editing with cross-boundary validation;
- global feature identity across chunks;
- deterministic handling of features crossing chunk boundaries.

The first implementation should profile realistic samples before committing to a complex streaming architecture. The project must not materialise every property of every 100-metre cell at all times unless profiling demonstrates that this is viable and necessary.

## 9. Godot and Custom Tooling

Godot is the selected initial runtime. It is appropriate for prototyping map rendering, spatial interaction, layered overlays and game-facing queries. It may also host early MapMaker tools if doing so reduces initial engineering cost.

A separate MapMaker application should be developed when actual authoring tests demonstrate that the Godot editor or in-game tooling is insufficient for efficient geometry editing, bulk data management, validation or large-world workflows.

A custom rendering engine is not a current requirement. It should only be reconsidered after measured performance tests show that Godot cannot meet defined targets after reasonable profiling and optimisation.

## 10. Non-Goals

MapMaker must not:

- own or independently simulate movement rules;
- own visibility, detection or combat algorithms;
- own economic, demographic, production or inventory ledgers;
- use render order to resolve simulation conflicts;
- store the only copy of mutable simulation state;
- require all world features to be rasterised into the Movement Grid;
- force all terrain into one exclusive label per cell;
- require a custom game engine before a Godot prototype is measured.

## 11. Implementation Phases

### Phase 1: Canonical Model and Import Prototype

Define the world-data schema, IDs, geometry conventions, coordinate reference, feature types and validation rules. Import a small representative region into Godot and verify rendering and spatial queries.

### Phase 2: Focused Authoring Tools

Add editing for Area, Line and Point features, overlap inspection, IDs, attribute editing, validation and export. Use representative roads, rivers, crossings, vegetation, ground and settlement geometry.

### Phase 3: Derived-Layer Preview

Add altitude, slope and Movement Grid inspection. Integrate movement and visibility previews through explicit interfaces to the owning systems.

### Phase 4: Scale and Performance Tests

Profile representative data sizes and feature densities. Test chunk boundaries, persistent state, save/load, spatial queries and concurrent system access. Use measurements to determine the required caching and partitioning approach.

### Phase 5: Dedicated Editor Decision

Only after the preceding phases, decide whether MapMaker remains Godot-based or becomes a separate application. The decision must be driven by measured authoring efficiency and technical constraints, not by the existence of a separate-tool ambition alone.

## 12. Acceptance Criteria

MapMaker architecture is conformant when:

1. It edits canonical World Definition rather than a renderer-specific copy.
2. Godot imports the same canonical model.
3. Area, Line and Point features can overlap and retain stable identities.
4. World Definition, World State and Derived Data are separated.
5. The 100-metre Movement Grid is inspectable without replacing vector geometry.
6. Validation catches geometry, schema and reference errors before scenario load.
7. Previews use the owning systems' rules and do not create competing simulation logic.
8. State persists independently of the active viewport and authoring application.
9. Large-world optimisation does not change geographic or simulation semantics.
10. Decisions about a separate editor or custom engine follow prototyping and profiling.
