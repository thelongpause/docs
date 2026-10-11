# Economy System

## 1. System Definition

The Economy System simulates the financial and commercial activity of Nations, Companies, Industries, Markets and economic participants.

It determines how economic entities acquire and allocate capital, employ labour, establish Industries, purchase inputs, price products, conduct transactions, generate revenue, incur expenditure, accumulate wealth and participate in domestic and international trade.

The Economy System operates alongside the Production, Government, Research, Population, Military, Terrain and Diplomacy Systems.

The Economy System does not independently simulate every physical process involved in producing a commodity. It determines the economic conditions under which economic activity takes place and exchanges the necessary state with the systems responsible for physical production, geography, research, population and government.

### 1.1 Core Responsibilities

The Economy System is responsible for:

1. Economic entities, including Companies and Industries.
2. Company Capital and economic financial state.
3. Industry establishment and economic operation.
4. Industry establishment requirements and capital requirements.
5. Economic product definitions and base prices.
6. Global and domestic market pricing.
7. Economic demand and supply aggregation.
8. Company pricing and commercial decisions.
9. Revenue, expenditure, operating income and financial performance.
10. Economic employment, labour demand and wage expenditure.
11. Economic transactions and commodity ownership transfers.
12. Economic availability of extracted or acquired Strategic Resources recorded by the Inventory System.
13. Domestic and international trade.
14. Foreign investment and foreign commercial operations.
15. Economic effects of Government policy.
16. Economic effects of sanctions and trade restrictions.
17. Subsidies and other defined financial transfers.
18. Economic allocation of available goods, services and resources.
19. Economic interfaces with Production, Government, Research, Population, Military, Terrain and Diplomacy.

### 1.2 Explicit Non-Responsibilities

The Economy System does not own the following mechanics:

| Mechanic | Owning System |
|---|---|
| Physical extraction, manufacturing and construction | Production System |
| Physical production capacity and actual output | Production System |
| Geographic terrain and map geometry | Terrain System |
| Physical Industry facility condition | Production System, with Terrain and Military interfaces |
| Population demographics and household composition | Population System |
| Population-level educational attainment | Population System |
| Education policy and public education funding | Government System |
| Education institutions and technological knowledge | Research System |
| Laws, taxation policy and Government budgets | Government System |
| Military organisation, equipment design and combat | Military System |
| Research projects, component designs and technology development | Research System |
| Diplomatic relations and treaty permissions | Diplomacy System |
| Political institutions, political groups and political authority | Government System |

These ownership assignments define responsibility for simulation rules. They do not prevent another system from supplying inputs, applying effects through a defined interface, or receiving resulting state.

---

## 2. Foundational Economic Rules

### 2.1 Simulation Start

The default starting date is 1 January 1955.

The starting economic model uses reference prices, wages and financial values calibrated to the historical Western economy of the 1950s. These values provide a common economic reference for the fictional world and do not imply that the fictional world uses the historical United States monetary or political system.

The starting values of individual goods, services, strategic resources, Industries, Companies and Nations must be defined in the relevant starting data.

### 2.2 Calendar and Time

Project Sovereign uses a Gregorian-style calendar without leap years.

Every simulation year contains exactly 365 days.

The world clock uses a 24-hour format. The calendar begins on 1 January and advances through 365 days per simulation year.

Economic production and consumption may be resolved daily where required by the relevant system. The Economy System must not assume that all financial transactions, market prices or Company decisions update at the same frequency.

Each economic mechanic must define its own processing frequency and the point at which its state becomes available to other systems.

### 2.3 Currency

The canonical currency is the **Dollar ($)**.

The Dollar is a fictional global currency used throughout Project Sovereign. It is not the United States Dollar and is not controlled by a real-world monetary authority.

All monetary values use Dollars unless an explicitly defined alternative denomination is introduced in a future design decision.

There is no foreign exchange system in the current design.

A transaction between two Nations does not require currency conversion. Differences in economic conditions are represented through prices, wages, taxes, trade restrictions, investment rules and other defined economic mechanics.

### 2.4 Reference Prices and Starting Values

Economic reference prices and wages are calibrated to a 1955 baseline.

A reference value does not automatically determine the price of an actual transaction. Market prices and transaction prices can diverge from reference values through explicitly defined economic rules.

The Economy System must distinguish:

- Base Price.
- Global Price.
- Domestic Market Price.
- Company Selling Price.
- Transaction Price.

These values are not interchangeable.

### 2.5 No Automatic Financial or Physical Effects

A financial transaction does not automatically produce a physical commodity.

Physical production does not automatically generate revenue.

A financial loss does not automatically destroy a Company.

An increase in Industry Scale does not automatically increase physical output unless the Production System can support that output.

A Government subsidy does not automatically increase production.

A change in Company Capital does not automatically change production capacity.

Each resulting effect must occur through the system that owns the affected mechanic.

---

## 3. System Ownership and Interfaces
### 3.1 Economy and Production

The Economy System owns the economic Industry and its commercial state.

The Production System owns the physical production process.

The Economy System determines the financial and commercial conditions under which an Industry operates, including:

- Company Capital available for economic decisions.
- Industry Scale.
- Economic labour demand.
- Intended production priorities.
- Economic demand for inputs.
- Commercial allocation priorities.
- Economic operating expenditure.
- Company financial performance.
- Commercial decisions concerning prices, investment and market participation.

The Production System determines:

- Physical production capacity.
- Actual physical output.
- Physical input consumption.
- Resource extraction.
- Manufacturing and construction processes.
- Physical production constraints.
- Production facility condition.
- Physical stockpile changes caused by production.
- Production losses resulting from physical damage or disruption.

The systems must exchange explicit information about intended production, required inputs, available inputs, physical capacity, actual output and associated economic transactions.

The Economy System must not independently calculate physical production capacity or actual physical output.

The Production System must not independently calculate Company revenue or operating income.

### 3.2 Cross-System Ownership Table

| Data or mechanic | Owner | Economy System responsibility |
|---|---|---|
| Industry Scale | Economy | Calculate and persist |
| Company Capital | Economy | Calculate and persist |
| Nation Treasury | Government | Receive available funding and record approved transfers |
| Industry Establishment Requirements | Economy | Define and validate |
| Physical Production Capacity | Production | Receive and use as an operating constraint |
| Actual Production | Production | Receive resulting output quantities |
| Strategic Resource Deposit | Production | Receive resource availability and extraction outcomes |
| Economic Stockpile Valuation | Economy | Calculate financial value where required |
| Physical inventory quantity, location, condition, reservation and transfer records | Inventory | Use authoritative inventory state and request permitted changes |
| Population Labour Force | Population | Receive available labour |
| Industry Employment | Economy | Allocate economic employment and report changes |
| Educational Attainment | Population | Receive workforce qualification information |
| Education policy, education laws, public funding and institutional governance | Government | Receive applicable policies and funding effects |
| Population-level educational attainment and workforce characteristics | Population | Receive workforce qualification information |
| Scientific research, accumulated technological knowledge, discoveries and technology prerequisites | Research | Receive applicable capabilities and prerequisites |
| Physical laboratories, facilities, equipment and capacity | Production | Receive applicable physical capability constraints |
| Education Policy | Government | Receive policy restrictions and funding effects |
| Government Tax Policy | Government | Apply the defined tax rules to economic accounts |
| Government Subsidies | Government | Record transfers and recipient financial effects |
| Military Procurement Demand | Military | Receive and resolve demand |
| Armament Design and Requirements | Military and Research, according to the defined interface | Receive product specifications and economic demand |
| Diplomatic Trade Permissions | Diplomacy | Apply permissions to economic transactions |
| Sanctions | Government and Diplomacy, according to the sanction mechanism | Apply the defined restrictions |
| Industry Area Geometry | Terrain | Use the geographic reference |
| Physical Facility Damage | Production | Receive damage events and calculate physical consequences |

Where two systems participate in the same mechanic, one must own the authoritative state and the other must interact through a defined interface.

### 3.3 Undefined External Mechanics

When an Economy mechanic depends on a system that has not yet been fully specified, the Economy document must identify the dependency without inventing the external system's rules.

For example, the Economy System may require an Industry to have access to a sufficiently qualified workforce. The Government and Population Systems determine the relevant qualifications and workforce characteristics. The Economy System determines how the Industry's labour requirement is matched against the available workforce.

---

## 4. Economic Entities

### 4.1 Nation

A Nation is a political and economic jurisdiction represented in the world simulation.

The Nation's Government owns its Treasury and public financial decisions. The Economy System simulates the commercial activity taking place within the Nation and the economic consequences of Government actions.

A Nation can contain Companies, Industries, Population Centres, physical production facilities, stockpiles and economic infrastructure.

A Nation's economic state is an aggregation of the economic activity and financial relationships associated with its territory and economic participants. Aggregation rules must prevent the same financial value from being counted multiple times.

### 4.2 Company

