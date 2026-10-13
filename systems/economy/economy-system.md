
# Economy System

## 1. System Definition

The Economy System simulates the financial and commercial activity of Nations, Companies, Industries, Markets and economic participants.

It determines how economic entities acquire and allocate capital, employ labour, establish Industries, purchase inputs, price products, conduct transactions, generate revenue, incur expenditure, accumulate wealth and participate in domestic and international trade.

The Economy System operates alongside the Production, Industry Data, Inventory, Government, Research, Population, Military, Terrain, Logistics and Diplomacy Systems.

The Economy System determines the economic conditions under which activity takes place. It does not independently simulate every physical process involved in producing, storing, moving or consuming a commodity.

### 1.1 Core Responsibilities

The Economy System is responsible for:

1. Company financial state, including Company Capital, revenue, expenditure, operating income and financial obligations.
2. Economic and commercial decisions made by Companies and Industries.
3. Industry Scale and its economic consequences.
4. Economic rules governing Industry establishment, in coordination with Industry Data and Government.
5. Establishment expenditure and Opening Company Capital movements.
6. Product Base Prices and economic price calculations.
7. Global and domestic market pricing.
8. Economic demand and supply aggregation.
9. Company pricing and commercial decisions.
10. Economic employment allocation, labour demand and wage expenditure.
11. Economic transactions, contracts, payments and financial settlement.
12. Economic ownership and commercial rights as recorded through authorised transactions.
13. Economic availability and valuation of physical goods recorded by the Inventory System.
14. Domestic and international trade.
15. Foreign investment and foreign commercial operations.
16. Economic effects of Government policy, taxes, subsidies and financial transfers.
17. Economic effects of sanctions and trade restrictions.
18. Commercial procurement and economic allocation decisions.
19. Financial and commercial interfaces with Production, Industry Data, Inventory, Government, Research, Population, Military, Terrain, Logistics and Diplomacy.

### 1.2 Explicit Non-Responsibilities

| Mechanic | Authoritative System |
|---|---|
| Canonical product identities, classifications, shared attributes and units | Good and Service Catalogue |
| Industry Types, Industry instances, establishment configuration and Industry data | Industry Data |
| Physical extraction, manufacturing, construction and production execution | Production |
| Physical production capacity and actual output | Production |
| Production processes, recipes, input/output ratios and yields | Production Processes and Recipes |
| Physical inventory quantities, lots, condition, reservations and stock movements | Inventory |
| Physical transportation, routes, cargo movement and delivery in transit | Logistics and Transportation |
| Geographic terrain, map geometry and elevation | Terrain |
| Population demographics and household composition | Population |
| Educational attainment and population-level workforce characteristics | Population |
| Education policy, public education funding and Government budgets | Government |
| Scientific research, designs and technology development | Research |
| Laws, taxation authority, public budgets and Government spending permissions | Government |
| Military organisation, operational requirements, equipment design and combat | Military, with Research and Production interfaces |
| Diplomatic relations and treaty permissions | Diplomacy |
| Political institutions, political groups and political authority | Government |

These assignments identify the authoritative owner of each kind of state. Other systems may supply inputs, request authorised changes or receive results through explicit interfaces.

The Economy System may store references to external entities and retain financial records concerning them. It must not become a second authoritative owner of their physical or institutional state.

---

## 2. Foundational Economic Rules

### 2.1 Simulation Start

The default starting date is 1 January 1955.

The initial economic model uses reference prices, wages and financial values calibrated to the historical Western economy of the 1950s. These values provide a common economic reference for the fictional world. They do not imply that the fictional world uses the historical United States monetary or political system.

Starting values for individual products, Industries, Companies and Nations must be defined in the relevant reference or scenario data.

Scenario-loaded economic entities and their starting financial state are initial conditions, not new establishment or investment events. Loading a starting Company or Industry must not automatically charge it establishment expenditure again.

### 2.2 Calendar and Time

Project Sovereign uses a Gregorian-style calendar without leap years.

Every simulation year contains exactly 365 days.

The world clock uses a 24-hour format. The calendar begins on 1 January and advances through 365 days per simulation year.

Economic mechanics may update daily or at another explicitly defined frequency. The Economy System must not assume that all financial transactions, market prices or Company decisions update at the same frequency.

Each economic mechanic must define its own processing frequency and when its updated state becomes available to other systems.

The global ordering and coordination of system updates belong in the shared simulation scheduler specification. This document defines economic dependencies, not an independent competing global schedule.

### 2.3 Currency

The canonical currency is the Dollar ($), with canonical identifier `DOLLAR`.

The Dollar is a fictional global currency used throughout Project Sovereign. It is not the United States Dollar and is not controlled by a real-world monetary authority.

All monetary values use Dollars unless an explicitly approved future design introduces another denomination.

There is no foreign exchange system in the current design.

Transactions between Nations do not require currency conversion. Differences in economic conditions are represented through prices, wages, taxes, trade restrictions, investment rules and other defined economic mechanics.

### 2.4 Reference Prices and Starting Values

Economic reference prices and wages are calibrated to a 1955 baseline.

A reference value does not automatically determine the price of an actual transaction. Market prices and transaction prices may diverge from reference values through explicitly defined economic rules.

The Economy System distinguishes the following price concepts:

- **Base Price:** the reference monetary value assigned to a canonical product unit.
- **Global Price:** the current global market reference price.
- **Domestic Market Price:** the reference price within a defined domestic market.
- **Company Selling Price:** the price at which a Company offers a product for sale.
- **Transaction Price:** the price agreed for a particular transaction.

These values are not interchangeable. Their source data, calculation rules, scope and update frequency must be defined explicitly.

### 2.5 No Automatic Financial or Physical Effects

A financial transaction does not automatically create, deliver or consume a physical commodity.

Physical production does not automatically generate revenue.

A financial loss does not automatically destroy a Company.

An increase in Industry Scale does not automatically increase physical output.

A purchase order does not prove that goods have been delivered. A dispatch does not prove that a shipment has arrived. A delivery does not prove that payment has been made.

Each state change must be committed by the system that owns the affected state through the relevant interface.

### 2.6 Units and Product Identity

The Good and Service Catalogue owns canonical product identities, product classifications, shared attributes, measurement units and defined unit conversions.

The Economy System references catalogue identifiers and compatible units. It must not create competing product identities or redefine canonical units.

Consumer consumption requirements may use a defined Consumption Interval, such as one standard individual requirement per day. A consumption unit is not necessarily the same as the physical unit used for production, storage, transport or trade.

Conversions between consumption requirements and physical quantities must be explicitly defined. They must not be assumed merely because two quantities refer to the same product.

---

## 3. System Ownership and Interfaces

### 3.1 Economy and Industry Data

Industry Data owns the authoritative definitions and configuration for Industry Types and Industry instances, including establishment prerequisites, Company relationships where specified, Industry Areas, workforce requirements and scenario starting data.

The Economy System owns the resulting financial movements and economic state, including Company Capital, Industry Scale, operating expenditure and financial performance.

For establishment:

1. Industry Data supplies the applicable Industry definition, establishment configuration and required entity relationships.
2. Government supplies the relevant authority, permissions and Treasury availability.
3. Economy validates and commits the required financial movements and economic state.
4. Production establishes or updates physical production entities only through its own authorised interface.
5. Inventory records physical stock only when an authorised Inventory transaction creates, receives or otherwise changes stock.

The establishment operation must be atomic across the required participating systems. If any mandatory step fails, no partial Industry, Company or financial state may remain.

Industry Data does not independently debit Treasury or Company Capital. Economy does not independently redefine Industry Types or establishment prerequisites.

### 3.2 Economy and Production

The Economy System owns the economic and commercial state associated with an Industry.

The Production System owns physical production execution.

The Economy System determines commercial priorities and economic requirements, including:

- Company Capital available for authorised spending.
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
- Eligible production facilities and lines.
- Actual physical output.
- Physical input requirements derived from selected recipes.
- Physical input consumption through authorised Inventory operations.
- Resource extraction.
- Manufacturing and construction execution.
- Physical production constraints.
- Facility condition and operational availability.
- Physical output and production losses.

Production receives economic priorities and relevant constraints, determines what is physically feasible, and reports the result. Economy must not independently calculate or override physical capacity or actual output.

Production must not independently calculate Company revenue, Company Capital or operating income.

### 3.3 Economy and Production Processes and Recipes

Production Processes and Recipes owns the definitions of physical transformations, including recipe inputs, outputs, quantities, yields, by-products and process losses.

Economy uses recipe-derived requirements to estimate economic input demand, procurement costs and potential output economics.

Economy must not independently define production ratios, substitute ingredients, process yields or physical production outcomes.

A recipe defines a transformation. It does not reserve, consume, transfer or create inventory. Production orchestrates execution, and Inventory commits the authorised physical stock changes.

Executed production records must retain the exact recipe version used so that historical outcomes remain reproducible.

### 3.4 Economy and Inventory

Inventory is the sole authoritative owner of physical inventory quantities, locations, condition, lots, reservations, transfers and stock-changing transactions.

Economy owns financial records, commercial orders, transaction prices, payment obligations and financial settlement.

