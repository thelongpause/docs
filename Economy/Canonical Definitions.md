# System Purpose
The Economy System simulates the economic activity of every Nation and the economic relationships between Nations. 

# Economic Foundations
## Currency
Project Sovereign uses a single global currency: Dollar ($). There is no foreign-exchange system. All monetary figures are denominated in Dollar. A transaction between two Nations therefore does not require currency conversion.

## Daily Economic Tick
The World Simulation operates on a 24-hour clock. The economic simulation uses a Daily Economic Tick. One Economic Day is: 24 hours. Calendar dates follow the real Gregorian calendar, excluding leap years.

Where an annual monetary value must be converted into a daily value, the Economy System uses: $Daily Value = Annual Value / 365$; unless the definition of that specific value explicitly states another calculation.

## Economic Base Date
A New Game begins on: 1 January 1955

The economic simulation therefore begins with a world whose nominal monetary values are expressed in 1955 United States Dollar as a reference. .

The Base Price of a Good, Service, or Military Ware represents its defined reference value at the beginning of the simulation.

## Strategic Resource
A Strategic Resource is a naturally occurring physical resource that: 
- exists at a geographic location;
- has an explicitly defined Deposit;
- can only be extracted when the relevant Industry is established for extraction;
- must be extracted in order to be:
  - stockpiled;
  - traded;
  - consumed by an Industry as an Input for that Industry's Production Process;

There are Eight (8) Strategic Resources in the World. They are as follows: 

| Resource          | Description | Extraction Industry      |
| :---------------- | :---------- | :----------------------- |
| Petroleum         |             | Oil                      |
| Steel             |             | Steelmaking              |
| Aluminium         |             | Industrial Manufacturing |
| Coal              |             | Mining                   |
| Tungsten          |             | Mining                   |
| Uranium           |             | Mining                   |
| Rare-earth Metals |             | Mining                   |
| Rich Soils        |             | Agriculture              |

### Resource Deposit
A Resource Deposit is the of a Strategic Resource. A deposit has:
- geographic location which is mapped per [Industry Geographic Representation](#Industry-Geographic-Representation) ;
- total resource quantity;
- currently available quantity;
- extraction capacity;
- extraction efficiency;
- associated technology (Bonuses?);
- environmental/state conditions.

### Resource Extraction
Resources are extracted by their relevant Industry. The resulting extracted resource is stockpiled and becomes economically available for Input according to the Production Requirements of other Industries, trade, and reserve stockpiling.

## Industry
An Industry represents a major economic sector operated within a Nation. A Nation has several Industries to choose from and can establish all provided they have the Strategic Resource and Treasury Amount to establish the Industry. An Industry is controlled by a single Company. Industries are as follows:

| Resource                 | Description |
| :----------------------- | :---------- |
| Transportation           |             |
| Oil                      |             |
| Construction             |             |
| Automotive               |             |
| Aerospace                |             |
| Maritime                 |             |
| Energy Production        |             |
| Industrial Manufacturing |             |
| Mining                   |             |
| Nuclear Energy           |             |
| Electronics              |             |
| Steelmaking              |             |
| Space                    |             |
| Agribusiness             |             |
| Consumer Goods           |             |
| Agriculture              |             |

###  Industry Geographic Representation
Every Industry has a geographic representation on the World Map. Industry locations are created using the Area Mapping Primitive. An Industry Area is therefore a geographic representation of where the Industry's economic activity is physically established. The Area is not itself the Industry's economic state. The Industry Area utilise the same attributes also used by Terrain Objects. These attributes allow Industry Areas to participate directly in relevant military and geographic simulation systems. Base Attributes include:

- Height
- Movement Effect
- Visibility Effect
- Hit Probability Effect
- Health Points

These attributes function according to the definitions established within Terrain Objects. However, the Industry Area also has its own unique attributes such as FINISH

### Industry Scale
Industry Scale represents the current economic size of an Industry. Industry Scale is dynamic. Industry Scale determines the magnitude of the Industry's:
- labour demand;
- capital employed;
- input demand;
- production capacity;
- output capacity;
- potential revenue;
- potential expenditure.

#### Industry Scale Calculation
Industry Scale must be calculated from explicitly defined economic constraints. The final calculation must account for the relevant:
- Company capital available;
- Nation labourforce available;
- Resource for the production of an Industry's output availability;
- Technologies a Nation has successfully researched;
- Government restrictions;
- The Company's Senior Executive Character AI Statistics.

## Company 
A Company is the economic organisation responsible for operating one Industry. A Company has:
- name;
- logo;
- ownership;
- capital;
- revenue;
- expenditure;
- employees;
- production decisions;
- pricing decisions;
- investment decisions;
- trade relationships.

### Company AI
Companies are controlled and owned by a Senior Executive Character AI unless explicitly controlled by the Player through Economic legislations that allows control of the Industry, nationalisation of the industry that allows ownership and control, or Distribution of Power legislation that allows ownership and control of the Industry. The Senior Executive Character AI is defined by the Character System.

The Senior Executive Character AI makes economic decisions within the constraints established by:
- Government;
- Standing Economy;
- Company ownership;
- Industry scale;
- available capital;
- available labour;
- available inputs;
- trade agreements;
- sanctions;
- Senior Executive Character statistics.

If the Company is controlled by the Senior Executive Character AI, the Player instead interacts with Companies through defined economic controls such as:
- production priorities;
- subsidies;
- capital injections;
- appointments;
- permitted trade;
- Government policy;
- procurement;
- strategic directives.

## Economic Commodities
The Economy System has  three fundamental economic output classes:
1. Goods
2. Services
3. Military Wares

Strategic Resources are physical resources used as Economic Inputs by Industries to Output one or more of the Output Classes.

### 1. Goods
A Good is a physical product that can be:
produced;
purchased;
consumed;
stockpiled;
exported;
imported;

A Good represents a defined economic product category rather than an individual physical object.