A Company is an economic organisation responsible for operating one or more Industries.

A Company may conduct production-related commercial activity, purchase inputs, sell outputs, employ workers, receive investment, incur expenditure and accumulate Company Capital.

Company state includes:

- Company identity.
- Company name.
- Company logo.
- Controlling Senior Executive.
- Associated Industries.
- Company Capital.
- Revenue.
- Expenditure.
- Operating income.
- Employment.
- Commercial priorities.
- Pricing decisions.
- Investment decisions.
- Trade relationships.
- Financial obligations.
- Stockpile ownership, where applicable.

The Senior Executive is the default owner, controller and chief executive of the Company. Ownership is not a separate Company attribute in the current design. An explicit authorised legal mechanism, such as nationalisation or transfer of control, may override this default. Legal state control and the operational executive role must remain distinct. This does not imply a complete equity or shareholder system.

A Company is not identical to an Industry. A Company is an economic organisation; an Industry is an economic activity and operating entity associated with a geographic Industry Area and physical production processes.

A Company may operate multiple Industries if the relevant Government rules permit it.

### 4.3 Senior Executive

Each Company has a Senior Executive Character who controls the Company's commercial decisions by default.

The Senior Executive belongs to the Character System and has Statistics, Traits, Interests and Political Affiliation as defined by that system.

The Economy System uses the Senior Executive's characteristics to inform Company AI decision-making.

Relevant decisions may include:

- Industry expansion or contraction.
- Investment allocation.
- Production priorities.
- Pricing strategy.
- Capital preservation or expenditure.
- Labour and wage decisions.
- Stockpile management.
- Trade participation.
- Response to economic restrictions.
- Negotiation and commercial risk-taking.

Character Statistics and Traits must have separately defined effects.

A Character Statistic informs decision-making according to the Character System and Company AI rules. A Trait may also have a direct mechanical effect on an economic value where its definition explicitly specifies one.

For example, a future Character Trait may provide a defined 15% increase to a particular production-related value. The exact affected value, calculation order, scope and stacking rules must be defined by the Character System and the relevant receiving system. The Economy System must not apply an undefined general-purpose Executive Modifier.

The detailed Character System remains outside the scope of this document.

### 4.4 Company AI

Company AI makes commercial decisions on behalf of the Senior Executive and the Company.

Its decisions are constrained by:

- Applicable Government laws and regulations.
- Available Company Capital.
- Industry Scale and establishment state.
- Available labour.
- Available inputs.
- Physical production capabilities.
- Market conditions.
- Trade permissions.
- Sanctions and restrictions.
- Company obligations.
- The Senior Executive's Statistics, Traits and Interests.
- Player-issued directives, where permitted.

Company AI does not override physical production constraints, Government authority or transaction validity rules.

The exact decision algorithms belong in the AI and Technical Specifications.

### 4.5 Player Control

The Player may influence economic activity through defined actions, including:

- Establishing an Industry.
- Establishing or restructuring a Company, where permitted.
- Setting production priorities.
- Directing the allocation of available output.
- Providing Government subsidies.
- Authorising capital injections.
- Appointing or replacing Senior Executives where the relevant authority permits.
- Setting procurement requirements.
- Establishing economic legislation.
- Nationalising or transferring control of Companies.
- Establishing price controls.
- Permitting or restricting trade.
- Directing strategic investment.

The Player does not automatically possess direct operational control over every Company.

The available actions depend on Government authority, laws, ownership and the relevant distribution of economic power.

---

## 5. Government Finance and Industry Establishment

### 5.1 Nation Treasury

The Nation Treasury is the authoritative account of public funds held by the Government.

It is owned by the Government System.

Treasury funds are distinct from Company Capital and must not be combined into a single account.

Government revenue and expenditure, including taxation, public spending, subsidies and Industry establishment payments, are governed by the Government System and recorded through explicit financial interfaces with the Economy System.

### 5.2 Company Capital

Company Capital is the Company's spendable financial balance from which purchases, wages and other payments are made.

Company Capital is distinct from:

- Nation Treasury.
- Company revenue.
- Company profit or loss.
- Physical inventories.
- The financial value of unsold stockpiles.
- Industry Establishment Capital Requirement.

Company Capital changes through explicitly defined financial events, including opening capital allocation, operating receipts and payments, investment, subsidies, capital injections, financing transactions and distributions where those mechanics are implemented. The same funds must not be represented as a second Company cash balance.

Company Capital does not automatically depreciate.

Any future reduction in Company Capital must arise from a defined financial transaction or rule, not an assumed depreciation process.

### 5.3 Industry Establishment

Industry Establishment is the process by which an Industry is authorised, funded and created within the simulation.

By default, the Government establishes an Industry and pays its Industry Establishment Capital Requirement from the Nation Treasury.

Establishment creates the relevant economic Industry and its associated Company arrangements, geographic representation and Production System interfaces.

Establishing an Industry does not automatically guarantee that it can operate at full capacity. Its operation remains subject to labour, inputs, technology, infrastructure, physical production capacity, legal restrictions and other applicable requirements.

### 5.4 Industry Establishment Capital Requirement

Each Industry Type defines an Industry Establishment Capital Requirement expressed in Dollars.

This is the required capital expenditure to establish the Industry under the relevant establishment rules.

The requirement is distinct from:

- Opening Company Capital.
- Ongoing operating expenditure.
- Capital required to expand Industry Scale.
- Capital required to construct additional physical facilities.
- Capital required to acquire inputs or stockpile outputs.

The precise establishment cost of each Industry Type must be defined in the Industry data.

### 5.5 Opening Company Capital

When an Industry and its associated Company are established, the Company receives an Opening Company Capital allocation.

The Opening Company Capital allocation is separate from the Industry Establishment Capital Requirement.

The Government's total establishment outlay therefore consists of separate, auditable financial movements:

1. The Industry Establishment Capital Requirement paid from the Nation Treasury to an explicitly defined recipient or expenditure account.
2. The Opening Company Capital allocation transferred from the Nation Treasury to the Company's Company Capital balance.

The amount of Opening Company Capital must be defined by the relevant establishment rules. It must not be assumed to equal the establishment cost. The establishment payment must not be implicitly converted into facility value, production capacity or Company Capital. Both movements must identify their source, destination, amount and accounting purpose.

### 5.6 Industry Establishment Requirements

Every Industry Type must have an explicit Establishment Requirements definition.

The definition identifies all prerequisites that must be satisfied before that Industry can be established.

Depending on the Industry Type, requirements may include:

- Industry Establishment Capital Requirement.
- Required Opening Company Capital.
- Strategic Resource access.
- Relevant technology.
- Geographic eligibility.
- Infrastructure access.
- Availability of qualified labour.
- Government authorisation.
- Legal permissions.
- Production facility requirements.
- Environmental or geographic constraints.
- Other explicitly defined prerequisites.

Not every Industry must require every category of prerequisite.

The establishment rules for each Industry Type must identify which requirements apply, how each requirement is validated and which system supplies the relevant state.

The existence of a Strategic Resource is not a universal prerequisite for establishing an Industry unless that Industry's Establishment Requirements explicitly require it.

### 5.7 Establishment Validation

An Industry may be established only when all mandatory Establishment Requirements are satisfied and the Government can fund the required Treasury expenditure.

If any mandatory requirement fails, or the Nation Treasury cannot fund every required establishment payment, establishment is rejected.

A rejected establishment attempt must not partially create the Industry or Company, deduct Treasury funds, transfer Opening Company Capital or create partial physical facilities.

If establishment succeeds, validation, entity creation and all required financial movements must be committed as one atomic operation. If any required part fails, the entire operation fails and no partial state change remains. Existing Industries loaded from scenario starting data are not new establishment events and incur no retroactive establishment charge.

The exact handling of interruptions, cancellation and save-state recovery belongs in the Technical Specifications.

### 5.8 Nationalisation and Transfer of Control

Government nationalisation or another legally authorised transfer of control may change who controls a Company.

Such an action does not automatically determine the value, ownership or destination of every asset and liability associated with the Company. These consequences must be defined by the relevant Government legislation and economic transaction rules.

The default ownership model remains that the Senior Executive is the Company's owner, controller and chief executive unless an authorised mechanism changes that control.

---

## 6. Industries

### 6.1 Industry Definition

An Industry represents a defined category of economic activity associated with a Company and a geographic Industry Area.

An Industry has economic state, commercial requirements, an associated physical production process and defined relationships with the relevant systems.

Industry Scale determines the economic magnitude at which the Industry is intended to operate. It is not itself a measure of physical output.

Industry Scale affects the Industry's economic requirements and potential activity, including:

- Labour demand.
- Capital requirements.
- Input demand.
- Intended production level.
- Potential output value.
- Potential revenue.
- Potential operating expenditure.
- Required infrastructure and operating resources, where applicable.

The relationship between Industry Scale and physical production capacity is defined through the Economy and Production interface.