Economy may request that Inventory reserve, transfer, receive or otherwise change stock through authorised interfaces. Inventory validates and commits the physical change. Economy must not directly modify physical inventory quantities or maintain a competing authoritative stock ledger.

Economic ownership and physical custody are separate concepts. An economic transaction may change ownership without changing physical location, while a physical shipment may change location or custody without immediately changing ownership.

A reservation keeps stock physically present but unavailable for incompatible uses. An order or expected delivery is not available inventory until the relevant Inventory and delivery conditions are satisfied.

### 3.5 Economy and Government

Government owns public authority, laws, tax policy, budget authority, Treasury state and Government expenditure permissions.

Economy applies the financial consequences of authorised Government rules.

Government subsidies, taxes, public procurement and establishment payments must use explicit financial interfaces. Economy must not create Government authority or change Treasury state outside an authorised operation.

Nation Treasury and Company Capital remain separate accounts.

### 3.6 Economy and Population

Population owns demographics, household composition, workforce characteristics, educational attainment and the population-level supply of labour.

Economy receives the relevant population and labour data, allocates employment and records wages and associated economic state.

Economy must not create population or independently change educational attainment.

### 3.7 Economy and Research

Research owns technology development, scientific knowledge, design research and technology prerequisites.

Economy may use research-derived information to assess eligible commercial opportunities, input demand and investment decisions. It does not discover technologies or make a technology available independently.

Production determines whether available technology can be realised as physical capability under its facilities, equipment and process rules.

### 3.8 Economy and Military

Military owns military requirements, procurement priorities, operational use and military acceptance of equipment.

Economy resolves commercial demand, procurement prices, financial commitments and relevant transaction state.

Production determines whether the required physical items can be manufactured or assembled. Inventory records the physical stock and its authorised movement.

A military requirement does not prove that the required items have been produced, purchased, delivered or accepted.

### 3.9 Economy and Diplomacy

Diplomacy owns diplomatic relations, treaty permissions and the diplomatic authority associated with international agreements.

Government owns domestic law and the relevant legal authority for sanctions and restrictions. The applicable system supplies each restriction through an explicit interface.

Economy applies these permissions and restrictions to commercial decisions and transactions. It must not invent diplomatic permissions or undocumented sanction effects.

### 3.10 Cross-System Ownership Table

| Data or mechanic | Authoritative owner | Economy responsibility |
|---|---|---|
| Company Capital | Economy | Calculate and persist |
| Company revenue and expenditure | Economy | Calculate and persist |
| Industry Scale | Economy | Calculate and persist |
| Industry Type definitions | Industry Data | Reference and apply |
| Industry instance configuration | Industry Data | Use in economic operations |
| Industry establishment prerequisites | Industry Data, with Government authority | Validate and execute economic effects |
| Nation Treasury | Government | Request or record authorised transfers |
| Physical Production Capacity | Production | Receive and use as a constraint |
| Actual Production | Production | Record economic consequences |
| Production recipe and yield | Production Processes and Recipes | Derive economic requirements |
| Physical stock quantity and condition | Inventory | Request authorised stock operations |
| Inventory valuation | Economy | Calculate financial value under defined accounting rules |
| Strategic Resource deposit and extraction | Terrain and Production, according to the defined resource interface | Use reported availability and extraction outcomes |
| Population Labour Force | Population | Receive available labour |
| Industry Employment | Economy | Allocate employment and report changes |
| Educational attainment | Population | Receive workforce qualification information |
| Education policy and public funding | Government | Apply relevant economic consequences |
| Scientific research and technology availability | Research | Use eligible capability information |
| Government taxes and subsidies | Government authority, Economy accounting | Apply and record authorised effects |
| Military procurement demand | Military | Resolve commercial demand and financial transactions |
| Diplomatic trade permissions | Diplomacy | Apply permissions to transactions |
| Industry Area geometry | Terrain | Reference geographic association |
| Physical facility damage | Production, with event sources such as Military | Apply reported economic consequences |
| Cargo movement and delivery in transit | Logistics and Transportation | Use confirmed delivery state |

Where several systems participate in a mechanic, one system must own each authoritative state. Other systems interact through defined interfaces.

### 3.11 Undefined External Mechanics

When an Economy mechanic depends on a system that has not yet been fully specified, this document identifies the dependency without inventing the external system's rules.

For example, Economy may require an Industry to have access to a sufficiently qualified workforce. Population determines workforce characteristics and availability. Economy determines how Industry labour requirements are matched against that available workforce.

The precise external algorithm belongs in the specification for the system that owns it.

---

## 4. Economic Entities

### 4.1 Nation

A Nation is a political and economic jurisdiction represented in the world simulation.

The Government owns the Nation Treasury and public financial decisions. Economy simulates commercial activity and the economic consequences of Government actions.

A Nation may contain Companies, Industries, Population Centres, physical production facilities, inventory holdings and economic infrastructure.

A Nation's economic state is an aggregation of relevant activity and financial relationships. Aggregation rules must prevent the same financial value or physical quantity from being counted multiple times.

### 4.2 Company

A Company is an economic organisation responsible for commercial activity and the operation of one or more Industries.

A Company may purchase inputs, sell outputs, employ workers, receive investment, incur expenditure and accumulate Company Capital.

Company state may include:

- Company identity.
- Company name and logo.
- Senior Executive association.
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
- References to inventory holdings owned or controlled by the Company.

Company state must not duplicate physical inventory quantities owned by Inventory.

The Senior Executive is the default owner, controller and chief executive of the Company under the current design. This default is subject to an explicit authorised legal mechanism, such as nationalisation or a transfer of control.

The model does not imply a complete equity or shareholder system. Legal ownership, control and the operational executive role must remain distinct concepts even where the initial rules assign them to the same Character.

A Company is not identical to an Industry. A Company is an economic organisation. An Industry represents an economic activity and operating entity defined through Industry Data and associated with geographic and physical production entities.

A Company may operate multiple Industries where the relevant legal and Industry Data rules permit it.

### 4.3 Senior Executive

Each Company has an associated Senior Executive Character who directs the Company's commercial decisions by default.

The Senior Executive belongs to the Character System and has Statistics, Traits, Interests and Political Affiliation as defined by that system.

Economy uses the Senior Executive's characteristics to inform Company AI decision-making.

Relevant decisions may include:

- Industry expansion or contraction.
- Investment allocation.
- Production priorities.
- Pricing strategy.
- Capital preservation or expenditure.
- Labour and wage decisions.
- Inventory procurement and commercial stockpile policy.
- Trade participation.
- Response to economic restrictions.
- Negotiation and commercial risk-taking.

Character Statistics and Traits must have separately defined effects.

A Character Statistic informs decision-making according to the Character System and Company AI rules. A Trait may have a direct mechanical effect on an economic value only where its definition explicitly specifies that effect.

For example, a future Character Trait may provide a defined 15% increase to a particular production-related value. The affected value, calculation order, scope and stacking rules must be defined by the Character System and the relevant receiving system. Economy must not apply an undefined general-purpose Executive Modifier.

The detailed Character System remains outside the scope of this document.

### 4.4 Company AI

Company AI makes commercial decisions on behalf of the Senior Executive and Company.

Its decisions are constrained by:

- Applicable Government laws and regulations.
- Available Company Capital.
- Industry Scale and establishment state.
- Available labour.
- Required inputs and their confirmed availability.
- Physical production capability.
- Market conditions.
- Trade permissions.
- Sanctions and restrictions.
- Company obligations.
- Senior Executive Statistics, Traits and Interests.
- Player-issued directives, where permitted.

Company AI cannot override physical production constraints, Government authority, inventory availability or transaction validity rules.

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
- Establishing economic legislation through the appropriate authority.
- Nationalising or transferring control of Companies through authorised mechanisms.
- Establishing price controls through the appropriate authority.
- Permitting or restricting trade through authorised rules.
- Directing strategic investment.

The Player does not automatically possess direct operational control over every Company.

Available actions depend on Government authority, laws, ownership, Company control and the relevant distribution of economic power.

---

## 5. Government Finance and Industry Establishment

### 5.1 Nation Treasury

The Nation Treasury is the authoritative account of public funds held by the Government.

It is owned by the Government System.

Treasury funds are distinct from Company Capital and must not be combined into a single account.

Government revenue and expenditure, including taxation, public spending, subsidies and Industry establishment payments, are governed by Government rules and recorded through explicit financial interfaces with Economy.

### 5.2 Company Capital

Company Capital is the Company's spendable financial balance from which authorised purchases, wages and other payments are made.

Company Capital is distinct from:

- Nation Treasury.
- Company revenue.
- Company Operating Income.
- Physical inventory.
- The financial value of unsold stock.
- Industry Establishment Capital Requirement.
- Unsettled financial obligations.

Company Capital changes through explicitly defined financial events, including opening capital allocation, operating receipts and payments, investment, subsidies, capital injections, financing transactions and distributions where those mechanics are implemented.

The same funds must not be represented as a second Company cash balance.

Company Capital does not automatically depreciate. Any reduction must arise from a defined financial transaction or rule.

### 5.3 Industry Establishment

