# World Map System

**Canonical Path:** `systems/world/world-map-system.md`  
**System:** World Map  
**Document Type:** Systems Specification  
**Status:** Foundational Specification  
**Starting World Date:** 1 January 1955

---

## 1. Purpose

The World Map System defines the geographic model through which Project Sovereign represents, queries, renders, authors and persists the physical world.

The World Map is a continuous geographic environment. It is not a collection of disconnected battle maps, nor is it a static background image. Geographic features are represented as structured spatial data and remain available to simulation systems regardless of the player's current viewport, zoom level, selected theatre or location.

The World Map provides geographic truth and spatial query interfaces to Movement, Visibility and Detection, Combat, Logistics, Infrastructure, Population, Economy, Weather, Climate, Construction and other systems.

## 2. Architectural Principles

### 2.1 One Continuous World

The world uses a common geographic coordinate space. The player may change scale, viewport, operational focus or theatre without replacing the underlying world or resetting its state.

Systems may partition data internally for storage and performance. Such partitions are implementation details and must not create artificial geographic discontinuities.

### 2.2 Vector-Defined Geography

Geographic features are authored as spatial objects rather than being limited to a painted raster background. The principal mapping primitives are:

- **Area:** a bounded polygonal feature.
- **Line:** a linear feature such as a road, railway or river.
- **Point:** a discrete location such as a bridge crossing, ford or other point feature.

Additional geometry types may be introduced only through a documented extension to the canonical spatial model.

Features may overlap. A location may simultaneously intersect forest, slope, road infrastructure, a river corridor and weather-affected ground. Visual stacking does not determine simulation precedence.

### 2.3 Movement Grid

The World Map exposes a **100-metre Movement Grid** for movement, routing and other systems that require a regular spatial sampling model.

The Movement Grid is a simulation abstraction over the continuous geographic model. It does not require the rendered map to appear as squares, nor does it replace the underlying vector features.

Each Movement Grid Cell must be addressable within the world coordinate system and able to query the geographic and environmental inputs relevant to simulation. A cell may derive its characteristics from multiple overlapping spatial features.

The grid is not the sole representation of geography. Features narrower than a cell, including roads, bridges, rivers and crossings, must retain their own geometry and identity. Systems must use feature-level queries where cell aggregation would erase a meaningful constraint.

### 2.4 Canonical Data and Derived Data

The World Map distinguishes three classes of information:

1. **World Definition:** authored geography, elevation, Terrain Objects, Population Centres, infrastructure geometry, initial conditions and stable feature identifiers.
2. **World State:** mutable conditions such as weather, flooding, mud, damage, destruction, repairs, occupation and other simulation-time changes.
3. **Derived Data:** calculated or cached outputs such as cell terrain composition, movement costs, visibility acceleration structures, route connectivity and rendered meshes.

World Definition is authoritative for authored geography. Each mutable state variable has one authoritative owning system. Derived Data must be reproducible from authoritative inputs and may be invalidated or rebuilt without changing simulation truth.

No rendering layer or cache may become an independent source of truth.

## 3. Coordinate and Elevation Model

### 3.1 Geographic Coordinates

Every spatial feature must use the canonical World Coordinate Reference System defined by the Technical Specification. The implementation must distinguish geographic coordinates used for world authoring from local or projected coordinates used for efficient distance, area and movement calculations.

Coordinate reference, projection, precision, origin, bounds and conversion rules must be explicitly documented before implementation data is finalised. Individual systems must not introduce incompatible coordinate conventions.

### 3.2 Altitude and Height

The World Map represents the elevation of the natural terrain surface through an **Altitude Layer**. Terrain Objects may additionally possess a **Terrain Object Height**, representing vertical extent above the local terrain surface.

For a relevant point on an object:

**Effective Elevation = Altitude Layer + Terrain Object Height**

This is a conceptual relationship, not a complete Line of Sight algorithm. The Visibility and Detection System defines how terrain profiles, observer and target heights, curvature if modelled, and the LOS corridor are evaluated.

An Area feature may have a spatially varying elevation or height profile. Implementations must not assume that one scalar height adequately describes every feature unless the feature definition explicitly declares a constant profile.

### 3.3 Slope

Slope is derived from the elevation surface and local sampling method. It is not an independently authored replacement for altitude. Movement and construction systems may use slope as an input, but must own their own rules for converting slope into movement, stability or construction effects.

## 4. World Data Ownership

The World Map System owns:

