# MapMaker Art and Asset Specification

**Canonical Path:** `technical/map-authoring/mapmaker-art-and-asset-specification.md`  
**Document Type:** Technical Architecture Specification  
**Status:** Foundational Visual Specification

## 1. Purpose

This document defines visual-art and asset-pipeline requirements for Project Sovereign's MapMaker and runtime map renderer. Command Ops 2 is the principal reference for terrain-pattern cartography, but Sovereign may use modern rendering and asset formats. This is an asset and presentation contract, not a final art pack. Production artwork should be generated or commissioned after terrain taxonomy, state models, rendering layers, and data contracts stabilise.

## 2. Primary Reference: Command Ops 2

Command Ops 2 is the principal reference for the desired operational-map appearance. Its mapping system separates terrain patterns, transparency information, and terrain-key graphics. Sovereign should preserve the useful concepts: pattern-based terrain rendering; transparency and blending; a recognisable terrain key or legend; distinct appearances for normal, mud, and ice conditions; and terrain that remains readable at several zoom levels.

The repository contains these user-provided reference assets:

- [MapPat.bmp](https://raw.githubusercontent.com/thelongpause/docs/refs/heads/main/MapPat.bmp)
- [MapPatA.bmp](https://raw.githubusercontent.com/thelongpause/docs/refs/heads/main/MapPatA.bmp)
- [MapKey.bmp](https://raw.githubusercontent.com/thelongpause/docs/refs/heads/main/MapKey.bmp)

They are reference material for visual analysis and prototyping. They must not be treated as a licence or technical requirement to copy Command Ops 2 assets into the shipped game. Final production artwork must be original, appropriately licensed, or otherwise authorised for use.

## 3. Separate Transparency Files Are Optional

Sovereign does not require a separate alpha-mask file. Command Ops 2 uses a separate transparency bitmap as part of its legacy asset pipeline, but Sovereign may use images with embedded alpha channels, separate masks where useful, texture atlases or arrays, vector fills and procedural patterns, shader-driven blending or state overlays, or a hybrid.

The pipeline must preserve functional control over opacity and blending while choosing the format best suited to the renderer, performance, tooling, and artist workflow. If separate masks are supported, MapMaker must validate dimensions, alignment, channel interpretation, and association with the correct pattern.

## 4. Terrain Pattern Assets

A terrain visual definition should identify the terrain type, relevant visual states, scale variants, legend swatch, and rendering parameters. Where relevant, it may reference base/normal, mud, ice/frozen, snow, flooding, fire, degradation, or destruction variants; a compact terrain-key swatch; pattern scale and repeat behaviour; rotation/orientation rules; opacity, blending, tint, or mask settings; minimum and maximum display zoom; and fallback appearance.

Visual state is selected from authoritative simulation state. Artwork must never create the state it depicts. Not all terrain types need every condition variant.

## 5. Zoom Levels and Cartographic Hierarchy

The renderer must support at least three conceptual scales. Exact thresholds can be tuned through testing.

### Strategic Zoom

Prioritise nation identity, flags or equivalent identifiers, readable national borders, frontlines, contested and occupied areas, major cities and geographic labels, major military installations and economic centres, selected player units and important formations, selected moving logistics activity such as cargo aircraft and ships, and company ownership or control of economic facilities through a toggleable economic layer.

Decluttering is essential. The map should not show every unit, transport movement, or settlement label simultaneously at strategic scale.

### Operational Zoom

Prioritise terrain patterns and states; roads, railways, rivers, crossings, obstacles and routes; useful local place names; airfields, runways, ports, depots and installation layouts where available; unit positions, formations, routes and orders; and frontline and territorial-control detail at a useful local scale.

### Local or Inspection Zoom

Where data supports it, show detailed facility geometry, individual runway and taxiway features, local damage, crossing conditions, buildings, and other features relevant to movement or operations. The 100 m Movement Grid remains an analysis layer and need not be drawn as visible cell boundaries.

## 6. Labels and Named Geography

Cities, towns, capitals, rivers, lakes, seas, mountain ranges, passes, and other designated geographic features must support stable names and localisation-ready text fields.

Labels must be zoom-dependent. The renderer should prioritise by feature importance, avoid collisions, manage overlapping labels, and declutter as the view changes. Label placement is a presentation property, not a change to canonical geometry. River names may follow curved lines, while city names may use point anchors or authored label positions.

## 7. National Identity and Political Overlays

The renderer must support national flags or other identifiers, political boundaries, disputed boundaries, armistice/ceasefire lines, frontlines, contested areas, and occupied territory.

These overlays use separate visual semantics. National borders are not a proxy for the frontline, and the frontline is not a proxy for sovereignty. Where legal sovereignty and effective control differ, the interface should make the distinction legible.

Players should be able to toggle layers and choose which political or military information is emphasised. Colour should be reinforced by line style, pattern, symbol, or label so that the display remains interpretable across display settings and for players with colour-vision differences.

## 8. Units and Moving Activity

At strategic zoom, the renderer may show selected military units, formations, aircraft, ships, and logistics movements to make the world feel active. It must not draw every simulation entity at every scale.

Moving cargo aircraft and ships are rendered from authoritative positions and movement state. Route lines, destination markers, or movement trails may be displayed when useful, but they are derived visualisations. Animation must not be the authoritative record of departure, arrival, cargo, or delivery.

Unit symbols must remain distinguishable from facility symbols, national flags, company marks, and logistics indicators. Clustering and filtering should be used where density would obscure the map.

## 9. Economic Control and Facility Ownership

The map must be able to show which company or organisation owns or operates an economic facility. This should be available through an economic map mode or layer and may appear at strategic zoom for selected or major facilities.

The presentation must distinguish national sovereignty, effective military or territorial control, legal facility ownership, facility operator, and facility operational status. These attributes can differ. The map must not infer company control from national colour, military control from company branding, or facility ownership from the surrounding flag.

## 10. MapMaker Authoring Workflow

MapMaker should allow authors to assign terrain patterns and visual-state variants to canonical terrain definitions; import and preview image assets and optional masks; inspect transparency, blending, tiling seams, pattern scale, and colour at multiple zoom levels; assign terrain-key swatches; preview normal, mud, ice, and other supported states; preview labels, borders, flags, frontlines, units, and installation symbols; toggle rendering layers; validate missing assets and invalid references; and package assets for runtime without changing simulation data.

Reference-image import and visual matching may assist the workflow, but must not silently generate authoritative geography or terrain states without explicit author confirmation.

## 11. Runtime Rendering and Performance

The runtime renderer should use an appropriate combination of tiled textures, atlases, cached vector geometry, level-of-detail representations, instancing, and culling. It should avoid requiring a unique full-resolution texture for every 100 m grid location.

Terrain patterns and overlays must support large maps and continuous zoom. Labels, units, and moving activity should be filtered or clustered by scale and visibility. Rendering caches may be rebuilt from authoritative data and must never become the only copy of world state.

## 12. Asset Naming and Versioning

Asset identifiers should be stable and independent of file names. Canonical terrain definitions reference asset IDs and variant roles, while an asset manifest resolves those IDs to runtime files.

The pipeline should record asset version, dimensions, format, colour-space expectations, alpha behaviour, intended use, and licence or provenance metadata. Updating artwork must not alter terrain identity, simulation effects, or saved scenario state.

## 13. Acceptance Criteria

- Visual direction is demonstrably informed by Command Ops 2's pattern-based operational cartography.
- Separate transparency files are optional rather than mandatory.
- Terrain patterns support state variants without owning the state itself.
- Terrain-key swatches are distinct from full map patterns.
- Named geographic features display names at suitable zoom levels.
- Strategic zoom can display national identity, borders, frontlines, selected units, logistics activity, and economic facility ownership without unacceptable clutter.
- Political sovereignty, military control, occupation, company ownership, and facility operation are visually distinguishable.
- MapMaker can preview patterns, masks, variants, and labels before export.
- Artwork changes cannot modify simulation state.
- Asset provenance and versioning are recorded.

## Governing Principle

**Command Ops 2 is the cartographic reference; Sovereign should preserve the authoring and visual strengths of its pattern system without inheriting unnecessary legacy file-format constraints.**