### 6.2 Industry Types

Project Sovereign contains exactly 16 Industry Types in the initial Industry taxonomy.

| No. | Industry Type | Primary Economic Function |
|---:|---|---|
| 1 | Transportation | Commercial movement of people and goods through transport services and associated operations |
| 2 | Oil | Extraction and supply of petroleum |
| 3 | Construction | Construction, repair and development of buildings, facilities and infrastructure |
| 4 | Automotive | Production of road vehicles and associated automotive products |
| 5 | Aerospace | Production of aircraft, aerospace vehicles and associated products |
| 6 | Maritime | Production of ships, boats and associated maritime products |
| 7 | Energy Production | Production and commercial supply of non-nuclear energy |
| 8 | Industrial Manufacturing | Production of industrial equipment, machinery, materials and other industrial products not assigned to a more specific Industry |
| 9 | Mining | Extraction of designated mineral and geological resources |
| 10 | Nuclear Energy | Production and commercial supply of nuclear energy |
| 11 | Electronics | Production of electronic equipment, electrical components and associated products |
| 12 | Steelmaking | Production of steel and associated steel products |
| 13 | Space | Production of space vehicles, launch systems and associated space products |
| 14 | Agribusiness | Processing, packaging, storage and commercial distribution of agricultural outputs |
| 15 | Consumer Goods | Production of household and personal-use goods not assigned to a more specific Industry |
| 16 | Agriculture | Production and extraction of agricultural outputs associated with an agricultural Industry Area |

These Industry Types define economic specialisation, not a restriction that each Industry can produce only one Good.

The precise outputs and input recipes available to each Industry must be defined in the Product and Production data.

### 6.3 Agriculture and Agribusiness

Agriculture represents primary agricultural activity. It operates within a geographic Industry Area and produces the outputs defined for agricultural production.

Agribusiness represents the processing, packaging, storage and commercial distribution of agricultural outputs.

Agriculture may produce agricultural outputs and extract Rich Soils where the relevant Production rules permit it.

Agribusiness may purchase agricultural outputs and transform them into other Goods through defined production processes.

The two Industries are distinct. Agricultural outputs do not automatically become processed or packaged consumer goods.

### 6.4 Industry Scale

Industry Scale represents the intended economic scale of an Industry.

Industry Scale is not interchangeable with:

- Physical Production Capacity.
- Actual Production.
- Number of employees.
- Company Capital.
- Number of facilities.
- Geographic Area size.

An Industry's Scale must be calculated from explicitly defined economic rules and available state.

Relevant inputs include:

- Available Company Capital.
- Available labour and required qualifications.
- Required input availability.
- Relevant technology.
- Government restrictions.
- Establishment requirements.
- Industry Type.
- Senior Executive decision-making.
- Applicable economic policies.

The calculation must identify how each input affects Scale. These relationships must not be implemented as undefined generic modifiers.

The Industry Scale calculation, permitted range, update frequency and adjustment limits must be specified in the relevant economic algorithms.

### 6.5 Industry Geographic Representation

Each Industry has a geographic Industry Area represented using the Terrain System's Area Mapping Primitive. The Industry Area is a geographic association, not the Industry's economic Scale or physical production capacity.

The Industry Area identifies the geographic footprint associated with the Industry. Its size and shape must be defined by the relevant geographic and establishment rules.

For Agriculture, the Industry Area represents the geographic area used for agricultural activity.

An Industry Area may overlap with other geographic and terrain objects where the map rules permit it.

The Industry Area may be rendered using an Industry-specific bitmap or visual representation. Rendering does not determine simulation priority or physical effects.

### 6.6 Industry Area State

The Industry Area, physical facilities, economic Industry and production processes are separate but connected entities.

Their state must not be collapsed into a single Industry Health value.

| State | Meaning | Owning System |
|---|---|---|
| Geographic footprint | Area occupied by the Industry | Terrain |
| Area integrity | Condition of the geographic Industry Area as a simulation object | Terrain, with Military interaction |
| Facility condition | Physical condition of the facilities used for production | Production |
| Operational status | Whether a facility or process can operate under current conditions | Production |
| Industry Scale | Intended economic scale of the Industry | Economy |
| Physical Production Capacity | Maximum physical output under current operating conditions | Production |
| Actual Production | Quantity physically produced | Production |
| Workforce | Employees associated with the Industry | Economy, using Population inputs |
| Stockpile quantity | Physical quantity of a commodity at a location | Designated inventory owner |
| Company financial state | Capital, revenue, expenditure and operating income | Economy |
| Infrastructure connections | Connections to transport, energy, logistics and other networks | Relevant infrastructure-owning system |

### 6.7 Damage and Disruption

Military action, bombardment, sabotage, accidents and other defined events may affect an Industry Area or its associated facilities.

Each event must identify the object affected and the state change caused.

Examples include:

- Damage to the geographic Industry Area.
- Damage to a physical production facility.
- Destruction of stored inputs or finished Goods.
- Interruption of power or transport connections.
- Loss of employees through casualties or displacement.
- Temporary suspension of a production process.
- Permanent loss of physical capacity.
- Repair expenditure.
- Reduced or restored operating availability.

These effects must be resolved by the systems that own the affected state.

Damage to an Industry Area does not automatically imply an equal reduction in production capacity. Physical capacity is determined by the condition and availability of the relevant facilities and processes.

Repairing an Industry Area does not automatically restore destroyed equipment, stockpiles, workforce or Company Capital.

### 6.8 Industry Expansion and Contraction

An Industry may expand or contract when its economic conditions, Government permissions, Company decisions or Player directives change.

Expansion may require additional Company Capital, labour, inputs, facilities, technology or infrastructure.

Contraction may reduce the Industry's economic Scale, labour demand or intended activity.

The Economy System determines economic Scale and associated economic requirements. The Production System determines the physical changes required to realise a change in production capability.

Industry Scale must not increase beyond what the defined economic and physical constraints permit.

### 6.9 Industry Persistence

Industry identity, Industry Type, Company association, Industry Scale, geographic association and financial relationships persist unless changed by an explicit simulation event or rule.

Physical capacity, facility condition, operational status, stockpile quantities and actual production persist according to their respective owning systems.

An Industry's economic existence does not imply that it is currently producing.

---

## 7. Economic Products

### 7.1 Product Definition

An Economic Product is a defined category of commodity or service that can participate in economic activity.

The Economy System uses Economic Products to represent demand, supply, prices, transactions, stockpiles, input requirements and commercial output.

Product categories should remain manageable and meaningful to the Player. Individual recipes, military designs and specific product configurations should be represented through more detailed definitions where required, rather than by creating an excessive number of generic economic categories.

The product taxonomy must support both the consumer economy of a 1950s/60s industrial society and the detailed research, component and armament design systems.

### 7.2 Economic Product Classes

Every Economic Product has exactly one primary class:

1. Good.
2. Service.
3. Designed Component.
4. Armament.

Strategic Resource is a separate designation that may be applied to an eligible physical product; it is not a mutually exclusive primary class. A product designated as a Strategic Resource retains its primary class and all applicable unit, inventory, production and transaction rules.

A Strategic Resource is a physical resource or material designated as strategically significant. It can be extracted or produced, stockpiled, transferred, sold or consumed according to its definition.

A Designed Component is a defined physical component that can be produced and used as an input in a larger product or system. Military-designed components may be researched and designed through the Research and Military Systems.

An Armament is a completed or independently usable military item or system, such as a rifle, missile, tank, aircraft, naval vessel or radar installation.

A Designed Component is not automatically an Armament. A radar unit designed as a component for an aircraft, for example, is distinct from the completed aircraft incorporating it.

The exact design, compatibility, assembly and performance rules for Components and Armaments belong to the Research, Military and Production Systems. The Economy System supports their economic costs, demand, supply, stockpiles and transactions.

### 7.3 Goods

Goods are physical products that can be produced, purchased, sold, consumed, stockpiled, imported, exported or used as production inputs.

Each Good must have a defined identity, category, unit, Base Price and relevant economic properties.

A Good may be an input to an Industry, an output of an Industry, a consumer product or an item used by another system.

A Good's economic category does not by itself determine its physical production recipe or all of its possible uses.

### 7.4 Mutually Exclusive Good Categories

Every product whose primary class is Good belongs to exactly one of the following three mutually exclusive economic Good Categories:

1. Staple Goods.
2. Luxury Goods.
3. Industrial Goods.

These categories are mutually exclusive. A Good cannot simultaneously be a Staple Good and a Luxury Good.

The categories describe the Good's primary economic role, not necessarily every possible consumer or use.

#### 7.4.1 Staple Goods

Staple Goods are everyday goods associated with ordinary household consumption and routine material needs.

Examples include:

- Grocery Staples.
- Basic Clothing.
- Household Cleaning Products.
- Personal Hygiene Products.
- Basic Household Supplies.
- Basic Furniture and Household Essentials.