Industry Establishment is the process by which an Industry is authorised, funded and created within the simulation.

Under the default establishment model, the Government authorises establishment and funds the Industry Establishment Capital Requirement from the Nation Treasury. Any alternative establishment authority or funding model must be explicitly defined.

Industry Data provides the applicable Industry definition, establishment requirements and configuration. Economy validates and records the required financial movements. The participating systems create their respective state through an atomic establishment operation.

Establishing an Industry does not automatically guarantee that it can operate at full capacity. Operation remains subject to labour, inputs, technology, infrastructure, physical production capability, legal restrictions and other applicable requirements.

### 5.4 Industry Establishment Capital Requirement

Each Industry Type defines an Industry Establishment Capital Requirement expressed in Dollars.

This is the required establishment expenditure under the applicable Industry establishment rules.

It is distinct from:

- Opening Company Capital.
- Ongoing operating expenditure.
- Capital required to expand Industry Scale.
- Capital required to construct additional physical facilities.
- Capital required to acquire inputs or stockpile outputs.

The Industry Establishment Capital Requirement is defined in the relevant Industry Data. Economy must not maintain a conflicting copy as an independent source of truth.

### 5.5 Opening Company Capital

When an Industry and its associated Company are established, the Company receives an Opening Company Capital allocation if required by the applicable establishment rules.

This allocation is separate from the Industry Establishment Capital Requirement.

Where the Government funds both, the establishment outlay consists of distinct, auditable financial movements:

1. The Industry Establishment Capital Requirement is paid from the Nation Treasury to the explicitly defined recipient or expenditure account.
2. Opening Company Capital is transferred from the Nation Treasury to the Company's Company Capital account.

The amount of Opening Company Capital must be defined by the establishment configuration. It must not be assumed to equal the establishment cost.

The establishment payment must not be implicitly converted into facility value, production capacity or Company Capital.

Each financial movement must identify its source, destination, amount, purpose and effective date.

### 5.6 Industry Establishment Requirements

Every Industry Type must have an explicit Establishment Requirements definition in Industry Data.

The definition identifies prerequisites that must be satisfied before the Industry can be established.

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

Not every Industry requires every category of prerequisite.

Each requirement must identify how it is validated and which system supplies the authoritative state.

The existence of a Strategic Resource is not a universal prerequisite for establishing an Industry unless the relevant Industry Data explicitly requires it.

### 5.7 Establishment Validation and Atomicity

An Industry may be established only when all mandatory Establishment Requirements are satisfied and all required funding movements can be completed.

If any mandatory requirement fails, or the Nation Treasury cannot fund every required payment, establishment is rejected.

A rejected establishment attempt must not partially create an Industry or Company, deduct Treasury funds, transfer Opening Company Capital or create partial physical facilities.

If establishment succeeds, validation, entity creation and required financial movements must be committed as one atomic operation across the participating systems.

If any required part fails, the entire operation fails and no partial state change remains.

Existing Industries and Companies loaded from scenario starting data are initial state. They do not constitute new establishment events and must not incur retroactive establishment charges.

The exact transaction protocol, rollback and save-state recovery mechanism belongs in the Technical Specifications.

### 5.8 Nationalisation and Transfer of Control

Government nationalisation or another legally authorised transfer may change control of a Company.

Such an action does not automatically determine the ownership, valuation or destination of every asset and liability associated with the Company. Those consequences must be defined by the applicable legislation and transaction rules.

The default Senior Executive ownership and control model remains in effect unless an authorised mechanism changes it.


---

## 6. Industries

### 6.1 Industry Definition

An Industry represents a defined category of economic activity configured through Industry Data and associated with a Company and relevant geographic and physical production entities.

An Industry has economic state, commercial requirements and defined relationships with other simulation systems.

Industry Scale determines the intended economic magnitude at which the Industry operates. It is not itself a measure of physical output.

Industry Scale may influence the Industry's economic requirements and intended activity, including:

- Labour demand.
- Capital requirements.
- Input demand.
- Intended production level.
- Potential output value.
- Potential revenue.
- Potential operating expenditure.
- Required infrastructure and operating resources, where applicable.

The relationship between Industry Scale and physical production capability must be defined through the Economy and Production interface.

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
| 16 | Agriculture | Primary agricultural production and extraction of agricultural outputs associated with an agricultural Industry Area |

These Industry Types define economic specialisation, not a restriction that each Industry can produce only one Good.

The precise outputs and input recipes available to each Industry must be defined by the Catalogue and Production Processes and Recipes. An Industry Type association alone does not prove that a particular output is currently producible.

### 6.3 Agriculture and Agribusiness

Agriculture represents primary agricultural activity. It operates within a geographic Industry Area and produces outputs defined by the relevant production processes.

Agribusiness represents the processing, packaging, storage and commercial distribution of agricultural outputs.

Agriculture and Agribusiness are distinct Industry Types. Agricultural outputs do not automatically become processed or packaged consumer Goods.

Agriculture may produce agricultural outputs and extract Rich Soils where the relevant Production rules permit it. Rich Soils are an explicit simulation abstraction, not a claim that fertile soil is ordinarily extracted as a commodity in the same manner as a mineral deposit.

### 6.4 Industry Scale

Industry Scale represents the intended economic scale of an Industry.

Industry Scale is not interchangeable with:

- Physical Production Capacity.
- Actual Production.
- Number of employees.
- Company Capital.
- Number of facilities.
- Geographic Area size.

Industry Scale must be calculated from explicitly defined economic rules and authoritative state.

Relevant inputs may include:

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

Industry Scale does not grant Production additional facilities, capacity or inputs. Any resulting physical expansion must be implemented by Production through the appropriate physical changes.

### 6.5 Industry Geographic Representation

Each Industry may be associated with a geographic Industry Area represented using the Terrain System's Area Mapping Primitive.

Industry Data owns the Industry's configuration and reference to its Industry Area. Terrain owns the geographic geometry and spatial state.

The Industry Area is a geographic association, not a measure of Industry Scale or physical production capacity.

Its size and shape must be defined by the relevant geographic and establishment rules.

For Agriculture, the Industry Area represents the geographic area used for agricultural activity.

An Industry Area may overlap with other geographic and terrain objects where the map rules permit it.

The Industry Area may be rendered using an Industry-specific visual representation. Rendering does not determine simulation priority or physical effects.

### 6.6 Industry Area and Operational State

The Industry Area, physical facilities, economic Industry and production processes are separate but connected entities. Their state must not be collapsed into a single Industry Health value.

| State | Meaning | Authoritative owner |
|---|---|---|
| Geographic footprint | Area occupied or associated with the Industry | Terrain geometry, referenced by Industry Data |
| Area integrity | Condition of the geographic Industry Area as a simulation object | Terrain, with authorised event interfaces |
| Facility condition | Physical condition of production facilities | Production |
| Operational status | Whether a facility or process can operate under current conditions | Production |
| Industry Scale | Intended economic scale | Economy |
| Physical Production Capacity | Maximum physical output under current operating conditions | Production |
| Actual Production | Quantity physically produced | Production |
| Workforce allocation | Workers allocated to the Industry | Economy, using Population inputs |
| Physical stock quantity | Quantity of a product held at a location | Inventory |
| Company financial state | Capital, revenue, expenditure and operating income | Economy |
| Infrastructure connections | Connections to transport, energy, logistics and other networks | Relevant infrastructure-owning system |

### 6.7 Damage and Disruption

Military action, bombardment, sabotage, accidents and other defined events may affect an Industry Area or its associated facilities.

Each event must identify the affected object and the state change it causes.

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

Each effect must be resolved by the system that owns the affected state.

Damage to an Industry Area does not automatically imply an equal reduction in Production Capacity. Physical capacity is determined by the condition and availability of the relevant facilities and processes.

Repairing an Industry Area does not automatically restore destroyed equipment, stockpiles, workforce or Company Capital.

### 6.8 Industry Expansion and Contraction

An Industry may expand or contract when its economic conditions, Government permissions, Company decisions or Player directives change.

Expansion may require additional Company Capital, labour, inputs, facilities, technology or infrastructure.

Contraction may reduce Industry Scale, labour demand or intended activity.

Economy determines Industry Scale and its economic requirements. Production determines the physical changes required to realise a change in production capability.

Industry Scale must not increase beyond what the defined economic rules permit. An increase in Industry Scale must not be treated as proof that physical expansion has been completed.

### 6.9 Industry Persistence

Industry identity, Industry Type, Company association, Industry Scale, geographic association and financial relationships persist unless changed by an explicit event or rule.

Physical capacity, facility condition, operational status, stock quantities and actual production persist according to their respective authoritative systems.

An Industry's economic existence does not imply that it is currently producing.

---

## 7. Economic Products

### 7.1 Product Definition

An Economic Product is a canonical product or service that can participate in economic activity.

The Good and Service Catalogue defines product identity, classification, canonical units and shared descriptive attributes. Economy uses those definitions to represent demand, supply, prices, transactions, procurement and commercial output.

Product categories should remain manageable and meaningful to the Player. Individual recipes, military designs and specific product configurations should be represented through more detailed definitions where required, rather than by creating excessive generic economic categories.

