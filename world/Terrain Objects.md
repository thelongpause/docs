# Terrain Objects
Terrain Objects are geographic Areas, Lines, and, in the case of Crossings, Points used to represent physical terrain, environmental features and terrain infrastructure within the World.

Terrain Objects form part of the physical geographic truth of the World and may directly or indirectly influence movement, visibility, combat, logistics, construction, and other simulation systems.

Terrain Objects may overlap spatially and are rendered according to the defined Map Layer Order. Rendering order determines only how Terrain Objects are visually represented and does not determine simulation priority.

Multiple Terrain Objects may simultaneously affect the same geographic position or Movement Grid Cell. For example:

> Forest + Slope + Mud + Road + Heavy Rain

where each applicable Terrain Object and environmental state contributes its own effects to the simulation. Terrain Objects therefore should not be considered mutually exclusive geographic classifications.

Where multiple Terrain Objects affect the same geographic position, each applicable object remains available to the relevant simulation system for evaluation. The method by which their effects are combined is defined by the relevant Simulation System.

## Terrain Object Categories
Terrain Objects are divided into the following categories:

* **Crossings:** Geographic points at which a Unit can cross an otherwise restrictive geographic feature such as a river or body of water.
* **Roads:** Linear transportation infrastructure used primarily by land Units and civilian transportation.
* **Railways:** Linear transportation infrastructure used by rail transportation and, where applicable, military logistics.
* **Rivers & Water:** Geographic water features which influence movement, visibility, combat and crossing requirements.
* **Vegetation:** Geographic areas representing vegetation and cultivated land.
* **Ground:** Geographic areas representing the underlying ground and surface conditions of land.

## Terrain Object Attributes
Every Terrain Object possesses a defined set of physical and simulation attributes. These attributes describe what the Terrain Object is and how it interacts with other systems.

Terrain Object attributes include:

* Height;
* Movement Effect;
* Visibility Effect;
* Hit Probability Effect;
* Health Points.

### Terrain Object Height
Every Terrain Object possesses a Height measurement. Height is the vertical extent of a Terrain Object above the underlying terrain surface, measured in metres.

Terrain Object Height is considered in conjunction with the underlying Altitude Layer when calculating the effective elevation of the relevant portion of the object for Line of Sight (LOS), visibility, and other systems requiring vertical position.

> **Effective Elevation = Altitude Layer + Terrain Object Height**

For example, if a Forest is located on an Altitude Layer of 50m and has a defined Terrain Object Height of 15m, its effective elevation for relevant LOS calculations is 65m.

This distinction is important because Altitude and Height represent different layers of the simulation:

* **Altitude** represents the elevation of the natural terrain surface.
* **Height** represents the vertical extent of the Terrain Object above that surface.
* **Effective Elevation** represents the resulting elevation of the relevant portion of the Terrain Object used by LOS and visibility calculations.

The exact treatment of Terrain Object vertical profiles and how their Height interacts with Line of Sight is defined by the relevant Simulation System.

### Movement Effect
Movement Effects determine the base modification applied to a Unit's movement rate while traversing the relevant Terrain Object. Movement Effects are expressed as a percentage of the Unit's applicable movement rate.

For example:

> Movement Effect: 75%

means that, before other applicable modifiers are applied, the Unit possesses 75% of its applicable base movement rate while traversing that Terrain Object.

The final movement outcome may subsequently be affected by Unit, Commander, Weather, Supply, Terrain State and other applicable simulation modifiers.

#### Movement Classes
Each Terrain Object defines Movement Effects for the applicable Unit Movement Classes:

* **Mechanised:** Movement rate percentage for Units whose primary movement capability is mechanised.
* **Motorised:** Movement rate percentage for Units whose primary movement capability is motorised.
* **Heavy Armour:** Movement rate percentage for Units classified as Heavy Armour.
* **Light Armour:** Movement rate percentage for Units classified as Light Armour.
* **None:** Movement rate percentage for Units without Mechanised, Motorised, Heavy Armour or Light Armour movement capabilities.

Additional movement classifications may be introduced where required by the Unit System.

#### Impassable Terrain
A Movement Effect of 0% indicates that the Terrain Object is impassable to the relevant Movement Class under normal conditions.

