# Economy System Overview

## 1. System Definition

The Economy System simulates the commercial activity, financial relationships, markets, labour allocation, and economic decision-making of Nations and their economic participants.

It determines how economic entities acquire and allocate capital, employ labour, obtain inputs, price and exchange products, respond to market conditions, and manage their financial position.

The Economy System connects the needs and resources of the population, the commercial decisions of Companies, the fiscal decisions of Governments, and the material requirements of industrial and military activity.

The system operates within the wider world simulation. Economic activity is constrained by the physical geography of the world, the availability of resources and labour, the capabilities of the Production System, applicable laws, technological development, infrastructure, and international relations.

The Economy System does not independently create physical goods or determine physical production capacity. It determines the economic conditions and decisions under which physical production and commercial exchange take place.

## 2. Design Objectives

The Economy System must:

- Simulate economic activity as an interconnected system rather than a collection of isolated numerical values.
- Represent Companies as economic actors capable of making decisions in response to their financial position, market conditions, constraints, and objectives.
- Model the relationship between supply, demand, prices, purchasing power, production requirements, and economic allocation.
- Represent Government finances and economic intervention through explicit relationships with the Government System.
- Connect labour availability and workforce qualifications to economic activity.
- Support domestic and international commerce between eligible economic entities.
- Connect economic demand to physical production, inventory, research, military procurement, and population consumption.
- Allow economic conditions to change dynamically in response to decisions, events, and the evolving state of the world.
- Preserve clear ownership of economic state and distinguish financial outcomes from physical outcomes.

The objective is to create an economy that responds to the simulated world rather than operating independently of it.

## 3. Foundational Economic Model

### 3.1 Currency and Economic Values

The simulation uses a single fictional global currency, the Dollar ($).

Economic reference prices and wages are calibrated to a 1955 baseline. These reference values provide a common economic foundation but do not automatically determine the price of every transaction.

The current design does not include foreign exchange. International economic differences are represented through defined economic conditions, prices, wages, taxation, trade restrictions, investment rules, and other applicable mechanics.

### 3.2 Economic Actors

Economic activity is conducted by entities with defined responsibilities, resources, and constraints.

The principal actors are:

- **Nations:** Political and economic jurisdictions containing economic activity and institutions.
- **Governments:** Public authorities responsible for fiscal policy, taxation, expenditure, regulation, and other authorised economic interventions.
- **Companies:** Commercial organisations that operate Industries, employ workers, purchase inputs, sell outputs, allocate capital, and make commercial decisions.
- **Industries:** Established economic operating entities associated with particular activities and geographic areas.
- **Population:** Individuals whose consumption, labour, income, qualifications, and economic circumstances contribute to the wider economy.
- **Markets:** Mechanisms through which economic supply, demand, prices, and eligible transactions interact.

Each actor operates within the authority, resources, and constraints defined by the relevant systems.

### 3.3 Economic Products and Services

The Economy System represents the commercial exchange of physical products and eligible Services.

Its economic product model distinguishes:

- **Goods:** Physical consumer and industrial products.
- **Services:** Outputs arising from activities rather than the transfer of a physical product.
- **Designed Components:** Physical components produced for incorporation into or use by other systems and products.
- **Armaments:** Military products intended for independent military use, deployment, operation, or installation.
- **Strategic Resources:** Designated physical resources and materials whose availability is important to economic and industrial activity.

These classifications support consistent treatment of economic demand, pricing, procurement, inventory, and production interfaces.

The canonical product identities and classifications are defined in the Good and Service Catalogue.

## 4. Core Economic Mechanics

### 4.1 Companies and Industries

Companies conduct commercial activity through their associated Industries.

Companies manage capital, employment, purchasing, selling, pricing, investment, and other commercial decisions. Their decisions are influenced by financial conditions, market opportunities, available resources, legal restrictions, and the characteristics of their controlling Senior Executives.

Industries represent established economic activities associated with geographic areas and physical production processes. They connect commercial activity to the physical world.

Industry establishment, expansion, contraction, and operation are subject to economic requirements and the capabilities of the Production System.

### 4.2 Supply, Demand, and Markets

Markets connect the availability of products and Services with the requirements of eligible buyers.

Demand may arise from population consumption, industrial input requirements, Government activity, military procurement, and international commerce.

Supply depends on available products and Services, including the outputs made available by production, existing inventories, imports, and other permitted sources.

Prices and allocation respond to the applicable market rules and economic conditions. Where supply is insufficient, competing demands must be resolved through defined allocation rules rather than allowing the same available quantity to satisfy multiple incompatible requirements.

A stated demand, commercial offer, or intended allocation does not automatically constitute a completed transaction or physical delivery.

### 4.3 Capital and Financial Performance

Companies require available capital to support authorised commercial and operating decisions.

Company Capital is distinct from revenue, profit or loss, physical inventory, and the Government's Treasury. Financial resources must enter or leave accounts through defined transactions.

The Economy System records the financial consequences of commercial activity, including revenue, expenditure, operating income, investment, and other applicable financial movements.

Financial performance influences subsequent commercial decisions, but financial outcomes do not automatically produce unrelated physical effects. For example, a financial loss does not independently destroy a Company, and additional capital does not automatically increase physical production capacity.

### 4.4 Labour and Wages

Economic activity requires workers with the relevant availability and qualifications.

The Population System supplies authoritative information about the available workforce and its characteristics. The Economy System determines how economic labour requirements are matched against eligible workers and how employment affects economic activity.

Wages and associated employment costs form part of Company expenditure and influence the financial conditions under which Industries operate.

