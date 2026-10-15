
# Industry Data

## 1. System Definition

### 1.1 Purpose

Industry Data defines the canonical Industry Types, Industry instance configuration, establishment profiles, economic requirements, geographic associations, workforce requirements and cross-system references used to represent Industries in Project Sovereign.

It supplies the authoritative industrial definitions and configuration consumed by the Economy, Production and other simulation systems.

Industry Data does not independently simulate financial accounting, physical production, transformation of inputs into outputs, physical inventory or population. Those mechanics remain with their respective owning systems.

### 1.2 Core Responsibilities

Industry Data defines:

1. The 16 canonical Industry Types in the initial simulation.
2. The distinction between Industry Types and Industry instances.
3. Industry, Company and Nation references.
4. Reusable Industry Type configuration.
5. Industry establishment requirements and profiles.
6. Industry Establishment Capital Requirements and Opening Company Capital requirements.
7. Industry Area references and geographic eligibility requirements.
8. Workforce requirements and employment-category configuration.
9. Education, qualification and technology prerequisites.
10. Strategic Resource and other product dependencies.
11. Industry Scale configuration references.
12. Permitted production configuration references.
13. Industry establishment and configuration lifecycle rules.
14. Starting-world Industry data requirements.
15. Validation rules, data contracts and cross-system interfaces.

### 1.3 Design Principles

- **Definition versus instance:** Industry Types are reusable definitions; Industry instances are individual establishments in the world.
- **Single ownership of state:** every kind of authoritative simulation state has one owning system.
- **Explicit financial separation:** Nation Treasury and Company Capital are separate financial accounts.
- **Economic and physical separation:** Industry Scale is not physical Production Capacity or Actual Production.
- **Explicit geography:** each Industry references an Industry Area or an approved alternative geographic representation.
- **Explicit prerequisites:** establishment and operational requirements are configured and validated rather than inferred from an Industry's name.
- **Data-driven configuration:** configurable properties must be stored as canonical data rather than unexplained constants embedded in simulation code.
- **Deterministic resolution:** identical initial state, configuration and simulation inputs must produce identical results under the same simulation rules.
- **Versioned definitions:** changes to canonical configuration must not silently rewrite the historical interpretation of existing entities or completed operations.

### 1.4 Relationship to the Economy System

Industry Data owns canonical Industry Type definitions, reusable Industry configuration and establishment profiles.

Economy owns live economic state, including Industry Scale, Company Capital, revenue, expenditure, employment records, financial obligations, transactions and economic decisions.

Economy consumes Industry Data to validate and initialise Industry instances and to calculate their economic requirements.

Industry Data must not maintain a competing authoritative copy of live economic state. It may expose configuration values and derived reference data required by Economy.

### 1.5 Explicit Non-Responsibilities

| Mechanic | Authoritative owner |
|---|---|
| Industry Type definitions and establishment configuration | Industry Data |
| Live Industry economic state and Industry Scale | Economy |
| Company Capital, revenue, expenditure and accounting | Economy |
| Nation Treasury and public financial authority | Government |
| Canonical Facility identity, shared physical condition/lifecycle and generic facility capacity | Installations and Facilities |
| Production-specific process capability, throughput, allocation, orders and execution | Production |
| Actual production quantities | Production |
| Input-to-output transformation definitions | Production Processes and Recipes |
| Physical inventory, reservations, transfers and consumption | Inventory |
| Good and Service identity, classification and canonical units | Good and Service Catalogue |
| Population demographics and available labour | Population |
| Educational attainment | Population |
| Education policy, legislation and public funding | Government |
| Scientific research, technology and discoveries | Research |
| Geographic geometry and Terrain Objects | Terrain |
| Physical resource extraction and deposit depletion | Production and the authoritative resource model |
| Military organisation, requirements and combat | Military |
| Diplomatic relations and international permissions | Diplomacy |
| Physical transportation and logistics execution | Logistics and Transportation |
| Detailed Company AI algorithms | AI and Technical Specifications |

Industry Data may define a prerequisite that depends on another system, but the referenced system remains authoritative for the underlying state.

---

## 2. Foundational Definitions

### 2.1 Industry Type

An Industry Type is a reusable definition of a category of economic activity.

It specifies shared properties, including:

- Stable Industry Type identity.
- Display name and description.
- Primary economic function.
- Establishment profiles and requirements.
- Industry Establishment Capital Requirement.
- Opening Company Capital requirements.
- Workforce requirement configuration.
- Education and qualification requirements.
- Technology prerequisites.
- Geographic eligibility.
- Strategic Resource and other product dependencies.
- Industry Scale configuration references.
- Permitted production configuration references.
- Operational requirements.
- Required system interfaces.

An Industry Type is a definition, not a physical establishment.

### 2.2 Industry Instance

An Industry instance is an individual industrial establishment represented in the simulation.

An Industry is not a Facility. An Industry may use one or more canonical Facilities, and a Facility may serve one or more Industries where the relevant legal and operational rules permit it. Industry Data must reference canonical Facility IDs and must not create or duplicate Facility identity, shared condition/lifecycle, ownership/operator/control, or generic capacity.

Each committed instance references exactly one canonical Industry Type and includes or references:

- A unique Industry ID.
- An Industry Type ID.
- An associated Company ID.
- A relevant Nation or economic jurisdiction.
- An Industry Area or approved geographic representation.
- An Industry configuration or establishment profile.
- Live Industry Scale, owned by Economy.
- Establishment and operational state.
- Workforce and employment records.
- Required technology and resource dependencies.
- Permitted production configurations.
- Relevant facilities and inventory locations.

An Industry is not identical to its Company, Industry Area, production facilities or inventory holdings.

### 2.3 Company

A Company is an economic organisation that owns or operates one or more Industries under the applicable legal and ownership rules.

Economy owns Company financial state, including Company Capital.

A Company may operate multiple Industries and may operate across multiple Nations where permitted.

Every Industry instance must reference exactly one associated Company at a given point in time. Changes to that association require an authorised transfer or restructuring operation.

Company ownership, executive control and operating responsibility are related but distinct concepts. The default Senior Executive relationship is defined in Section 2.5 and must not override an explicitly modelled legal ownership or control structure.

### 2.4 Nation

A Nation is the political and economic jurisdiction relevant to an Industry's operation.

Each Industry must reference one relevant Nation for jurisdictional and economic purposes.

The Nation association informs applicable:

- Government authorisation.
- Laws and regulations.
- Taxation.
- Labour availability.
- Infrastructure access.
- Economic statistics.
- Trade restrictions.
- Industrial policy.

A Company operating across multiple Nations must identify the relevant Nation for each Industry. A Company's home jurisdiction does not automatically determine the jurisdiction of all its Industries.

### 2.5 Senior Executive

Under the default Company model, the Senior Executive is the principal executive decision-maker and default controller of a Company.

The Senior Executive is associated with the Company, not independently assigned to every Industry it operates.

Legal ownership, beneficial ownership, executive authority and operational control must remain distinct where the simulation models them separately. An explicit legal or institutional arrangement may override the default control model.

Industry Data references the associated Company and does not duplicate executive identity, attributes or decision-making state on each Industry instance.

The rules governing Character identity, succession, appointment, removal and Company AI decision-making belong to the relevant Character, Government, Economy and AI specifications.

### 2.6 Industry Area

An Industry Area is the geographic representation associated with an Industry instance.

It uses the Terrain System's Area Mapping Primitive or another explicitly approved geographic representation.

An Industry Area represents the geographic association of an Industry. It does not independently determine Industry Scale, workforce, financial state, physical Production Capacity or Actual Production.

### 2.7 Industry Establishment

Industry Establishment is the authorised process through which a new Industry instance is created and its required organisational and financial arrangements are committed.

Establishment must validate all mandatory requirements in the applicable Industry Type configuration and establishment profile.

Successful establishment creates a valid Industry instance and its required references. It does not guarantee that the Industry can immediately operate or produce at its intended Scale.

### 2.8 Industry Scale

Industry Scale represents the intended economic magnitude at which an Industry operates.

It informs economic requirements such as labour demand, input demand, capital requirements and intended production activity.

Industry Scale is distinct from physical Production Capacity, Actual Production, employee count, Industry Area size, facility count and Company Capital.

Economy owns the live Industry Scale value and its calculation rules. Industry Data supplies the relevant configuration.

---

## 3. Industry Type Catalogue

### 3.1 Initial Industry Taxonomy

Project Sovereign contains exactly 16 Industry Types in the initial Industry taxonomy.