- the shared spatial reference and geographic feature identity;
- spatial geometry and spatial query contracts;
- the Altitude Layer interface;
- Movement Grid addressing and geographic sampling contracts;
- import, validation and versioning of authored world definitions;
- the separation of World Definition, World State and Derived Data.

It does not own the detailed rules for movement speed, detection, weapon effects, logistics throughput, population demographics, economic valuation or weather simulation.

### 4.1 Cross-System Responsibilities

| System | Responsibility |
| --- | --- |
| World Map | Geographic identity, geometry, coordinates, altitude interface and spatial queries |
| Terrain Objects | Terrain feature definitions, physical attributes and supported states |
| Movement | Unit capability, passability, movement cost and route traversal |
| Visibility and Detection | LOS, visibility degradation, signatures, observation and sensor effects |
| Combat and Weapons | Hit probability, weapon effects, damage and engagement outcomes |
| Weather and Climate | Environmental conditions and the rules for weather-driven state transitions |
| Infrastructure / Construction | Building, repairing, upgrading and maintaining infrastructure |
| Population | Population totals, demographics, labour, needs and demographic change |
| Economy | Financial records, prices, revenue, expenditure, taxes and economic transactions |
| Industry Data | Canonical Industry Types and establishment configuration |
| Production | Facilities, capacity, orders and production execution |
| Inventory | Authoritative physical stock and inventory movements |
| Logistics | Transport networks, shipment, capacity, transit and delivery |

Cross-system integrations must use explicit interfaces and stable identifiers. A geographic feature may be referenced by several systems, but that does not transfer ownership of its canonical definition.

## 5. Spatial Feature Identity and Lifecycle

Every authored spatial feature must have a stable identifier unique within the relevant namespace. Names and display labels are not identifiers.

References to a feature must remain resolvable across save/load, scenario changes and data-version updates. Deleting or replacing a feature referenced by persistent state requires a documented migration or retirement procedure.

Where a feature is divided, merged or redrawn, the authoring pipeline must record a migration relationship when the change affects persistent references. A geometric edit must not silently orphan infrastructure, ownership, population, damage, reservations or other state attached to the feature.

## 6. Spatial Queries

The World Map exposes common queries for:

- point containment and feature intersection;
- line traversal through Areas and grid cells;
- nearest-feature and nearest-location searches;
- feature overlap and adjacency;
- elevation and slope sampling;
- Movement Grid Cell lookup;
- geographic distance and area calculation;
- feature lookup by stable identifier.

Query semantics must define boundary behaviour, coordinate precision, invalid geometry handling and deterministic tie-breaking. Systems must not implement subtly different geographic interpretations for the same query.

Spatial queries return relevant features and raw geographic facts. The consuming simulation system determines how those facts affect its own rules.

## 7. Movement Grid Contract

Each Movement Grid Cell has a stable address derived from its grid coordinates and world-grid definition. Cell properties may include derived terrain composition, elevation samples, slope samples and references to intersecting Terrain Objects.

The Movement Grid does not itself determine a universal movement cost. The Movement System evaluates Unit capabilities, Terrain Objects, Terrain State, weather, supply, orders and other relevant modifiers.

Cell-level aggregation must preserve critical feature constraints. In particular:

- a bridge or ford cannot be treated as ordinary traversable ground merely because it occupies a small fraction of a cell;
- a narrow river or road must not disappear solely because it is smaller than the cell;
- impassable features and crossing requirements must remain queryable at feature level;
- overlapping terrain must not be collapsed into one mutually exclusive terrain label.

## 8. World State and Persistence

World state persists independently of rendering and player attention. Leaving an area, changing zoom, changing theatre or removing an area from the active viewport must not reset its state.

The system must support persistent references to mutable state owned by the relevant simulation system. Examples include damaged bridges, flooded land, road repairs and destroyed infrastructure.

The World Map stores or exposes the geographic association of these states but must not duplicate the authoritative state held by Weather, Infrastructure, Combat, Logistics or other owning systems.

## 9. Rendering Contract

Rendering is a presentation of World Definition, World State and selected Derived Data. Rendering order controls visual layering only.

The renderer must not decide:

- which Terrain Object takes precedence in simulation;
- whether a Unit can traverse a feature;
- whether a target can be observed;
- whether a bridge is operational;
- whether a Population Centre has been damaged;
- whether stock, people or infrastructure exist.

Visual representations may vary by zoom level and may use simplified geometry, symbols, textures or aggregation. Such simplification must not mutate authoritative simulation data.

## 10. Map Authoring and Runtime

Project Sovereign retains a dedicated **MapMaker** as the intended world-authoring capability. Godot is the initial runtime and may also be used to prototype authoring workflows, but the simulation data model must remain independent of the renderer and editor.