Staple Goods are intended to represent categories of recurring household demand rather than hundreds of individual product models.

Demand for Staple Goods is primarily associated with population, household structure, consumption requirements, purchasing power and availability.

Different Population Types may have different demand levels, preferences and affordability for Staple Goods. The Population System defines the relevant population characteristics; the Economy System resolves the associated economic demand.

#### 7.4.2 Luxury Goods

Luxury Goods are discretionary or premium products whose economic role is distinguished from ordinary staple consumption.

Examples include:

- Luxury Clothing and Accessories.
- Premium Jewellery and Watches.
- Luxury Cosmetics and Perfumes.
- Luxury Furniture and Home Furnishings.
- Premium Consumer Electronics.
- Luxury Motor Vehicles.
- Premium Recreational Products.

Luxury Goods are separate from Staple Goods even where the products serve related broad needs. For example, Basic Clothing is a Staple Good, whereas Luxury Clothing and Accessories are Luxury Goods.

Demand for Luxury Goods may depend on income, wealth, preferences, status-related consumption and cultural factors. The Population and relevant social systems define these factors; the Economy System resolves their economic consequences through explicit demand interfaces.

#### 7.4.3 Industrial Goods

Industrial Goods are physical products primarily intended for production, infrastructure, transport, construction, commercial operations or other productive uses.

Examples include:

- Industrial Machinery.
- Machine Tools.
- Industrial Equipment.
- Construction Materials.
- Electrical Equipment.
- Industrial Chemicals.
- Commercial Vehicles.
- Agricultural Machinery.
- Transport Equipment.
- General-Purpose Industrial Components.

Industrial Goods may be purchased by Companies, Industries, Government entities or other authorised economic participants.

Industrial Goods are not interchangeable with Strategic Resources. A Strategic Resource is explicitly designated as a strategically significant resource or material; an Industrial Good is a defined economic product category.

A particular material may have both raw-resource and processed-product representations where the relevant definitions distinguish them. These representations must not be treated as the same inventory item.

### 7.5 Initial Economic Goods Catalogue

The initial catalogue should use manageable product categories rather than a separate Good for every brand, model, colour or quality variation.

The following is the proposed starting catalogue. It establishes economic product categories, not the complete production recipes for every item.

| Good Category | Economic Good | Primary Industry Association |
|---|---|---|
| Staple | Grocery Staples | Agriculture; Agribusiness |
| Staple | Basic Clothing | Consumer Goods |
| Staple | Household Cleaning Products | Consumer Goods |
| Staple | Personal Hygiene Products | Consumer Goods |
| Staple | Basic Household Supplies | Consumer Goods |
| Staple | Basic Furniture and Household Essentials | Consumer Goods |
| Luxury | Luxury Clothing and Accessories | Consumer Goods |
| Luxury | Jewellery and Watches | Consumer Goods |
| Luxury | Luxury Cosmetics and Perfumes | Consumer Goods |
| Luxury | Luxury Furniture and Home Furnishings | Consumer Goods |
| Luxury | Premium Consumer Electronics | Electronics |
| Luxury | Luxury Motor Vehicles | Automotive |
| Luxury | Premium Recreational Products | Consumer Goods |
| Industrial | Industrial Machinery | Industrial Manufacturing |
| Industrial | Machine Tools | Industrial Manufacturing |
| Industrial | Industrial Equipment | Industrial Manufacturing |
| Industrial | Construction Materials | Construction; Industrial Manufacturing; Steelmaking |
| Industrial | Electrical Equipment | Electronics; Industrial Manufacturing |
| Industrial | Industrial Chemicals | Industrial Manufacturing |
| Industrial | Commercial Vehicles | Automotive |
| Industrial | Agricultural Machinery | Industrial Manufacturing; Automotive |
| Industrial | Transport Equipment | Automotive; Aerospace; Maritime |
| Industrial | General-Purpose Industrial Components | Industrial Manufacturing; Electronics |

This catalogue is an initial economic taxonomy. Additional categories may be introduced where a distinct demand profile, production process, strategic role or economic behaviour justifies the distinction.

A Good may have more than one relevant Industry association. The association identifies Industries that may produce, process, supply or use the Good; it does not guarantee that every associated Industry can produce it without the required technology, inputs, facilities and production recipe.

### 7.6 Services

Services are economic outputs whose value derives primarily from an activity performed for a consumer or recipient rather than from the transfer of a physical product.

Services may be supplied by Companies, Government entities or other authorised economic participants.

Initial service categories may include:

- Transportation Services.
- Commercial Logistics Services.
- Construction Services.
- Repair and Maintenance Services.
- Professional and Business Services.
- Financial Services.
- Communications Services.
- Retail and Distribution Services.
- Personal Services.
- Other explicitly defined service categories.

A Service must have a defined unit of sale or delivery, Base Price and applicable demand rules.

The physical production and delivery requirements of a Service must be defined by its responsible system. For example, Transportation Services depend on transport capacity and infrastructure, while the Economy System resolves commercial demand, prices and payments.

Whether a Service can be imported, exported or traded internationally must be explicitly defined for that Service category.

### 7.7 Designed Components

Designed Components support the component-based Research, Military and Production model.

A Component may have a design specification, technology requirements, production recipe, compatibility constraints, performance characteristics and economic value.

Examples include:

- Engines.
- Radar Sets.
- Sensors.
- Optical Sights.
- Rangefinders.
- Communications Equipment.
- Electronic Countermeasure Equipment.
- Missiles.
- Missile Guidance Systems.
- Fire-Control Systems.
- Weapons.
- Protective Equipment.
- Camouflage Materials.
- Specialist Military Clothing.
- Ghillie Suits.
- Ammunition Components.

These examples are not a closed list. The detailed Component catalogue and design system belong to Research, Military and Production.

A designed Component can be produced as an industrial output, purchased by an authorised buyer, stockpiled and consumed as an input into a larger design or production process.

Component performance is defined by the relevant design and simulation systems. The Economy System determines economic demand, supply, costs and transactions without independently inventing performance values.

### 7.8 Armaments

Armaments are military products intended for independent military use, deployment, issue, operation or installation.

Examples include:

- Rifles and other small arms.
- Ammunition.
- Infantry equipment.
- Military uniforms and specialist camouflage equipment.
- Tanks and other armoured vehicles.
- Military aircraft.
- Naval vessels.
- Missiles and independently deployable weapon systems.
- Radar stations.
- Military communications installations.
- Other independently usable military equipment.

An Armament may incorporate multiple Designed Components.

The Military System defines operational requirements and military demand. The Research System defines applicable technology and design knowledge. The Production System determines how the Armament is physically produced or assembled. The Economy System resolves the associated economic requirements and transactions.

A component does not become an Armament solely because it is used by the military. A radar component incorporated into an aircraft remains a Component; a separately deployable radar station is an Armament.

The exact distinction between Components and Armaments must be established in the product and design definitions so that demand, production, procurement and inventory systems use consistent identities.

### 7.9 Strategic Resources

A Strategic Resource is a designated physical resource or material whose availability is important to economic or industrial activity.

Strategic Resources may be extracted, produced, stockpiled, transferred, traded and consumed according to their defined rules.

The initial Strategic Resource catalogue contains exactly eight categories:

| Strategic Resource | Primary Industry Association | Resource Type |
|---|---|---|
| Petroleum | Oil | Extracted resource |
| Steel | Steelmaking | Produced strategic material |
| Aluminium | Industrial Manufacturing | Produced strategic material |
| Coal | Mining | Extracted resource |
| Tungsten | Mining | Extracted resource |
| Uranium | Mining | Extracted resource |
| Rare-earth Metals | Mining | Extracted resource |
| Rich Soils | Agriculture | Extracted agricultural resource |

These categories are retained as the initial Strategic Resource taxonomy.

Steel and Aluminium are treated as strategic materials produced through industrial processes, rather than as naturally occurring geological deposits. Their physical production rules must define the required inputs and outputs.

Rich Soils are represented as a commodity extracted through Agriculture and recorded in stockpiles. This is a deliberate abstraction for the economic simulation.

The Production System defines the physical extraction or production mechanics for each Strategic Resource. The Economy System defines its economic identity, price, demand, trade, ownership and financial treatment.

### 7.10 Product Units and Consumption Intervals

Every Good must have a defined unit of measurement.

For consumer Goods, a Good Unit represents the quantity required to satisfy the standard consumption requirement of **one individual for the Good's specified Consumption Interval**.

The Consumption Interval defines the period for which one Good Unit satisfies that standard requirement.

For example, if a Good has a Consumption Interval of one day, one Good Unit represents the standard daily requirement of one individual. If its interval is one year, one Good Unit represents the standard annual requirement.

This definition does not mean every individual consumes exactly one unit in every interval. Actual demand may depend on Population Type, household circumstances, income, preferences, availability and other explicitly defined variables.

The Population System owns individual and household characteristics. The Economy System receives demand quantities and resolves their economic consequences.