Impassability does not necessarily mean that the geographic feature can never be traversed. Units possessing the appropriate Speciality, Equipment, Training or Capability may overcome otherwise impassable Terrain.

Examples include:

* Special Operations Forces conducting combat swimming across a Lake;
* Infantry crossing difficult terrain using specialised training;
* Amphibious Vehicles crossing bodies of water;
* RHIBs crossing water;
* Engineering Units constructing crossings;
* Units using specialised bridging equipment.

The Movement System therefore determines whether a Unit is capable of overcoming a 0% Movement Effect based upon its capabilities.

### Visibility Effect
The Visibility Effect of a Terrain Object determines the degree to which visual observation is reduced when a Line of Sight passes through that Terrain Object. Visibility is calculated progressively according to the distance that the Line of Sight travels through the relevant Terrain Object.

Visibility begins at 100% at the point at which the Line of Sight enters the Terrain Object. A Terrain Object then applies a defined **Visibility Reduction Per 100m** (effectively per 1× Movement Grid Segment) for each Observation Target.

The Visibility Reduction is a fixed percentage-point reduction for every 100 metres travelled through the Terrain Object.

#### Visibility Reduction Per 100m
Each Terrain Object possesses a **Visibility Reduction Per 100m** value for each applicable Observation Target.

For example, a Terrain Object with a Visibility Reduction Per 100m of 20% produces the following visibility:

| Distance Through Terrain | Remaining Visibility |
| -----------------------: | -------------------: |
|                      0 m |                 100% |
|                    100 m |                  80% |
|                    200 m |                  60% |
|                    300 m |                  40% |
|                    400 m |                  20% |
|                    500 m |          Not Visible |

The Visibility Effect therefore represents percentage points of visibility lost per 100 metres, rather than a percentage reduction of the remaining visibility.

The base calculation is:

> **Remaining Visibility = 100 − (Distance Through Terrain ÷ 100 × Visibility Reduction Per 100m)**

The resulting value cannot fall below 0%.

Where Remaining Visibility reaches 0%, the target is considered not visually observable through that Terrain Object under the current conditions, before additional Sensor, Detection or Observation System effects are applied.

The final observation outcome is derived from the Terrain Object's Visibility Effect together with the Observation Target, Target Observation State, Environmental Conditions and Observation Capability of the observing Unit. The exact combination of these factors is defined by the Visibility and Detection Systems.

#### Observation Target
Visibility is dependent upon the physical characteristics of the target being observed. Terrain Objects therefore define base Visibility Effects against the following target classifications:

* **Personnel:** The base visibility characteristics of Units without Mechanised, Motorised, Heavy Armour or Light Armour movement capabilities.
* **Vehicle:** The base visibility characteristics of Units with Mechanised, Motorised, Heavy Armour or Light Armour movement capabilities.
* **Guns:** The base visibility characteristics of Artillery, Rocket, Anti-Tank, and Anti-Aircraft Units.

Additional Observation Target classifications may be introduced where required by the Visibility and Sensor Systems.

##### Observation Target Activity
A Unit's current Movement Status, Engagement Status and Deployment Status affect its observability.

Each Terrain Object therefore defines additional Visibility Effects for each Observation Target classification according to the Unit's current state. These modifiers modify the observability of the Observation Target after the Terrain Object's base Visibility Effect has been calculated.

**Movement Status:** Movement increases the observability of a Unit by changing its physical and visual signature. Each Observation Target possesses the following Movement Statuses:
* **Stationary:** The Unit is not undertaking meaningful movement and is standing still.
* **Careful:** The Unit is moving at a paced movement speed.
* **Moving:** The Unit is moving at a normal movement speed.
* **Fast:** The Unit is moving at a high speed.

**Engagement Status:** Engagement activity may substantially increase the observability of a Unit.
* **Firing:** The Unit is actively firing.

