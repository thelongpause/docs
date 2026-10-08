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

### Capital and Establishment
Each Industry has an Industry Establishment Capital Requirement. The requirement is Industry-specific. Therefore: Establishment Capital ≠ Universal Constant
An Electronics Industry may require a different establishment capital requirement from an Agribusiness Industry. The requirement is denominated in USD. Strategic Resources are not themselves used as the monetary establishment cost.

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
3. Armaments

Strategic Resources are physical resources used as Economic Inputs by Industries to Output one or more of the Output Classes.

### 1. Goods
A Good is a physical product that can be:
- produced;
- purchased;
- consumed;
- stockpiled;
- exported;
- imported;
- used as an Industry input.

A Good represents a defined economic product category rather than an individual physical object. Different goods may have an impact on the culture of a Nation FINISH.

#### Goods Categories
Goods are divided into three categories:
- Staple Goods: Staple goods are everyday goods that a consumer needs to live, such as Grocery Staples to eat, Hygiene Products for their personal care, and Wardrobe Staples to wear. Staple goods tend to be purchased in vast quantities by Low and Middle Class consumers, while Upper Class consumers generally avoid them in favour of their luxury variants.
- Luxury Goods: FINISH
- Industrial Goods: FINISH (paid for by governments for military wares; when the Player designs, for instance, an engine for a Fighter Jet, it is R&D and produced by the relevant Industry which would be Aerospace and then sold to the Government)

#### Good Unit
Every Good has a defined Good Unit. One Good Unit represents the quantity of that Good required to satisfy the standard consumption requirement of one individual for the Good's specified Consumption Interval. 

(PLEASE ADD ALL GOODS, AND THEIR RELEVANT INDUSTRY)

#### Consumption Interval
Every Good has a Consumption Interval. Consumption Interval defines how frequently one Good Unit satisfies its associated consumption requirement.

### 2. Services
A Service is an economic output whose value derives from an activity performed for a consumer rather than from a physical product being transferred.

### 3. Armaments
FINISH

### Base Price
Base Price is the static reference monetary value assigned to one unit of a Good or Service. Base Price is denominated in nominal 1955 USD. Base Price is not the price actually paid by every consumer.
It provides the economic reference from which dynamic pricing is calculated.

#### Base Price Ownership
The Economy System owns Base Price. A Good, Service, or Armament therefore has:
- Base Price

and may separately have:
- Global Price
- Local Market Price
- Company Selling Price
- Transaction Price

These values must not be treated as interchangeable.

### Global Price
Global Price is the current global reference price for a Good or Service. It is derived from:
- Base Price;
- Global Supply;
- Global Demand;
- the defined price-response function.
  
The Global Price is dynamic.

#### Price Calculation Requirement
The final implementation must use an explicit mathematical price-response function. The following concepts are not acceptable as final definitions:
- Supply Modifier
- Demand Modifier
- Scarcity Modifier
- Market Modifier
unless each has a defined:
- input;
- mathematical operation;
- range;
- calculation order;
- owner;
- update frequency.

The final pricing equation must therefore explicitly define how a change in supply or demand changes price.

### Actual Selling Price
Actual Selling Price is the price at which a Company, Government or other authorised economic entity offers a Good, Service, or Armament for sale.

Actual Selling Price is distinct from Global Price. A Company may therefore face:
$Global Price = $X$
$Actual Selling Price = $Y$

The difference must arise from defined economic conditions.

#### Company Pricing
The Company AI determines the Company's Selling Price subject to the pricing rules established by the Economy and Government Systems.

The Company AI does not determine an undefined "asking price". The final pricing model must define:
- the reference price available to the Company;
- minimum permitted price;
- maximum permitted price;
- relevant Government price controls;
- Company pricing behaviour;
- Executive characteristics affecting pricing;
- stockpile conditions;
- demand conditions;
- competitive conditions;
- sanctions or trade restrictions;
- whether the Company may deliberately operate below cost.
  