For Industrial Goods, Strategic Resources, Components, Armaments and Services, the relevant unit must reflect the item's defined production, consumption, sale or delivery requirements. The individual-consumption definition must not be applied indiscriminately to products that are not consumer goods.

---

## 8. Product Prices

### 8.1 Base Price

Every tradable Good, Service, Strategic Resource, Component and Armament must have a defined Base Price where the relevant product is assigned a monetary value.

Base Price is the static reference monetary value assigned to one defined unit of the product at the beginning of the simulation.

Base Prices are calibrated to the 1955 economic baseline.

Base Price is not necessarily the actual market price, Company Selling Price or Transaction Price.

Base Prices are owned by the Economy System and persist unless changed by an explicit data or simulation rule.

### 8.2 Global Price

Global Price is the current global reference price of an eligible product.

It represents the global market's aggregate economic conditions and provides a reference for domestic pricing and international transactions where applicable.

Global Price may differ from Base Price as global supply, global demand and other explicitly defined market conditions change.

The Global Price calculation must identify:

- The relevant product unit.
- The aggregation period.
- Global supply.
- Global demand.
- Eligible stockpile availability.
- Restrictions on market participation.
- The defined price-response function.
- The permitted price range, if any.
- The update frequency.

The price-response function must be explicitly defined in the Economic Algorithms or Technical Specifications. The Economy System must not apply an unspecified market modifier.

### 8.3 Domestic Market Price

Domestic Market Price is the reference price for a product within a defined domestic market.

A domestic market may reflect the relevant Nation's economic conditions, domestic availability, purchasing power, transport costs, restrictions and other defined inputs.

Domestic Market Price is distinct from Global Price and Company Selling Price.

The rules defining domestic market boundaries, price aggregation and regional market separation must be specified in the market model.

### 8.4 Company Selling Price

Company Selling Price is the price at which a Company offers a product for sale.

The Senior Executive and Company AI determine the Company's pricing decisions within applicable Government rules, contractual obligations, market conditions and available state.

Relevant considerations may include:

- Base Price.
- Global Price.
- Domestic Market Price.
- Available inventory.
- Intended sales volume.
- Demand.
- Competition, where represented.
- Input and operating costs.
- Company financial state.
- Price controls.
- Taxes and transaction costs.
- Trade restrictions.
- Contractual obligations.
- Senior Executive decision-making.

These considerations must be implemented through defined decision rules. The list does not imply that every consideration automatically changes price.

A Company may offer a product at a price above, below or equal to its reference prices when permitted by the applicable rules.

### 8.5 Transaction Price

Transaction Price is the actual price agreed for a completed economic transaction.

It may equal the Company Selling Price, but this is not mandatory where a permitted negotiated price, contract price, Government procurement price or other explicitly defined transaction rule applies.

Transaction Price is the authoritative monetary value used to calculate the payment for that transaction.

### 8.6 Price Controls

Government may impose price controls where authorised by its laws and institutions.

A price control must specify:

- The authority imposing it.
- The affected product or product class.
- The affected market or jurisdiction.
- The type of price restriction.
- The permitted price range or required price.
- The effective date and duration.
- The entities and transactions to which it applies.

The Economy System applies the restriction to eligible pricing and transactions.

Price controls do not automatically guarantee that a product will be supplied or that demand will be fulfilled. Supply, production, allocation and transaction rules continue to apply.

---

## 9. Supply, Demand and Markets

### 9.1 Market Definition

A market is an economic aggregation and transaction context in which demand, supply, prices and economic exchanges are resolved.

Markets do not create physical commodities. They aggregate and resolve the economic availability of commodities supplied through production, stockpiles, imports and other permitted sources.

A market may be global or domestic.

The market model must identify which products participate, which entities may trade, the relevant geographic or economic scope, the aggregation period and the rules for resolving transactions.

### 9.2 Global Market

The Global Market aggregates eligible economic supply and demand across participating Nations.

It provides a global reference context for product availability and Global Price.

The Global Market is not a physical location or a universal stockpile. A quantity being represented in global market statistics does not mean that it can automatically be purchased and delivered to any Nation. Physical access, transport, delivery time, permissions, sanctions and existing commitments constrain fulfilment.

Global market participation is subject to:

- Trade permissions.
- Export and import restrictions.
- Sanctions.
- Physical availability.
- Transport and infrastructure.
- Contractual commitments.
- Applicable Government policy.
- The product's eligibility for international trade.

The Global Market must distinguish aggregate global production, available stockpiles, domestic use, export availability, restricted quantities and quantities already committed to other transactions.

### 9.3 Global Supply

Global Supply represents the quantity of an eligible product available to satisfy demand within the defined global market period.

Global Supply is not automatically equal to Global Production.

Its calculation must distinguish:

- Newly produced quantity.
- Eligible quantities released from stockpiles.
- Quantities reserved for existing obligations.
- Quantities allocated to domestic use.
- Quantities available for export.
- Quantities restricted from trade.
- Quantities physically unavailable for delivery.
- Quantities destroyed or lost.

The exact Global Supply calculation must prevent the same physical quantity from being counted more than once.

### 9.4 Global Demand

Global Demand is the aggregate demand for an eligible product within the defined global market period.

The canonical demand sources are:

1. Population Demand.
2. Industry Demand.
3. Government Demand.
4. Military Demand.
5. Foreign Demand, where not already represented through the preceding sources.
6. Other demand sources explicitly defined by a system.

A demand quantity must have a defined originating entity, product, quantity, time period and fulfilment status.

Foreign Demand must not duplicate the same demand already represented as Population, Industry, Government or Military Demand within another Nation.

### 9.5 Population Demand

Population Demand originates in the Population System.

It represents the quantity of consumer Goods and eligible Services required or desired by the population.

Relevant inputs may include:

- Population.
- Household structure.
- Population Types.
- Consumption requirements.
- Consumption Intervals.
- Income and purchasing power.
- Preferences.
- Availability.
- Other defined population conditions.

The Population System owns the population characteristics and demand-generation rules. The Economy System receives and resolves the resulting economic demand.

Demand does not guarantee a completed purchase. Actual purchases depend on affordability, availability, market access and the applicable transaction and allocation rules.

### 9.6 Industry Demand

Industry Demand represents the Goods and Strategic Resources required by Industries for their intended operations.

Physical input requirements are defined by Production recipes.

The Economy System receives the required input quantities and resolves their commercial availability, procurement, costs and allocation.

The intended input requirement for a production process is derived from its defined recipe and intended production quantity.

The Economy System must not invent input ratios for a physical production process.

### 9.7 Government Demand

Government Demand represents economic demand generated by Government activity.

Examples include:

- Public-service procurement.
- Public infrastructure projects.
- Strategic stockpile acquisition.
- Government facility requirements.
- Public-sector equipment and supplies.
- Other authorised Government purchases.

The Government System defines Government requirements, budget authority and purchasing permissions. The Economy System resolves the economic demand, available supply, transaction prices and financial transfers.

Government demand must not be treated as unlimited merely because the Government has issued a requirement.

### 9.8 Military Demand

Military Demand represents economic demand originating from the Military System.

It may include:

- Armaments.
- Ammunition.
- Military vehicles.
- Military aircraft.
- Naval vessels.
- Military components.
- Uniforms and protective equipment.
- Camouflage and specialist field equipment.
- Fuel and other supplies.
- Maintenance and repair services.
- Other explicitly defined military requirements.

The Military System determines the required items, quantities, timing and military priorities.

The Economy System resolves the associated economic demand, procurement costs, commercial availability and financial transactions.

The Production System determines whether the required items can physically be produced.

### 9.9 Demand Fulfilment

Demand Fulfilment represents the quantity of a demand that has been supplied through completed or otherwise valid fulfilment.

The system must distinguish:

- Requested quantity.
- Quantity available.
- Quantity allocated.
- Quantity ordered.
- Quantity delivered.
- Quantity paid for.
- Unfulfilled quantity.
- Cancelled or expired quantity.

These states must not be treated as equivalent.

An order does not count as delivered supply before the applicable delivery conditions are satisfied. A delivery does not automatically count as paid revenue until the financial transaction rules establish that payment has occurred.

### 9.10 Economic Allocation

Available output may be allocated to:

- Population consumption.
- Industry inputs.
- Government use.
- Military procurement.
- Export.
- Stockpiling.
- Other explicitly defined destinations.

Allocation determines the intended destination of a quantity. It does not automatically mean the quantity has been physically delivered or consumed.

Allocation rules must identify applicable priority, reservation, contractual obligation, Government directive, Military requirement, export commitment and stockpile policy.

The rules must define how competing demands are resolved when available supply is insufficient.

The Economy System must not silently allocate the same quantity to multiple destinations.

---

## 10. Production and Inventory Interfaces

### 10.1 Production Capacity

Production Capacity is the maximum quantity of a defined output that a production process can physically produce per Production Day under its current operating conditions.

