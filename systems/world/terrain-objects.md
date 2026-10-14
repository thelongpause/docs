# Terrain Objects

**Canonical Path:** `systems/world/terrain-objects.md`  
**System:** Terrain Objects  
**Document Type:** Systems Specification and Canonical Terrain Definition  
**Status:** Foundational Specification  
**Starting World Date:** 1 January 1955

---

## 1. Purpose

Terrain Objects represent geographic Areas, Lines and Points used to describe physical terrain, environmental features and terrain infrastructure within the World.

Terrain Objects form part of the geographic truth of Project Sovereign. They may directly or indirectly influence movement, visibility, combat, logistics, construction, infrastructure, civilian transportation and other simulation systems.

Terrain Objects may overlap spatially. For example, a geographic location may simultaneously intersect Forest, Slope, Mud, Road and Heavy Rain. These are not mutually exclusive classifications. Each applicable object and environmental state remains available to the relevant simulation system.

The Map Layer Order determines visual representation only. It does not establish simulation priority or determine how effects combine.

## 2. Scope and Ownership

This document defines the Terrain Object model, canonical categories, base attributes, Terrain State principles and the initial catalogue of Terrain Object types.

It does not define the complete algorithms for Movement, Visibility and Detection, Combat, Weather, Engineering, Construction or Logistics. Those systems consume Terrain Object data and own their calculations.

The World Map System owns the common spatial reference, coordinate model, geometry queries and Movement Grid interface. Terrain Objects use that shared model rather than introducing a separate coordinate system.

## 3. Mapping Primitives

Terrain Objects use the following primitives:

- **Area:** a bounded geographic region, such as Forest, Lake, Marsh or Grassland.
- **Line:** a linear feature, such as Road, Railway, Stream or River.
- **Point:** a discrete feature or crossing location, such as a Ford or Bridge.

A primitive describes geometry, not the complete simulation behaviour of the object. A road remains a Line even when it intersects a settlement Area. A crossing, including a ford or bridge, is represented by a Point primitive only. The crossing Point is associated with the relevant transport and water geometry through explicit feature relationships. Its symbol or artwork may visually span a road and river, but this does not create a Line or Area crossing primitive. More detailed physical structure may be introduced in a later, separately specified model, but it is not required for the initial crossing model and must not change the authoritative crossing primitive.

## 4. Terrain Object Definition

A Terrain Object definition describes the kind of geographic feature and its default physical properties. A placed Terrain Object instance supplies its stable identifier, geometry, location and any scenario-specific overrides permitted by the schema.

At minimum, an instance must identify:

- stable Terrain Object ID;
- canonical Terrain Object type;
- mapping primitive and geometry;
- relevant elevation or height profile;
- initial Terrain State;
- applicable attributes and overrides;
- links to associated features where required;
- source or authoring provenance where available.

Names and labels are descriptive and must not be used as persistent identity.

## 5. Terrain Object Categories

Initial categories are:

- **Crossings:** geographic locations or structures through which a Unit may cross an otherwise restrictive feature.
- **Roads:** linear land-transport infrastructure used by military Units and civilian transportation.
- **Railways:** rail infrastructure used by rail transport and, where applicable, military logistics.
- **Rivers and Water:** water features that may influence movement, visibility, combat, hydrology and crossing requirements.
- **Vegetation:** natural or cultivated vegetation and land-cover Areas.
- **Ground:** surface and ground-condition Areas.

Categories organise definitions. They do not prescribe exclusive geographic coverage or simulation priority.

## 6. Base Attributes

Terrain Object types may define the following base attributes:

- Height;
- Movement Effect;
- Visibility Effect;
- Hit Probability Effect;
- Health Points;
- physical properties and supported Terrain States;
- relevant relationships to other spatial features.

Not every attribute is meaningful for every type. A Lake does not ordinarily require structural Health Points. A Bridge or Road may require integrity, damage thresholds and operational states. The type definition must declare which attributes apply, which are optional and what their units and valid ranges are.

Attributes must be typed and unit-defined. Missing, undefined and zero values must not be treated as interchangeable.

## 7. Height, Altitude and Effective Elevation

Terrain Object Height is the vertical extent of an object above the underlying natural terrain surface, measured in metres. The Altitude Layer represents the elevation of the natural terrain surface.

For a relevant point on an object:

**Effective Elevation = Altitude Layer + Terrain Object Height**

For example, Forest on terrain at 50 metres altitude with an object height of 15 metres has a nominal effective elevation of 65 metres at the relevant point.

This is a conceptual relationship. The Visibility and Detection System defines how object profiles, observer height, target height and the Line of Sight corridor interact. Area features may have varying terrain altitude and object height; a single constant value must not be assumed unless declared by the definition.