The product taxonomy must support both the consumer economy of a 1950s industrial society and the detailed research, component and armament design systems.

### 7.2 Product Classification

Every Economic Product must have exactly one primary economic class:

1. Good.
2. Service.
3. Designed Component.
4. Armament.

The Catalogue must represent primary economic class separately from physical form. The class determines the product's economic and simulation treatment; physical form determines whether it represents a physical product or a non-physical service.

For the initial taxonomy:

- **Good:** a physical product that does not require the more specific Designed Component or Armament class.
- **Service:** a non-physical economic activity or deliverable.
- **Designed Component:** a physical component defined for use in a larger product, system or assembly.
- **Armament:** a physical military item or system defined for independent military use, deployment, operation or installation.

These classes are mutually exclusive. A product must not be assigned multiple primary economic classes to represent multiple possible uses.

Strategic Resource is an additional designation for an eligible physical product. It is not a primary economic class and does not replace the product's existing class.

### 7.3 Physical Inventory Eligibility

Physical products may be recorded as Inventory holdings. Services do not automatically create physical inventory.

A Service may have associated physical inputs, facilities, equipment or deliverables, but those items retain their own Catalogue identities and inventory rules.

The Catalogue identifies whether a product is physical or non-physical. Inventory determines the authoritative physical stock state for eligible physical products.

Economy may record the commercial value of a Service or physical product without assuming that the underlying service activity or physical stock exists.

### 7.4 Good Categories

Every product whose primary economic class is Good belongs to exactly one of the following three mutually exclusive Good Categories:

1. Staple.
2. Luxury.
3. Industrial.

These categories describe the primary economic role of Goods. They do not replace primary economic class and do not apply automatically to Services, Designed Components or Armaments.

A Good cannot simultaneously be a Staple Good and a Luxury Good. If a broad product family contains both basic and premium products, those products must have distinct Catalogue identities or variants with an explicitly defined classification rule.

#### 7.4.1 Staple Goods

Staple Goods are everyday goods associated with ordinary household consumption and routine material needs.

Initial examples include:

- Grocery Staples.
- Basic Clothing.
- Household Cleaning Products.
- Personal Hygiene Products.
- Basic Household Supplies.
- Basic Furniture and Household Essentials.

Staple Goods represent categories of recurring household demand rather than every individual product model.

Demand is primarily associated with population, household structure, consumption requirements, purchasing power and availability.

Population defines relevant population characteristics. Economy resolves the associated economic demand.

#### 7.4.2 Luxury Goods

Luxury Goods are discretionary or premium products whose economic role is distinguished from ordinary staple consumption.

Initial examples include:

- Luxury Clothing and Accessories.
- Jewellery and Watches.
- Cosmetics and Perfumes.
- Furniture and Home Furnishings.
- Premium Electronics.
- Luxury Motor Vehicles.
- Premium Recreational Products.

Luxury Goods remain distinct from Staple Goods even where they serve related broad needs. Basic Clothing is a Staple Good, whereas Luxury Clothing and Accessories are Luxury Goods.

Demand may depend on income, wealth, preferences, status-related consumption and cultural factors. Population and other relevant systems define those characteristics; Economy resolves their economic consequences through explicit demand interfaces.

#### 7.4.3 Industrial Goods

Industrial Goods are physical products primarily intended for production, infrastructure, transport, construction, commercial operations or other productive uses.

Initial examples include:

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

Industrial Goods are not interchangeable with Strategic Resources. A Strategic Resource is explicitly designated as strategically significant; an Industrial Good is a product class and category.

A raw material and a processed product may have separate Catalogue identities where their physical form, production process, handling, quality or economic role differs. They must not be treated as the same inventory item merely because they contain the same material.

### 7.5 Initial Economic Goods Catalogue

The following establishes the initial economic Good categories and their primary Industry associations. It is an initial economic taxonomy, not a complete production recipe catalogue.

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
| Luxury | Cosmetics and Perfumes | Consumer Goods |
| Luxury | Furniture and Home Furnishings | Consumer Goods |
| Luxury | Premium Electronics | Electronics |
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

This table identifies broad economic product categories. Each category must be assigned a canonical Catalogue identifier and compatible unit in the Catalogue.

A Good may have more than one relevant Industry association. An association indicates potential relevance to production, processing, supply or use. It does not guarantee that every associated Industry can produce the Good without the required technology, inputs, facilities and recipe.

Additional categories may be introduced where a distinct demand profile, production process, strategic role or economic behaviour justifies the distinction.

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

Each Service must have a canonical Catalogue identity, a defined unit of sale or delivery, and applicable pricing and demand rules.

The responsible system defines the operational requirements for delivering the Service. For example, Transportation Services depend on transport capacity and infrastructure, while Economy resolves commercial demand, prices and payments.

Whether a Service can be imported, exported or traded internationally must be explicitly defined for that Service or service category.

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
- Missile Guidance Systems.
- Fire-Control Systems.
- Weapons intended for incorporation into a larger assembly.
- Protective Equipment.
- Camouflage Materials.
- Specialist Military Clothing.
- Ghillie Suits.
- Ammunition Components.

These examples are not a closed list. The detailed Component catalogue and design rules belong to the Catalogue, Research, Military and Production specifications.

A Designed Component can be produced as an industrial output, purchased by an authorised buyer, held in Inventory and consumed as an input into a larger design or production process.

Component performance is defined by the relevant design and simulation systems. Economy determines economic demand, supply, prices and transactions without independently inventing performance values.

Where an item is independently usable as a complete military system, the Catalogue must classify it according to the established distinction between Designed Component and Armament.

### 7.8 Armaments

Armaments are physical military products intended for independent military use, deployment, issue, operation or installation.

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

Military defines operational requirements and military demand. Research defines applicable technology and design knowledge. Production determines physical production or assembly. Inventory records resulting physical stock. Economy resolves the associated economic requirements and transactions.

A component does not become an Armament solely because it is used by the military. A radar component incorporated into an aircraft remains a Component; a separately deployable radar station is an Armament.

The Catalogue and relevant design specifications must establish consistent identities so that demand, production, procurement and inventory systems classify the same item consistently.

### 7.9 Strategic Resources

A Strategic Resource is an additional designation applied to a physical product or material whose availability is important to economic or industrial activity.

A Strategic Resource retains its canonical product identity, primary economic class, physical form, unit and applicable category. The designation does not create a second product identity or separate duplicate inventory.

Strategic Resources may be extracted, produced, stockpiled, transferred, traded and consumed according to their definitions.

The initial Strategic Resource taxonomy contains exactly eight categories:

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

Steel and Aluminium are strategic materials produced through industrial processes, not naturally occurring geological deposits. Their production rules must define the required inputs and outputs.

Rich Soils are represented as a commodity extracted through Agriculture and recorded in Inventory. This is a deliberate abstraction for the economic simulation.

Production defines the physical extraction or production mechanics. The Catalogue defines product identity and shared attributes. Inventory records physical stock. Economy defines economic demand, price, commercial ownership and financial treatment.

### 7.10 Product Units and Consumption Intervals

Every economic product must have a canonical unit or defined service-delivery unit in the Good and Service Catalogue.

For consumer Goods, Economy may express demand using a Good Unit defined in relation to the standard consumption requirement of one individual over a specified Consumption Interval. This is an economic demand convention governed by the Good and Service Catalogue, not a physical measurement unit.

The Catalogue defines the canonical physical unit for each physical product and keeps it distinct from any consumption interval, standard consumption requirement or economic valuation. Economy must not redefine the Catalogue's physical unit through the Good Unit convention.

The Consumption Interval defines the period over which a standard consumption requirement is assessed. For example, a daily or annual consumption interval may be used to express demand for a product, while the product's physical stock continues to be measured in its canonical unit, such as kilograms, litres or individual items.

This convention does not mean every individual consumes exactly one Good Unit in every interval. Actual demand may depend on Population Type, household circumstances, income, preferences, availability and other explicitly defined variables.

The Population System owns individual and household characteristics. Economy receives demand quantities and resolves their economic consequences.

For Industrial Goods, Strategic Resources, Components, Armaments and Services, the applicable unit must reflect the product's defined production, consumption, sale or delivery requirements and must remain compatible with the Catalogue's canonical unit or defined service-delivery unit.

The consumer-consumption convention must not be applied indiscriminately to products that are not consumer Goods.

A Consumption Interval is a demand concept. It must not replace or redefine the physical units used for production, storage, inventory, transport or trade. Economic price denominators must explicitly identify the unit to which the price applies.

---

## 8. Product Prices

### 8.1 Base Price

Every economic product assigned a monetary value must have a defined Base Price where required by its economic rules.

Base Price is the reference monetary value assigned to one canonical product unit at the beginning of the simulation.

Base Prices are calibrated to the 1955 economic baseline.

Base Price is not necessarily the actual market price, Company Selling Price or Transaction Price.

Economy owns the economic Base Price state. The product identity and unit used by the price must reference the Catalogue. A Base Price persists unless changed by an explicit data or simulation rule.

### 8.2 Global Price

Global Price is the current global reference price of an eligible product.

It represents aggregate global market conditions and provides a reference for domestic pricing and international transactions where applicable.