No Executive Trait may directly create an undefined percentage modifier. Executive behaviour must operate through defined pricing variables.

## Production Capacity
Production Capacity is the maximum quantity of a defined Good, Service, or Armament that an Industry can produce during one Production Day under its current operating conditions. Production Capacity is measured in the Good or Service's defined Unit per Day.

For example:
$Production Capacity = 10,000 Good Units / Day$  or $Production Capacity = 50,000 Passenger Journeys / Day$

### Capacity Is Not Actual Production
Production Capacity represents potential output. Actual Production may be lower. The distinction is:
Production Capacity
        ↓
Operating Conditions
        ↓
Actual Production

Actual Production cannot exceed Production Capacity.

### Actual Production
Actual Production is the quantity of a Good, Service, or Armament physically produced by an Industry during one Production Day. The fundamental constraint is:

$Actual Production ≤ Production Capacity$ 

Actual Production is additionally constrained by:
- required inputs;
- available labour;
- operational state;
- physical production capability;
- applicable Government restrictions;
- applicable technology;

#### Input Requirement
Every production process has an explicit Input Recipe. An Input Recipe defines:
$Input Type + Required Quantity + Production Quantity$

For example: 100 units of Input A + 50 units of Input B → 100 units of Output C

The numerical relationship is defined by the Industry's Production Recipe. There is no universal assumption that one unit of input produces one unit of output.

##### Input Availability
For each required input, the Industry evaluates the quantity available at the beginning of the Production Day plus any qualifying deliveries scheduled for that production cycle.

The Economy/Production interface must provide: Available Input Quantity
The Industry compares this against: Required Input Quantity

##### Mandatory Input Shortfall
If a mandatory input cannot be supplied at the quantity required by the production recipe, the default production rule is: Actual Production = 0

unless that production process explicitly defines a Partial Production Rule. The Industry does not silently consume insufficient inputs. If production does not occur, the required input is not consumed.

## Stockpile
A Stockpile is a persistent quantity of a physical economic commodity held at a defined location and under a defined owner.

A Stockpile may contain:
- Strategic Resources;
- Goods;
- Industry Inputs;
- Industry Outputs;
- Armaments.

### Stockpile State
A Stockpile has:
- Commodity Type;
- Quantity;
- Location;
- Owner;
- Allocation Status;
- incoming transfers;
- outgoing transfers.
  
### Stockpile Equation
For a commodity during one economic day:
$Closing Quantity = Opening Quantity + Incoming Quantity - Consumed Quantity - Transferred Quantity - Sold Quantity - Destroyed Quantity$ 

The resulting quantity cannot be below zero. If a transaction would reduce the Stockpile below zero, that transaction cannot complete unless an explicitly defined borrowing, reservation or deficit mechanic permits it.

### Stockpiled Output
Goods produced by an Industry do not automatically become revenue. Unless an output is immediately transferred through a defined transaction, it enters the relevant Output Stockpile.

Therefore: $Production ≠ Revenue$

Revenue occurs only when an economic transaction generates income for the selling entity. An Industry may therefore produce Goods while generating no immediate revenue if those Goods remain unsold.

## Global Market
The Global Market is the world-wide economic reference layer through which supply, demand and prices for tradable Goods are compared. The Global Market is not a physical location. A Good does not need to physically pass through a single global marketplace. Instead, the Global Market aggregates information from all participating Nations.

### Global Market Inputs
For each Good, the Global Market tracks:
- Base Price;
- global production;
- global consumption;
- global demand;
- global supply;
- global stockpiles;
- imports;
- exports;
- available trade quantity;
- restricted trade quantity;
- Global Price.
  
The Global Market may also receive restrictions from:
- Government;
- Diplomacy;
- Sanctions;
- Military activity;
- Infrastructure;
- Production;
- Trade Agreements.
  