Altitude and Height are distinct. The former describes the underlying terrain surface; the latter describes the object's vertical extent above that surface.

## 8. Movement Effects

Movement Effects describe the base movement-rate factor associated with a Terrain Object for a given Unit Movement Class. A factor of 75% means that, before other modifiers, the relevant movement rate is 75% of the applicable baseline.

Movement Effect is not itself a complete movement cost or travel-time calculation. The Movement System determines how overlapping objects, Unit capabilities, orders, weather, supply, Terrain State, engineering actions and other modifiers combine.

### 8.1 Initial Movement Classes

The initial classes are:

- **Mechanised**
- **Motorised**
- **Heavy Armour**
- **Light Armour**
- **None**

The Unit System owns the authoritative definition and assignment of Unit Movement Classes. The list may be extended through a coordinated change to the Unit and Movement specifications.

The class definitions must not be assumed to be mutually exclusive until the Unit System specifies whether a Unit can carry multiple movement capabilities and how its effective class is selected for a particular action.

### 8.2 Impassability

A Movement Effect of 0% indicates that the Terrain Object is normally impassable to the relevant Movement Class. It does not necessarily mean that traversal is impossible under all circumstances.

The Movement System may permit traversal when a Unit possesses a relevant capability, equipment, training or support, or when an engineering action changes the crossing conditions. Examples include amphibious vehicles, combat swimming, specialised bridging equipment and constructed crossings.

The Terrain Object defines the physical constraint. The Movement System determines whether the Unit can overcome it. A special capability must not silently rewrite the underlying Terrain Object definition.

### 8.3 Combining Movement Effects

Overlapping Terrain Objects must remain individually queryable. This document does not prescribe whether the Movement System combines effects additively, multiplicatively, by constraint, by route mode or through another explicit rule.

The Movement System must document a deterministic composition rule and must distinguish a hard traversal constraint from a rate modifier. Rendering order is never a valid combination rule.

## 9. Visibility Effects

Visibility Effects describe how a Terrain Object degrades visual observation through its geometry. Visibility degradation and physical Line of Sight obstruction are separate concepts.

A Terrain Object may reduce observation without fully blocking LOS. Conversely, a feature may physically obstruct LOS because its effective vertical profile intersects the LOS corridor, regardless of its visibility-degradation value.

### 9.1 Base Visibility Reduction

The initial authoring model uses **Visibility Reduction per 100 metres**. This is a reduction in percentage points for each 100 metres travelled through the relevant feature, not a percentage of the remaining visibility.

For a single Terrain Object, the draft linear model is:

**Remaining Visibility = max(0, 100 − (Distance Through Terrain ÷ 100 × Visibility Reduction per 100m))**

For a reduction of 20 percentage points per 100 metres:

| Distance through feature | Remaining visibility |
| ---: | ---: |
| 0 m | 100% |
| 100 m | 80% |
| 200 m | 60% |
| 300 m | 40% |
| 400 m | 20% |
| 500 m | 0% |

This is an initial terrain-authoring convention, not the final sensor detection probability. The Visibility and Detection System must define whether this linear model is retained, how distances are sampled, and how effects from multiple objects and environmental conditions are combined.

The final result must be clamped to its valid range. Reaching zero in this terrain-degradation model does not by itself define all sensor or non-visual detection outcomes.

### 9.2 Observation Target Classes

Initial terrain visibility profiles may distinguish:

- **Personnel**
- **Vehicle**
- **Guns**

These are terrain-facing target profiles, not a replacement for canonical Unit categories. The Visibility and Detection System owns the final target-signature taxonomy and mapping from Units and equipment to observation profiles.

Additional profiles may be added for relevant sensors or target types. A terrain definition should not encode every possible Unit state as a separate Terrain Object.

### 9.3 Target Observation State

The target's current state may modify observability after the terrain's base effect has been evaluated. Initial state dimensions include:

**Movement Status**
- Stationary
- Careful
- Moving
- Fast

**Engagement Status**
- Not Firing
- Firing

**Deployment Status**
- None
- Taking Cover
- Dug-In
- Entrenched
- Fortified

These are provisional shared vocabulary items. Their canonical definitions and permitted transitions must be owned by the relevant Unit, Military and Visibility specifications. Terrain Objects may supply modifiers by applicable target profile and state, but must not become the authority for Unit activity or deployment state.

A stationary, entrenched Unit in Forest may be less observable than personnel moving quickly and firing in the same Forest. The difference arises from the interaction between the terrain and the target's state, not from a change to the terrain itself.