Production Capacity is owned by the Production System.

It may depend on facilities, equipment, labour availability, technology, physical inputs, operational status, infrastructure and other explicitly defined production conditions.

The Economy System receives Production Capacity as a constraint on the Industry's intended activity.

Production Capacity is not a guarantee that the full quantity will be produced.

### 10.2 Intended Production

Intended Production represents the quantity or production priorities the Industry is seeking to realise under its current economic and commercial decisions.

The Economy System supplies economic priorities and relevant constraints. The Production System determines the physically feasible output.

Intended Production must be compatible with:

- Industry Scale.
- Available Company Capital.
- Available labour.
- Input availability.
- Commercial demand and allocation priorities.
- Government restrictions.
- Production Capacity.
- The production process's technical requirements.

The exact relationship between Industry Scale and Intended Production must be defined by the Economy and Production interface.

### 10.3 Actual Production

Actual Production is the quantity physically produced by the Production System during the relevant production period.

Actual Production is constrained by the applicable physical production rules, including input availability, operational status and Production Capacity.

The Economy System records the economic consequences of the reported output. It must not independently recalculate or override Actual Production.

Actual Production does not automatically equal quantity sold, quantity delivered or revenue earned.

### 10.4 Production Recipes

A Production Recipe defines the physical inputs and outputs required for a production process.

Each recipe must specify:

- Production Process identity.
- Required input product.
- Required input quantity.
- Output product.
- Output quantity.
- Relevant unit definitions.
- Applicable technology or process requirements.
- Any defined by-products, waste or losses.
- The production period or batch basis.

The exact recipe data and processing algorithms belong to the Production System and Technical Specifications.

The Economy System uses recipe-derived input demand for economic planning and procurement.

There is no universal assumption that one unit of input produces one unit of output.

### 10.5 Input Availability

The Production System determines the physical quantities available to production processes.

The Economy System may procure or allocate inputs through economic transactions and inventory reservations.

Only quantities that are validly available to a process may be treated as available inputs.

The input interface must distinguish quantities held in stock, quantities reserved for other processes, quantities in transit and quantities that have arrived and are eligible for use.

A scheduled delivery must not be treated as physically available before its delivery conditions are satisfied.

### 10.6 Input Shortfalls

The Production System determines how a process responds to insufficient inputs according to its explicit production rules.

A production process that requires all mandatory inputs must not silently consume nonexistent quantities or produce output without satisfying those requirements.

If a recipe permits partial production, substitution or alternative inputs, that behaviour must be explicitly defined by the recipe and Production rules.

The Economy System records the commercial consequences of the resulting production state.

### 10.7 Stockpiles

A Stockpile is an inventory of a defined physical commodity held at a location or by an economic entity.

Stockpiles may contain Strategic Resources, Goods, Components, Armaments, Industry Inputs and Industry Outputs.

A stockpile record must identify:

- Commodity identity.
- Quantity.
- Unit.
- Location.
- Owner or controlling entity.
- Allocation or reservation status.
- Incoming transfers.
- Outgoing transfers.
- Applicable storage restrictions.
- Relevant physical condition, where applicable.

The Inventory System is the single authoritative owner of physical inventory records, including quantities, locations, condition, reservations, transfers and inventory movements. Economy and Production use this shared state and must not maintain competing inventories. Legal or organisational ownership is recorded as a field on the authoritative inventory record.

### 10.8 Stockpile Accounting

For a defined inventory period, the basic quantity reconciliation is:

**Closing Quantity = Opening Quantity + Incoming Quantity − Consumed Quantity − Transferred Out − Sold Quantity − Destroyed Quantity**

Each term must refer to a distinct, valid inventory movement. A sale and a transfer must not both deduct the same physical quantity for one transaction.

The resulting quantity cannot be negative.

A transaction that would reduce available stock below the required quantity must be rejected, reduced or deferred according to the relevant transaction and allocation rules. Borrowing, negative inventory or deficit inventory must not be assumed unless explicitly introduced as a separate mechanic.

### 10.9 Stockpile Valuation

The financial value of a stockpile is distinct from its physical quantity.

Where inventory valuation is required for Company financial reporting, the valuation method must be explicitly defined in the Economy System or its Technical Specifications.

A change in Global Price must not automatically create Company revenue or cash.

A physical inventory loss does not automatically deduct Company Capital by the same amount unless the financial accounting rules define that consequence.

### 10.10 Unsold Output

Output that has been physically produced but not sold or otherwise transferred remains inventory.

Production does not automatically create revenue.

Revenue is recorded only when a valid economic transaction or other explicitly defined revenue event occurs.

---

## 11. Labour and Wages

### 11.1 Labour Ownership

The Population System determines population composition, workforce characteristics, educational attainment and the Nation's available labour supply.

The Economy System receives the available labour information and allocates workers to economic employment according to Industry requirements and the relevant employment rules.

The Economy System does not independently create population or change educational attainment.

### 11.2 Labour Demand

Labour Demand represents the number and type of workers an Industry requires for its intended economic activity.

Labour Demand must identify relevant employment categories and qualifications.

The calculation may depend on:

- Industry Type.
- Industry Scale.
- Intended Production.
- Production process requirements.
- Technology.
- Required qualifications.
- Operating schedule.
- Defined labour intensity.

The relevant labour requirements must be explicit. Labour demand must not be calculated using an undefined generic labour modifier.

### 11.3 Labour Availability

Labour Availability represents the quantity of eligible labour that can be allocated to an Industry.

The Population System supplies the available labour and relevant workforce characteristics.

The Economy System allocates labour subject to:

- Available workers.
- Required qualifications.
- Employment rules.
- Industry demand.
- Government restrictions.
- Wage and employment conditions.
- Other explicitly defined labour constraints.

The Economy System must not allocate the same worker or labour quantity to multiple incompatible full-time positions.

### 11.4 Employment

Employment records the labour allocated to an Industry.

Industry employment must identify the quantity and relevant type of workers employed.

Where an Industry cannot fill its Labour Demand, its employment remains below its required level.

An Industry's employment state is distinct from its physical production capacity.

A reduction in employment may constrain production through the Production System's labour interface. The precise physical effect depends on the relevant production process.

Workers released from an Industry return to the available labour pool according to the Population and Economy employment rules.

### 11.5 Labour Fulfilment

Labour Fulfilment measures the proportion of Labour Demand satisfied by actual employment.

When Labour Demand is greater than zero:

**Labour Fulfilment = Actual Employment / Labour Demand**

The result is bounded between 0 and 1.

When Labour Demand is zero, Labour Fulfilment is not calculated by dividing by zero. Its reported state must follow the explicit zero-demand convention defined in the Technical Specifications.

Labour Fulfilment is an economic measure of staffing, not a universal production multiplier. Its physical production consequences are defined by Production.

### 11.6 Wages

Each Industry defines the relevant wage categories and Base Gross Salary values for its employment requirements.

Base Gross Salary is the reference annual gross salary for a defined employment category.

Actual Gross Salary may differ from the reference value according to explicitly defined economic, institutional, labour-market and employment rules.

The wage model must define the inputs and calculation order rather than applying an unspecified Wage Modifier.

Where annual salary is used and the employment contract pays evenly across the simulation year:

**Daily Gross Salary = Annual Gross Salary / 365**

The conversion reflects the simulation calendar, which contains exactly 365 days.

The financial system must distinguish gross wages paid to workers from employer-side taxes, contributions or other employment costs. Any such costs must be defined by the relevant Government and Economy rules.

### 11.7 Wage Expenditure

Labour Expenditure represents the Company's financial expenditure on employee compensation over the relevant accounting period.

It must be calculated from actual employment, applicable compensation and the defined payment schedule.

For daily-paid compensation, the basic relationship is:

**Daily Gross Wage Expenditure = Sum of Daily Gross Salary for All Employed Workers**

Where employment categories contain multiple workers, the calculation must account for the quantity of workers in each category.

Additional employer costs must be included only where a defined rule applies.

### 11.8 Employment and Population Types

The Population System owns Population Types and their relevant characteristics.

The Economy System uses those characteristics to define or receive employment eligibility, workforce composition and economic relationships where required.

Industry employment may be represented by employment categories linked to Population Types.

The mapping between Population Types and Industry employment categories must be explicitly defined. The Economy System must not invent demographic or educational characteristics for a Population Type.

---

## 12. Revenue, Expenditure and Financial Performance

### 12.1 Revenue

Revenue is money received or recognised from completed economic transactions according to the applicable accounting rules.

Physical production, inventory ownership, market demand and potential sales do not automatically create revenue.

For a completed sale:

**Transaction Revenue = Quantity Sold × Transaction Price**

Where a transaction contains multiple products or transaction lines, each line must be accounted for using its own quantity and Transaction Price.

Daily Revenue is the sum of the revenue recognised from qualifying transactions during the relevant day.