| ID | Industry Type | Primary economic function |
|---|---|---|
| `transportation` | Transportation | Commercial movement of people and goods through transport services and associated operations. |
| `oil` | Oil | Extraction and supply of petroleum. |
| `construction` | Construction | Construction, repair and development of buildings, facilities and infrastructure. |
| `automotive` | Automotive | Production of road vehicles and associated automotive products. |
| `aerospace` | Aerospace | Production of aircraft, aerospace vehicles and associated products. |
| `maritime` | Maritime | Production of ships, boats and associated maritime products. |
| `energy_production` | Energy Production | Production and commercial supply of non-nuclear energy. |
| `industrial_manufacturing` | Industrial Manufacturing | Production of industrial equipment, machinery, materials and other industrial products not assigned to a more specific Industry. |
| `mining` | Mining | Extraction of designated mineral and geological resources. |
| `nuclear_energy` | Nuclear Energy | Production and commercial supply of nuclear energy. |
| `electronics` | Electronics | Production of electronic equipment, electrical components and associated products. |
| `steelmaking` | Steelmaking | Production of steel and associated steel products. |
| `space` | Space | Production of space vehicles, launch systems and associated space products. |
| `agribusiness` | Agribusiness | Processing, packaging, storage and commercial distribution of agricultural outputs. |
| `consumer_goods` | Consumer Goods | Production of household and personal-use Goods not assigned to a more specific Industry. |
| `agriculture` | Agriculture | Primary agricultural production and associated agricultural outputs. |

The IDs are stable machine-readable identifiers. Display names may be localised without changing the IDs.

The initial taxonomy must not silently add, remove, merge or rename Industry Types through ordinary scenario configuration. Changes to the canonical taxonomy require an explicit data-model and compatibility decision.

### 3.2 Industry Type Versus Product Classification

An Industry Type defines economic specialisation. It does not define a complete list of every Good or Service an Industry may produce.

One Industry Type may support multiple products, and one product may be produced by multiple Industry Types where the relevant production configurations permit it.

Product identities, economic classifications, physical form and canonical units belong to the Good and Service Catalogue.

Input-to-output transformation definitions belong to Production Processes and Recipes.

Industry Data must reference these definitions rather than duplicating them.

### 3.3 Transportation

Transportation represents commercial movement of people and goods and associated transport services.

Its economic requirements may include qualified labour, transport equipment, access to routes, energy or fuel, maintenance, infrastructure and Government authorisation.

Vehicle capacities, route capacities, movement constraints and physical delivery belong to the relevant Production, Logistics and Transportation systems.

Transportation may provide a Service without directly producing a physical Good.

### 3.4 Oil

Oil represents petroleum extraction and supply.

An Oil Industry may require access to a suitable petroleum deposit where its configured production activity involves extraction.

Establishment requirements must explicitly identify any mandatory deposit, technology, geographic eligibility, legal permission or infrastructure prerequisites.

The existence of petroleum does not automatically establish an Oil Industry or make the resource available for consumption.

Physical extraction and deposit depletion belong to Production and the authoritative resource model. Extracted physical stock belongs to Inventory.

### 3.5 Construction

Construction represents economic activity associated with building, repairing and developing structures and infrastructure.

A Construction Industry may provide Services or produce defined construction-related Goods through configured production processes.

Industry Data defines its economic configuration and prerequisites. Production and the relevant infrastructure systems govern physical construction, project completion, facility creation and infrastructure modification.

Establishing a Construction Industry does not automatically construct a facility.

### 3.6 Automotive

Automotive represents production of road vehicles and associated automotive products.

Its requirements may include qualified labour, industrial inputs, technology, infrastructure and appropriate physical facilities.

Vehicle designs and physical production configurations must be referenced through the relevant Research, Catalogue, Production and Military systems. The Industry Type does not independently define vehicle designs or recipes.

### 3.7 Aerospace

Aerospace represents production of aircraft, aerospace vehicles and associated products.

Its requirements may include specialised technology, qualified labour, industrial inputs, infrastructure and physical facilities.

Technology prerequisites must reference canonical Research System identifiers. Aircraft specifications, component designs and military eligibility belong to the relevant Research, Catalogue and Military systems.

### 3.8 Maritime

Maritime represents production of ships, boats and associated maritime products.

Its requirements may include geographic suitability, transport access, industrial inputs, qualified labour, technology and suitable facilities.

Geographic eligibility must be explicitly configured. A coastline, navigable waterway or port is not automatically mandatory for every possible maritime activity.

Physical shipbuilding and vessel creation belong to Production and the relevant civilian asset or Military systems.

### 3.9 Energy Production

Energy Production represents production and commercial supply of non-nuclear energy.

Its requirements depend on the configured generation method and may include fuel, water, infrastructure, qualified labour, technology and geographic suitability.

The Industry Type does not represent a single physical generation technology. Generation methods, conversion efficiency, input consumption and actual energy output belong to the relevant Production and energy systems.

### 3.10 Industrial Manufacturing

Industrial Manufacturing represents production of industrial equipment, machinery, materials and other industrial products not assigned to a more specific Industry Type.

Each Industry instance must reference the applicable production configurations. It must not assume that every Industrial Manufacturing establishment can produce every industrial Good.

Equipment, facilities, inputs and physical output capacity belong to Production. Product identities belong to the Catalogue.

### 3.11 Mining

Mining represents extraction of designated mineral and geological resources.

Its establishment requirements depend on the relevant resource, deposit and extraction configuration.

A Mining Industry may operate one or more extraction operations where permitted. A geological deposit does not itself establish an Industry, create an extraction operation or create physical inventory.

Resource quantities, extraction limits and depletion belong to the authoritative resource model and Production.

### 3.12 Nuclear Energy

Nuclear Energy represents production and commercial supply of nuclear energy.

Its establishment requirements may include technology, Government authorisation, qualified labour, infrastructure, physical facilities, required inputs and applicable safety or environmental permissions.

Mandatory prerequisites must be explicitly configured. The Industry Type does not automatically confer nuclear technology, materials, qualified workforce or operating facilities.

Physical generation, input consumption, facility state and Production Capacity belong to the relevant Production and energy systems.

### 3.13 Electronics

Electronics represents production of electronic equipment, electrical components and associated products.

Its requirements may include specialised technology, qualified labour, industrial inputs, energy and appropriate facilities.

Product identities belong to the Catalogue. Transformation processes and input/output quantities belong to Production Processes and Recipes.

### 3.14 Steelmaking

Steelmaking represents production of steel and associated steel products.

Its requirements may include appropriate facilities, qualified labour, energy, industrial inputs, infrastructure and applicable technology.

Steelmaking is distinct from Mining. Mining extracts designated geological resources; Steelmaking transforms configured inputs into steel and associated products.

The physical process, required raw materials, capacity and output quantities belong to Production and the relevant recipe definitions.

### 3.15 Space

Space represents production of space vehicles, launch systems and associated space products.

Its establishment requirements may include specialised technology, qualified labour, appropriate facilities, industrial inputs and Government authorisation.

The Industry Type does not automatically grant spaceflight capability, launch infrastructure or advanced technology. Research owns technological knowledge; Production owns physical facilities and production capability.

### 3.16 Agribusiness

Agribusiness represents processing, packaging, storage and commercial distribution of agricultural outputs.

It is distinct from Agriculture. Agriculture produces primary agricultural outputs; Agribusiness may acquire those outputs and transform them into processed Goods or provide associated Services.

Agribusiness does not automatically produce agricultural inputs merely because it is established near agricultural land. Its inputs and outputs must be defined through the Catalogue and production configuration.

### 3.17 Consumer Goods

Consumer Goods represents production of household and personal-use Goods not assigned to a more specific Industry Type.

An Industry instance may produce multiple configured consumer products.

Its establishment and operating requirements depend on its configuration, workforce, technology, inputs and physical facilities.

Consumer Goods does not independently define consumer demand, prices or household purchasing behaviour. Those mechanics belong to Economy, Population and Production.

### 3.18 Agriculture

Agriculture represents primary agricultural production associated with an eligible geographic Industry Area.

Its requirements may include suitable land, environmental conditions, qualified labour, agricultural inputs, technology, water and Government permissions where explicitly required.

Agriculture may produce agricultural outputs and interact with the Rich Soils Strategic Resource where its configured production model requires it.

Rich Soils availability, agricultural yield and physical extraction must be governed by the relevant geographic, resource and Production rules. Rich Soils are not a universal prerequisite for all agricultural activity.

---

## 4. Industry Instance Data Model

### 4.1 Required Identity Fields

Every committed Industry instance must contain or reference the following fields.

| Field | Requirement | Description |
|---|---|---|
| `industry_id` | Required | Unique, stable identifier for the Industry instance. |
| `industry_type_id` | Required | Reference to one of the 16 canonical Industry Types. |
| `company_id` | Required | Reference to the associated Company. |
| `nation_id` | Required | Reference to the relevant economic jurisdiction. |
| `industry_area_id` | Required unless an approved alternative applies | Reference to the Industry Area or geographic representation. |
| `establishment_state` | Required | State of establishment or closure. |
| `operational_state` | Required | Current operational classification. |
| `industry_configuration_id` | Required | Reference to the configuration governing permitted activity. |
| `industry_scale` | Required in committed economic state | Live Industry Scale, owned by Economy. |

An Industry instance is invalid if a mandatory reference cannot be resolved.

