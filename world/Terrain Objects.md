# Terrain Objects
Terrain Objects are geographic Areas, Lines, and, in the case of Crossings, Points used to represent physical terrain and environmental features within the World. 

Terrain Objects may overlap spatially and are rendered according to the defined Map Layer order. Rendering order does not determine simulation priority. Multiple Terrain Objects may simultaneously affect a Movement Grid Cell.

## All Terrain Objects
Terrain Objects are as follows:

| Name  | Category | Mapping Primitive |
| ------------- | ------------- | ------------- |
| Ford  | Crossings | Point |
| Light Road Bridge  | Crossings | Point |
| Medium Road Bridge  | Crossings | Point |
| Heavy Road Bridge  | Crossings | Point |
| Rail Bridge  | Crossings | Point |
| Destroyed Crossing | Crossings | Point |
| Track | Roads | Line |
| Unpaved Road | Roads | Line |
| Paved Road | Roads | Line |
| Highway | Roads | Line |
| Muddy Road | Roads | Line |
| Icy Road | Roads | Line |
| Icy Highway | Roads | Line |
| Destroyed Road | Roads | Line |
| Destroyed Highway | Roads | Line |
| Railway | Railways | Line |
| Stream | Rivers & Water | Line |
| Minor River | Rivers & Water | Line |
| Major River | Rivers & Water | Line |
| Lake | Rivers & Water | Area |
| Moor | Vegetation | Area |
| Rice Paddy | Vegetation | Area |
| Agricultural Field | Vegetation | Area |
| Orchard | Vegetation | Area |
| Light Woods | Vegetation | Area |
| Forest | Vegetation | Area |
| Jungle | Vegetation | Area |
| Tree Savanna | Vegetation | Area |
| Grassland | Ground | Area |
| Sandy Desert | Ground | Area |
| Beach | Ground | Area |
| Marsh | Ground | Area |

## Terrain Objects Effects
Each Terrain Object possesses defined Terrain Effects which determine how the object interacts with simulation systems. Terrain Object Effects include: 

### Movement Effect
Movement Effects are expressed in percentage terms of a Unit's current movement rate when on the Terrain Object, which is also affected by the unit’s Morale, Cohesion, Fatigue, Experience, Fitness, Aggression, Stubbornness, Speciality, Formation, Status, completed Training Programs, Traditions, their Commander's Traits and Statistics as well as his Capacity and Load, any National and/or Cultural Bonuses, and their Supplies. Movement Effect for each Terrain Object is broken down to:

1. Mechanised: The Movement Rate Percentage for Mechanised Units
2. Motorised: FINISH
3. Heavy Armour FINISH
4. Light Amour: FINISH
5. None: The Movement Rate Percentage for Units with no Mechanised Equipment, Motorised Equipment, Heavy or Light Armour

Where the Movement Effect is '0', that Terrain Type is impassable. Some Units with Specialities and/or specific Training Programs, such as a Special Operations (SF) Unit, can overcome impassable Terrain Objects such as a Navy SEALS equivalent being able to combat swim a Lake, which would have a 0 rate. Likewise, if a Unit is equipped with the necessary vehicles to cross impassable INSERT, they can, whether that is Amphibious Vehicles, whether a 'Assault Amphibious Vehicle, Model 7' or RHIB. 

### Visibility
Visbility Effects are expressed in percentage terms, where the effect is the reduction in visibility for each 100M through which the line of sight passes. The smaller the value, the further a Unit can see through this terrain. For instance, a visibility reduction value of 33% means a line of sight would be blocked after 300m. Visibility is also effected by Weather, Time in the Day (whether Night or highest point of the sun in the Day), equipment the Unit has (whether binoculars, more advanced electro-optical systems, or weapons equipped with scopes, etc). The values are based on observing a person, vehicle, or field gun:

1. Person: The Visibility Rate for INSERT
2. Vehicle: The Visibility Rate for all vehicles, whether trunk or tank.
3. Field Gun: The Visibility Rate for all field guns. 

Units moving and firing are easier to see.

### Hit Probability
### Health Points
### Heavy Rain Weather Transformation
### Heavy Snow Weather Transformation
### Cold Climate Transformation
### Destruction Transformation
### Prime Times