### 9.4 Weather, Illumination and Sensors

Relevant environmental inputs may include clear weather, rain, fog, snow, atmospheric conditions, time of day and lunar illumination. The Weather and Climate System owns weather state; the World Time and Illumination specification owns the canonical illumination model.

Observation equipment may include binoculars, optical and telescopic sights, rangefinders, night-vision devices, thermal imaging, infrared, radar and electro-optical systems. The Sensor System determines what each sensor can detect and how it is affected by terrain and environment.

Terrain Objects describe physical properties and terrain-specific effects. They do not grant sensor capability or remove a feature's physical geometry.

### 9.5 Multiple Terrain Objects

A Line of Sight may pass through several Terrain Objects. Each relevant feature contributes the portion of its effect supported by the Visibility and Detection rules.

The Visibility and Detection System must define how overlapping effects are combined and prevent duplicate application when the same feature is encountered through multiple spatial-query paths. An implementation must not blindly sum every overlapping modifier without regard to geometry, distance travelled within the feature or effect type.

## 10. Hit Probability and Weapon Effects

Hit Probability Effect is a terrain-facing input to Combat and Weapons calculations. It may represent the influence of a Terrain Object on direct-fire engagements or area effects.

Relevant engagement classes include:

- **Direct Fire:** for example, rifles, machine guns, tank guns, autocannons and anti-tank weapons.
- **Area Fire:** for example, mortars, artillery, rockets and air-delivered area effects.

Terrain may influence target exposure, line of sight, fragmentation, blast, airburst effects and protection. The effect depends on weapon, ammunition, fuze, target, geometry and engagement conditions.

A generic Hit Probability Effect must not be treated as a universal multiplier for every weapon. Combat and Weapons own the authoritative calculation and may require separate parameters for hit probability, damage attenuation, protection and area-effect interaction.

## 11. Health, Damage and Destructibility

Health Points represent physical integrity only for Terrain Objects whose structure or condition can be damaged in a meaningful way. The type definition determines whether integrity is applicable, its unit or scale, damage thresholds and supported outcomes.

Health Points reaching a threshold may cause a Terrain State transition. The Combat, Weapons, Infrastructure and Engineering systems determine how damage is inflicted and how repairs are executed. Terrain Objects define the physical state model and relevant base properties, not the full attack or repair algorithm.

A Lake does not ordinarily require structural Health Points. A bridge, road, railway or other infrastructure feature may possess integrity and damage thresholds.

## 12. Terrain State

Terrain State describes the current physical condition of a Terrain Object at a point in simulation time. The Terrain Object type describes what the feature is; Terrain State describes its current condition and operational capability.

For example:

- Terrain Object: Unpaved Road
- Current State: Muddy

The underlying road remains an Unpaved Road. Its current state modifies the effects and appearance applicable under the relevant system rules.

### 12.1 State Definitions

Possible states include, where meaningful to the type:

- Base
- Muddy
- Frozen
- Snow-Covered
- Flooded
- Damaged
- Destroyed
- Under Repair
- Degraded

These are not universal states that every Terrain Object must support. Each type declares its supported states and legal transitions. State names should use a controlled vocabulary where they represent the same physical condition across multiple types.

### 12.2 State Effects

A Terrain State may modify applicable base attributes, including Height, Movement Effect, Visibility Effect, Hit Probability Effect, Health Points or other type-specific properties.

A state modifier must be explicit about whether it replaces, adds to or scales a base value. This specification does not permit an ambiguous modifier to be interpreted differently by separate systems.

The owning simulation system defines the transition conditions and applies the state change. For example, Weather and Climate may trigger flooding or freezing; Combat may cause damage; Engineering and Infrastructure may repair a crossing.

### 12.3 Transition Classes

State transitions may be:

- **Temporary:** the state ends when its triggering condition ends or a recovery period completes.
- **Persistent:** the state remains until an action or condition changes it.
- **Progressive:** the feature moves through multiple states over time.
- **Reversible:** the previous condition can be restored.
- **Irreversible:** the original condition cannot naturally be restored.

Examples include heavy rain causing an unpaved road to become muddy, sustained cold freezing a lake, bombardment damaging a bridge, or engineering restoring a destroyed crossing.

### 12.4 Persistence

Terrain State persists independently of the active viewport. A bridge destroyed while the player is operating elsewhere remains destroyed and continues to affect Movement, civilian transportation, Trade, Supply, Logistics and other relevant systems.

State must not be reset when a player leaves an area, changes theatre or changes zoom. State ownership and persistence mechanisms must be defined by the responsible simulation system and save/load architecture.

### 12.5 State and Rendering