An implementation may temporarily construct an incomplete entity in memory, but an incomplete entity must not be committed to authoritative simulation state.

### 4.2 Optional or Conditional References

Depending on its configuration, an Industry may reference:

- One or more production configurations.
- One or more physical facilities.
- One or more inventory locations.
- One or more resource deposits.
- Required infrastructure connections.
- Required technology identifiers.
- Industry-specific licences or authorisations.
- Employment records.
- Market participation.
- Construction or expansion projects.

References must use stable identifiers defined by the owning systems. Industry Data must not embed independent copies of another system's authoritative state.

### 4.3 Industry Identity

An Industry ID remains stable throughout the instance's lifetime.

Changes to display name, Industry Scale, Company association, geographic location or operational status must not silently generate a new ID. A new ID is required only when the relevant mechanic creates a genuinely new Industry instance.

IDs must not depend solely on display names, Company names, Industry Type names, geographic coordinates, array positions or current Scale.

### 4.4 Industry Type Configuration Versus Instance State

Industry Type configuration defines reusable properties. Instance state records the configuration and conditions of a specific establishment.

| Configuration property | Instance state |
|---|---|
| Canonical Industry Type ID | Industry Type reference |
| Default establishment requirements | Validated establishment result |
| Establishment Capital Requirement | Recorded establishment expenditure |
| Opening Company Capital requirement | Actual opening capital transfer |
| Default workforce requirements | Current workforce requirements and employment references |
| Scale configuration | Live Industry Scale |
| Geographic eligibility | Validated geographic association |
| Required technology identifiers | Verified prerequisite state |
| Permitted production configurations | Selected configuration references |

An Industry instance must not overwrite canonical Industry Type configuration when its own state changes.

Any permitted instance-specific override must be explicit, validated and versioned.

### 4.5 Industry State Separation

The following states must remain distinct:

- Establishment state.
- Operational state.
- Industry Scale.
- Workforce demand and fulfilment.
- Company financial state.
- Canonical Facility identity and shared physical condition/lifecycle, owned by Installations and Facilities.
- Production-specific process capability and throughput, owned by Production.
- Actual Production.
- Inventory availability.
- Geographic Industry Area condition.

A change to one state must not implicitly change another unless a defined rule or interface requires it.

---

## 5. Industry Scale

### 5.1 Definition

Industry Scale represents the intended economic magnitude at which an Industry operates.

It informs the Industry's labour demand, input demand, capital requirements, intended production activity, potential output value, potential revenue, potential operating expenditure and other explicitly configured economic requirements.

Industry Scale is a live economic property owned by Economy. Industry Data provides its configuration.

### 5.2 Distinction from Physical Production

Industry Scale, Physical Production Capacity and Actual Production are separate values:

`Industry Scale ≠ Physical Production Capacity ≠ Actual Production`

An Industry may have a high intended Scale but insufficient facilities, inputs, energy or labour to achieve its intended activity.

An Industry may also possess physical facilities capable of supporting more production than its current economic Scale intends.

Production determines physical capacity and actual output. Economy determines economic intent and Industry Scale.

### 5.3 Scale Inputs

The Scale configuration must explicitly identify which of the following influence Scale and which constrain only intended activity or actual production:

1. Industry Type and its Scale rules.
2. Available Company Capital.
3. Available labour.
4. Qualified labour availability.
5. Required input availability.
6. Strategic Resource access, where applicable.
7. Relevant technology.
8. Government restrictions and permissions.
9. Infrastructure access.
10. Establishment and operational state.
11. Senior Executive decisions.
12. Applicable economic policy.
13. Physical constraints reported by Production.
14. Other explicitly configured Industry-specific conditions.

Not every factor must directly modify Scale.

### 5.4 Scale Calculation

The Economy System's Scale specification must define:

- Canonical representation and units.
- Permitted minimum and maximum values.
- Source of each input.
- Mathematical operation and order of operations.
- Update frequency.
- Treatment of missing data.
- Treatment of insufficient resources.
- Government restrictions.
- Changes in Company Capital.
- Expansion and contraction.
- Any smoothing, hysteresis or adjustment limits.
- Rounding rules.

Undefined parameters such as `Scale Modifier` or `Economic Efficiency` must not be used without explicit definitions.

### 5.5 Scale Changes

Scale may increase or decrease as economic conditions, Company decisions and applicable rules change.

A change may alter labour demand, intended input demand, intended production, expected expenditure, potential revenue or infrastructure requirements where explicitly configured.

A Scale change does not automatically construct, acquire, upgrade or destroy physical facilities. Those changes require the relevant Production, construction or asset-management operations.

### 5.6 Scale and Establishment

The initial Industry Scale must be explicitly supplied by the establishment profile, scenario data or a valid calculation rule.

An Industry must not receive an arbitrary Scale merely because it has been created.

If mandatory Scale data cannot be resolved, establishment or initialisation must fail unless a defined initialisation rule provides a valid result.

---

## 6. Industry Establishment and Financing

### 6.1 Establishment Authority

Industry establishment is subject to Government authority and Economy's financial and transaction rules.

Government owns the Nation Treasury and the public authority to approve or prohibit establishment. Industry Data defines the required prerequisites and configuration. Economy validates and records the associated economic and financial state changes.

The precise political authority to propose, approve, restrict or prohibit establishment belongs to Government.

### 6.2 Industry Establishment Capital Requirement

Each applicable Industry Type establishment profile must define an Industry Establishment Capital Requirement denominated in Dollars.

The requirement represents the specified establishment expenditure associated with creating an Industry.

It is distinct from:

- Opening Company Capital.
- Ongoing operating expenditure.
- Capital required to expand Industry Scale.
- Physical facility construction costs unless explicitly included.
- Equipment acquisition costs unless explicitly included.
- Input procurement.
- Inventory acquisition.
- Future maintenance or replacement expenditure.

The requirement is not a measure of physical Production Capacity or facility value.

### 6.3 Establishment Expenditure

The establishment profile must identify the financial treatment of the Industry Establishment Capital Requirement.

The resulting movement must identify:

- Source account.
- Destination account or expenditure category.
- Amount and currency.
- Transaction purpose.
- Establishment request.
- Effective simulation date.
- Resulting financial record.

The payment must not be silently discarded or converted into Company Capital, physical facility value or inventory.

Government authorises public expenditure; Economy records its financial effect under the applicable accounting rules.

### 6.4 Opening Company Capital

An establishment profile must explicitly define whether Opening Company Capital is required and, if required, its amount.

Opening Company Capital is transferred from the Nation Treasury to the associated Company's Company Capital account.

It is distinct from establishment expenditure. Its amount must not be assumed to equal the Industry Establishment Capital Requirement.

Company Capital is a financial balance. It is not physical inventory, Production Capacity, facility value or a separate Industry-level cash account.

### 6.5 Total Establishment Outlay

Where establishment expenditure and Opening Company Capital are both required:

`Total Establishment Outlay = Establishment Expenditure + Opening Company Capital Allocation`

Any additional establishment payments must be defined and recorded as separate components.

Each component must be recorded exactly once. An amount must not be treated simultaneously as both expenditure and a transfer of spendable funds unless the accounting model explicitly supports and reconciles both effects.

### 6.6 Establishment Prerequisites

Every establishment profile must define its mandatory prerequisites.

Depending on the Industry Type, these may include:

- Sufficient Nation Treasury funds.
- Required Opening Company Capital.
- Geographic eligibility.
- Strategic Resource access.
- Required technology.
- Infrastructure access.
- Qualified labour availability.
- Government authorisation.
- Legal permissions.
- Environmental restrictions.
- Valid production configuration.
- Required facilities, where explicitly mandatory at establishment.
- Other explicitly configured requirements.

Each prerequisite must identify the condition, whether it is mandatory, its authoritative owner, the validation rule and the result of failure.

A prerequisite is not satisfied merely because a similarly named entity exists somewhere in the world.

### 6.7 Strategic Resource Prerequisites

Strategic Resource access is not a universal prerequisite for establishing an Industry.

Where a resource is required, the configuration must identify the resource and the necessary access condition. These may include:

- Geographic eligibility to operate at a deposit.
- A valid extraction operation.
- Legal access.
- An available supply route.
- Access to extracted and inventoried stock.

These conditions are distinct. A deposit may exist without an Industry operating there. An Industry may exist without extracting the resource. Extracted stock may be inaccessible because it has not been delivered to a usable inventory location.

Production and Inventory determine physical extraction, stock and delivery. Industry Data defines any prerequisite that depends on those conditions.

### 6.8 Establishment Validation

Before authoritative state changes, validation must confirm:

- The Industry Type and establishment profile exist.
- The proposed Industry ID is valid and unused.
- The Company association is valid or can be created through the authorised operation.
- The Nation association is valid.
- The Industry Area is valid or can be created through the authorised geographic operation.
- All mandatory prerequisites are satisfied.
- Establishment expenditure is defined.
- Opening Company Capital requirements are defined.
- The Nation Treasury can fund all required movements.
- Initial Industry Scale can be established.
- Required system references can be resolved.
- The resulting state will satisfy all mandatory invariants.