### Global Supply
Global Supply represents the quantity of a Good available to satisfy global demand during the relevant economic period. The precise composition of Global Supply must distinguish between:
- newly produced Goods;
- existing stockpiles;
- Goods available for domestic consumption;
- Goods available for export;
- Goods restricted from trade;
- Goods destroyed or otherwise unavailable.

Therefore Global Supply is not simply Global Production The final Global Supply calculation must explicitly define which quantities are economically available.

### Global Demand
Global Demand is the aggregate quantity demanded by all economic demand sources. The initial canonical demand sources are:
- Population Demand;
- Industry Demand;
- Government Demand;
- Military Demand;
- Other explicitly defined economic demand.

Each source remains separately identifiable. Therefore:
$Global Demand = Population Demand + Industry Demand + Government Demand + Military Demand + Other Defined Demand$ 

A demand source may not be silently incorporated into another source.

## Demand
Demand represents the quantity of a Good or Service that an economic actor seeks to obtain during a defined period at the applicable economic conditions. Demand sources are separately identified. Initial demand sources are:

- Population Demand;
- Industry Demand;
- Government Demand;
- Military Demand;
- Foreign Demand;
- Other explicitly defined demand.
  
### Population Demand
Population Demand originates in the Population System. The Population System determines:
- population;
- household structure;
- Population Types;
- consumption requirements;
- consumption intervals;
- income;
- preferences;
- applicable consumption conditions.

The Economy System aggregates this into market demand. The Economy System does not independently invent Population consumption requirements.

### Industry Demand
Industry Demand represents the quantity of Goods or Strategic Resources required by Industries for their defined production processes. Industry Demand is generated from Production Recipes.

For a required input:

Industry Input Demand = Required Input Quantity per Production Unit × Intended Production Quantity

The final demand is therefore directly connected to the Industry's intended production.

### Government Demand
Government Demand represents economic demand generated by Government activity. Examples include:
- public services;
- strategic reserves;
Government Demand is generated by the Government System and exposed to the Economy System.

### Military Demand
Military Demand represents economic demand generated by the Military System per Armanents. Examples include:
- weapons;
- ammunition;
- fuel;
- vehicles;
- uniforms;
- supplies;

Military Demand is owned by the Military System. The Economy System receives the demand quantity and resolves its economic fulfilment.

### Economic Allocation
A produced Good may have multiple possible destinations. Examples include:
- Population
- Government
- Military
- Industry
- Export
- Stockpile
  
Allocation rules determine which destination receives available supply.The allocation system must define:
- priority;
- reservation;
- contractual obligations;
- Government directives;
- Military priority;
- market demand;
- export commitments;
stockpile policy.
No destination automatically receives output unless an explicit allocation rule exists.

## Labour
### Labour Force
The Population System determines the Nation's Labour Force. The Economy System receives:
$Available Labour$ as an economic input.

The Economy System does not create individual workers.

### Aggregate Labour Allocation
Labour allocation is performed at an aggregate level. Individual Population members do not continuously move between Industries as simulated entities. Instead, the Economy System allocates quantities of available labour to Industries.

Example:

> National Available Labour = 10,000,000 workers
> Industry A Demand = 2,000,000
> Industry B Demand = 3,000,000
> Industry C Demand = 1,000,000

The Economy System allocates labour according to the defined national labour-allocation rules.

### Labour Demand
Labour Demand is the number of workers an Industry requires to operate at its defined Intended Production Level. Labour Demand is therefore an output of Industry operating requirements rather than a fixed attribute of every worker.

> Labour Demand = Workers Required for Intended Production

The final Labour Demand calculation must be derived from the Industry's defined:
- Production Scale;
- labour intensity;
- required education;
- technology.

### Labour Fulfilment
For an Industry with positive Labour Demand: Labour Fulfilment = Actual Employees / Labour Demand

with:

0 ≤ Labour Fulfilment ≤ 1

A value of:

1.00 = 100% labour fulfilment
0.75 = 75% labour fulfilment
0.00 = no labour allocated