Rendering may communicate Terrain State through visual changes, including a collapsed bridge, degraded road, reduced vegetation, flooded field or frozen lake.

**Rendering communicates state; it does not determine state.**

Visual state representations are presentation assets or derived geometry. They must not be the only record of the underlying state.

## 13. Initial Terrain Object Catalogue

The following is the initial catalogue of types and primitives. It is a starting definition set, not a claim that every type has all numerical attributes fully balanced. Type-specific attributes, valid states, crossing rules and effect values must be completed in canonical reference data before scenarios rely on them.

| Name | Category | Primitive |
| --- | --- | --- |
| Ford | Crossings | Point |
| Light Road Bridge | Crossings | Point |
| Medium Road Bridge | Crossings | Point |
| Heavy Road Bridge | Crossings | Point |
| Rail Bridge | Crossings | Point |
| Track | Roads | Line |
| Unpaved Road | Roads | Line |
| Paved Road | Roads | Line |
| Highway | Roads | Line |
| Railway | Railways | Line |
| Stream | Rivers and Water | Line |
| Minor River | Rivers and Water | Line |
| Major River | Rivers and Water | Line |
| Lake | Rivers and Water | Area |
| Moor | Vegetation | Area |
| Rice Paddy | Vegetation | Area |
| Agricultural Field | Vegetation | Area |
| Orchard | Vegetation | Area |
| Light Woods | Vegetation | Area |
| Forest | Vegetation | Area |
| Jungle | Vegetation | Area |
| Tree Savanna | Vegetation | Area |
| Grassland | Ground | Area |
| Dirt | Ground | Area |
| Sandy Desert | Ground | Area |
| Beach | Ground | Area |
| Marsh | Ground | Area |
| Mud | Ground | Area |

Future additions may include further terrain, water, infrastructure and surface types. Adding a type requires a canonical definition and compatibility review against World Map, Movement, Visibility and Detection, Combat, Weather, Infrastructure, Logistics and MapMaker.

## 14. Interaction with Economy, Industry and Population

Terrain Objects may provide geographic inputs to economic and civilian systems, but they do not own economic or demographic state.

- **Economy:** terrain may affect access, transport costs, damage-related disruption or other explicitly defined economic modifiers. Financial value, price, revenue, expenditure and transactions remain owned by Economy.
- **Industry Data:** an Industry establishment may reference a location or geographic area and may have terrain-related eligibility constraints. Industry Type definitions and establishment profiles remain owned by Industry Data.
- **Production:** terrain may indirectly constrain facility access or infrastructure. Production owns facilities, capacity, orders and execution.
- **Inventory:** terrain does not represent physical stock. Inventory remains the authority for stock quantities, locations, custody and movements.
- **Population:** Terrain Objects may overlap Population Centres and influence accessibility or exposure. Population totals, demographics, labour and needs remain owned by Population.
- **Logistics:** transport infrastructure geometry is a geographic input; network connectivity, transport capacity, shipments and delivery remain owned by Logistics.

Terrain Object definitions must not duplicate prices, GDP, inventory quantities, industrial output, labour availability or Population Centre demographics.

## 15. Validation and Acceptance Criteria

Terrain Object data must be validated for:

- stable and unique IDs;
- supported geometry and valid coordinates;
- valid type, category and primitive combinations;
- valid attribute types, units and ranges;
- supported initial Terrain State;
- valid state-transition definitions;
- consistent height and altitude semantics;
- valid references to associated features;
- explicit handling of overlapping geometry;
- declared movement and observation profile applicability;
- no use of rendering order as simulation precedence;
- no duplicate ownership of Economy, Inventory, Production or Population state.

The specification is conformant when Terrain Objects preserve identity and geometry, support overlapping features and persistent states, expose typed physical attributes to consuming systems, and leave simulation-specific calculations to their owning systems.

## Crossing Representation Clarification

Crossings are represented as **Point primitives only**. A crossing identifies a discrete location at which a route or movement path may pass across an otherwise restrictive feature, such as a river. It is not a Line primitive and does not represent the entire bridge, ford, or river segment as a continuous geometry.

A Crossing Point may reference the relevant intersecting Terrain Objects and infrastructure features. Its simulation meaning is determined by the systems responsible for movement, infrastructure, logistics, and combat. Where the crossing represents a destructible bridge or other structure, condition, damage, restrictions, repair requirements, and repair progress are authoritative state owned by the appropriate infrastructure feature, not by the visual point marker alone.

The MapMaker may render bridge or ford artwork that visually spans the route and watercourse, but the visual footprint does not change the canonical Point geometry or turn the crossing into a Line primitive.