A failed mandatory validation rejects the establishment request.

### 6.9 Atomic Establishment

Industry establishment is one atomic logical operation across all required entity and financial changes.

On success, the operation commits all required entity references and financial movements.

On failure, it must leave no partial authoritative state.

A failed attempt must not:

- Create a partially registered Industry.
- Create an orphaned Company.
- Deduct Treasury funds.
- Transfer Opening Company Capital.
- Create an invalid Industry Area.
- Create partial physical facilities.
- Leave incomplete production or inventory references.

If a Company must be created as part of establishment, that creation must be included in the atomic operation. If the Company is created separately, the process must validate its eligibility.

Physical facility creation must use the relevant Production or construction interface. Establishing an Industry must not independently create facilities unless the establishment configuration explicitly invokes that authorised operation.

The Technical Specifications must define transaction boundaries, rollback and save-state recovery.

### 6.10 Starting-World Initialisation

Industries loaded from scenario starting data are not new establishment events.

They must not incur retroactive establishment expenditure merely because the simulation loads them.

Starting-world data must provide valid Industry, Company, Nation, geographic and configuration references together with the required starting financial and operational state.

Initial balances must be distinguished from financial transactions occurring after simulation begins.

---

## 7. Industry Geographic Representation

### 7.1 Geographic Association

Every Industry must reference an Industry Area or an approved alternative geographic representation.

The representation uses the Terrain System's Area Mapping Primitive and relevant geographic data structures.

Industry Areas represent geographic association, not Industry Scale or physical Production Capacity.

### 7.2 Industry Area Properties

Industry Data references geographic properties owned by Terrain, including applicable:

- Geometry.
- Elevation and height references.
- Terrain effects.
- Spatial overlap and containment.
- Geographic identifiers.
- Area-condition attributes, where defined.

The Terrain Objects specification governs the meaning, permitted range and simulation effects of those properties. Industry Data must not redefine their mathematical behaviour.

### 7.3 Industry-Specific Geographic Properties

Industry configuration may define requirements for:

- Permitted terrain categories.
- Minimum or maximum area.
- Coastline or navigable-waterway access.
- Transport infrastructure access.
- Energy infrastructure access.
- Geographic restrictions.
- Environmental suitability.
- Proximity.
- Resource deposit access.
- Other explicitly configured geographic conditions.

Every requirement must identify its source data and validation rule. Requirements must not be inferred from the Industry Type name alone.

### 7.4 Industry Area Size

Industry Area size and Industry Scale are independent.

A large Industry Area does not automatically indicate a large Industry Scale, and a small area does not automatically indicate a small Scale.

The geographic footprint must satisfy the relevant eligibility rules. Economy determines Industry Scale. Production consumes geographic constraints only through explicit interfaces.

### 7.5 Multiple Facilities

An Industry may use multiple physical facilities where its production configuration permits this.

The Industry Area identifies the geographic association of the Industry. Individual facilities belong to the Production System's physical entity model.

An Industry Area is not a single production facility, and a physical facility is not automatically a separate Industry instance.

Relationships among Industry, Industry Area and facilities must use explicit identifiers.

### 7.6 Geographic Overlap

Industry Areas may overlap with other geographic objects where Terrain permits it.

Overlap does not imply shared ownership, Company Capital, Production Capacity or inventory.

Effects arising from overlap are governed by Terrain and the relevant receiving systems.

### 7.7 Area Condition and Facility Condition

The condition of an Industry Area and the condition of physical facilities remain distinct.

An Industry Area may remain geographically valid while facilities are damaged. A facility may remain intact while access routes or surrounding infrastructure are disrupted.

Operational eligibility must be evaluated from the relevant conditions without collapsing them into one ambiguous health value.

---

## 8. Workforce Requirements

### 8.1 Workforce Ownership

Population owns population demographics, Population Types, available labour and population-level educational attainment.

Industry Data defines the workforce requirements against which labour is assessed.

Economy owns Industry labour demand, employment allocations and their financial consequences.

Industry Data must not independently create population, education or qualified workers.

### 8.2 Workforce Requirement Definition

Each Industry Type must define or reference a workforce requirement model specifying:

- Relevant employment categories.
- Required labour quantities or calculation rules.
- Canonical Population Types or approved employment categories.
- Required education levels, where applicable.
- Required professional qualifications, where applicable.
- Explicitly modelled workforce skills.
- Relationship between Industry Scale and labour demand.
- Applicable wage configuration.
- Labour fulfilment calculation.

Not every Industry Type requires every category or qualification.

### 8.3 Employment Composition

An Industry's employment composition is represented by its workers across relevant Population Types and employment categories.

Population defines the canonical Population Types. Industry Data defines required categories. Economy records the actual employment allocation.

Possible categories include Rural Labourers, Industrial Labourers, Professional Class, Civil Servants, Academics and Researchers, and other explicitly defined categories.

These examples do not establish that every category exists in every scenario or is required by every Industry.

### 8.4 Required Labour

Industry labour demand is derived from the applicable workforce configuration and current Industry Scale.

The calculation must define:

- Labour units.
- Relationship between Scale and required workers.
- Treatment of different employment categories.
- Fractional or part-time labour, if supported.
- Education and qualification requirements.
- Treatment of labour shortfalls.
- Update frequency.
- Rounding rules.

Undefined parameters such as `Labour Multiplier` must not be used.

### 8.5 Available Labour

Population supplies available labour. Total population must not be treated as equivalent to available workforce.

Labour allocation must account for the applicable population, labour-force participation and qualification rules.

An Industry cannot receive allocations exceeding the available eligible labour or violate the Population System's allocation rules.

The detailed allocation algorithm belongs to the Economy and Population interface specifications.

### 8.6 Labour Fulfilment

Labour Fulfilment represents the extent to which an Industry's defined labour requirements are met.

Where a normalised ratio is used:

`0 ≤ Labour Fulfilment ≤ 1`

Under this representation, `1.00` represents 100% fulfilment, `0.75` represents 75% and `0.00` represents no allocated labour.

For multiple employment categories, the calculation must define how category-specific fulfilment is aggregated. A simple average must not be assumed universally correct.

Labour Fulfilment is an economic workforce measure. Production defines its effects on physical output.

### 8.7 Employment Changes

Hiring and releasing workers must update the respective authoritative states in Economy and Population.

Hiring must not create population. Released workers return to the available labour pool according to Population rules.

Released workers do not automatically become individually simulated unemployed Characters unless that mechanic is explicitly defined elsewhere.

### 8.8 Wages

Workforce configuration must reference the applicable base wage data for each employment category.

Where the wage model uses annual gross salary:

`Daily Gross Salary = Annual Gross Salary / 365`

The simulation calendar contains exactly 365 days per year.

Where actual salaries depend on economic conditions, the wage calculation must identify each input and its mathematical effect. Undefined wage modifiers are not permitted.

Economy calculates wage expenditure and records resulting financial movements. Industry Data does not maintain a separate wage-payment account.

### 8.9 Education Requirements

An Industry Type may define minimum education requirements for employment categories.

Government owns education policy, legislation, public funding and institutional governance. Population owns individual educational attainment. Research owns scientific and technological knowledge.

Industry configuration must reference canonical education and qualification identifiers from the relevant systems. It must not create a parallel education hierarchy.

### 8.10 Insufficient Qualified Labour

Where qualified labour is insufficient, the actual employment allocation must reflect the shortfall.

The economic consequences must be explicitly defined. The shortfall must not automatically create qualified workers, grant technology or change education policy.

Production determines the physical production consequences.

---

## 9. Strategic Resource and Input Dependencies

### 9.1 Resource Dependency Definition

An Industry Type may define dependencies on Strategic Resources or other Goods and Services.

Each dependency must specify:

- Canonical resource or product ID.
- Whether it is an establishment prerequisite or operating requirement.
- Required geographic or logistical conditions.
- Required access condition.
- Authoritative validation system.
- Mandatory or optional status.
- Defined consequences of a shortfall.

Industry Data must not embed a duplicate product definition.

### 9.2 Initial Strategic Resource Taxonomy

The initial Economy taxonomy defines exactly eight Strategic Resources.

| Strategic Resource | Associated Industry Type | Resource classification |
|---|---|---|
| Petroleum | Oil | Extracted resource |
| Steel | Steelmaking | Produced strategic material |
| Aluminium | Industrial Manufacturing | Produced strategic material |
| Coal | Mining | Extracted resource |
| Tungsten | Mining | Extracted resource |
| Uranium | Mining | Extracted resource |
| Rare-earth Metals | Mining | Extracted resource |
| Rich Soils | Agriculture | Extracted agricultural resource |

These associations are retained as canonical initial configuration. They identify the designated Industry Type associated with the resource's extraction or production role, not a guarantee that every Industry instance of that type can produce it.