The financial rules must define when revenue is recognised for cash transactions, credit transactions, contracts, Government procurement and other transaction types.

### 12.2 Expenditure

Expenditure represents a financial cost recognised by an economic entity according to the applicable accounting rules.

Initial expenditure categories include:

- Labour Expenditure.
- Input Expenditure.
- Operating Expenditure.
- Government Taxes and Charges.
- Financing Costs.
- Other explicitly defined expenditure categories.

Each category must have a defined source, accounting treatment and period.

### 12.3 Input Expenditure

Input Expenditure represents the cost of inputs acquired through economic transactions.

For a completed input purchase:

**Input Transaction Cost = Quantity Purchased × Transaction Price**

Input Expenditure is aggregated across qualifying input purchases within the accounting period.

Using an input already owned by the Company or Industry does not create a new purchase transaction. Any inventory-consumption cost recognised in financial accounts must follow the defined inventory valuation and accounting rules.

### 12.4 Operating Expenditure

Operating Expenditure represents the costs recognised from the ongoing operation of a Company or Industry.

It may include labour, purchased inputs, maintenance services, energy purchases, logistics, taxes and other defined operating costs.

The category must not automatically include every cash outflow. Capital investment, financing transactions, asset purchases, inventory acquisition and other financial movements must be classified according to explicit accounting rules.

### 12.5 Operating Income

Operating Income represents Revenue less the applicable Operating Expenditure for the same accounting period.

**Operating Income = Revenue − Operating Expenditure**

A positive result represents an operating surplus.

A result of zero represents operating break-even.

A negative result represents an operating loss.

Operating Income is not automatically equal to the change in Company Capital. The difference may include capital investment, financing, subsidies, transfers, asset transactions, inventory accounting and other defined financial movements.

### 12.6 Financial State

Company financial state includes Company Capital, recognised Revenue, Expenditure, Operating Income, financial obligations and other defined accounting values.

The financial model must distinguish:

- Cash or funds available for spending.
- Company Capital.
- Revenue.
- Operating Income.
- Physical inventory.
- Financial obligations.
- Capital investment.
- Subsidies and transfers.

These values may be related but must not be treated as interchangeable.

### 12.7 Financial Failure

A Company experiencing an operating loss does not automatically cease to exist.

Any transition to financial distress, insolvency, default, closure or restructuring must be governed by explicit financial rules.

The rules must identify the triggering conditions, calculation period, affected obligations, permitted interventions and resulting state changes.

Those rules are not implied merely by a negative Operating Income.

---

## 13. Capital, Investment and Subsidies

### 13.1 Capital Sources

Company Capital may change through defined financial transactions, including:

- Opening Company Capital allocation.
- Retained operating surpluses.
- Domestic investment.
- Foreign investment.
- Government subsidies.
- Authorised capital injections.
- Other explicitly defined capital transactions.

Each source must specify the source account, receiving account, amount, authority, effective date and financial treatment.

Capital must not be created without a defined source.

### 13.2 Capital Allocation

Company Capital is available for authorised commercial decisions.

The Senior Executive and Company AI may allocate capital among eligible activities, subject to Company rules, legal restrictions, available funds and any Player or Government directives.

The Economy System must validate that an expenditure or investment is permitted and financially supportable.

The same funds cannot be committed to incompatible expenditures simultaneously.

The rules governing reserved capital, commitments and unspent allocations must be defined in the financial model.

### 13.3 Government Subsidies

A Subsidy is a defined Government financial transfer to an eligible recipient.

The Government System owns subsidy authority, eligibility and public expenditure rules.

The Economy System records the recipient's financial effect and the relevant economic state.

A subsidy definition must identify:

- Issuing Government.
- Recipient.
- Amount or calculation basis.
- Purpose.
- Eligibility conditions.
- Start date.
- End date or termination condition.
- Payment schedule.
- Any attached obligations.
- Financial treatment.

A subsidy does not automatically increase Industry Scale, Production Capacity, Actual Production or Company profitability. Any such result must arise from the recipient's decisions and the relevant system mechanics.

### 13.4 Capital Injections

A Capital Injection is an explicitly authorised transfer of funds into a Company.

It must identify the source, recipient, amount, date and resulting financial treatment.

A capital injection from the Government must debit the appropriate public account and credit the Company according to the defined financial rules.

A Player-authorised capital injection must use the account and authority specified by the applicable gameplay and Government rules.

A capital injection does not automatically imply ownership changes unless the relevant transaction or legislation explicitly provides for them.

### 13.5 Foreign Investment

Foreign Investment occurs when an economic entity invests capital in an entity or operation located in another Nation.

Foreign investment must be resolved without currency conversion under the current single-Dollar model.

Its permissions and effects depend on:

- The investor.
- The recipient.
- The source and destination Nations.
- Applicable Government investment legislation.
- Diplomatic and investment agreements.
- Sanctions.
- Ownership and control restrictions.
- The terms of the investment.
- Applicable taxation and financial rules.

An investment must specify the rights it grants. Voting rights, control rights, profit distribution, capital recovery, withdrawal and ownership changes must not be inferred solely from the transfer of funds.

---

## 14. Trade and International Commerce

### 14.1 Trade

Trade is the transfer of a Good, Strategic Resource, Component, Armament or eligible Service between authorised economic entities.

A trade transaction must identify:

- Seller.
- Buyer.
- Product.
- Quantity and unit.
- Transaction Price.
- Origin.
- Destination.
- Applicable permissions.
- Delivery conditions.
- Payment conditions.
- Contractual obligations.
- Transaction status.

A transaction must satisfy the relevant commercial, legal, inventory and delivery rules.

### 14.2 Domestic Trade

Domestic Trade occurs between economic entities within the same Nation.

It is subject to applicable domestic laws, market rules, transaction requirements and Government restrictions.

Domestic Trade does not require an international trade permission unless another defined rule applies.

### 14.3 Imports and Exports

Imports and Exports represent eligible international transfers of products between Nations.

Imports and Exports may include Goods, Strategic Resources, Components, Armaments and Services where the relevant product and transaction rules permit international trade.

An international transaction must satisfy the applicable export, import, diplomatic, transport, payment and sanction rules.

A product being available in the Global Market does not automatically mean it can be imported by every Nation.

### 14.4 Trade Agreements

Trade Agreements define permissions and obligations governing economic relations between participating entities or Nations.

Depending on their explicit terms, they may govern:

- Import and export permissions.
- Tariffs or other trade charges.
- Product eligibility.
- Quotas.
- Investment permissions.
- Foreign Company operations.
- Subsidiary establishment.
- Strategic Resource access.
- Procurement.
- Service access.
- Other defined economic rights and restrictions.

The Diplomacy and Government Systems own the legal and diplomatic authority associated with Trade Agreements.

The Economy System applies the defined commercial consequences.

The terms of a Trade Agreement must not be inferred from its name or diplomatic relationship alone.

### 14.5 Subsidiaries and Foreign Operations

A foreign subsidiary is a commercial operation established in a Nation that is owned or controlled by, or otherwise legally linked to, a Company based in another Nation.

Foreign operations must operate subject to the host Nation's applicable laws and economic conditions.

The legal and economic relationship between the parent Company and foreign operation must be explicitly defined.

Relevant rules include:

- Establishment permissions.
- Capital allocation.
- Local Industry establishment.
- Local Company and Industry state.
- Employment.
- Taxation.
- Trade restrictions.
- Profit distribution.
- Transfers to the parent Company.
- Ownership and control.
- Applicable Trade Agreements.

A foreign subsidiary does not automatically receive unrestricted access to the host Nation's market, workforce, resources or infrastructure.

### 14.6 Trade Restrictions

Trade restrictions limit specified economic activities.

A restriction must identify:

- Issuing authority.
- Target entity or Nation.
- Affected products or product categories.
- Affected transactions.
- Geographic scope.
- Effective date.
- Duration or termination condition.
- Restriction type.
- Applicable exceptions.
- Resulting transaction rules.

The Economy System applies the restriction to eligible economic transactions.

The precise restriction categories and their effects must be defined by the Government and Diplomacy Systems. The Economy System must not invent a generic sanction severity modifier.

---

## 15. Economic Transactions and Accounting

### 15.1 Transaction Validation

Before an economic transaction is completed, the relevant rules must validate:

- Buyer and seller eligibility.
- Product identity.
- Quantity and unit.
- Available inventory.
- Product ownership or transfer authority.
- Transaction Price.
- Legal permissions.
- Trade restrictions.
- Payment conditions.
- Delivery conditions.
- Contractual obligations.

A transaction that fails a mandatory validation must not complete.

### 15.2 Transaction States

The transaction model must distinguish the relevant stages of a transaction, including:

- Proposed.
- Accepted.
- Contracted or ordered.
- Reserved.
- In transit, where applicable.
- Delivered.
- Paid or settled.
- Completed.
- Cancelled.
- Failed.