The availability of workers does not guarantee that every Industry can employ the quantity or qualifications it requires.

### 4.5 Trade and International Commerce

Eligible economic entities may exchange products and Services through domestic and international transactions.

Trade is subject to applicable commercial terms, inventory availability, payment and delivery conditions, legal permissions, Government restrictions, and diplomatic arrangements.

International trade connects the economic conditions of different Nations without requiring currency conversion under the current single-Dollar model.

Trade agreements, restrictions, and sanctions affect economic activity only through their defined rules and interfaces.

### 4.6 Government Economic Intervention

Governments influence economic activity through authorised fiscal and regulatory actions.

These may include taxation, public expenditure, Industry establishment, subsidies, capital injections, procurement, price controls, trade restrictions, and other permitted interventions.

The Government System determines the relevant political and legal authority. The Economy System resolves the associated commercial and financial consequences.

Government intervention does not guarantee a particular economic outcome. For example, a subsidy may improve a Company's financial position, but increased production depends on the Company's decisions and the physical capabilities and constraints of the Production System.

## 5. Relationship with the Production System

The Economy and Production Systems are closely connected but have separate responsibilities.

The Economy System determines the commercial and financial conditions under which economic activity is planned and undertaken. It manages economic demand, Industry Scale, Company Capital, labour allocation, input procurement, commercial priorities, and financial performance.

The Production System determines what can physically be produced, the inputs and processes required, the available production capacity, the actual output achieved, and the physical consequences of production.

The systems exchange information about intended production, input requirements, available resources and inventory, physical capacity, actual output, and the associated economic consequences.

Neither system may substitute its own calculations for state owned by the other.

This separation ensures that economic demand does not automatically create physical goods, financial transactions do not create inventory, and physical production does not automatically generate revenue.

## 6. Relationship with Other Systems

The Economy System depends on explicit interfaces with the wider simulation.

| System | Relationship with Economy |
|---|---|
| Government | Supplies fiscal policy, legislation, taxation, subsidies, public expenditure authority, and economic restrictions. |
| Population | Supplies workforce availability, qualifications, population characteristics, and relevant consumption requirements. |
| Production | Supplies physical production capabilities, input requirements, actual output, and production-related state changes. |
| Inventory | Supplies authoritative physical quantities, locations, ownership, condition, reservations, and availability for economic allocation. |
| Research | Supplies applicable technological capabilities and requirements affecting economic and industrial activity. |
| Military | Supplies procurement demand and relevant requirements for Armaments and other military products. |
| Terrain | Supplies geographic references and physical-world context relevant to Industries and economic activity. |
| Diplomacy | Supplies applicable international permissions, agreements, and restrictions affecting commerce. |
| Character | Supplies Senior Executive characteristics used by Company decision-making. |

Where multiple systems participate in a mechanic, one system must own its authoritative state. Other systems interact with that state through defined interfaces.

The Economy System must identify dependencies on systems that are not yet fully specified without inventing their rules.

## 7. Economic Processing

Economic state changes through defined processing and transaction rules.

At a high level, economic processing considers the current state of the world, validates eligible activity, establishes available resources and labour, determines economic requirements, coordinates intended activity with physical production, resolves markets and transactions, records financial consequences, and updates economic actors.

The sequence must preserve dependencies between operations. For example, an Industry cannot treat an input as available merely because a purchase has been proposed, and a Company cannot recognise output as sold merely because it has been produced.

The detailed processing order, transaction rules, update frequencies, and calculation methods are specified in the authoritative Economy System specification and the relevant connected systems.

## 8. Player Interaction

The Player influences the economy through actions permitted by their political authority, legal framework, and position within the simulation.

Depending on those conditions, the Player may establish Industries, direct economic priorities, allocate public funds, provide subsidies, authorise capital injections, set procurement requirements, appoint or replace executives, enact economic legislation, regulate markets, and permit or restrict trade.

The Player does not automatically exercise direct operational control over every Company. Commercial decisions remain subject to the Company's authority, Senior Executive, available resources, legal constraints, and applicable directives.

This creates a distinction between influencing economic conditions and directly controlling individual commercial decisions.

## 9. Simulation Outcomes

The Economy System contributes to the changing condition of Nations and their economic participants.

Its outcomes include changes in Company financial health, market prices, labour allocation, product availability, commercial activity, trade relationships, Government finances, and the economic feasibility of industrial and military activity.

These outcomes influence other systems and are themselves affected by changes elsewhere in the simulation.

The Economy System therefore acts as the financial and commercial layer connecting population needs, Company decisions, Government policy, physical production, and international exchange.

## 10. Scope and Specification Boundaries

This document establishes the Economy System's purpose, principal entities, core mechanics, player interaction, and relationships with other systems.

It does not define every calculation, data structure, transaction state, product record, production recipe, or implementation detail.

The following documents provide the authoritative detail:

- `docs/systems/economy/economy-system.md` defines the formal economic rules, state ownership, financial mechanics, interfaces, processing dependencies, and validation requirements.
- `docs/systems/economy/industry-data.md` defines the data and rules required to represent Industries and their economic establishment and operating state.
- `docs/reference/economy/good-and-service-catalogue.md` defines canonical economic product and Service identities and classifications.
- `docs/systems/production/production-system.md` defines physical production behaviour and its interface with Economy.
- `docs/systems/inventory/inventory-system.md` defines authoritative physical inventory state and inventory operations.

Where a detailed system specification defines a mechanic, that specification is authoritative for its rules. This overview must remain consistent with those rules without duplicating their full implementation detail.