MapMaker authors World Definition, validates it and exports versioned canonical data. Godot imports the same canonical model for rendering, interaction and simulation queries. Neither tool may create a competing set of terrain or movement rules.

The MapMaker specification is maintained at `technical/map-authoring/mapmaker-specification.md`.

## 11. Performance and Scale

The world must support a continuous operational map while avoiding the requirement to materialise every possible property for every cell at all times.

Implementation should evaluate:

- spatially partitioned or tiled storage;
- sparse feature storage;
- derived-data caching and invalidation;
- hierarchical routing and local detailed routing;
- on-demand terrain sampling;
- local visibility acceleration structures;
- asynchronous world-data loading;
- persistence of state outside the active viewport.

Partitioning must remain invisible to simulation semantics. Data structures and performance targets are implementation decisions to be validated through profiling against representative world sizes, feature densities and simultaneous simulation workloads.

## 12. Validation Requirements

World data validation must reject or report:

- invalid or self-intersecting geometry where unsupported;
- missing, duplicate or malformed stable identifiers;
- unknown feature types or invalid attribute values;
- unresolved cross-feature references;
- invalid coordinate systems or out-of-bounds geometry;
- impossible elevation or height values under the defined schema;
- unsupported Movement Grid configuration;
- state records referencing missing features;
- duplicate authoritative definitions for the same feature;
- derived data that does not match the source version from which it was generated.

Validation errors must identify the affected feature and the corrective action where possible.

## 13. Acceptance Criteria

The World Map System is conformant when:

1. The world is represented as one continuous geographic coordinate space.
2. Area, Line and Point features can overlap without losing identity.
3. The 100-metre Movement Grid is available as a simulation abstraction, not as the sole geographic representation.
4. Altitude, object height and effective elevation remain distinct concepts.
5. World Definition, World State and Derived Data have explicit ownership boundaries.
6. Simulation state persists outside the active viewport.
7. Rendering and visual layer order do not determine simulation rules.
8. Godot and MapMaker consume the same canonical world-data model.
9. Spatial queries have deterministic, documented semantics.
10. Large-world performance can be improved through partitioning and caching without changing simulation truth.

## Political Geography and Continuous Movement Clarifications

### Political Boundaries Are Not Terrain Objects

National boundaries are first-class political-geography features and must be renderable as visible lines at appropriate zoom levels. They may follow rivers, ridgelines, coastlines, surveyed borders, or authored geometric paths, but they are not inferred automatically from the terrain that they follow.

The World Map System stores and serves the spatial geometry required to display and query boundaries. The National Territory and Control System is authoritative for each boundary's political meaning, legal status, claims, and effective territorial control. A boundary's visible geometry must not itself determine sovereignty, occupation, military control, or access permissions.

The world must support, at minimum, internationally recognised borders, disputed boundaries, and temporary or negotiated lines such as armistice and ceasefire lines. These may overlap or differ. Their legal and simulation status is data, not a property of line colour or draw order.

### The Movement Grid Is a Spatial Analysis Layer

The 100 m Movement Grid is not a discrete unit movement board. Units retain continuous geographic positions and move through continuous space over time. Movement resolution considers orders, routes, speed, formation, terrain, roads and other infrastructure, obstacles, weather, unit capabilities, and the current simulation state.

The grid supports local spatial calculations such as terrain sampling, movement-cost evaluation, line-of-sight queries, local effects, and other analyses where a regular sampling surface is useful. Routing may use coarser hierarchical graphs, road and rail networks, continuous geometry, or other optimised representations. The simulation must not require a unit to enter and leave each 100 m cell as its fundamental movement action.

Grid resolution is an implementation parameter and must not become a gameplay assumption. Continuous positions and authoritative vector geometry remain available even when calculations use grid-derived caches.

### Strategic Information Layers

The map renderer may display national flags, political borders, frontlines, contested areas, military units, active transport, installations, and company control of economic facilities. These are presentation layers driven by the authoritative systems that own each fact.

The renderer must support layer visibility, filtering, and zoom-dependent simplification. Displaying a symbol or line must never create or modify the underlying political, military, economic, or logistics state.


## Related Specifications

- [Movement and Transportation System](movement-and-transportation-system.md) — physical movement and route traversal use World Map geometry and spatial queries.
- [Weather and Environment System](weather-and-environment-system.md) — environmental state is spatially referenced through the World Map.
- [Terrain Objects](terrain-objects.md) — terrain features and persistent terrain state.