Global Price may differ from Base Price as global supply, global demand and other explicitly defined market conditions change.

The Global Price calculation must identify:

- Canonical product identity and unit.
- Aggregation period.
- Global supply.
- Global demand.
- Eligible stock availability.
- Restrictions on market participation.
- Defined price-response function.
- Permitted price range, if any.
- Update frequency.

The price-response function must be explicitly defined in the Economic Algorithms or Technical Specifications. Economy must not apply an unspecified market modifier.

### 8.3 Domestic Market Price

Domestic Market Price is the reference price for a product within a defined domestic market.

A domestic market may reflect the relevant Nation's economic conditions, domestic availability, purchasing power, transport costs, restrictions and other defined inputs.

Domestic Market Price is distinct from Global Price and Company Selling Price.

The market model must define domestic market boundaries, price aggregation, regional market separation and the relevant calculation rules.

### 8.4 Company Selling Price

Company Selling Price is the price at which a Company offers a product for sale.

The Senior Executive and Company AI determine pricing decisions within applicable Government rules, contractual obligations, market conditions and available state.

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

A Company may offer a product above, below or equal to its reference prices when permitted by applicable rules.

### 8.5 Transaction Price

Transaction Price is the price agreed for a particular economic transaction.

It may equal the Company Selling Price, but this is not mandatory where a permitted negotiated price, contract price, Government procurement price or other explicitly defined transaction rule applies.

Transaction Price is the authoritative monetary value used to calculate the contractual payment for that transaction.

The agreed price, payment obligation, actual payment and revenue recognition are separate states.

### 8.6 Price Controls

Government may impose price controls where authorised by its laws and institutions.

A price control must specify:

- Authority imposing it.
- Affected product or product class.
- Affected market or jurisdiction.
- Type of price restriction.
- Permitted price range or required price.
- Effective date and duration.
- Entities and transactions to which it applies.

Economy applies the restriction to eligible pricing and transactions.

Price controls do not automatically guarantee that a product will be supplied or demand fulfilled. Supply, production, allocation and transaction rules continue to apply.

---

## 9. Supply, Demand and Markets

### 9.1 Market Definition

A market is an economic aggregation and transaction context in which demand, supply, prices and economic exchanges are resolved.

Markets do not create physical commodities. They aggregate economic availability from production, inventory, imports and other permitted sources.

A market may be global or domestic.

The market model must identify participating products, eligible entities, geographic or economic scope, aggregation period and transaction rules.

### 9.2 Global Market

The Global Market aggregates eligible economic supply and demand across participating Nations.

It provides a global reference context for product availability and Global Price.

The Global Market is not a physical location or a universal stockpile. A quantity represented in global market statistics cannot automatically be purchased and delivered to any Nation.

Physical access, transport, delivery time, permissions, sanctions and existing commitments constrain fulfilment.

Global market participation is subject to:

- Trade permissions.
- Export and import restrictions.
- Sanctions.
- Physical availability.
- Transport and infrastructure.
- Contractual commitments.
- Applicable Government policy.
- The product's eligibility for international trade.

The Global Market must distinguish aggregate production flows from physical stock and from quantities that are genuinely available for additional commitments.

### 9.3 Global Production and Global Supply

Global Production is the aggregate quantity reported as physically produced during a defined period by Production.

Global Supply represents the quantity eligible to satisfy demand within the defined global market period.

Global Production and Global Supply are distinct measures. Production is a flow over a period. Inventory is physical stock at a point in time. Supply is a market calculation that determines which quantities may be offered to fulfil demand.

The Global Supply calculation must account for:

- Newly produced quantity eligible for the market.
- Eligible quantities released from stock.
- Quantities reserved for existing obligations.
- Quantities committed to domestic use.
- Quantities available for export.
- Quantities restricted from trade.
- Quantities physically unavailable for delivery.
- Quantities destroyed or lost.
- Other explicitly defined availability constraints.

The calculation must prevent the same physical quantity from being counted more than once. A quantity cannot be counted both as newly available output and as an additional stockpile quantity unless the accounting explicitly reconciles the production inflow and resulting stock.

Global Supply must not be treated as a physical inventory record. Inventory remains the authoritative source of physical quantities.

### 9.4 Global Demand

Global Demand is the aggregate demand for an eligible product within the defined global market period.

Canonical demand sources include:

1. Population Demand.
2. Industry Demand.
3. Government Demand.
4. Military Demand.
5. Foreign Demand, where it does not duplicate another source.
6. Other demand sources explicitly defined by a system.

A demand quantity must identify its originating entity, product, quantity, unit, time period and fulfilment status.

Foreign Demand must not duplicate the same demand already represented as Population, Industry, Government or Military Demand within another Nation.

Demand is not necessarily effective purchasing demand. Where affordability, budget authority or commercial eligibility constrains demand, the market model must distinguish requested need from effective demand eligible to participate in market resolution.

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

Population owns population characteristics and demand-generation rules. Economy receives and resolves the resulting economic demand.

Demand does not guarantee a completed purchase. Actual purchases depend on affordability, availability, market access and applicable transaction and allocation rules.

### 9.6 Industry Demand

Industry Demand represents the Goods and Strategic Resources required by Industries for their intended operations.

Physical input requirements are defined by Production Processes and Recipes. Production supplies the relevant requirements for an intended production quantity and selected recipe.

Economy resolves commercial availability, procurement, costs and allocation.

The intended input requirement for a production process is derived from its defined recipe and intended production quantity.

Economy must not invent input ratios for a physical production process. Planned demand must not be confused with a valid Production Order or with a guarantee that the required inputs will be available.

### 9.7 Government Demand

Government Demand represents economic demand generated by Government activity.

Examples include:

- Public-service procurement.
- Public infrastructure projects.
- Strategic stockpile acquisition.
- Government facility requirements.
- Public-sector equipment and supplies.
- Other authorised Government purchases.

Government defines requirements, budget authority and purchasing permissions. Economy resolves the associated demand, available supply, transaction prices and financial transfers.

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

Military determines the required items, quantities, timing and military priorities.

Economy resolves the associated economic demand, procurement costs, commercial availability and financial transactions.

Production determines whether required items can physically be produced. Inventory records stock and authorised movement. Military determines whether delivered items satisfy the relevant military acceptance requirements.

### 9.9 Demand Fulfilment

Demand Fulfilment represents the quantity of a demand that has been supplied through valid fulfilment.

The system must distinguish:

- Requested quantity.
- Effective demand quantity.
- Quantity available.
- Quantity allocated.
- Quantity ordered.
- Quantity dispatched.
- Quantity delivered.
- Quantity accepted, where applicable.
- Quantity paid for.
- Unfulfilled quantity.
- Cancelled or expired quantity.

These states must not be treated as equivalent.

An order does not count as delivered supply before delivery conditions are satisfied. A delivery does not automatically count as paid revenue until the financial rules establish that payment has occurred.

### 9.10 Economic Allocation

Available output may be allocated to:

- Population consumption.
- Industry inputs.
- Government use.
- Military procurement.
- Export.
- Stockpiling.
- Other explicitly defined destinations.

Allocation determines the intended destination or commitment of a quantity. It does not automatically mean the quantity has been physically delivered or consumed.

Allocation rules must identify applicable priority, reservation, contractual obligation, Government directive, Military requirement, export commitment and stockpile policy.

The rules must define how competing demands are resolved when eligible supply is insufficient.

Economy must not silently allocate the same quantity to multiple incompatible destinations. Physical reservation and stock commitment must be performed through the Inventory interface.


---

## 10. Production and Inventory Interfaces

### 10.1 Production Capacity

Production Capacity is the maximum quantity of a defined output that a production process can physically produce per Production Day under its current operating conditions.

Production Capacity is owned by the Production System.

It may depend on facilities, equipment, labour availability, technology, physical inputs, operational status, infrastructure and other explicitly defined production conditions.

Economy receives Production Capacity as a constraint on intended Industry activity.

Production Capacity is not a guarantee that the full quantity will be produced. It is also not equivalent to Industry Scale, inventory quantity or Actual Production.

### 10.2 Intended Production

Intended Production represents the quantity or production priorities an Industry seeks to realise under its current economic and commercial decisions.

Economy supplies commercial priorities and relevant economic constraints. Production determines the physically feasible output and executes authorised Production Orders.

Intended Production must be compatible with:

- Industry Scale.
- Available Company Capital and authorised operating expenditure.
- Available labour.
- Input requirements derived from the selected recipe.
- Commercial demand and allocation priorities.
- Government restrictions.
- Production Capacity.
- The production process's technical requirements.

The exact relationship between Industry Scale and Intended Production must be defined by the Economy and Production interface.

An economic intention is not itself a Production Order, and a Production Order is not proof that production has completed.

### 10.3 Actual Production

Actual Production is the quantity physically produced by Production during the relevant production period.

Actual Production is constrained by applicable physical production rules, including input availability, operational status and Production Capacity.

Economy records the economic consequences of reported output. It must not independently recalculate or override Actual Production.

Actual Production does not automatically equal quantity sold, quantity delivered, quantity accepted or revenue earned.