Labour Fulfilment is an input into production calculations.The final relationship between Labour Fulfilment and Actual Production must be explicitly defined by the Industry's production model.

### Employment Failure
    
If an Industry reduces employment:

Industry Employees ↓
National Available Labour ↑

subject to Population System employment rules. The released workers do not become individually simulated unemployed characters. They return to the aggregate labour pool represented by the Population System.

### Wages
#### Base Gross Salary
Each Industry has a defined Base Gross Salary for each relevant employment category. Salary is denominated in annual USD unless otherwise specified.

#### Actual Gross Salary
Actual Gross Salary is dynamically determined from the Base Gross Salary and the defined economic and institutional conditions affecting labour compensation. The final wage calculation must explicitly define each input. The system must not use an undefined: 

Wage Modifier

without specifying its source and mathematical operation.

#### Daily Salary
Where an annual salary is converted into a daily economic flow:
Daily Gross Salary = Annual Gross Salary / 365

#### Industry Employment Composition
Industry employment is divided according to Population Types. An Industry therefore records its labour composition, for example:
- Rural Labourers
- Industrial Labourers
- Professional Class
- Civil Servants
- Academics & Researchers

The Population System owns the definitions of Population Types. The Economy System records the Industry's employment composition.

### Education Requirements
An Industry defines minimum education requirements for the labour it employs. Education is supplied by the Government System.

The Economy System does not independently create education. Where an Industry requires a particular education level, the labour allocation system determines whether sufficient qualified labour exists.

The effect of insufficient qualified labour must be explicitly defined in the Industry's Labour Requirement model.

## Revenue
Revenue is money received by an economic entity from completed economic transactions. Revenue is not equivalent to:
- production;
- inventory value;
- potential sales;
- demand.
  
### Daily Revenue
For each completed sale:
Transaction Revenue = Quantity Sold × Transaction Price
For an Industry:

Daily Revenue = Σ Transaction Revenue

across all completed revenue-generating transactions during the Economic Day. Unsold inventory does not create revenue.

### Expenditure
Industry expenditure is separated into defined categories. Initial categories are:
- Labour
- Inputs
- Operating Costs
- Government Taxes
- Financing Costs

#### Labour Expenditure
Daily Labour Expenditure = Σ Employees × Daily Gross Salary

The calculation is performed separately for each relevant employment category where salaries differ.

#### Input Expenditure
For each purchased input: 

Input Expenditure = Quantity Purchased × Transaction Price

Therefore: Daily Input Expenditure = Σ Input Expenditure

An Industry consuming inventory it already owns does not automatically create a new purchase transaction. The financial treatment of internal stockpile consumption must therefore distinguish:

Physical Consumption from: Cash Purchase

### Operating Income
Operating Income represents the economic result of an Industry's operating activity before any separately defined non-operating financial items.
Operating Income = Revenue - Operating Expenditure

Where Operating Expenditure includes the categories explicitly classified as operating expenditure.

#### Operating States
An Industry may therefore have: 

- Operating Income > 0, meaning an operating surplus.
- Operating Income = 0, meaning break-even operating performance.
- Operating Income < 0, meaning an operating loss.

A loss does not automatically cause immediate Industry failure. Industry failure, bailout, insolvency and liquidation require explicit Company Financial rules.

## Capital
Capital represents financial resources available to a Company for economic activity. Capital may be provided through:
- existing Company capital;
- retained earnings;
- domestic investment;
- foreign investment;
- Government subsidy;