The precise state machine belongs in the Technical Specifications.

Not every transaction type must use every state, but the applicable state transitions must be explicitly defined.

### 15.3 Financial and Physical Settlement

Financial settlement and physical delivery are related but distinct.

A transaction may require a payment, a physical transfer or both.

The transaction rules must specify when:

- The seller's inventory is reduced.
- The buyer's inventory is increased.
- Funds are transferred.
- Revenue is recognised.
- Expenditure is recognised.
- Outstanding obligations are cleared.

The same transaction must not create duplicate inventory transfers, payments or revenue.

### 15.4 Revenue Recognition

Revenue is recognised according to the transaction's defined accounting rules.

The creation of a sales offer, purchase order or contract does not automatically constitute realised revenue.

Where revenue recognition depends on delivery, payment or another contractual condition, that condition must be explicit.

### 15.5 Transaction Auditability

Economic transactions must retain sufficient information to explain resulting financial and inventory changes.

The exact event schema belongs in the Technical Specifications.

At minimum, the system must be able to determine which transaction caused a change to an account, stockpile, product quantity, financial obligation or recorded revenue.

---

## 16. Daily Economic Processing

The Economy System processes economic state through an ordered sequence of operations.

The exact implementation schedule and processing architecture belong in the Technical Specifications, but the logical dependencies must remain consistent.

### 16.1 External State Collection

Receive the current authoritative state required from external systems, including:

- Population and available labour.
- Government laws, taxes, subsidies and restrictions.
- Research and technology availability.
- Military procurement demand.
- Production capacity and physical production state.
- Resource and inventory availability.
- Terrain and infrastructure conditions.
- Diplomatic permissions and trade restrictions.

Only state required by Economy should be imported. The Economy System must not take ownership of external state merely because it needs to read it.

### 16.2 Establishment and Eligibility Validation

Validate Industry establishment requests and other economic operations requiring prerequisites.

Confirm the relevant legal permissions, Treasury availability, Company Capital requirements, resource access and other defined Establishment Requirements.

### 16.3 Resource and Inventory Availability

Receive authoritative physical inventory and resource availability.

Identify quantities that are available, reserved, committed, in transit, restricted or unavailable for economic allocation.

Prevent duplicate availability across competing transactions and production processes.

### 16.4 Labour Availability

Receive available labour and workforce characteristics from Population.

Determine eligible labour quantities and the workforce information required for Industry allocation.

### 16.5 Industry Economic Requirements

Calculate or update the economic requirements of each Industry, including:

- Industry Scale.
- Labour Demand.
- Intended Production priorities.
- Input demand.
- Capital availability.
- Commercial commitments.
- Relevant operating expenditure requirements.

Physical Production Capacity remains owned by Production.

### 16.6 Input Procurement and Allocation

Resolve input procurement and allocation decisions using available stockpiles, supplier offers, transaction rules and Production-derived input requirements.

Create valid economic orders or allocations without treating undelivered goods as physically available.

### 16.7 Production Interface

Send the relevant economic priorities and input arrangements to Production.

Receive the resulting physical production state and Actual Production.

Do not calculate physical output within the Economy System.

### 16.8 Inventory Reconciliation

Record qualifying inventory changes resulting from production, consumption, destruction, delivery, sale and transfer.

Maintain a consistent authoritative inventory state.

### 16.9 Demand Aggregation

Receive and aggregate Population, Industry, Government, Military and other explicitly defined demand.

Avoid duplicate demand across domestic and international aggregation.

### 16.10 Market Calculation

Calculate eligible Global Market and Domestic Market state using defined supply, demand, stockpile availability and market rules.

Update Global Price and Domestic Market Price according to their specified calculations.

### 16.11 Company Pricing Decisions

Company AI determines Company Selling Prices and other commercial decisions within applicable legal and market constraints.

Pricing decisions must use the current state made available by the processing schedule.

### 16.12 Transaction Resolution

Resolve eligible domestic and international transactions according to availability, permissions, pricing, contracts, allocation and delivery rules.

Validate that each transaction can be completed without duplicate or negative inventory. Transaction state must distinguish order, reservation, dispatch, delivery and payment. An order or dispatch does not establish delivery or payment. Inventory movements are committed by the Inventory System, and a single physical quantity must not be deducted twice because a sale is also represented as a transfer.

### 16.13 Financial Accounting

Record qualifying Revenue, Expenditure, transfers, payments, taxes, financing costs and other financial movements.

Update Company financial state according to the accounting rules.

### 16.14 Company Financial and Operating State

Update Company Capital and other financial values using the completed transactions and explicitly defined financial rules.

Calculate Operating Income for the relevant accounting period.

### 16.15 Company AI Decisions

Company AI evaluates the updated state and determines subsequent commercial decisions.

Decisions may affect future Industry Scale, pricing, capital allocation, procurement, trade participation and other eligible economic actions.

A decision must not retroactively change a transaction that has already completed unless an explicit correction or reversal mechanic permits it.

### 16.16 Cross-System Outputs

Publish the resulting economic state required by other systems.

Potential outputs include:

- Updated Company Capital.
- Revenue and expenditure records.
- Industry Scale.
- Employment and labour demand.
- Economic product demand.
- Purchase orders.
- Transaction results.
- Stockpile changes.
- Market prices.
- Government financial effects.
- Military procurement fulfilment.
- Trade and investment results.
- Economic conditions relevant to Population and other systems.

Each output must identify its authoritative source, effective period and intended receiving system.

---

## 17. Economic State Persistence

Economic state persists between simulation updates unless an explicit event or rule changes it.

Persistent Economy state includes:

- Company identity and Company-to-Industry relationships.
- Senior Executive association.
- Company Capital.
- Industry Scale.
- Industry establishment state.
- Economic financial records required by the simulation.
- Company pricing and commercial decisions.
- Employment allocations.
- Trade contracts and agreements represented by Economy.
- Investment relationships.
- Foreign commercial operations.
- Economic transaction state.
- Product prices and relevant market state.
- Stockpile ownership and economic reservations, where owned by Economy.
- Other explicitly defined economic relationships.

Physical Production Capacity, actual production, facility condition, physical stockpile quantities and resource deposits remain authoritative in their respective systems.

Persistence does not imply immutability. State changes only through defined simulation events, calculations or authorised actions.

---

## 18. Validation and Invariants

The Economy System must enforce the following invariants.

### 18.1 Financial Invariants

- Nation Treasury and Company Capital are separate accounts.
- Every financial transfer has a defined source and destination, except for explicitly defined creation or destruction of money by a future monetary mechanic.
- Industry Establishment Capital Requirement and Opening Company Capital are separate financial movements.
- A financial expenditure cannot be completed if the relevant account lacks the required available funds, unless an explicit credit or deficit mechanic permits it.
- Company Capital does not automatically depreciate.
- Revenue is not created solely by production or inventory accumulation.
- Operating Income and Company Capital are distinct values.

### 18.2 Inventory Invariants

- Physical inventory quantities cannot be negative.
- The same physical quantity cannot be allocated to incompatible uses simultaneously.
- A transaction cannot transfer a quantity the seller does not validly own or control.
- Inventory movements must reconcile with the authoritative Inventory System records.
- Economy and Production must not maintain competing authoritative physical inventories.
- Reservations do not remove physical stock, but reduce the quantity available for other uses.
- A purchase order or dispatch does not make stock available before delivery is recorded.
- A sale and a transfer must not deduct the same physical quantity twice.
- Global Supply must not count the same physical quantity more than once.
- Production must not consume unavailable inputs unless an explicit production rule permits the relevant alternative.

### 18.3 Industry Invariants

- Every Industry has a defined Industry Type.
- Every Industry Type has an explicit Establishment Requirements definition.
- Industry establishment cannot succeed while a mandatory requirement remains unsatisfied.
- Industry Establishment Capital Requirement is not the same as Opening Company Capital.
- Industry Scale is not identical to Production Capacity.
- Production Capacity and Actual Production are owned by Production.
- An Industry may exist without currently producing.
- Physical damage, financial loss and operational suspension remain distinct states.

### 18.4 Product and Market Invariants

- Every economic product has a defined identity and unit.
- Every Good belongs to exactly one of the three mutually exclusive Good Categories.
- Base Price, Global Price, Domestic Market Price, Company Selling Price and Transaction Price are distinct values.
- A reference price does not guarantee availability.
- Demand does not guarantee fulfilment.
- An accepted order does not automatically mean delivery has occurred.
- An offer does not automatically mean a sale has occurred.
- International trade must satisfy applicable permissions and restrictions.

### 18.5 Cross-System Invariants

- The Economy System does not independently create population, technology, physical production capacity or military capability.
- External state remains authoritative in its owning system.
- Cross-system effects must be transmitted through defined interfaces.
- A mechanic must not be implemented as an undocumented generic modifier.
- Each derived value must identify its source variables, units, valid range, calculation order and update frequency in the relevant specification.

---