Steel and Aluminium are produced strategic materials, not naturally occurring geological deposits. Rich Soils is a deliberate simulation abstraction represented as an agricultural resource.

Product identity and classification belong to the Catalogue. Physical extraction and production belong to Production. Physical stock belongs to Inventory. Economy determines economic demand, prices and transactions.

### 9.3 Resource Deposits

A Resource Deposit is a geographic source of a designated resource.

Its authoritative data must define the resource identity, geographic location and physical resource state required by the resource and Production models.

Industry Data may reference a deposit where establishment or operation depends on it. It must not independently maintain deposit quantity, extraction capacity, extraction efficiency or depletion state.

### 9.4 Extraction Versus Availability

The existence of a deposit does not mean that its resource is available for consumption.

The resource must be extracted or made available through an explicitly defined mechanism. Extracted stock must be represented by Inventory.

Access may depend on extraction, ownership, legal permissions, logistics, reservation and delivery.

### 9.5 Input Demand

Industry Data may define configuration needed to calculate intended input demand.

Specific recipe inputs, transformation ratios, process constraints and physical quantities consumed belong to Production Processes and Recipes and Production.

Industry Scale may inform intended demand, but Industry Data must not independently determine actual physical consumption.

### 9.6 Input Shortages

Input shortages must be communicated through the appropriate Economy, Production and Inventory interfaces.

Economy may reflect commercial constraints. Production determines whether physical production can proceed and the output physically produced. Inventory owns stock, reservations, transfers, delivery and consumption.

Industry Data must not directly decrement physical stock or resource deposits.


---

## 10. Industry Operating and Economic State

### 10.1 Establishment State

Each Industry instance must maintain a defined establishment state.

The initial model distinguishes:

- `pending_establishment`: a request exists but has not been committed as an Industry instance.
- `established`: the Industry has been successfully created.
- `closed`: the Industry has been formally closed under an authorised mechanism.

`rejected` is a valid establishment-request outcome, but a rejected request must not remain as a partially established Industry instance. Request status and persistent Industry state should be represented separately where appropriate.

The Economy and Technical Specifications must define closure, historical records and reopening transitions.

### 10.2 Operational State

Operational state represents the Industry's current operating classification.

The canonical state model must distinguish whether an Industry:

- Is eligible to operate.
- Is operating under normal conditions.
- Is operating under constraints.
- Is temporarily suspended.
- Is unable to operate.
- Has permanently ceased operation.

Canonical enum values and transition rules must be defined by the relevant Economy and Production interfaces.

Operational state must not substitute for Industry Scale, facility condition, Production Capacity or Actual Production.

### 10.3 Operational Eligibility

An Industry's eligibility to operate depends on the applicable economic, legal, workforce and physical conditions.

Relevant conditions may include:

- Valid establishment state.
- Valid Company association.
- Applicable legal permissions.
- Available workforce.
- Required input availability.
- Required technology.
- Physical facility availability.
- Infrastructure access.
- Energy availability.
- Physical Production Capacity.
- Geographic and environmental constraints.

Industry Data defines the configured requirements. The owning systems provide authoritative state for validation.

### 10.4 Company Financial State

An Industry references its associated Company's financial state. It does not maintain an independent Company Capital balance.

Economy owns:

- Company Capital.
- Revenue.
- Expenditure.
- Operating Income.
- Financial obligations where modelled.
- Commercial transaction history.

Industry Data may define economic activity categories and configuration used by Economy but must not duplicate Company accounts.

### 10.5 Revenue

Revenue arises from qualifying economic transactions under Economy's accounting rules.

Production alone does not create revenue. Increasing the value of unsold Inventory does not automatically create revenue.

A sale must reference the relevant seller, buyer where applicable, product or Service, quantity, Transaction Price and qualifying transaction state.

Economy calculates and records revenue.

### 10.6 Expenditure

Industry-related expenditure may include:

- Labour.
- Inputs.
- Operating costs.
- Government taxes and charges.
- Financing costs.
- Other explicitly defined expenditure.

Economy owns the applicable accounting categories and financial treatment.

Consumption of an already-owned input does not automatically create a new cash purchase. Inventory ownership, physical consumption and financial expenditure remain distinct.

### 10.7 Operating Income

Where the Economy accounting model defines Operating Income as Revenue less Operating Expenditure:

`Operating Income = Revenue - Operating Expenditure`

The included expenditure categories must be explicit.

Positive Operating Income represents an operating surplus under this definition. Negative Operating Income represents an operating loss.

An operating loss does not automatically close an Industry, destroy its facilities or liquidate its Company. Financial distress, insolvency, bailout, closure and liquidation require explicit Economy and Government rules.

---

## 11. System Interfaces

### 11.1 Interface Principles

Industry Data must identify the authoritative source of every external dependency.

Interfaces must use stable identifiers, explicit units and defined update semantics. A receiving system must not silently replace or reinterpret another system's authoritative state.

Where an interface is not fully specified, the required information must be documented without inventing the receiving system's internal algorithm.

### 11.2 Economy System Interface

Economy consumes Industry Type configuration and owns the live economic state of Industry instances.

It uses Industry Data to:

- Validate establishment.
- Resolve Industry Type and establishment-profile configuration.
- Associate Industries with Companies and Nations.
- Initialise Industry Scale.
- Calculate labour demand and employment state.
- Determine economic operating requirements.
- Calculate economic input demand where specified.
- Record revenue, expenditure and Operating Income.
- Resolve financial transactions.
- Aggregate economic statistics.

Economy must not use Industry Data as a substitute for physical production or inventory state.

### 11.3 Production System Interface

Production consumes Industry identity, type, geographic association, selected production configuration and relevant economic intent. It references canonical Facility IDs through the Installations and Facilities System and must not create a competing facility identity.

The interface must support exchange of:

- Industry identity and Industry Type.
- Relevant geographic references.
- Intended production activity.
- Scale-derived economic requirements where applicable.
- Required input information.
- Workforce fulfilment where applicable.
- Technology and operating prerequisites.
- Applicable legal or operational restrictions.
- Canonical Facility references and facility-to-Industry associations.
- Production capability eligibility and process-specific throughput references.
- Actual Production outcomes.
- Physical operating constraints.

Installations and Facilities owns canonical Facility identity, shared physical state, generic capabilities and generic capacity definitions. Production owns process-specific capability mappings, throughput basis and calculations, capacity allocation, orders and execution. Economy supplies economic intent and receives production results. Industry Scale is not physical capacity, and neither Industry Data nor Economy may duplicate Facility state.

### 11.4 Production Processes and Recipes Interface

Industry configuration may reference production processes eligible for an Industry.

Production Processes and Recipes owns transformation definitions, input and output quantities, yields, process losses and recipe versions.

Industry Data must not embed complete recipes, transformation ratios, production cycles or process-level operating constraints.

Interfaces must resolve eligible process identifiers, Industry Type compatibility, applicable technology prerequisites and Industry-specific restrictions.

### 11.5 Inventory System Interface

Industry Data identifies Industry instances and relevant inventory relationships.

Inventory owns physical stock, reservations, transfers, delivery and consumption.

Interfaces must support:

- Industry-to-inventory-location references.
- Queries for available inputs.
- Authorised stock reservations.
- Input delivery.
- Output registration.
- Physical consumption records.
- Ownership or location transfers.
- Inventory events affecting economic activity.

Industry Data must not maintain a competing physical quantity. Economy may value inventory, but Inventory remains authoritative for physical quantities and movements.

### 11.6 Good and Service Catalogue Interface

Industry Type configuration references canonical product identifiers wherever Goods or Services are required.

These references may be used for input requirements, output eligibility and economic transactions.

Industry Data must not define a parallel product catalogue. Product IDs must resolve against the Good and Service Catalogue, which owns product identity, classification, physical form and canonical units.

### 11.7 Population System Interface

Population supplies the workforce information required to determine available labour, including relevant Population Types, available labour and education or qualification information where modelled.

Economy owns Industry labour demand and employment allocations.

The interface must support employment allocation updates and the return of released labour to the appropriate pool. Population remains authoritative for population and labour-force state.

### 11.8 Government System Interface

Government supplies or authorises Government-controlled prerequisites, including:

- Establishment authority.
- Legal permissions.
- Industry-specific restrictions.
- Subsidies.
- Taxation rules.
- Education policy.
- Public funding.
- Industrial policy.
- Nationalisation and other authorised transfers of control.

Government owns the Nation Treasury and public financial authority. Economy records financial effects through its accounting interfaces without creating an independent Treasury balance.

### 11.9 Research System Interface

Research owns scientific research, technological knowledge, discoveries and technology prerequisites.

Industry configuration may reference canonical technology identifiers required for establishment or operation.

The interface must validate technology through Research. Industry Data must not independently grant technology or maintain a parallel technology tree.

Physical facilities and production capabilities remain subject to Production's rules.

### 11.10 Terrain System Interface