**Deployment Status:** A Unit's Deployment Status affects the degree to which its physical profile is exposed to observation. Each Unit possesses the following Deployment Statuses:
* **Taking Cover:** The Unit is actively positioned behind available local cover but has not established a prepared defensive position. Taking Cover reduces the observable physical profile of the Unit and provides a Visibility Reduction modifier.
* **Dug-In:** The Unit has dug and now occupies an earthwork or shallow defensive position. Dug-In substantially reduces the exposed profile of the Unit and provides both Visibility and Protection effects.
* **Entrenched:** The Unit has dug and created a more substantial trench defensive position, providing increased concealment and protection. Entrenched positions substantially reduce the visible profile of personnel and equipment.
* **Fortified:** The Unit has constructed a defensive position designed to provide substantial protection and concealment. Fortified positions provide significant Visibility Reduction in addition to their defensive and protection effects.

A Stationary Unit Entrenched within Forest may therefore have substantially different observability from the same Unit moving rapidly through the same Forest.

#### Combined Observation State
The Visibility System evaluates the target's current state in combination with the Terrain Object through which observation occurs.

For example:

> Terrain Object: Dense Forest
> Target: Personnel
> Movement Status: Stationary
> Engagement Status: Not Firing
> Deployment Status: Entrenched

would produce substantially different observability from:

> Terrain Object: Dense Forest
> Target: Personnel
> Movement Status: Fast
> Engagement Status: Firing
> Deployment Status: None

The underlying Terrain Object remains unchanged in both cases. The difference arises from the interaction between the Terrain Object's Base Visibility Effect and the target's current Observation State.

#### Weather and Atmospheric Conditions

The Terrain Object's base Visibility Effect is modified by environmental conditions.

Weather conditions include:

* Clear Weather;
* Light Rain;
* Heavy Rain;
* Fog;
* Snow.

Additional atmospheric conditions may be introduced by the Weather System.

The exact interaction between Terrain Objects and Weather is defined by the Weather System.

#### Time of Day

Visibility is affected by available illumination.

Visibility is affected by:

* Dawn;
* Day;
* Full Moon;
* 3/4 Moon;
* Half Moon;
* Quarter Moon;
* New Moon.

The World Map's Time & Illumination System provides the environmental illumination state used by the Visibility System.

Night does not necessarily make visual observation impossible. Units equipped with appropriate sensors may continue to detect and identify targets despite reduced natural illumination.

#### Unit Characteristics

A Unit's ability to observe targets is additionally affected by its own characteristics.

Relevant factors include:

* Base Training;
* Role;
* Experience;
* Traditions;
* Military Courses;
* Commander Traits.

These modifiers represent the Unit's ability to observe, interpret and respond to available information. They do not alter the physical properties of the Terrain Object.

#### Observation Equipment

The final observation capability of a Unit is affected by the equipment available to it.

Examples include:

* Binoculars;
* Telescopic Sights;
* Optical Sights;
* Rangefinders;
* Night Vision;
* Thermal Imaging;
* Infrared Systems;
* Radar;
* Electro-Optical Systems;
* and other sensors.

Equipment does not remove the Terrain Object's physical properties. Instead, the Sensor System determines how effectively a Unit can overcome environmental visibility degradation and detect available signatures.

A modernised Unit may therefore possess substantially greater observation capability than an otherwise identical Unit operating with basic optical equipment.

#### Multiple Terrain Objects

A Line of Sight may pass through multiple Terrain Objects.

Each Terrain Object contributes its own Visibility Effect according to the distance that the Line of Sight travels through it.

For example:

> Observer
> │
> ├── 100m Grassland
> │
> ├── 200m Light Woods
> │
> ├── 100m Forest
> │
> └── Target

The Visibility System evaluates each Terrain Object independently and combines their effects to determine the resulting observation condition.

Terrain Objects do not necessarily replace one another merely because they overlap spatially. A geographic location may therefore simultaneously possess multiple applicable environmental characteristics.

Where multiple Terrain Objects affect the same Line of Sight, each applicable object remains available to the Visibility System for evaluation.

#### Elevation, Height and Line of Sight

Visibility Effect and Line of Sight are separate calculations.

A Terrain Object may reduce visibility without completely obstructing Line of Sight. Conversely, a Terrain Object may physically obstruct Line of Sight because its Effective Elevation exceeds the LOS corridor between the Observer and Target.

If the relevant portion of a Terrain Object rises above the Observer's LOS, the Terrain Object may block Line of Sight entirely, regardless of its Visibility Reduction value.

Therefore:

> **Visibility Reduction determines how clearly something can be observed through terrain.**

> **Altitude and Terrain Object Height determine whether the Line of Sight can physically pass over or through terrain.**

### Hit Probability Effect

Hit Probability Effects determine the base modification of the probability that a weapon successfully engages a target located on or within a Terrain Object.

Hit Probability Effects are applied differently depending upon the type of engagement:

* **Direct Fire:** Direct Fire includes weapons where the firer directly engages the target with direct-fire weapons such as Rifles, Machine Guns, Anti-Tank Weapons, Tank Guns, Autocannons and other direct-fire weapons.
* **Area Fire:** Area Fire includes weapons whose effects are distributed across an area by weapons such as Mortars, Artillery, Air Strikes, Rockets, Cruise Missiles and other area-effect weapons.

Terrain Objects may modify the effectiveness of Area Fire because physical terrain can interact with fragmentation, airbursts, blast effects, concealment and target protection.

For example, Forest may modify the effectiveness of Mortar or Artillery fire because vegetation can interact with fragmentation and airburst effects. The resulting effect depends upon the weapon, ammunition, fuze and engagement conditions.

The exact calculation of Hit Probability and weapon effectiveness is defined by the Combat and Weapons Systems.

### Health Points

Health Points represent the remaining physical integrity of a Terrain Object where the object possesses a destructible physical structure or condition.

When a Terrain Object's Health Points reach defined thresholds, its Terrain State may change.

The exact thresholds and resulting Terrain States are defined individually by Terrain Object.

Not every Terrain Object requires Health Points. For example, a Lake does not ordinarily require Health Points, while a Bridge or Road may possess structural Health Points.

## Terrain State

Terrain State represents the current physical condition of a Terrain Object at a particular point in time.

A Terrain Object defines what a geographic feature fundamentally is, while its Terrain State defines its current condition and operational capability.

For example, the Terrain Object **Unpaved Road** may possess the following Terrain States:

* **Base:** The normal or default state of the Terrain Object.
* **Muddy:** The Terrain Object has become affected by water saturation and mud.
* **Frozen:** The Terrain Object has become affected by sustained freezing conditions.
* **Damaged:** The Terrain Object has sustained physical damage but remains partially operational.
* **Destroyed:** The Terrain Object can no longer perform its normal function.
* **Under Repair:** The Terrain Object is undergoing repair or reconstruction.

Terrain States are defined individually according to the physical characteristics and possible transformations of each Terrain Object.

Terrain State is dynamic and may change throughout the simulation as a result of:

* Weather;
* Climate and Seasonal Conditions;
* Military Action;
* Engineering Operations;
* Infrastructure Maintenance;
* Construction and Reconstruction;
* Natural Events;
* and the passage of Time.

Terrain State therefore allows the World to remain physically persistent while its conditions change.

### Base Terrain Object and Current State

Terrain Objects retain their underlying geographic identity regardless of their current state.

For example:

> Terrain Object: Unpaved Road
> Current State: Muddy

The underlying geographic feature remains an Unpaved Road. Its Muddy State modifies its current simulation effects.

Similarly:

> Terrain Object: Light Road Bridge
> Current State: Destroyed

The geographic crossing remains at its original location and retains its underlying identity, but its current state prevents normal crossing until repaired.

The simulation should therefore not need to delete and recreate geographic objects whenever their physical condition changes.

Terrain States are not necessarily represented as separate Terrain Objects in the simulation. Certain Terrain States may nevertheless possess dedicated visual or authoring representations within the Map Authoring System.

### Terrain State Effects

Terrain States may modify the base attributes of the underlying Terrain Object, including:

* Height;
* Movement Effect;
* Visibility Effect;
* Hit Probability Effect;
* Health Points;
* and other applicable attributes.

The exact modification applied by a Terrain State is defined by the relevant Simulation System.

### State Transitions

Terrain Objects may transition between Terrain States when defined environmental, military, engineering or other simulation conditions occur.

State transitions may be:

* **Temporary:** The Terrain Object naturally returns to its previous state after the relevant condition ends or a recovery period elapses.
* **Persistent:** The Terrain Object remains in its new state until an external action occurs.
* **Progressive:** The Terrain Object passes through multiple states over time.
* **Reversible:** The original state can be restored.
* **Irreversible:** The original state cannot naturally be restored.