Physical output becomes available stock only when Production reports completion and the corresponding Inventory operation successfully records the output.

### 10.4 Production Processes and Recipes

Production Processes and Recipes defines the physical inputs and outputs required for a transformation.

Each applicable recipe must specify:

- Production Process identity.
- Recipe identity and version.
- Required input products and quantities.
- Output products and quantities.
- Canonical product units or valid unit conversions.
- Applicable technology or process requirements.
- Defined co-products, by-products, recoverable materials, waste or process losses.
- Production batch or period basis.
- Other explicit execution constraints.

The detailed recipe schema and transformation calculations belong in the Production Processes and Recipes specification.

Economy uses recipe-derived requirements for economic planning and procurement.

There is no universal assumption that one unit of input produces one unit of output.

Recipes do not independently reserve, consume, transfer or create Inventory. Production orchestrates execution and Inventory commits physical stock changes.

### 10.5 Input Availability

Inventory is the authoritative source of physical stock and reservation state.

Production validates the physical inputs required for an order through its Inventory interface. Economy may procure or commercially allocate inputs, but cannot declare undelivered goods physically available.

The input interface must distinguish quantities that are:

- Physically present and available.
- Reserved for a production order or another valid purpose.
- Committed to an existing obligation.
- In transit.
- Restricted from use.
- Damaged, unsuitable or otherwise unavailable.

A scheduled delivery must not be treated as physically available before the applicable delivery and receipt conditions are satisfied.

### 10.6 Input Shortfalls

Production determines how a process responds to insufficient inputs according to its explicit production rules.

A production process that requires all mandatory inputs must not consume nonexistent quantities or produce output without satisfying those requirements.

Partial production, input substitution or alternative recipes are permitted only where explicitly defined by the relevant recipe and Production rules.

The Economy System records the commercial consequences of the resulting production state. It must not independently alter recipe requirements to resolve a physical input shortfall.

### 10.7 Physical Inventory

Inventory is the single authoritative owner of physical inventory records, including:

- Product identity.
- Physical quantity and unit.
- Location.
- Lot or batch identity, where applicable.
- Condition and quality.
- Ownership and custody references.
- Reservation and commitment state.
- Restrictions.
- Incoming and outgoing movements.
- Production provenance.
- Destruction, loss and consumption records.

Economy may hold references to inventory records and calculate their economic value. It must not maintain a competing authoritative inventory quantity.

A physical inventory record must reference the canonical product identity and unit defined in the Good and Service Catalogue.

### 10.8 Inventory Reconciliation

For a defined inventory holding and accounting period, the basic physical quantity reconciliation is:

**Closing Quantity = Opening Quantity + Physical Receipts + Production Outputs − Consumption − Physical Dispatches − Destruction − Other Explicit Physical Losses**

Every term must refer to a distinct, valid physical movement.

The reconciliation must not deduct the same quantity twice because it was both sold and transferred. A sale is a commercial event; the associated physical removal is recorded once through the applicable Inventory transaction.

Likewise, an inventory transfer between locations is not a global loss of stock. The origin and destination movements must reconcile with any in-transit holding or cargo record so that the quantity remains accounted for throughout the movement.

Closing physical quantity cannot be negative.

The detailed movement ledger, lot accounting, reservations and transaction atomicity rules belong to the Inventory System.

### 10.9 Stockpile Valuation

The financial value of a stockpile is distinct from its physical quantity.

Where inventory valuation is required for Company financial reporting, the valuation method must be explicitly defined in the Economy System or its Technical Specifications.

A change in Global Price must not automatically create Company revenue, cash or a new physical stock quantity.

A physical inventory loss does not automatically deduct Company Capital by the same amount unless the financial accounting rules define that consequence.

Inventory quantity and inventory valuation must be separately identifiable.

### 10.10 Unsold Output

Output that has been physically produced but not sold or otherwise transferred remains Inventory.

Production does not automatically create revenue.

Revenue is recognised only when a qualifying economic transaction or other explicitly defined revenue event occurs.

An output that is reserved for a customer remains physical stock until the relevant physical transfer is committed. Reservation alone does not prove dispatch or delivery.

---

## 11. Labour and Wages

### 11.1 Labour Ownership

Population determines population composition, workforce characteristics, educational attainment and the Nation's available labour supply.

Economy receives available labour information and allocates workers to economic employment according to Industry requirements and the relevant employment rules.

Economy does not independently create population or change educational attainment.

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

Where a Production recipe or process defines operational staffing requirements, Production supplies those requirements through the defined interface. Economy determines the economic employment and wage consequences.

### 11.3 Labour Availability

Labour Availability represents the quantity of eligible labour that can be allocated to an Industry.

Population supplies available labour and relevant workforce characteristics.

Economy allocates labour subject to:

- Available workers.
- Required qualifications.
- Employment rules.
- Industry demand.
- Government restrictions.
- Wage and employment conditions.
- Other explicitly defined labour constraints.

Economy must not allocate the same worker or labour quantity to multiple incompatible full-time positions.

### 11.4 Employment

Employment records the labour allocated to an Industry.

Industry employment must identify the quantity and relevant type of workers employed.

Where an Industry cannot fill its Labour Demand, actual employment remains below the required level.

An Industry's employment state is distinct from its physical Production Capacity.

A reduction in employment may constrain production through the Production System's labour interface. The precise physical effect depends on the relevant production process.

Workers released from an Industry return to the available labour pool according to the Population and Economy employment rules.

### 11.5 Labour Fulfilment

Labour Fulfilment measures the proportion of Labour Demand satisfied by actual employment.

When Labour Demand is greater than zero:

**Labour Fulfilment = Actual Employment / Labour Demand**

The result is bounded between 0 and 1.

When Labour Demand is zero, Labour Fulfilment must not be calculated by dividing by zero. Its reported state must follow the explicit zero-demand convention defined in the Technical Specifications.

Labour Fulfilment is an economic staffing measure, not a universal Production multiplier. Its physical production consequences are defined by Production.

### 11.6 Wages

Each Industry defines the relevant wage categories and Base Gross Salary values for its employment requirements.

Base Gross Salary is the reference annual gross salary for a defined employment category.

Actual Gross Salary may differ from the reference value according to explicitly defined economic, institutional, labour-market and employment rules.

The wage model must define its inputs and calculation order rather than applying an unspecified Wage Modifier.

Where annual salary is paid evenly across the simulation year:

**Daily Gross Salary = Annual Gross Salary / 365**

The conversion reflects the simulation calendar, which contains exactly 365 days.

The financial system must distinguish gross wages paid to workers from employer-side taxes, contributions or other employment costs. Any such costs must be defined by the relevant Government and Economy rules.

### 11.7 Wage Expenditure

Labour Expenditure represents the Company's financial expenditure on employee compensation over the relevant accounting period.

It must be calculated from actual employment, applicable compensation and the defined payment schedule.

For daily-paid compensation:

**Daily Gross Wage Expenditure = Sum of Daily Gross Salary for All Employed Workers**

Where employment categories contain multiple workers, the calculation must account for the quantity of workers in each category.

Additional employer costs must be included only where a defined rule applies.

### 11.8 Employment and Population Types

Population owns Population Types and their relevant characteristics.

Economy uses those characteristics to define or receive employment eligibility, workforce composition and economic relationships where required.

Industry employment may be represented by employment categories linked to Population Types.

The mapping between Population Types and Industry employment categories must be explicitly defined. Economy must not invent demographic or educational characteristics for a Population Type.

---

## 12. Revenue, Expenditure and Financial Performance

### 12.1 Revenue

Revenue is money recognised from qualifying economic transactions according to the applicable accounting rules.

Physical production, inventory ownership, market demand and potential sales do not automatically create revenue.

For a completed sale:

**Transaction Revenue = Quantity Sold × Transaction Price**

Where a transaction contains multiple products or transaction lines, each line must be accounted for using its own quantity and Transaction Price.

Daily Revenue is the sum of revenue recognised from qualifying transactions during the relevant day.

The financial rules must define when revenue is recognised for cash transactions, credit transactions, contracts, Government procurement and other transaction types.

Revenue recognition must not be conflated with physical delivery, customer acceptance or cash settlement. The applicable accounting policy must explicitly identify which event establishes revenue for each transaction type.

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

A financial outflow, an accounting expense and a physical inventory movement are related but distinct records. The same event must not create duplicate expenses or duplicate cash movements.

### 12.3 Input Expenditure

Input Expenditure represents the cost of inputs acquired through economic transactions.

For a qualifying input purchase:

**Input Transaction Cost = Quantity Purchased × Transaction Price**

Input Expenditure is aggregated across qualifying purchases within the accounting period.

Using an input already owned by the Company or Industry does not create a new purchase transaction. Any inventory-consumption cost recognised in financial accounts must follow the defined inventory valuation and accounting rules.

Purchasing an input does not prove that the input has been delivered or received. Physical availability is established through Inventory and the applicable delivery interface.

### 12.4 Operating Expenditure

Operating Expenditure represents costs recognised from the ongoing operation of a Company or Industry.

It may include labour, purchased inputs, maintenance services, energy purchases, logistics, taxes and other defined operating costs.