Terrain provides geographic geometry and the relevant Area Mapping Primitive.

The interface must support:

- Industry Area identity.
- Geographic eligibility checks.
- Spatial association.
- Relevant terrain properties.
- Overlap and containment rules.
- Resource deposit associations where applicable.
- Other configured geographic constraints.

Industry Data must not independently define geographic geometry or duplicate Terrain Objects rules.

### 11.11 Infrastructure and Logistics Interfaces

Where an Industry requires transport, energy, water, communications or other infrastructure, its configuration must identify the dependency.

The owning infrastructure system supplies authoritative connection, capacity and operational state.

Economy may use that information to evaluate economic feasibility. Production uses applicable physical constraints to determine actual production.

Geographic proximity alone does not prove functional infrastructure access.

### 11.12 Military and Diplomacy Interfaces

Military and diplomatic rules may affect Industry establishment or operation.

Examples include military procurement demand, industrial restrictions, sanctions, trade restrictions, wartime priorities and damage or disruption affecting facilities and routes.

Industry Data records relevant Industry associations and configured requirements. Government, Diplomacy, Military, Terrain, Production and Logistics determine the authoritative effects within their respective domains.

### 11.13 Interface Ownership Summary

| Data | Authoritative owner |
|---|---|
| Industry Type definitions and establishment profiles | Industry Data |
| Live Industry economic state | Economy |
| Industry Scale | Economy |
| Company Capital | Economy |
| Nation Treasury | Government |
| Workforce configuration | Industry Data |
| Labour demand and employment allocation | Economy |
| Available labour and population characteristics | Population |
| Education policy and public funding | Government |
| Educational attainment | Population |
| Technology and discoveries | Research |
| Geographic Industry Area | Terrain |
| Physical facilities and Production Capacity | Production |
| Transformation definitions and recipes | Production Processes and Recipes |
| Physical stock and stock movements | Inventory |
| Good and Service identity | Good and Service Catalogue |
| Establishment authorisation | Government |
| Physical resource extraction | Production and resource model |
| Economic transactions and Company accounting | Economy |

---

## 12. Industry Type Configuration Requirements

### 12.1 Required Configuration Fields

Every canonical Industry Type definition must provide the following fields or explicitly identify the configuration mechanism that supplies them.

| Field | Requirement |
|---|---|
| `industry_type_id` | Stable unique identifier. |
| `display_name` | Canonical display name. |
| `description` | Primary economic function. |
| `industry_establishment_capital_requirement` | Establishment expenditure in Dollars, or explicit valid no-charge configuration. |
| `opening_company_capital_requirement` | Required opening allocation or explicit zero/none configuration. |
| `establishment_profiles` | Explicit establishment arrangements and their prerequisites. |
| `geographic_requirements` | Geographic eligibility and Industry Area requirements. |
| `workforce_requirements` | Labour categories and calculation configuration. |
| `technology_requirements` | Canonical Research identifiers where required. |
| `resource_dependencies` | Strategic Resource and other product dependencies where applicable. |
| `industry_scale_configuration` | Reference to Economy's Scale rules and parameters. |
| `permitted_production_configurations` | References to eligible production configurations. |
| `operational_requirements` | Configured conditions required for operation. |
| `system_interfaces` | Required dependencies and authoritative data sources. |

Any additional required fields must be documented in the canonical schema and must not contradict the ownership rules.

### 12.2 Explicitly Undefined Values

The Industry taxonomy does not, by itself, establish a complete numerical configuration for every Industry Type.

The following must be configured before the affected mechanics are operational:

- Establishment Capital Requirement for each establishment profile.
- Opening Company Capital requirement for each profile.
- Workforce requirements by Industry Type and Scale.
- Base wage data for relevant employment categories.
- Industry Scale calculation parameters.
- Industry-specific geographic eligibility.
- Required technology identifiers.
- Industry-specific resource dependencies.
- Permitted production configuration references.
- Industry-specific establishment and operating restrictions.

Missing required values must produce a validation error or an explicitly defined unresolved-configuration state. The implementation must not silently substitute arbitrary defaults.

### 12.3 Establishment Profiles

One Industry Type may support multiple establishment profiles where distinct arrangements are required.

Each profile must define its applicable:

- Establishment expenditure.
- Opening Company Capital.
- Mandatory and optional prerequisites.
- Geographic requirements.
- Initial Scale configuration.
- Required production configuration references.
- Other applicable establishment conditions.

Profiles must not conceal different financial treatments or bypass validation.

### 12.4 Configuration Versioning

Canonical Industry Type configuration and establishment profiles must be versioned.

A change to establishment costs, workforce requirements, geographic restrictions or Scale parameters must specify whether it affects newly established Industries, existing Industries or both.

Configuration changes must not retroactively create establishment payments, change Company Capital or create physical facilities without an explicit rule.

Existing instances must retain the configuration version required to interpret their state and historical events. A migration to a new version must be explicit, validated and reproducible.

---

## 13. Starting-World Data

### 13.1 Starting Date

The default starting date for Project Sovereign is 1 January 1955.

Starting-world Industry data must initialise a coherent economic world at that date and distinguish scenario-provided starting state from events occurring after simulation begins.

### 13.2 Starting-World Requirements

Every starting Industry must have:

- A unique Industry ID.
- A valid Industry Type.
- A valid associated Company.
- A valid Nation association.
- A valid Industry Area or approved geographic representation.
- An initial Industry Scale.
- A valid establishment and operational state.
- Valid Industry configuration references.
- Valid workforce state or an explicit initialisation procedure.
- Valid technology and resource references where required.
- Valid production configuration references where required.
- Valid inventory location references where required.

Where a starting Industry depends on a system that has not yet initialised, the world loader must respect the logical dependencies between systems.

### 13.3 Starting Company State

Each starting Industry's Company must exist in the starting data or be created through an explicit initialisation procedure.

The Company must have a valid Company Capital balance and any other financial state required by the scenario.

Starting Company Capital is an initial condition, not an establishment payment.

It must not also be added to Nation Treasury unless a separate, explicitly defined financial relationship justifies both balances.

### 13.4 Starting Industry Scale

Every starting Industry must have an explicit initial Scale or a valid calculation procedure.

The initial Scale must be consistent with the Industry configuration and starting economic state.

Scenario data may specify Scale directly, provided the value uses the canonical representation and satisfies validation rules. Alternatively, Scale may be calculated using the canonical Economy rules.

The loader must not assign every Industry the same Scale by default.

### 13.5 Starting Operational State

A starting Industry may be operational, constrained, suspended or otherwise classified according to the scenario.

Its state must be consistent with available workforce, Company state, facilities, production configuration and other mandatory conditions.

The existence of an Industry entity does not prove that it is fully operational.

### 13.6 Starting-World Industry Catalogue

Starting-world data may contain instances of any of the 16 canonical Industry Types.

The taxonomy does not require every Industry Type to exist in every Nation. The starting scenario determines the presence, Scale and state of each instance.

An Industry must not be created merely because its Industry Type is available.

### 13.7 Starting Resource Associations

Where a starting Industry depends on a Strategic Resource deposit, the deposit and access relationship must be valid.

Starting deposit quantities and extracted stock must be initialised through the authoritative resource and Inventory models.

An unextracted deposit must not be treated as an available physical stockpile.

### 13.8 Starting Financial Consistency

Starting-world initialisation must preserve the distinction between:

- Nation Treasury.
- Company Capital.
- Revenue.
- Expenditure.
- Physical inventory.
- Industry Establishment Capital Requirement.
- Opening Company Capital.

Starting Industries must not incur retroactive establishment charges merely because they are loaded.

The scenario must define initial financial balances without generating duplicate or retroactive transactions.

### 13.9 Initialisation Dependencies

The logical dependency order is:

1. Load canonical Industry Type and establishment configuration.
2. Initialise Nations and Government-owned Treasury state.
3. Initialise Companies and their financial state.
4. Initialise geographic data and Industry Areas.
5. Initialise Population and workforce state.
6. Initialise Research and technology state.
7. Initialise resource deposits and Inventory state.
8. Create Industry instances and resolve mandatory references.
9. Initialise or calculate Industry Scale.
10. Resolve permitted production configurations and required interfaces.
11. Validate operating state against applicable prerequisites.
12. Commit the completed starting-world state.

The technical loading sequence may differ if dependencies are safely resolved. All authoritative references and initial conditions must be valid before the world becomes available for simulation.

### 13.10 Starting-World Validation

The starting world must fail validation if it contains:

- Duplicate Industry IDs.
- Unknown Industry Type IDs.
- Missing Company or Nation references.
- Invalid geographic associations.
- Missing mandatory Industry configuration.
- Invalid Industry Scale values.
- Invalid Company Capital state.
- Invalid technology references.
- Invalid mandatory resource dependencies.
- Invalid required production configuration references.
- Invalid inventory location references.
- Contradictory establishment or operational states.
- Financial state inconsistent with canonical accounting rules.

Errors must identify the affected entity and failed condition.