34. Subsidies
A Subsidy is a defined transfer of economic resources from Government to an eligible economic entity.
Subsidies are controlled by the Government System.
The Economy System records:
subsidy recipient;
subsidy amount;
subsidy type;
subsidy duration;
subsidy conditions;
subsidy expenditure;
subsidy effect.
A subsidy does not automatically increase production.
Its economic effect depends on the defined subsidy type.
35. Capital Injection
A Capital Injection is a direct transfer of capital into an Industry or Company.
The Player may request or authorise a Capital Injection where permitted by Government and Company rules.
The receiving entity's available capital increases by the injected amount.
Capital Injection does not automatically imply:
increased production;
increased employment;
increased profitability.
Those outcomes occur only if the receiving entity uses the capital to alter an economic variable.
36. Foreign Investment
Foreign Investment is capital supplied by an entity located in one Nation to an eligible economic entity located in another Nation.
Because Project Sovereign uses a single global currency, no currency conversion occurs.
Foreign Investment is governed by:
Government investment legislation;
diplomatic relations;
trade agreements;
investment agreements;
sanctions;
ownership rules;
Company rules.
The final Foreign Investment system must define:
investor ownership;
voting rights;
profit rights;
capital recovery;
taxation;
investment restrictions;
political restrictions;
withdrawal conditions.
Foreign Investment therefore cannot be represented as a generic Industry bonus.
37. Trade
Trade is the transfer of Goods, Strategic Resources or eligible Services between economic entities.
A trade transaction requires:
Seller
Buyer
Commodity or Service
Quantity
Transaction Price
Origin
Destination
Applicable restrictions
Delivery status
A transaction is not considered completed until the defined delivery condition is satisfied.
38. Imports and Exports
An Export is a qualifying transaction in which a Good or eligible Service leaves the economic territory of the seller's Nation for a foreign destination.
An Import is a qualifying transaction in which a Good or eligible Service enters the economic territory of the buyer's Nation from a foreign origin.
Because there is no foreign exchange:
Import Value = USD
Export Value = USD
without currency conversion.
39. Trade Agreements
A Trade Agreement is a Government/Diplomacy-owned agreement that establishes defined economic permissions between Nations.
A Trade Agreement may govern:
tariff rates;
import access;
export access;
investment access;
subsidiary establishment;
resource access;
service access;
procurement;
trade quotas.
The Economy System consumes the resulting permissions.
It does not independently create diplomatic relations.
40. Subsidiaries
A Subsidiary is a geographically established economic operation belonging to, controlled by, or economically linked to a parent Company in another Nation.
A Subsidiary has its own geographic Area.
A Subsidiary may therefore exist as:
Parent Company
      │
      ├── Domestic Industry Area
      │
      ├── Domestic Industry Area
      │
      └── Foreign Subsidiary Area
The Subsidiary operates under the laws and economic conditions of its host Nation.
The parent Company may receive economic income from the Subsidiary according to the applicable:
ownership structure;
tax rules;
Trade Agreement;
investment agreement;
profit distribution rules.
41. Sanctions
A Sanction is a Government/Diplomacy-owned restriction imposed by one Nation or defined group of Nations against a target.
Sanctions are not represented by one universal production penalty.
Each Sanction has:
Sanction Type;
Issuing Entity;
Target Entity;
Severity;
Start Date;
End Date where applicable;
affected transactions;
affected commodities;
affected territories;
exemptions where applicable.
39.1 Sanction Types
Initial sanction categories may include:
Import Restriction;
Export Restriction;
Strategic Resource Embargo;
Investment Restriction;
Financial Restriction;
Technology Restriction;
Transportation Restriction;
Insurance Restriction;
Market Access Restriction.
The final sanction catalogue is defined by the Government/Diplomacy systems.
39.2 Sanction Effects
A sanction modifies the availability of the specific affected economic action.
For example:
Export Restriction
affects the ability to export the defined commodity.
It does not automatically reduce every Industry's production.
A:
Strategic Resource Embargo
may prevent a Nation from acquiring the required input.
If that input is mandatory for a production process and no valid alternative exists:
Actual Production = 0
The causal chain is therefore explicit:
Sanction
→
Input Access Reduced
→
Required Input Unavailable
→
Production Process Cannot Operate
→
Actual Production = 0
rather than:
Sanction
→
Industry Production -50%
without an economic cause.