#### Environmental State Transitions

Environmental conditions can cause Terrain State to change without direct human intervention.

Examples include:

| Terrain Object     | Environmental Event | Resulting State |
| ------------------ | ------------------- | --------------- |
| Unpaved Road       | Heavy Rain          | Muddy           |
| Dirt               | Heavy Rain          | Muddy           |
| Road               | Heavy Snow          | Snow-Covered    |
| Lake               | Sustained Cold      | Frozen          |
| River              | Sustained Cold      | Frozen          |
| Marsh              | Heavy Rain          | Flooded         |
| Agricultural Field | Heavy Rain          | Muddy           |

The Weather and Climate Systems determine the conditions and duration required for these transitions.

#### Military State Transitions

Military action may cause Terrain Objects to enter damaged or destroyed states.

Examples include:

> Bridge → Artillery Strike → Damaged → Additional Damage → Destroyed

> Road → Air Strike → Damaged → Reduced Movement Capacity

> Forest → Bombardment / Fire → Reduced Vegetation Density → Degraded Forest

Military damage may therefore alter the physical characteristics of the World rather than simply creating temporary visual effects.

#### Repair and Recovery

Certain Terrain States may be recoverable through natural recovery, maintenance, repair or reconstruction.

Recovery may require:

* Time;
* Engineering Units;
* Civilian Labour;
* Construction Equipment;
* Strategic Resources;
* Spare Parts;
* Government Funding;
* Infrastructure Capacity;
* and other applicable resources.

The relevant Military, Infrastructure and Economy Systems determine the resources, capabilities and organisations required for recovery.

### Terrain State Persistence

Terrain State is persistent within the World Simulation.

Changes to Terrain State therefore remain present when the Player moves away from an area, changes Theatre, changes zoom level, or otherwise ceases to directly observe the geographic location.

For example, if an enemy destroys a bridge while the Player is operating elsewhere, the bridge remains destroyed and its changed state continues to affect:

* Military Movement;
* Civilian Transportation;
* Trade;
* Supply;
* Logistics;
* Economic Activity;
* and other relevant systems.

The World therefore does not reset Terrain States when an area leaves the Player's active viewport.

### Terrain State and Rendering

Terrain State may alter the visual representation of a Terrain Object.

For example:

* A destroyed bridge may display as a collapsed crossing;
* A muddy road may display as a visibly degraded road;
* A bomb-damaged forest may display reduced vegetation;
* A flooded field may display standing water;
* A frozen lake may display an ice surface.

The visual representation of a Terrain State is separate from its simulation effects.

**Rendering communicates the state to the Player; it does not determine the state.**

## All Terrain Objects

Terrain Objects are as follows:

| Name               | Category       | Mapping Primitive |
| ------------------ | -------------- | ----------------- |
| Ford               | Crossings      | Point             |
| Light Road Bridge  | Crossings      | Point             |
| Medium Road Bridge | Crossings      | Point             |
| Heavy Road Bridge  | Crossings      | Point             |
| Rail Bridge        | Crossings      | Point             |
| Track              | Roads          | Line              |
| Unpaved Road       | Roads          | Line              |
| Paved Road         | Roads          | Line              |
| Highway            | Roads          | Line              |
| Railway            | Railways       | Line              |
| Stream             | Rivers & Water | Line              |
| Minor River        | Rivers & Water | Line              |
| Major River        | Rivers & Water | Line              |
| Lake               | Rivers & Water | Area              |
| Moor               | Vegetation     | Area              |
| Rice Paddy         | Vegetation     | Area              |
| Agricultural Field | Vegetation     | Area              |
| Orchard            | Vegetation     | Area              |
| Light Woods        | Vegetation     | Area              |
| Forest             | Vegetation     | Area              |
| Jungle             | Vegetation     | Area              |
| Tree Savanna       | Vegetation     | Area              |
| Grassland          | Ground         | Area              |
| Dirt               | Ground         | Area              |
| Sandy Desert       | Ground         | Area              |
| Beach              | Ground         | Area              |
| Marsh              | Ground         | Area              |
| Mud                | Ground         | Area              |