The loader must not silently create missing Companies, invent Industry Types or assign arbitrary financial balances to repair invalid state.

---

## 14. Validation and Invariants

### 14.1 Industry Identity

- Every committed Industry has a unique, stable Industry ID.
- Every Industry references exactly one valid Industry Type.
- Every Industry references exactly one associated Company and relevant Nation.
- Every mandatory geographic reference resolves.
- Every mandatory configuration reference resolves.

### 14.2 Industry Type Integrity

- Only the 16 canonical Industry Types are permitted in the initial taxonomy.
- Scenario data may create multiple instances of those types where simulation rules permit.
- A new Industry Type cannot be introduced merely by inventing an identifier in scenario data.
- Display-name changes must not alter stable identifiers.

### 14.3 Financial Integrity

- Company Capital and Nation Treasury remain separate authoritative accounts.
- Establishment expenditure and Opening Company Capital remain separate values and financial movements.
- Every establishment movement identifies its purpose and accounting treatment.
- Treasury funds cannot be spent beyond the available balance unless an explicit credit or deficit mechanic permits it.
- Failed establishment does not change Treasury balances or Company Capital.
- Starting-world initialisation does not trigger retroactive establishment payments.
- Establishment payments are not duplicated.
- Industry Data does not independently mutate financial accounts.

### 14.4 Geographic Integrity

- Every committed Industry has a valid geographic association or an approved alternative representation.
- Geographic representations conform to Terrain's geometry and spatial validation rules.
- Industry Area geometry is not a substitute for Industry Scale or Production Capacity.
- Geographic overlap does not imply shared ownership or shared stock.

### 14.5 Workforce Integrity

- Workforce requirements reference valid Population Types or approved employment categories.
- Labour fulfilment uses an explicit calculation.
- Labour allocations comply with Population rules.
- Educational attainment and qualification data come from their authoritative systems.
- Industry Data does not create workers or educational attainment.

### 14.6 Resource Integrity

- Resource dependencies reference valid canonical resource or product identifiers.
- Required access conditions are validated against their authoritative sources.
- The existence of a deposit is not proof that extracted stock is available.
- Industry Data does not independently decrement deposits or inventory.

### 14.7 Production Integrity

- Industry Data may reference production configurations but does not calculate physical Production Capacity or Actual Production.
- Industry Scale is not a substitute for Production Capacity.
- Production inputs and outputs reference canonical Catalogue entries where applicable.
- Industry establishment does not automatically create physical facilities unless an authorised interface explicitly performs that operation.

### 14.8 State Integrity

- Establishment state, operational state, Industry Scale, workforce fulfilment, Company financial state, facility condition and inventory availability remain distinct.
- State transitions follow explicit rules.
- An Industry is not considered fully operational solely because its entity record exists.
- Changes to one state do not implicitly change another without a defined rule.

### 14.9 Referential Integrity

All mandatory references must resolve before the corresponding state is committed.

Deleting or changing a referenced entity must not silently leave an invalid Industry.

Removal, transfer or migration must use an authorised mechanism and update dependent references consistently.

### 14.10 Deterministic Validation

Validation must produce consistent results for the same canonical configuration, entity state and simulation inputs.

It must not depend on incidental file ordering, display names or non-deterministic iteration.

Where several validation failures exist, diagnostics should follow a stable and useful order.

---

## 15. Required Data Schemas

### 15.1 Schema Purpose

The following schemas define the logical data contracts required by Industry Data. They do not mandate a particular programming language, serialisation format or database.

The implementation must preserve the required fields, constraints, references and ownership boundaries.

### 15.2 Industry Type Definition

```yaml
industry_type_id: steelmaking
display_name: Steelmaking
description: Production of steel and associated steel products.

configuration_version: 1

industry_establishment_capital_requirement:
  amount: null
  currency: DOLLAR

opening_company_capital_requirement:
  required: true
  amount: null
  currency: DOLLAR

establishment_profiles: []

geographic_requirements: []
workforce_requirements: []
technology_requirements: []
resource_dependencies: []

industry_scale_configuration:
  configuration_id: null

permitted_production_configurations: []
operational_requirements: []

system_interfaces:
  - economy
  - production
  - production_processes_and_recipes
  - inventory
  - population
  - government
  - research
  - terrain
```

Null amounts and configuration references are unresolved placeholders, not valid final operating values. Required values must be supplied, or a valid explicit no-requirement configuration must be defined before the affected operation is permitted.

The canonical currency identifier is `DOLLAR`. The display label is Dollar.

### 15.3 Industry Instance

```yaml
industry_id: industry_example_001
industry_type_id: steelmaking
industry_configuration_id: steelmaking_standard
configuration_version: 1

company_id: company_example_001
nation_id: nation_example_001
industry_area_id: industry_area_example_001

establishment_state: established
operational_state: constrained

industry_scale: null



created_at_simulation_date: null
```

This is an illustrative schema, not valid starting-world data. A committed instance must have a valid Industry Scale, required references and valid operational state.

Relationships to Facilities, Production capability configurations, Inventory locations and resource sources are not stored as unvalidated ID arrays on the Industry instance. Each relationship must use the relevant domain's canonical identity and an explicit normalised association or interface. Industry Data owns the Industry-to-Facility association only; Production owns process capability, Inventory owns stock locations, and the resource-source model owns deposit/source identity and state.

### 15.3A Industry-to-Facility Association

Store Industry-to-Facility relationships as normalised association records rather than a `facility_ids` array on the Industry instance. This permits one Industry to use multiple Facilities and one Facility to serve multiple Industries where the applicable legal and operational rules permit it.

```yaml
industry_facility_association_id: industry_facility_assoc_example_001
industry_id: industry_example_001
facility_id: facility_example_001
association_role: primary_production_site
valid_from_simulation_date: null
valid_until_simulation_date: null
configuration_version: 1
```

The association references canonical Industry and Facility identities. It does not copy Facility condition, lifecycle, ownership, operator, generic capacity or Production capability. The association role must use a defined vocabulary; validity and configuration fields must follow shared simulation-time and versioning conventions. Both foreign keys must resolve. Overlapping associations must not conflict for the same declared scope. Association alone does not establish Production eligibility or available throughput.

### 15.4 Industry Establishment Configuration

```yaml
establishment_profile_id: steelmaking_standard
configuration_version: 1

industry_type_id: steelmaking

industry_establishment_capital_requirement:
  amount: null
  currency: DOLLAR
  accounting_treatment_id: null

opening_company_capital:
  required: true
  amount: null
  currency: DOLLAR

mandatory_requirements: []
optional_requirements: []

geographic_requirements: []
technology_requirements: []
resource_access_requirements: []
workforce_requirements: []

initial_scale_configuration_id: null
permitted_production_configuration_ids: []
```

All required amounts, accounting treatments, prerequisites and configuration references must be defined before this profile can be used.

### 15.5 Workforce Requirement Configuration

```yaml
workforce_requirement_id: steelmaking_workforce
configuration_version: 1

industry_type_id: steelmaking

scale_relationship:
  configuration_id: null

employment_categories: []
education_requirements: []
qualification_requirements: []

base_wage_configuration_id: null
labour_fulfilment_configuration_id: null
```

Employment categories must reference canonical Population Types or approved employment categories. Scale relationships, wage configuration and fulfilment rules must be defined by the relevant authoritative specifications.

### 15.6 Industry Area Reference

```yaml
industry_area_id: industry_area_example_001
industry_id: industry_example_001

terrain_area_reference_id: terrain_area_example_001
geographic_requirements_configuration_id: null
```

Terrain remains authoritative for geographic geometry and spatial attributes. The Industry Area reference must not duplicate the authoritative geographic representation.

---

## 16. Industry Establishment Workflow

### 16.1 Request

An authorised establishment request identifies:

- Requested Industry Type.
- Relevant Nation.
- Associated Company or permitted Company-creation arrangement.
- Proposed Industry Area.
- Applicable establishment profile.
- Required configuration choices.
- Requested initial Scale or its calculation rule.

The request must be evaluated against current authoritative world state.

### 16.2 Prerequisite Validation

The establishment process resolves the canonical Industry Type and establishment profile.

It validates mandatory financial, geographic, legal, technological, workforce and resource requirements.

Requirements supplied by another system must be checked against that system's authoritative state.

### 16.3 Financial Validation

The establishment process calculates the complete required Treasury outlay from establishment expenditure, Opening Company Capital and any other explicit establishment payments.

It verifies that the Nation Treasury can fund all required movements.

Validation must not deduct funds or create financial transactions.

### 16.4 Entity Preparation

The process prepares the Industry instance and any required Company or Industry Area entities.

All required identifiers and references must be valid.

Initial Industry Scale must be supplied or calculated through the configured rules.

The operation must not create physical production facilities merely because an Industry is established. Any required facility creation must use the authorised Production or construction interface.

### 16.5 Commit

If all mandatory validation succeeds, the operation commits the entity changes and required financial movements as one atomic logical operation.