The category must not automatically include every cash outflow. Capital investment, financing transactions, asset purchases, inventory acquisition and other financial movements must be classified according to explicit accounting rules.

### 12.5 Operating Income

Operating Income represents Revenue less applicable Operating Expenditure for the same accounting period.

**Operating Income = Revenue − Operating Expenditure**

A positive result represents an operating surplus.

A result of zero represents operating break-even.

A negative result represents an operating loss.

Operating Income is not automatically equal to the change in Company Capital. The difference may include capital investment, financing, subsidies, transfers, asset transactions, inventory accounting and other defined financial movements.

### 12.6 Financial State

Company financial state includes Company Capital, recognised Revenue, Expenditure, Operating Income, financial obligations and other defined accounting values.

The financial model must distinguish:

- Funds available for spending.
- Company Capital.
- Revenue.
- Operating Income.
- Physical inventory.
- Financial obligations.
- Capital investment.
- Subsidies and transfers.
- Accounts payable and receivable, where implemented.
- Settled and unsettled payments.

These values may be related but must not be treated as interchangeable.

### 12.7 Financial Failure

A Company experiencing an operating loss does not automatically cease to exist.

Any transition to financial distress, insolvency, default, closure or restructuring must be governed by explicit financial rules.

Those rules must identify triggering conditions, calculation periods, affected obligations, permitted interventions and resulting state changes.

Such outcomes are not implied merely by negative Operating Income.

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

Capital must not be created without a defined source, except through an explicitly defined scenario initialisation or other authorised rule.

### 13.2 Capital Allocation

Company Capital is available for authorised commercial decisions.

The Senior Executive and Company AI may allocate capital among eligible activities, subject to Company rules, legal restrictions, available funds and Player or Government directives.

Economy must validate that expenditure or investment is permitted and financially supportable.

The same funds cannot be committed to incompatible expenditures simultaneously.

Rules governing reserved capital, commitments and unspent allocations must be defined in the financial model. A commitment or reservation of funds must be distinguishable from a completed payment.

### 13.3 Government Subsidies

A Subsidy is a defined Government financial transfer to an eligible recipient.

Government owns subsidy authority, eligibility and public expenditure rules. Economy records the recipient's financial effect and resulting economic state.

A subsidy definition must identify:

- Issuing Government.
- Recipient.
- Amount or calculation basis.
- Purpose.
- Eligibility conditions.
- Start date.
- End date or termination condition.
- Payment schedule.
- Attached obligations, if any.
- Financial treatment.

A subsidy does not automatically increase Industry Scale, Production Capacity, Actual Production or Company profitability. Any such result must arise from the recipient's decisions and the relevant system mechanics.

### 13.4 Capital Injections

A Capital Injection is an explicitly authorised transfer of funds into a Company.

It must identify the source, recipient, amount, date and resulting financial treatment.

A Government capital injection must debit the appropriate public account and credit the Company Capital account according to the defined financial rules.

A Player-authorised capital injection must use the account and authority specified by the applicable gameplay and Government rules.

A capital injection does not automatically imply a change in ownership unless the relevant transaction or legislation explicitly provides for it.

### 13.5 Foreign Investment

Foreign Investment occurs when an economic entity invests capital in an entity or operation located in another Nation.

Foreign investment must be resolved without currency conversion under the current single-Dollar model.

Its permissions and effects depend on:

- Investor.
- Recipient.
- Source and destination Nations.
- Applicable Government investment legislation.
- Diplomatic and investment agreements.
- Sanctions.
- Ownership and control restrictions.
- Terms of the investment.
- Applicable taxation and financial rules.

An investment must specify the rights it grants. Voting rights, control rights, profit distribution, capital recovery, withdrawal and ownership changes must not be inferred solely from the transfer of funds.

The financial transfer and any resulting ownership or control change must be recorded as distinct but linked effects where applicable.

---

## 14. Trade and International Commerce

### 14.1 Trade

Trade is a commercial transaction involving a Good, Strategic Resource, Designed Component, Armament or eligible Service between authorised economic entities.

A trade transaction must identify:

- Seller.
- Buyer.
- Canonical product identity.
- Quantity and unit.
- Transaction Price.
- Origin.
- Destination.
- Applicable permissions.
- Delivery conditions.
- Payment conditions.
- Contractual obligations.
- Transaction status.

A transaction must satisfy the relevant commercial, legal, Inventory and delivery rules.

Trade is the commercial relationship and agreement. Physical dispatch, transportation, receipt and inventory changes are recorded through the systems that own those operations.

### 14.2 Domestic Trade

Domestic Trade occurs between economic entities within the same Nation.

It is subject to applicable domestic laws, market rules, transaction requirements and Government restrictions.

Domestic Trade does not require an international trade permission unless another defined rule applies.

Domestic trade may involve transportation and delivery between locations. The fact that buyer and seller are in the same Nation does not remove the need to record physical movement where it occurs.

### 14.3 Imports and Exports

Imports and Exports represent eligible international transfers of products between Nations.

They may include Goods, Strategic Resources, Designed Components, Armaments and Services where the relevant product and transaction rules permit international trade.

An international transaction must satisfy applicable export, import, diplomatic, transport, payment and sanction rules.

A product being represented in the Global Market does not automatically mean that it can be imported by every Nation.

The economic transaction, legal permission, physical movement and financial settlement remain separate but linked state changes.

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

Diplomacy and Government own the legal and diplomatic authority associated with Trade Agreements.

Economy applies their defined commercial consequences.

The terms of a Trade Agreement must not be inferred from its name or diplomatic relationship alone.

### 14.5 Subsidiaries and Foreign Operations

A foreign subsidiary is a commercial operation established in a Nation that is owned, controlled or otherwise legally linked to a Company based in another Nation.

Foreign operations must comply with the host Nation's applicable laws and economic conditions.

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

Economy applies the restriction to eligible economic transactions.

The precise restriction categories and effects must be defined by Government and Diplomacy. Economy must not invent a generic sanction severity modifier.

---

## 15. Economic Transactions and Accounting

### 15.1 Transaction Validation

Before an economic transaction reaches a committed state, the relevant rules must validate:

- Buyer and seller eligibility.
- Canonical product identity.
- Quantity and unit.
- Commercial authority and transfer rights.
- Applicable inventory availability or supply commitment.
- Transaction Price.
- Legal permissions.
- Trade restrictions.
- Payment conditions.
- Delivery conditions.
- Contractual obligations.

A transaction that fails a mandatory validation must not complete the affected operation.

Validation of a commercial order does not prove that physical stock has been reserved, dispatched or delivered. Each of those operations must be confirmed by the system that owns it.

### 15.2 Transaction States

The transaction model must distinguish relevant stages, including:

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

The exact state machine and permitted transitions belong in the Technical Specifications.

Not every transaction type must use every state. The applicable state transitions, prerequisites and resulting financial or physical effects must be explicitly defined.

A transaction may be delivered but unpaid, paid but awaiting delivery, or cancelled after an earlier non-final state, if its terms and the defined state machine permit those conditions.

### 15.3 Financial and Physical Settlement

Financial settlement and physical delivery are related but distinct.

A transaction may require payment, physical transfer or both.

The transaction rules must specify when:

- A commercial obligation is created.
- Funds are reserved or committed.
- The seller's physical inventory is reduced.
- A quantity becomes in transit.
- The buyer's physical inventory is increased.
- Funds are transferred.
- Revenue is recognised.
- Expenditure is recognised.
- Outstanding obligations are cleared.

The same transaction must not create duplicate inventory transfers, payments or revenue.

Inventory changes must be committed through Inventory. Financial movements must be committed through Economy and the appropriate account authority. Where an operation spans multiple systems, the Technical Specifications must define the atomicity, recovery and reconciliation mechanism.

### 15.4 Revenue Recognition

Revenue is recognised according to the transaction's defined accounting rules.

The creation of a sales offer, purchase order or contract does not automatically constitute realised revenue.

Where revenue recognition depends on delivery, payment, customer acceptance or another contractual condition, that condition must be explicit.

Revenue recognition must not cause a duplicate cash movement. Recognising revenue and receiving payment are separate accounting events where the applicable transaction rules distinguish them.

### 15.5 Transaction Auditability

Economic transactions must retain sufficient information to explain resulting financial and inventory changes.

The exact event schema belongs in the Technical Specifications.

At minimum, the system must be able to determine which transaction caused a change to an account, stock reservation, physical movement reference, financial obligation, recorded revenue or expenditure.

Corrections and reversals must reference the original event and follow explicit rules. A historical transaction must not be silently overwritten in a way that makes the resulting state irreproducible.

---

## 16. Daily Economic Processing

The Economy System processes economic state according to the shared simulation scheduler and the update frequencies defined for individual mechanics.

This section establishes logical dependencies and required economic operations. It does not establish a competing global execution order for the simulation.

### 16.1 External State Collection

Receive the current authoritative state required from external systems, including:

- Population and available labour.
- Industry Data and establishment configuration.
- Government laws, taxes, subsidies and restrictions.
- Research and technology availability.
- Military procurement demand.
- Production capacity and physical production state.
- Inventory quantities, reservations and availability.
- Terrain and infrastructure conditions.
- Logistics and delivery state.
- Diplomatic permissions and trade restrictions.

Only state required by Economy should be imported. Economy must not take ownership of external state merely because it needs to read it.

### 16.2 Establishment and Eligibility Validation

Validate Industry establishment requests and other economic operations requiring prerequisites.

Use Industry Data for Industry definitions and establishment configuration, Government for authority and Treasury state, and the relevant systems for other prerequisites.

Establishment must follow the atomic operation defined in Section 5.

### 16.3 Resource and Inventory Availability

Receive authoritative physical inventory and resource availability.

Identify quantities that are available, reserved, committed, in transit, restricted or otherwise unavailable for economic allocation.

Prevent duplicate availability across competing transactions and production processes.

Economy may make commercial allocation decisions but must request physical reservations through Inventory.

### 16.4 Labour Availability

Receive available labour and workforce characteristics from Population.

Determine eligible labour quantities and the workforce information required for Industry allocation.

Do not duplicate Population's demographic or educational state.

### 16.5 Industry Economic Requirements

Calculate or update the economic requirements of each Industry, including:

- Industry Scale.
- Labour Demand.
- Intended Production priorities.
- Input demand derived from Production requirements.
- Capital availability.
- Commercial commitments.
- Relevant operating expenditure requirements.

Physical Production Capacity remains owned by Production.

### 16.6 Input Procurement and Allocation

Resolve input procurement and allocation decisions using available stock, supplier offers, transaction rules and Production-derived input requirements.

Create valid economic orders or allocations without treating undelivered goods as physically available.

Where physical stock must be committed to a production order, request the appropriate Inventory reservation.

### 16.7 Production Interface

Send relevant economic priorities and commercial constraints to Production.

Production validates physical feasibility and executes authorised orders according to its own rules.

Economy receives reported production results and their associated economic consequences. It must not calculate physical output within the Economy System.

### 16.8 Inventory Reconciliation

Receive and reconcile authoritative Inventory results associated with production, consumption, destruction, delivery, sale and transfer.

Do not directly apply duplicate stock changes in Economy.

Maintain consistent financial and commercial references to the authoritative inventory state.

### 16.9 Demand Aggregation

Receive and aggregate Population, Industry, Government, Military and other explicitly defined demand.

Avoid duplicate demand across domestic and international aggregation.

Distinguish requested needs from effective demand, allocated quantities and fulfilled quantities.

### 16.10 Market Calculation

Calculate eligible Global Market and Domestic Market state using defined supply, demand, stock availability and market rules.

Update Global Price and Domestic Market Price according to their specified calculations.

Global Production, physical Inventory and Global Supply must remain distinct measures.

### 16.11 Company Pricing Decisions

Company AI determines Company Selling Prices and other commercial decisions within applicable legal and market constraints.

Pricing decisions must use the authoritative state available for the relevant update.

### 16.12 Transaction Resolution

Resolve eligible domestic and international transactions according to availability, permissions, pricing, contracts, allocation and delivery rules.

Validate that each transaction can proceed without duplicate or negative inventory.

Transaction state must distinguish order, reservation, dispatch, delivery and payment.

Inventory movements are committed by Inventory. Financial movements are committed through Economy's financial transaction rules. A single physical quantity must not be deducted twice because a sale is also represented as a transfer.

### 16.13 Financial Accounting

Record qualifying revenue, expenditure, transfers, payments, taxes, financing costs and other financial movements.

Update Company financial state according to the accounting rules.

### 16.14 Company Financial and Operating State

Update Company Capital and other financial values using completed financial transactions and explicitly defined accounting rules.

Calculate Operating Income for the relevant accounting period.

Do not infer cash movement solely from a change in inventory valuation or an accounting entry.

### 16.15 Company AI Decisions

Company AI evaluates updated state and determines subsequent commercial decisions.

Decisions may affect future Industry Scale, pricing, capital allocation, procurement, trade participation and other eligible economic actions.

A decision must not retroactively change a transaction that has already completed unless an explicit correction or reversal mechanic permits it.

### 16.16 Cross-System Outputs

Publish resulting economic state required by other systems.

Potential outputs include:

- Updated Company Capital.
- Revenue and expenditure records.
- Industry Scale.
- Employment and labour demand.
- Economic product demand.
- Purchase orders.
- Transaction results.
- Economic stock reservations and commitments.
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
- Senior Executive association and authorised control state.
- Company Capital.
- Industry Scale.
- Industry establishment and economic operating state.
- Economic financial records required by the simulation.
- Company pricing and commercial decisions.
- Employment allocations.
- Trade contracts and agreements represented by Economy.
- Investment relationships.
- Foreign commercial operations.
- Economic transaction state.
- Product prices and relevant market state.
- Economic obligations, commitments and payment status.
- References to inventory holdings and commercial stock reservations, where applicable.

Physical Production Capacity, actual production, facility condition, physical stock quantities, inventory lots and resource deposits remain authoritative in their respective systems.

Persistence does not imply immutability. State changes only through defined simulation events, calculations or authorised actions.

Executed transactions and production-related economic records must retain enough references to reproduce their financial consequences. Recipe versions, Catalogue definitions and other relevant reference data must not silently rewrite the historical interpretation of completed operations.

---

## 18. Validation and Invariants

The Economy System must enforce the following invariants.

### 18.1 Financial Invariants

- Nation Treasury and Company Capital are separate accounts.
- Every financial transfer has a defined source and destination, except for explicitly defined creation or destruction of money by a future monetary mechanic or authorised scenario initialisation.
- Industry Establishment Capital Requirement and Opening Company Capital are separate financial movements.
- A financial expenditure cannot be completed if the relevant account lacks the required available funds, unless an explicit credit or deficit mechanic permits it.
- Company Capital does not automatically depreciate.
- Revenue is not created solely by production or inventory accumulation.
- Operating Income and Company Capital are distinct values.
- Inventory valuation changes do not automatically create cash or revenue.
- A financial transaction must not be committed twice.
- Financial settlement must follow the applicable transaction state and accounting rules.

### 18.2 Inventory and Transaction Invariants

- Inventory owns physical stock quantities and physical stock movements.
- Physical inventory quantities cannot be negative.
- The same physical quantity cannot be allocated to incompatible uses simultaneously.
- A transaction cannot transfer a quantity the seller does not validly own or control.
- Inventory movements must reconcile with authoritative Inventory records.
- Economy and Production must not maintain competing authoritative physical inventories.
- Reservations do not remove physical stock, but reduce the quantity available for other uses.
- A purchase order or dispatch does not make stock available before the relevant delivery and receipt conditions are satisfied.
- A sale and its associated transfer must not deduct the same physical quantity twice.
- A physical transfer between locations must not be counted as a global loss of stock.
- In-transit quantities must not be counted as available at both origin and destination.
- Global Supply must not count the same physical quantity more than once.
- Production must not consume unavailable inputs unless an explicit production rule permits the relevant alternative.
- A Service must not automatically create physical inventory.
- Corrections and reversals must be traceable to the original transaction or event.

### 18.3 Industry Invariants

- Every Industry has a defined Industry Type.
- Every Industry Type has an explicit Establishment Requirements definition in Industry Data.
- Industry establishment cannot succeed while a mandatory requirement remains unsatisfied.
- Industry establishment is atomic across all required participating systems.
- Existing scenario-loaded Industries do not incur retroactive establishment charges.
- Industry Establishment Capital Requirement is not the same as Opening Company Capital.
- Industry Scale is not identical to Production Capacity.
- Production Capacity and Actual Production are owned by Production.
- An Industry may exist without currently producing.
- Physical damage, financial loss and operational suspension remain distinct states.
- Industry Data remains authoritative for Industry definitions and configuration.

### 18.4 Product and Market Invariants

- Every economic product references a canonical Catalogue identity.
- Every product has a defined canonical unit or service-delivery unit where applicable.
- Every product has exactly one primary economic class.
- Every Good belongs to exactly one of the three mutually exclusive Good Categories.
- Physical form is distinct from primary economic class.
- Strategic Resource is an additional designation, not a replacement primary class or duplicate product identity.
- Base Price, Global Price, Domestic Market Price, Company Selling Price and Transaction Price are distinct values.
- A reference price does not guarantee availability.
- Demand does not guarantee fulfilment.
- An accepted order does not automatically mean delivery has occurred.
- An offer does not automatically mean a sale has occurred.
- Global Production, physical Inventory and Global Supply are distinct measures.
- International trade must satisfy applicable permissions and restrictions.
- A change to Catalogue definitions must not silently rewrite the historical interpretation of completed transactions.

### 18.5 Cross-System Invariants

- Economy does not independently create population, technology, physical production capacity or military capability.
- External state remains authoritative in its owning system.
- Cross-system effects must be transmitted through defined interfaces.
- Each kind of state has one authoritative owner.
- A mechanic must not be implemented as an undocumented generic modifier.
- Each derived value must identify its source variables, units, valid range, calculation order and update frequency in the relevant specification.
- System update dependencies must be compatible with the shared simulation scheduler.
- Historical economic records must retain sufficient references to explain and reproduce their effects.