The resulting Industry must have valid identity, Company association, jurisdiction, geographic association, configuration and initial economic state.

### 16.6 Rejection

If mandatory validation fails, the request is rejected.

The operation must leave no partial entity creation or financial changes.

The failure must identify the relevant validation condition so that the requesting system or Player can understand why establishment was rejected.

### 16.7 Post-Establishment Operation

After successful establishment, the Industry becomes eligible for operating-state evaluation.

Its ability to operate depends on the relevant economic, workforce, legal, geographic, technological, inventory and physical production conditions.

Successful establishment is not proof of full production readiness.

---

## 17. Data Lifecycle and Change Management

### 17.1 Industry Creation

Industry creation must follow the establishment workflow or an authorised starting-world initialisation procedure.

Direct creation of an Industry instance outside these mechanisms is not permitted in authoritative simulation state.

### 17.2 Industry Configuration Changes

A configuration change must identify the affected fields and update rules.

It must not automatically create financial transactions, alter physical facilities or change inventory unless an explicit rule requires those effects.

Changes must preserve the configuration version needed to interpret historical state.

### 17.3 Industry Transfer

Where an Industry may be transferred to another Company or Nation, the transfer must use an explicit authorised mechanism.

It must define the resulting Company association, jurisdiction, financial treatment, legal permissions and geographic relationships.

A transfer must not silently move Company Capital, inventory, facilities or liabilities unless the relevant rules explicitly require it.

### 17.4 Industry Expansion

Expansion may change Industry Scale or economic requirements.

Any required physical expansion must be resolved through Production and the relevant construction systems.

Additional establishment expenditure, capital allocation, facility construction or input acquisition must be separately defined.

An increase in Industry Scale must not automatically create Production Capacity.

### 17.5 Industry Contraction

Contraction may reduce intended economic activity and associated requirements.

The resulting treatment of employment, financial commitments, facilities, inventory and production processes must follow the respective system rules.

A reduction in Scale must not automatically destroy facilities, eliminate inventory or close the Industry.

### 17.6 Industry Closure

Industry closure must use an explicit authorised mechanism.

Closure must define the resulting establishment and operational state and the treatment of Company relationships and outstanding obligations.

Closure must not automatically destroy the associated Company or erase financial and historical records.

Physical facilities and inventory must be handled by their respective owning systems.

### 17.7 Industry Reopening

If reopening is supported, its requirements must be explicitly defined.

Reopening must validate current configuration, legal eligibility, Company relationship and applicable workforce, technology, geographic and physical conditions.

It must not automatically recreate destroyed facilities or replenish depleted inventory.

### 17.8 Historical Records

Historical Industry events must preserve stable Industry identity and relevant event details.

Changes to display names or configuration must not invalidate historical references.

The historical record schema belongs to the relevant Economy and Technical Specifications. Configuration versions must remain identifiable where required to reproduce historical decisions and outcomes.

---

## 18. Implementation Requirements

### 18.1 Canonical Configuration

The initial Industry taxonomy must be stored as canonical configuration data.

The 16 Industry Type definitions must not be duplicated across unrelated systems. Other systems must reference their canonical identifiers.

### 18.2 Data Validation

Industry Type configuration and Industry instances must be validated at load time and at relevant state transitions.

Invalid mandatory configuration must produce explicit errors.

The implementation must not silently substitute arbitrary establishment costs, Scale values, workforce requirements or resource prerequisites.

### 18.3 Authoritative State

The implementation must preserve authoritative ownership of economic, geographic, population, research, production and inventory data.

References and cached derived values may be used for performance but must not become competing authoritative state.

Cache invalidation and state synchronisation rules belong to the Technical Specifications.

### 18.4 Deterministic State Transitions

Industry establishment, configuration changes and other authoritative transitions must produce deterministic results.

Financial movements and entity changes must be committed consistently. A failed operation must not leave partial authoritative state.

### 18.5 Save and Load

Saved worlds must preserve Industry identities, configuration references and state required to resume simulation.

Loading a saved Industry must not repeat establishment payments or recreate its original entities.

The loader must validate saved state against the relevant schema and migration rules.

### 18.6 Error Reporting

Industry configuration and instance validation errors must identify:

- Affected Industry Type or Industry ID.
- Field or reference that failed validation.
- Relevant validation condition.
- Whether the error prevents loading, establishment, operation or another defined action.

Errors must not be hidden by assigning arbitrary defaults.

### 18.7 Testing

The implementation must include tests for:

- Canonical Industry Type configuration.
- Industry instance identity.
- Company and Nation references.
- Industry Area references.
- Industry Scale validation.
- Establishment requirements.
- Treasury sufficiency.
- Separate establishment expenditure and Opening Company Capital.
- Atomic establishment and rollback.
- Starting-world initialisation.
- Workforce requirement validation.
- Technology and resource prerequisite validation.
- Production and Inventory interface integrity.
- Industry closure and transfer where implemented.
- Configuration versioning and migration.
- Save and load consistency.
- Deterministic state transitions.

---

## 18A. Facility Reference and Ownership Contract

Industry Data must represent the relationship between an Industry and physical Facilities through stable references, not copied Facility records.

- Each referenced Facility ID must resolve to the canonical Installations and Facilities record.
- Industry-to-Facility associations must support the declared cardinality. If a Facility can serve multiple Industries, use a normalised association rather than a single overwritten Industry field.
- Industry Data may state which Facility or capability is required for an establishment or activity. It must not define the Facility's identity, shared physical condition, lifecycle, owner/operator/control, or generic capacity.
- A facility association does not prove that a Facility is production-eligible, has a compatible process capability, has available throughput, possesses required inputs, or has produced output.
- Process-specific capability and throughput references must resolve through Production and must identify the process/basis, unit, interval, applicable configuration version and validity period where those are modelled.
- A geographic Industry Area is not a Facility and does not itself confer physical capacity.
- Starting-world Industry initialisation must reference pre-existing or explicitly created canonical Facilities. It must not create duplicate Facility identities or charge for a Facility merely because an Industry references it.

This contract supplements the Industry instance schema and cross-system interfaces; it does not establish the Facility or resource-source schema.

## 19. Acceptance Criteria

Industry Data is ready for implementation when the following conditions are satisfied.

### 19.1 Industry Taxonomy

- [ ] All 16 canonical Industry Types are defined.
- [ ] Every Industry Type has a stable identifier.
- [ ] Industry Types are distinct from Industry instances.
- [ ] Product definitions are referenced rather than duplicated.
- [ ] Industry-specific requirements are explicitly configured.
- [ ] Configuration versions are identifiable.

### 19.2 Industry Instances

- [ ] Every Industry instance has a unique ID.
- [ ] Every Industry references one valid Industry Type.
- [ ] Every Industry references one associated Company.
- [ ] Every Industry references its relevant Nation.
- [ ] Every Industry has a valid geographic representation or approved alternative.
- [ ] Required configuration and system references resolve.
- [ ] Industry-to-Facility relationships use a normalised association to canonical Facility IDs; the Industry instance does not carry a denormalised `facility_ids` array.

### 19.3 Industry Scale

- [ ] Industry Scale is distinct from physical Production Capacity.
- [ ] Scale configuration defines its inputs and calculation rules.
- [ ] Initial Scale is supplied or calculated through an explicit rule.
- [ ] Scale changes do not automatically create physical facilities.
- [ ] Scale-related economic requirements are defined through Economy.

### 19.4 Establishment and Financing

- [ ] Every Industry Type has an explicit establishment configuration.
- [ ] Establishment expenditure is distinct from Opening Company Capital.
- [ ] Financial movements identify their purpose and accounting treatment.
- [ ] Mandatory prerequisites are validated before commit.
- [ ] Failed establishment produces no partial entity or financial changes.
- [ ] Government retains authority over Nation Treasury expenditure.
- [ ] Economy records the relevant financial effects.
- [ ] Starting Industries incur no retroactive establishment payments.

### 19.5 Geographic and Workforce Requirements

- [ ] Industry Areas reference Terrain's geographic representation.
- [ ] Geographic requirements are explicitly configured.
- [ ] Workforce requirements reference canonical Population data.
- [ ] Education policy and educational attainment remain with their respective owning systems.
- [ ] Labour fulfilment is calculated through explicit rules.

### 19.6 Cross-System Interfaces

- [ ] Economy owns live Industry economic state and Company accounting.
- [ ] Government owns Treasury state and public financial authority.
- [ ] Production owns physical production capacity and actual output.
- [ ] Production Processes and Recipes own input-to-output transformations.
- [ ] Inventory owns physical stock, reservations, transfers, delivery and consumption.
- [ ] The Good and Service Catalogue owns product definitions and canonical units.
- [ ] Population owns demographics, labour-force state and educational attainment.
- [ ] Research owns technological knowledge and discoveries.
- [ ] Terrain owns geographic geometry.
- [ ] Physical and economic state cannot silently diverge through duplicate ownership.
