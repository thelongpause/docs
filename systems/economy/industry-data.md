# Industry Data

## 1. System Definition

### 1.1 Purpose

Industry Data defines the economic, organisational, financial, geographic and workforce properties of Industries in Project Sovereign.

It establishes the data model used to represent Industries, classify their economic activity, associate them with Companies, determine their economic Scale, validate their establishment requirements, define their geographic Industry Areas and exchange state with other simulation systems.

Industry Data provides the authoritative configuration and state definitions required by the Economy System to represent industrial activity.

It does not independently simulate physical production, manufacturing processes, physical inventory or the transformation of inputs into outputs. Those mechanics belong to the Production, Production Processes and Recipes, and Inventory Systems.

### 1.2 Core Responsibilities

Industry Data defines:

1. The 16 Industry Types available in the initial simulation.
2. The distinction between Industry Types and individual Industry instances.
3. The relationship between Industries, Companies, Nations and Senior Executives.
4. The economic attributes of an Industry.
5. Industry Scale and its relationship to economic requirements.
6. Industry establishment prerequisites and financing.
7. Opening Company Capital allocations.
8. Industry Areas and their geographic representation.
9. Industry workforce requirements and employment composition.
10. Industry-specific education and technology prerequisites.
11. Industry access to Strategic Resources and other required inputs.
12. Industry establishment, operational and economic state.
13. Interfaces with Production, Inventory, Population, Government, Research, Terrain and other systems.
14. The Industry data required to initialise a starting world.
15. Industry data validation, consistency requirements and simulation invariants.

### 1.3 Design Principles

Industry Data follows these principles:

- **Separation of definition and instance:** Industry Types define reusable industrial categories and their configuration. Industry instances represent individual establishments in the simulated world.
- **Explicit economic ownership:** each Industry belongs to one Company, and each Company operates within an explicitly defined economic jurisdiction.
- **Distinct financial accounts:** the Nation Treasury and Company Capital are separate financial accounts.
- **Distinct economic and physical state:** Industry Scale does not directly represent physical production capacity or actual output.
- **Explicit geographic association:** every Industry instance has a geographic Industry Area or an explicitly defined geographic representation appropriate to its activity.
- **Explicit prerequisites:** establishment requirements are defined for each Industry Type and validated before establishment.
- **Authoritative system ownership:** systems exchange state through defined interfaces rather than independently maintaining conflicting versions of the same information.
- **Data-driven configuration:** Industry Types, establishment costs, workforce requirements and other configurable properties must be defined in data rather than embedded as unexplained constants in simulation code.
- **Deterministic resolution:** identical initial state, configuration and simulation inputs must produce identical results under the same simulation rules.

### 1.4 Relationship to the Economy System

The Economy System defines the rules governing economic activity, including financial transactions, Company Capital, revenue, expenditure, employment, Industry establishment and economic decision-making.

Industry Data specifies the industrial entities and configuration consumed by those rules.

The Economy System remains responsible for the authoritative economic state of each Industry and Company. Industry Data defines the structure and permitted values of that state.

### 1.5 Explicit Non-Responsibilities

Industry Data does not own:

| Mechanic | Owning System |
|---|---|
| Physical production capacity | Production System |
| Physical production facilities and equipment | Production System |
| Actual production quantities | Production System |
| Input-to-output transformation | Production Processes and Recipes |
| Physical inventory quantities, reservations and transfers | Inventory System |
| Physical resource extraction and deposit depletion | Production System, with Terrain and Inventory interfaces |
| Good and Service definitions | Good and Service Catalogue |
| Population demographics and available labour | Population System |
| Individual educational attainment | Population System |
| Education policy, legislation, public funding and institutional governance | Government System |
| Scientific research, technological knowledge and discoveries | Research System |
| Geographic terrain and Area Mapping Primitive | Terrain System |
| Physical facility damage and production interruption | Production System |
| Government Treasury ownership and public financial policy | Government System |
| Company revenue, expenditure and Company Capital | Economy System |
| Military organisation and combat | Military System |
| Diplomatic relations and international permissions | Diplomacy System |
| Detailed Company AI algorithms | AI and Technical Specifications |

These ownership boundaries do not prevent Industry Data from defining the prerequisites or economic requirements that depend on those systems.

---

## 2. Foundational Definitions

### 2.1 Industry Type

An Industry Type is a reusable definition of a category of economic activity.

It specifies the characteristics shared by all Industry instances of that type, including:

- Industry Type identity.
- Industry classification.
- Primary economic function.
- Establishment requirements.
- Industry Establishment Capital Requirement.
- Opening Company Capital requirements.
- Workforce requirements.
- Education requirements.
- Technology prerequisites.
- Geographic eligibility.
- Strategic Resource dependencies.
- Permitted economic activities.
- Industry Scale configuration.
- Relevant system interfaces.

An Industry Type is a definition, not a physical establishment.

For example, `Steelmaking` is an Industry Type. An individual steelmaking establishment operating in a particular Nation is an Industry instance.

### 2.2 Industry Instance

An Industry instance is an individual industrial establishment represented in the simulation.

Each instance references exactly one Industry Type.

An Industry instance contains or references:

- A unique Industry ID.
- An Industry Type ID.
- An associated Company ID.
- An associated Nation or economic jurisdiction.
- An Industry Area or geographic representation.
- Industry Scale.
- Establishment state.
- Operational state.
- Workforce state.
- Economic operating state.
- Relevant technology and resource dependencies.
- References to its production configuration.
- References to authoritative inventory locations where applicable.

An Industry instance is not identical to its Company, geographic Industry Area, physical production facilities or inventory.

These entities are related through explicit identifiers and system interfaces.

### 2.3 Company

A Company is an economic organisation that owns or operates one or more Industries under the applicable ownership and control rules.

A Company maintains its own Company Capital balance and other economic state through the Economy System.

A Company may operate multiple Industries where permitted by the relevant rules.

Each Industry instance must be associated with exactly one Company at any given point in time.

The relationship between an Industry and its Company must not be inferred from names, geographic proximity or shared Industry Types.

### 2.4 Nation

A Nation is the political and economic jurisdiction within which an Industry operates.

Each Industry instance must have an explicitly defined Nation association.

The Nation association determines the jurisdiction relevant to:

- Government authorisation.
- Industry establishment financing.
- Applicable laws and regulations.
- Taxation.
- Economic statistics.
- Labour availability.
- Infrastructure access.
- Trade restrictions.
- National economic aggregation.

A Company and its Industries must not be assumed to have identical geographic footprints.

Where a Company operates across multiple Nations, each Industry must identify its own relevant operating jurisdiction.

### 2.5 Senior Executive

The Senior Executive is the default owner, controller and chief executive of a Company under the existing Economy System rules.

The Industry does not independently own a Senior Executive.

The Industry references its associated Company, and the Company's executive relationship is resolved through the Company entity.

Changes to executive control must use the relevant Character, Company and Government mechanisms. Industry Data must not duplicate executive attributes on each Industry instance.

### 2.6 Industry Area

An Industry Area is the geographic representation associated with an Industry instance.

It identifies the area in which the Industry's economic activity is geographically established or represented.

Industry Areas use the Terrain System's Area Mapping Primitive and the relevant geographic data structures.

An Industry Area is not the Industry itself and does not independently determine its economic Scale, workforce, financial state or physical production capacity.

### 2.7 Industry Establishment

Industry Establishment is the authorised process of creating a new Industry instance and completing the required financial and organisational arrangements.

Establishment requires the validation of all mandatory prerequisites defined by the relevant Industry Type.

A successful establishment operation must create a valid Industry instance and all mandatory associated economic entities and references.

Establishment does not guarantee that the Industry can immediately operate at its intended Scale.

### 2.8 Industry Scale

Industry Scale represents the intended economic magnitude of an Industry.

It informs economic requirements such as labour demand, input demand, capital requirements and intended production activity.

Industry Scale is distinct from physical production capacity, actual production, geographic area size, employee count and Company Capital.

The exact calculation and permitted numerical representation of Industry Scale must be defined by the Economy System's Industry Scale rules.

---

## 3. Industry Type Catalogue

### 3.1 Initial Industry Taxonomy

Project Sovereign contains exactly 16 Industry Types in the initial Industry taxonomy.

| ID | Industry Type | Primary Economic Function |
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
| `consumer_goods` | Consumer Goods | Production of household and personal-use goods not assigned to a more specific Industry. |
| `agriculture` | Agriculture | Production and extraction of agricultural outputs associated with an agricultural Industry Area. |

The IDs above are stable machine-readable identifiers. Display names may be localised independently without changing the IDs.

The initial taxonomy must not silently add, remove, merge or rename Industry Types through ordinary scenario configuration.

A change to the canonical taxonomy requires an explicit data-model and compatibility decision.

### 3.2 Industry Type Versus Product Classification

An Industry Type defines economic specialisation. It does not define a complete list of all Goods or Services that an Industry may produce.

The same Good may be produced by more than one Industry Type where the relevant production rules permit it.

Likewise, one Industry Type may produce multiple Goods or Services.

The relationship between an Industry Type and its permitted outputs must be expressed through the relevant production configuration and Good and Service Catalogue.

Industry Data must not duplicate complete product definitions, product prices, units of measurement or transformation recipes.

### 3.3 Transportation

Transportation represents commercial movement of people and goods and associated transport services.

Its economic requirements may include:

- Qualified labour.
- Access to transport infrastructure.
- Access to relevant transport equipment.
- Fuel or energy inputs where required.
- Maintenance and operating expenditure.
- Access to routes and destinations.
- Relevant Government authorisation.

Transportation is an Industry Type even where its economic activity produces a Service rather than a physical Good.

Its physical operating model, vehicle capacities, route capacities and movement constraints belong to the relevant Production, Transport and Logistics systems.

### 3.4 Oil

Oil represents petroleum extraction and supply.

An Oil Industry may require access to a suitable petroleum deposit where its production configuration involves extraction.

Its establishment requirements must explicitly identify whether a suitable deposit, extraction technology, geographic eligibility or other prerequisites are mandatory.

The presence of petroleum in the world does not automatically create an Oil Industry or make the resource available for consumption.

Physical extraction and deposit depletion belong to the Production System. Extracted stock belongs to the Inventory System.

### 3.5 Construction

Construction represents economic activity associated with building, repair and development of structures and infrastructure.

Construction may provide construction Services or produce defined construction-related Goods through configured production processes.

Its geographic eligibility, labour requirements, financing and commercial state are defined through Industry Data and the Economy System.

The physical construction process, project completion, facility creation and infrastructure modification belong to the relevant Production and infrastructure systems.

Establishing a Construction Industry does not automatically construct a facility.

### 3.6 Automotive

Automotive represents the production of road vehicles and associated automotive products.

Its requirements may include access to qualified labour, industrial inputs, technology, infrastructure and appropriate physical facilities.

Its production processes determine which components and inputs are transformed into vehicles or other outputs.

The Industry Type does not itself define vehicle designs, military vehicle specifications or production recipes.

### 3.7 Aerospace

Aerospace represents the production of aircraft, aerospace vehicles and associated products.

Its requirements may include specialised technology, qualified labour, industrial inputs, appropriate infrastructure and production facilities.

Technology prerequisites must reference Research System technology identifiers rather than creating duplicate technology definitions.

Aircraft specifications, component designs and military eligibility belong to the relevant Research and Military systems.

### 3.8 Maritime

Maritime represents the production of ships, boats and associated maritime products.

Its requirements may include geographic suitability, transport access, industrial inputs, qualified labour, technology and appropriate facilities.

The Industry's geographic eligibility must be defined through explicit rules. A coastline, navigable waterway or port must not be assumed to be mandatory for every possible maritime activity without a corresponding establishment requirement.

Physical shipbuilding and the creation of vessels belong to the Production System and relevant Military or civilian asset systems.

### 3.9 Energy Production

Energy Production represents the production and commercial supply of non-nuclear energy.

Its establishment and operating requirements depend on the configured energy production method.

Potential dependencies include fuel, water, infrastructure, qualified labour, technology and geographic suitability.

The Industry Type must not be treated as a single physical generation technology.

Specific generation methods, input consumption, conversion efficiency and actual energy output belong to the relevant Production and energy systems.

### 3.10 Industrial Manufacturing

Industrial Manufacturing represents the production of industrial equipment, machinery, materials and other industrial products not assigned to a more specific Industry Type.

It may support multiple production processes and output categories.

Its Industry instance must reference the applicable production configuration rather than assuming that every Industrial Manufacturing establishment can produce every industrial Good.

The specific equipment, facilities, input requirements and physical output capacities belong to the Production System.

### 3.11 Mining

Mining represents extraction of designated mineral and geological resources.

Its establishment requirements depend on the relevant resource, deposit and extraction configuration.

A Mining Industry may be associated with one or more extraction operations where permitted by its production configuration.

The existence of a geological deposit does not itself establish an Industry, create an extraction operation or create inventory.

Resource quantities, extraction limits, physical extraction and deposit depletion belong to the Production System and the relevant geographic and resource models.

### 3.12 Nuclear Energy

Nuclear Energy represents the production and commercial supply of nuclear energy.

Its establishment requirements may include:

- Required technology.
- Government authorisation.
- Qualified labour.
- Appropriate infrastructure.
- Appropriate physical facilities.
- Access to required inputs.
- Applicable safety and environmental permissions.

All mandatory prerequisites must be explicitly configured.

The Industry Type must not automatically imply that a Nation has the necessary technology, nuclear materials, qualified workforce or operating facilities.

Physical energy generation, input consumption, facility state and production capacity belong to the relevant Production and energy systems.

### 3.13 Electronics

Electronics represents the production of electronic equipment, electrical components and associated products.

Its requirements may include specialised technology, qualified labour, industrial inputs, energy and suitable physical facilities.

The Industry may support multiple production configurations.

Component definitions and transformation processes belong to Production Processes and Recipes. Product identity belongs to the Good and Service Catalogue.

### 3.14 Steelmaking

Steelmaking represents the production of steel and associated steel products.

Its requirements may include appropriate facilities, qualified labour, energy, industrial inputs, infrastructure and applicable technology.

Steelmaking must not be treated as synonymous with Mining.

Mining extracts designated geological resources. Steelmaking transforms configured inputs into steel and associated products through physical production processes.

The specific raw materials, conversion processes, capacity and output quantities are defined by the Production System and production recipes.

### 3.15 Space

Space represents the production of space vehicles, launch systems and associated space products.

Its establishment requirements may include specialised technology, qualified labour, appropriate facilities, industrial inputs and Government authorisation where required.

The Industry Type does not automatically grant spaceflight capability, launch infrastructure or access to advanced technology.

Technology prerequisites belong to the Research System, while physical production and launch-related facilities belong to their respective owning systems.

### 3.16 Agribusiness

Agribusiness represents the processing, packaging, storage and commercial distribution of agricultural outputs.

It is distinct from Agriculture.

Agriculture produces primary agricultural outputs through configured production processes. Agribusiness may acquire those outputs and transform them into processed Goods or provide associated Services.

Agribusiness does not automatically produce agricultural inputs merely because it is established near agricultural land.

Its required inputs and outputs must be defined through production configuration and the Good and Service Catalogue.

### 3.17 Consumer Goods

Consumer Goods represents the production of household and personal-use Goods not assigned to a more specific Industry Type.

An Industry instance may produce one or more configured consumer products.

Its establishment and operating requirements depend on its production configuration, workforce, technology, inputs and physical facilities.

Consumer Goods does not itself define consumer demand, product prices or household purchasing behaviour.

Those mechanics belong to the Economy, Population and Production Systems.

### 3.18 Agriculture

Agriculture represents primary agricultural production associated with a geographic Industry Area.

Its requirements may include:

- Eligible agricultural land.
- Appropriate environmental conditions.
- Qualified labour.
- Required agricultural inputs.
- Relevant technology.
- Access to water or other resources where explicitly required.
- Government permissions where applicable.

Agriculture may produce agricultural outputs and interact with the Rich Soils Strategic Resource where the configured production model requires it.

Rich Soils availability, agricultural yield and physical resource extraction must be governed by their respective geographic and production rules.

The presence of Rich Soils must not be treated as a universal prerequisite for every agricultural activity unless the relevant Industry Type or production configuration explicitly requires it.

---

## 4. Industry Instance Data Model

### 4.1 Required Identity Fields

Every Industry instance must contain or reference the following information:

| Field | Requirement | Description |
|---|---|---|
| `industry_id` | Required | Unique, stable identifier for the Industry instance. |
| `industry_type_id` | Required | Reference to one of the 16 canonical Industry Types. |
| `company_id` | Required | Reference to the associated Company. |
| `nation_id` | Required | Reference to the relevant economic jurisdiction. |
| `industry_area_id` | Required | Reference to the Industry Area or approved geographic representation. |
| `establishment_state` | Required | State of the Industry's establishment process. |
| `operational_state` | Required | Current operational classification. |
| `industry_scale` | Required after initialisation | Current intended economic Scale under the Industry Scale rules. |
| `industry_configuration_id` | Required | Reference to the configuration governing the instance's permitted economic and production activity. |

An Industry instance must not be valid if any required reference cannot be resolved.

The initialisation process may temporarily construct an incomplete entity in memory, but an incomplete entity must not be committed to authoritative simulation state.

### 4.2 Optional or Conditional References

Depending on the Industry Type and its configuration, an Industry instance may also reference:

- One or more production configurations.
- One or more physical facilities.
- One or more inventory locations.
- One or more Strategic Resource deposits.
- Required infrastructure connections.
- Required technology identifiers.
- Industry-specific licences or authorisations.
- Associated employment records.
- Economic market participation.
- Construction or expansion projects.

These references must use the authoritative identifiers defined by their owning systems.

An Industry must not embed an independent copy of another system's authoritative state merely to avoid resolving a reference.

### 4.3 Industry Instance Identity

An Industry ID must remain stable throughout the lifetime of the instance.

Changes to Industry Type, Company association, geographic location or operational status must not silently generate a new ID unless the relevant mechanic explicitly creates a new Industry.

Industry IDs must not depend solely on:

- Display names.
- Company names.
- Industry Type names.
- Geographic coordinates.
- Array positions.
- Current Industry Scale.

This allows Industries to be referenced reliably by economic transactions, production operations, inventory movements and historical records.

### 4.4 Industry Type Configuration Versus Instance State

Industry Type configuration defines reusable properties.

Industry instance state records the properties and conditions of a specific establishment.

For example:

| Configuration property | Instance property |
|---|---|
| Industry Type ID | Industry Type reference |
| Default establishment requirements | Validated establishment state |
| Industry Establishment Capital Requirement | Recorded establishment expenditure |
| Default workforce requirements | Current workforce requirement |
| Default Scale configuration | Current Industry Scale |
| Eligible geographic conditions | Actual geographic eligibility |
| Required technology identifiers | Verified technology prerequisites |
| Permitted production configurations | Selected production configuration |
| Default Opening Company Capital requirement | Actual Opening Company Capital transfer |

The instance must not overwrite canonical Industry Type configuration when its own state changes.

Any permitted instance-specific override must be explicit, validated and governed by the relevant rules.

### 4.5 Industry State Separation

The following states must remain distinct:

- Establishment state.
- Operational state.
- Industry Scale.
- Workforce fulfilment.
- Company financial state.
- Physical facility condition.
- Physical production capacity.
- Actual production.
- Inventory availability.
- Geographic Industry Area condition.

A change in one state must not implicitly change another unless a defined rule or interface requires it.

For example, a Company may have insufficient Company Capital while its physical facilities remain intact. An Industry may have available workers while lacking necessary production inputs. An Industry may have a high intended Scale while physical capacity limits actual output.

Each condition must be represented independently.

---

## 5. Industry Scale

### 5.1 Definition

Industry Scale represents the intended economic magnitude at which an Industry operates.

It is a dynamic economic property determined through the Economy System's Industry Scale rules.

Industry Scale informs the magnitude of the Industry's:

- Labour demand.
- Capital requirements.
- Input demand.
- Intended production activity.
- Potential output value.
- Potential revenue.
- Potential operating expenditure.
- Infrastructure requirements where explicitly linked to Scale.

Industry Scale is not itself a measure of physical output.

### 5.2 Distinction from Physical Production Capacity

Industry Scale and Physical Production Capacity must not be treated as interchangeable.

Industry Scale represents the intended economic scale of activity.

Physical Production Capacity represents the maximum output that the configured physical production system can support under its current constraints.

Actual Production represents the quantity physically produced during the relevant simulation interval.

These are separate values:

`Industry Scale ≠ Physical Production Capacity ≠ Actual Production`

An Industry may have a high intended Scale but insufficient facilities, inputs, energy or labour to achieve the associated production objective.

Conversely, an Industry may possess physical facilities capable of supporting more production than its current economic Scale intends.

The Production System resolves physical capacity and actual output.

### 5.3 Scale Inputs

The Industry Scale rules must explicitly define the influence of the following relevant conditions:

1. Industry Type and its configured Scale rules.
2. Available Company Capital.
3. Available labour.
4. Availability of appropriately qualified labour.
5. Required input availability.
6. Required Strategic Resources, where applicable.
7. Relevant technology.
8. Government restrictions and permissions.
9. Available infrastructure.
10. Industry establishment and operating state.
11. Senior Executive decision-making.
12. Applicable economic policy.
13. Physical production constraints supplied by the Production System.
14. Other explicitly defined Industry-specific constraints.

Not every factor must directly modify Industry Scale. The configuration must specify which factors affect Scale, which constrain intended activity and which affect only actual production.

### 5.4 Scale Calculation

The Industry Scale calculation must use an explicitly defined algorithm.

It must identify:

- The canonical representation of Industry Scale.
- The permitted minimum and maximum values.
- The source of each input.
- The calculation performed on each input.
- The order of operations.
- The update frequency.
- The treatment of missing inputs.
- The treatment of insufficient resources.
- The treatment of Government restrictions.
- The treatment of changes in Company Capital.
- The treatment of Industry expansion and contraction.
- Any hysteresis, smoothing or adjustment limits.
- Any relevant rounding rules.

An implementation must not use undefined values such as `Scale Modifier` or `Economic Efficiency` without defining their source and mathematical operation.

The exact Scale algorithm must be finalised in the Economy System's economic calculation specification and reflected in the Technical Specifications.

### 5.5 Scale Changes

Industry Scale may increase or decrease as the economic conditions governing it change.

A change in Scale must update the relevant economic requirements through the Economy System.

Depending on the configured model, this may affect:

- Required labour.
- Intended input demand.
- Intended production activity.
- Expected operating expenditure.
- Potential revenue.
- Capital requirements.
- Infrastructure requirements.

A Scale change does not automatically construct or destroy physical facilities.

If physical capacity must change, the relevant expansion, construction, decommissioning or facility acquisition mechanism must resolve that change.

### 5.6 Scale and Establishment

Establishment defines the initial conditions under which an Industry instance is created.

The initial Industry Scale must be explicitly defined by the applicable establishment configuration or initialisation rules.

The Industry must not receive an arbitrary Scale simply because it has been created.

If the initial Scale cannot be calculated because mandatory data is unavailable, the establishment operation must fail or follow an explicitly defined initialisation policy.

---

## 6. Industry Establishment and Financing

### 6.1 Establishment Authority

Industry establishment is subject to the applicable Government and Economy rules.

Under the existing default model, the Government authorises establishment and funds the required establishment expenditure from the Nation Treasury.

The exact political authority to propose, approve, restrict or prohibit establishment belongs to the Government System.

Industry Data defines the prerequisites and economic data needed to validate the establishment request.

### 6.2 Industry Establishment Capital Requirement

Every Industry Type must define an Industry Establishment Capital Requirement denominated in Dollars.

This requirement represents the specified establishment expenditure associated with creating the Industry under the relevant rules.

It is distinct from:

- Opening Company Capital.
- Ongoing operating expenditure.
- Capital needed to expand Industry Scale.
- Physical facility construction costs unless explicitly included in the establishment definition.
- Equipment acquisition costs unless explicitly included in the establishment definition.
- Input procurement.
- Inventory acquisition.
- Future maintenance or replacement expenditure.

The establishment requirement must not be interpreted as a direct measure of physical production capacity or facility value.

The precise establishment cost of each Industry Type must be supplied by canonical Industry configuration.

### 6.3 Establishment Expenditure Recipient

The establishment configuration must identify the accounting destination of the Industry Establishment Capital Requirement.

The financial movement must specify:

- Source account.
- Destination account or expenditure category.
- Amount.
- Currency.
- Transaction purpose.
- Associated Industry establishment request.
- Relevant economic date.
- Resulting financial record.

The implementation must not silently discard the establishment payment or convert it into Company Capital, physical facility value or inventory.

Where the payment is an expenditure rather than a transfer to another spendable account, the accounting treatment must be defined by the Economy and Government financial rules.

### 6.4 Opening Company Capital

Every establishment configuration must explicitly define whether an Opening Company Capital allocation is required and, where applicable, its amount.

Opening Company Capital is transferred from the Nation Treasury to the associated Company's Company Capital balance.

The allocation is distinct from the Industry Establishment Capital Requirement.

The amount must not be assumed to equal the establishment expenditure.

The resulting Company Capital is the Company's spendable financial balance. It is not physical inventory, production capacity, facility value or a separate Industry-level cash account.

### 6.5 Total Establishment Outlay

Where both establishment expenditure and Opening Company Capital are required, the total Treasury outlay is:

`Total Establishment Outlay = Industry Establishment Expenditure + Opening Company Capital Allocation`

Any additional explicitly defined establishment payments must be added as separate, auditable components.

Each component must be recorded exactly once.

The same expenditure must not be counted twice in Government accounts or simultaneously treated as both an expenditure and a transfer of spendable funds without an explicit accounting rule.

### 6.6 Establishment Prerequisites

Every Industry Type must define its mandatory establishment prerequisites.

Depending on the Industry Type, these may include:

- Sufficient Nation Treasury funds.
- Required Opening Company Capital.
- Appropriate geographic eligibility.
- Required Strategic Resource access.
- Required technology.
- Infrastructure access.
- Qualified labour availability.
- Government authorisation.
- Legal permissions.
- Environmental restrictions.
- Appropriate production configuration.
- Required facility conditions where the establishment rules explicitly require them.
- Other explicitly configured prerequisites.

Each prerequisite must identify:

1. The condition to be satisfied.
2. Whether the condition is mandatory or optional.
3. The system that owns the relevant state.
4. The validation procedure or referenced validation rule.
5. The result if the condition is not satisfied.

A prerequisite must not be considered satisfied merely because a similarly named entity exists somewhere in the world.

For example, access to a Strategic Resource must be validated against the relevant geographic, ownership, legal, logistical and resource-access rules where those conditions apply.

### 6.7 Strategic Resource Prerequisites

The existence of a Strategic Resource is not a universal prerequisite for establishing an Industry.

A Strategic Resource prerequisite applies only when the relevant Industry Type or establishment configuration explicitly requires it.

The Industry configuration must identify the relevant resource and the required form of access.

Possible access requirements include:

- Geographic eligibility to operate at a deposit.
- A valid extraction operation.
- Legal access to the resource.
- An available supply route.
- Access to extracted and inventoried stock.

These conditions are not equivalent.

A deposit may exist without an Industry operating there. An Industry may exist without extracting a resource. A resource may be extracted but unavailable to another Industry because it has not been delivered to an accessible inventory location.

The Production and Inventory Systems determine physical extraction, stock and delivery. Industry Data defines any establishment prerequisite requiring access to those conditions.

### 6.8 Establishment Validation

An establishment request must be validated before authoritative state is changed.

Validation must confirm:

- The Industry Type exists.
- The proposed Industry ID is valid and unused.
- The Company association is valid.
- The Nation association is valid.
- The geographic Industry Area is valid or can be created through the authorised establishment process.
- All mandatory prerequisites are satisfied.
- The Industry Establishment Capital Requirement is defined.
- The Opening Company Capital requirement is defined.
- The Nation Treasury can fund every required establishment payment.
- The initial Industry Scale can be initialised.
- Required system references can be resolved.
- The resulting entity state satisfies all required invariants.

If any mandatory condition fails, establishment must be rejected.

### 6.9 Atomic Establishment

Industry establishment must be treated as one atomic logical operation.

On success, the operation creates or commits all required entity references and financial movements.

On failure, the operation must leave no partial authoritative state.

A failed attempt must not:

- Create a partially registered Industry.
- Create an orphaned Company.
- Deduct Treasury funds.
- Transfer Opening Company Capital.
- Create an invalid Industry Area.
- Create partial physical facilities.
- Leave incomplete production or inventory references.

Where an associated Company must be created as part of establishment, that creation must be included in the same atomic operation.

Where the Company is created separately under an explicitly defined mechanism, the establishment process must validate that the existing Company is eligible to operate the Industry.

The exact transaction, rollback and save-state recovery implementation belongs in the Technical Specifications.

### 6.10 Starting-World Initialisation

Industries loaded from scenario starting data are not new establishment events.

They must not incur retroactive establishment expenditure merely because they are loaded into the simulation.

Starting-world initialisation must provide valid Industry, Company, Nation and geographic references, together with the financial and operational state required by the scenario.

Initialisation must distinguish historical or scenario-provided starting balances from financial movements occurring after the simulation begins.

---

## 7. Industry Geographic Representation

### 7.1 Geographic Association

Every Industry instance must reference a geographic Industry Area or an explicitly approved alternative representation appropriate to its activity.

The geographic representation uses the Terrain System's Area Mapping Primitive.

Industry Areas represent the spatial association of an Industry with the world. They do not independently own the Industry's economic state or physical production capacity.

### 7.2 Industry Area Properties

Industry Area data must reference the geographic properties defined by the Terrain System rather than creating a competing geographic schema.

Where the Terrain System defines the applicable attributes, Industry Areas may use:

- Geographic geometry.
- Height and elevation references.
- Movement effects.
- Visibility effects.
- Hit probability effects.
- Health points or other relevant area-condition properties.
- Spatial overlap and containment relationships.
- Relevant geographic identifiers.

The exact meaning, permitted range and simulation effects of these attributes remain governed by the Terrain Objects specification.

Industry Data must not redefine their mathematical behaviour.

### 7.3 Industry-Specific Geographic Properties

Industry configuration may define additional geographic requirements, including:

- Permitted terrain categories.
- Minimum or maximum area requirements.
- Access to coastlines or navigable waterways.
- Access to transport infrastructure.
- Access to energy infrastructure.
- Geographic restrictions.
- Environmental suitability.
- Proximity requirements.
- Resource deposit access.
- Other explicitly defined geographic prerequisites.

Each geographic requirement must identify the source data and validation rule.

No geographic requirement should be inferred from the Industry Type name alone.

### 7.4 Industry Area Size

Industry Area size and Industry Scale are independent properties.

A large Industry Area does not automatically indicate a large Industry Scale.

A small Industry Area does not automatically indicate a small Industry Scale.

The geographic footprint must satisfy the relevant geographic requirements. Industry Scale must be determined through the Economy System's Scale rules.

Where a geographic area constrains production, the Production System must consume the relevant constraint through an explicit interface.

### 7.5 Multiple Facilities

An Industry may use multiple physical production facilities if its production configuration permits this.

The Industry Area identifies the geographic association of the Industry, while individual facilities are represented through the Production System's physical entity model.

An Industry Area must not be treated as a single production facility.

Similarly, a physical facility must not automatically be treated as a separate Industry instance.

The relationship between Industries, Industry Areas and facilities must be explicit in the authoritative data.

### 7.6 Geographic Overlap

Industry Areas may overlap with other geographic objects where the Terrain System permits it.

Overlap does not imply shared ownership, shared Company Capital, shared production capacity or shared inventory.

Any effects arising from geographic overlap must be governed by the Terrain System and the relevant receiving system.

### 7.7 Industry Area Condition and Physical Facility Condition

The condition of an Industry Area and the condition of the Industry's physical facilities must remain separate.

An Industry Area may remain geographically valid while one or more facilities are damaged.

A facility may remain physically intact while access routes or surrounding infrastructure are disrupted.

The Industry's operational state must be derived from the relevant constraints without collapsing them into one ambiguous health value.

---

## 8. Workforce Requirements

### 8.1 Workforce Ownership

The Population System owns population demographics, Population Types, the available labour force and population-level educational attainment.

The Economy System owns the Industry's economic labour demand and records its employment state.

Industry Data defines the workforce requirements against which available labour is assessed.

The Industry does not independently create population, education or qualified workers.

### 8.2 Workforce Requirement Definition

Each Industry Type must define its workforce requirement model.

The model must identify:

- Relevant employment categories.
- Required labour quantities or the rules for calculating them.
- Relevant Population Types.
- Required education levels where applicable.
- Required professional qualifications where applicable.
- Required workforce skills where explicitly modelled.
- The relationship between Industry Scale and labour demand.
- The applicable wage configuration.
- The permitted method of calculating labour fulfilment.

Not every Industry Type requires every employment category or qualification.

Requirements must be explicitly configured rather than inferred from an Industry Type's name.

### 8.3 Employment Composition

An Industry's employment composition is represented by its assigned workers across the relevant Population Types and employment categories.

The Population System defines the Population Types.

Industry Data defines which categories of labour the Industry requires.

The Economy System records the employment allocation and its economic consequences.

Possible categories include:

- Rural Labourers.
- Industrial Labourers.
- Professional Class.
- Civil Servants.
- Academics and Researchers.
- Other explicitly defined Population Types or employment categories.

The list above does not establish that every category is available in every scenario or required by every Industry.

The canonical Population Type identifiers must be supplied by the Population System.

### 8.4 Required Labour

Industry labour demand must be derived from the applicable workforce requirement configuration and the Industry's current economic Scale.

The precise calculation must define:

- The unit of labour.
- The relationship between Scale and required workers.
- The treatment of different employment categories.
- The treatment of part-time or fractional labour if supported.
- The treatment of education and qualification requirements.
- The handling of insufficient labour.
- The update frequency.
- The rounding rules.

The system must not use an undefined Labour Multiplier without specifying its source and operation.

### 8.5 Available Labour

Available labour is supplied by the Population System.

The Economy System must not assume that the total population equals the available workforce.

The relevant workforce allocation must account for the applicable Population System rules, including population composition, labour-force participation and qualification availability where modelled.

An Industry cannot employ more workers of a particular category than the allocation rules permit.

The exact labour allocation algorithm belongs to the Economy and Population interface specifications.

### 8.6 Labour Fulfilment

Labour Fulfilment represents the extent to which an Industry's defined labour requirements are met.

Where a normalised fulfilment ratio is used, its permitted range is:

`0 ≤ Labour Fulfilment ≤ 1`

Under this representation:

- `1.00` represents 100% fulfilment.
- `0.75` represents 75% fulfilment.
- `0.00` represents no allocated labour.

Where an Industry has multiple required employment categories, the calculation must explicitly define how fulfilment is calculated across those categories.

A simple average must not be assumed to be correct for every Industry.

Labour Fulfilment is an economic workforce measure. Its effect on actual physical production must be defined by the Production System's operating rules.

Industry Data must not independently calculate actual production from Labour Fulfilment.

### 8.7 Employment Changes

When an Industry hires or releases workers, the Economy and Population Systems must update their respective authoritative states.

Hiring must not create new population.

Releasing workers must return the relevant allocation to the available labour pool according to Population System rules.

The released workers do not automatically become individually simulated unemployed characters unless that mechanic is explicitly defined elsewhere.

### 8.8 Wages

Industry workforce configuration must define the applicable base wage data for each relevant employment category.

Where the canonical wage model uses annual gross salaries, the daily conversion is:

`Daily Gross Salary = Annual Gross Salary / 365`

Where actual gross salaries depend on economic conditions, the wage calculation must identify each relevant input and its mathematical effect.

The system must not apply an undefined Wage Modifier.

The Economy System calculates wage expenditure and records the resulting financial transactions under the applicable payment rules.

Industry Data defines the relevant workforce and wage configuration, not a separate financial account for wage payments.

### 8.9 Education Requirements

An Industry Type may define minimum education requirements for specific employment categories.

Education policy, laws, public funding and institutional governance belong to the Government System.

Population-level educational attainment belongs to the Population System.

Scientific and technological knowledge belongs to the Research System.

The Economy System uses the resulting workforce qualification information to validate or calculate labour allocation.

An Industry must reference the canonical education or qualification identifiers supplied by the relevant system.

It must not create an independent education hierarchy.

### 8.10 Insufficient Qualified Labour

When the available qualified workforce is insufficient, the Industry's labour allocation must reflect the actual shortfall.

The response to insufficient qualified labour must be explicitly defined.

Depending on the relevant rules, the shortfall may constrain employment fulfilment, intended activity or another defined economic measure.

It must not automatically create qualified workers, grant technology or change Government education policy.

The effect on actual production belongs to the Production System.

---

## 9. Strategic Resource and Input Dependencies

### 9.1 Resource Dependency Definition

An Industry Type may define dependencies on Strategic Resources or other Goods and Services.

Each dependency must specify:

- The required resource or product identifier.
- Whether it is an establishment prerequisite or an operating requirement.
- The relevant geographic or logistical conditions.
- The required access condition.
- The system responsible for validating availability.
- The effect of a shortfall.
- Whether the dependency is mandatory or optional.

Industry Data must not embed a separate definition of the resource or product itself.

### 9.2 Strategic Resource Catalogue

The initial Economy System defines the following eight Strategic Resources:

| Strategic Resource | Associated Extraction Industry |
|---|---|
| Petroleum | Oil |
| Steel | Steelmaking |
| Aluminium | Industrial Manufacturing |
| Coal | Mining |
| Tungsten | Mining |
| Uranium | Mining |
| Rare-earth Metals | Mining |
| Rich Soils | Agriculture |

These associations are the existing canonical Economy System configuration.

They identify the designated Industry Type associated with the resource's extraction or production role. They do not imply that every Industry instance of that type automatically extracts the resource.

The physical meaning of each resource, its deposit representation and its extraction rules must be defined by the relevant resource and Production specifications.

### 9.3 Resource Deposits

A Resource Deposit is a geographic source of a designated Strategic Resource.

The deposit's authoritative data must include the relevant resource identity, geographic location and physical resource state as defined by the resource and Production models.

Industry Data may reference a deposit when an establishment or operating requirement depends on it.

Industry Data must not independently maintain deposit quantity, extraction capacity, extraction efficiency or depletion state.

Those properties belong to the authoritative resource and Production models.

### 9.4 Extraction Versus Availability

The existence of a deposit does not mean that its resource is available for consumption.

The relevant physical resource must be extracted or otherwise made available through an explicitly defined mechanism.

Extracted stock must be represented through the Inventory System.

An Industry's access to a resource therefore depends on the relevant physical and economic conditions, including any applicable extraction, ownership, logistics, reservation and delivery requirements.

The exact conditions depend on the consuming mechanic.

### 9.5 Input Demand

Industry Data may define the configuration required to calculate intended input demand.

The specific inputs, transformation ratios, process constraints and quantities consumed by a production process belong to Production Processes and Recipes and the Production System.

Industry Scale may inform intended input demand, but Industry Data must not independently determine actual physical consumption.

### 9.6 Input Shortages

A shortage of a required input must be communicated through the appropriate Economy, Production and Inventory interfaces.

The Industry's economic state may reflect constrained activity according to the defined economic rules.

The Production System determines whether physical production can proceed and the quantity that can actually be produced.

The Inventory System determines authoritative physical stock, reservations, transfers, delivery and consumption.

Industry Data must not directly decrement physical inventory.

---

## 10. Industry Operating and Economic State

### 10.1 Establishment State

Each Industry instance must maintain a defined establishment state.

The initial state model must distinguish at least:

- `pending_establishment`: establishment has been requested but not committed.
- `established`: the Industry has been successfully created.
- `rejected`: the establishment request failed validation.
- `closed`: the Industry has been formally closed under an authorised mechanism.

Transient request states may be implemented separately from persistent Industry instance state.

A rejected request must not remain as a partially established Industry.

The exact persistent representation of closure, historical records and reopening must be defined by the relevant Economy rules.

### 10.2 Operational State

Operational state represents the Industry's current economic operating classification.

The state model must distinguish whether an Industry:

- Is available to operate.
- Is operating under normal conditions.
- Is operating under constraints.
- Is temporarily suspended.
- Is unable to operate.
- Has permanently ceased operation.

The canonical enum values and transition rules must be defined in the Economy and Production interfaces.

Operational state must not be used as a substitute for Industry Scale, facility condition, physical production capacity or actual output.

### 10.3 Operational Eligibility

An Industry's ability to operate depends on the applicable economic, legal, workforce and physical conditions.

Relevant conditions may include:

- Valid establishment state.
- Company eligibility.
- Applicable legal permissions.
- Available workforce.
- Available inputs.
- Required technology.
- Physical facility availability.
- Infrastructure access.
- Energy availability.
- Production capacity.
- Relevant environmental or geographic constraints.

Industry Data defines the requirements. The relevant systems provide the authoritative state used to evaluate them.

### 10.4 Company Financial State

The Industry references its associated Company's financial state.

It does not maintain an independent Company Capital balance.

The Economy System is responsible for the Company's financial state, including:

- Company Capital.
- Revenue.
- Expenditure.
- Operating income.
- Financial obligations where modelled.
- Relevant commercial transaction history.

Industry Data may define the categories of economic activity used to calculate Industry expenditure, but it must not duplicate Company financial accounts.

### 10.5 Revenue

Revenue arises from completed revenue-generating transactions.

Production alone does not create revenue.

The existence or increase in value of unsold inventory does not automatically create revenue.

Where the Industry sells a Good or Service, the relevant economic transaction must identify the seller, buyer where applicable, product or Service, quantity, transaction price and completion condition.

Revenue is calculated by the Economy System under its transaction rules.

### 10.6 Expenditure

Industry-related expenditure may include:

- Labour.
- Inputs.
- Operating costs.
- Government taxes.
- Financing costs.
- Other explicitly defined economic expenditure.

The applicable expenditure categories must be configured and classified under the Economy System's accounting rules.

Physical consumption of an already-owned input does not automatically create a new cash purchase.

Inventory ownership, physical consumption and financial expenditure must remain distinct.

### 10.7 Operating Income

Operating income represents the economic result of the Industry's operating activity under the Economy System's accounting rules.

Where the relevant calculation defines operating income as revenue less operating expenditure:

`Operating Income = Revenue - Operating Expenditure`

The included expenditure categories must be explicitly specified.

A positive operating income represents an operating surplus under this definition. A negative operating income represents an operating loss.

An operating loss does not automatically close the Industry, destroy its facilities or liquidate the Company.

Insolvency, financial distress, bailout, closure and liquidation must be resolved through explicit Economy and Government rules.

---

## 11. System Interfaces

### 11.1 Interface Principles

Industry Data must identify the authoritative source for every external dependency.

A receiving system must not silently replace or independently reinterpret the source system's state.

Interfaces must use stable identifiers, explicit units and defined update semantics.

Where an interface is not yet fully specified, its required information must be documented without inventing the receiving system's internal algorithm.

### 11.2 Economy System Interface

The Economy System consumes Industry Type configuration and maintains the authoritative economic state of Industry instances.

It uses Industry Data to:

- Validate establishment.
- Resolve Industry Type configuration.
- Associate Industries with Companies and Nations.
- Initialise Industry Scale.
- Calculate labour demand.
- Evaluate economic operating requirements.
- Determine economic input demand where specified.
- Calculate revenue, expenditure and operating income.
- Record economic transactions.
- Aggregate economic statistics.

The Economy System must not use Industry Data as a substitute for physical production or inventory state.

### 11.3 Production System Interface

The Production System consumes Industry identity, type, Scale-related economic intent and the relevant production configuration.

The interface must support the exchange of:

- Industry identity.
- Industry Type.
- Associated geographic representation.
- Intended production activity.
- Relevant Scale-derived economic requirements.
- Required input information.
- Workforce fulfilment where applicable.
- Technology and operating prerequisites.
- Applicable legal or operational restrictions.
- Physical production capacity.
- Actual production outcomes.
- Production-related operating constraints.

The Economy System supplies economic intent and receives the relevant physical production results.

The Production System determines physical production capacity and actual output.

Industry Scale must not be treated as a direct measurement of production capacity.

### 11.4 Production Processes and Recipes Interface

Industry Type and Industry instance configuration may reference the production processes available to an Industry.

The relevant process definitions specify the transformation of inputs into outputs.

Industry Data must not embed complete recipes, transformation ratios, production cycles or process-level operating constraints.

The interface must resolve:

- Eligible production process identifiers.
- Required Industry Type compatibility.
- Applicable input and output definitions.
- Any relevant technology prerequisites.
- Any relevant Industry-specific restrictions.

Production Processes and Recipes remain the authoritative source for transformation definitions.

### 11.5 Inventory System Interface

Industry Data identifies the Industry and its relevant inventory relationships.

The Inventory System owns authoritative physical stock, reservations, transfers, delivery and consumption.

The interface must support:

- Industry-to-inventory-location references.
- Requests for available inputs.
- Relevant stock availability queries.
- Reservations where authorised.
- Input delivery.
- Output registration.
- Physical consumption records.
- Transfer of ownership or location where applicable.
- Inventory-related events affecting economic activity.

Industry Data must not maintain a competing authoritative quantity for physical stock.

Economic valuation may be calculated by the Economy System, but physical inventory quantities and movements remain authoritative in Inventory.

### 11.6 Good and Service Catalogue Interface

Industry Type configuration references product identifiers from the Good and Service Catalogue wherever products are required.

These references may be used by production configurations, input requirements, output eligibility and economic transactions.

Industry Data must not define a parallel Good or Service catalogue.

Product identifiers must be stable and must resolve against the canonical catalogue.

### 11.7 Population System Interface

The Population System supplies the workforce information required to determine available labour.

The interface must support:

- Available labour by relevant category.
- Population Type identifiers.
- Educational attainment or qualification information where modelled.
- Workforce availability.
- Employment allocation updates.
- The return of released labour to the appropriate pool.

The Economy System owns the Industry's economic labour demand and employment records.

The Population System remains authoritative for population and labour-force state.

### 11.8 Government System Interface

The Government System supplies or authorises the state required to validate Government-controlled prerequisites.

These may include:

- Industry establishment authority.
- Legal permissions.
- Industry-specific restrictions.
- Government subsidies.
- Applicable taxation rules.
- Education policy.
- Public funding.
- Relevant infrastructure or industrial policy.
- Nationalisation or other authorised transfers of control.

The Government System owns the Nation Treasury and public financial decisions.

Industry establishment must record the required financial movements without creating an independent Treasury balance in the Economy System.

### 11.9 Research System Interface

The Research System owns scientific research, technological knowledge, discoveries and technology prerequisites.

Industry Type configuration may reference technology identifiers required for establishment or operation.

The interface must validate the relevant technology state through the Research System.

Industry Data must not independently grant technology or maintain an alternative authoritative technology tree.

Physical facilities, equipment and their production capabilities remain subject to the relevant Production System rules.

### 11.10 Terrain System Interface

The Terrain System provides geographic geometry and the relevant Area Mapping Primitive.

Industry Data references the resulting geographic representation.

The interface must support:

- Industry Area identity.
- Geographic eligibility checks.
- Spatial association.
- Relevant terrain properties.
- Geographic overlap rules.
- Resource deposit associations where applicable.
- Other geographic constraints explicitly required by Industry configuration.

Industry Data must not independently define terrain geometry or duplicate Terrain Objects rules.

### 11.11 Infrastructure and Logistics Interfaces

Where an Industry requires transport, energy, water, communications or other infrastructure, its configuration must identify the relevant dependency.

The owning infrastructure system supplies the authoritative connection, capacity and operational state.

The Economy System may use that information to evaluate economic feasibility or operating requirements.

The Production System uses relevant physical constraints when determining actual production.

Industry Data must not assume that geographic proximity guarantees functional infrastructure access.

### 11.12 Military and Diplomacy Interfaces

Military and diplomatic rules may affect Industry establishment or operation.

Examples include:

- Military procurement demand.
- Government restrictions on industrial activity.
- Sanctions.
- Trade restrictions.
- Diplomatic permissions.
- Wartime industrial priorities.
- Damage to Industry Areas or facilities.
- Disruption of transport routes.

Industry Data records the relevant Industry associations and configured requirements.

The authoritative effects are determined by the relevant Government, Diplomacy, Military, Terrain and Production rules.

### 11.13 Interface Ownership Summary

| Data | Authoritative owner |
|---|---|
| Industry Type configuration | Industry Data / Economy configuration |
| Industry economic state | Economy |
| Company Capital | Economy |
| Nation Treasury | Government |
| Industry Scale | Economy |
| Workforce demand | Economy, configured by Industry Data |
| Available labour and population characteristics | Population |
| Education policy and public funding | Government |
| Educational attainment | Population |
| Technological knowledge and discoveries | Research |
| Geographic Industry Area | Terrain |
| Physical facilities and production capacity | Production |
| Production process definitions | Production Processes and Recipes |
| Physical stock and stock movements | Inventory |
| Good and Service identity | Good and Service Catalogue |
| Government establishment authorisation | Government |
| Physical resource extraction | Production |
| Economic transaction and Company accounting | Economy |

---

## 12. Industry Type Configuration Requirements

### 12.1 Required Configuration Fields

Every canonical Industry Type definition must provide the following fields or explicitly identify the configuration mechanism that supplies them.

| Field | Requirement |
|---|---|
| `industry_type_id` | Stable unique identifier. |
| `display_name` | Canonical display name. |
| `description` | Primary economic function. |
| `industry_establishment_capital_requirement` | Establishment expenditure in Dollars. |
| `opening_company_capital_requirement` | Required opening allocation or an explicit zero/none configuration. |
| `establishment_requirements` | Explicit list of mandatory and optional prerequisites. |
| `geographic_requirements` | Geographic eligibility and Industry Area requirements. |
| `workforce_requirements` | Labour categories and calculation rules. |
| `technology_requirements` | Canonical Research System identifiers where required. |
| `resource_dependencies` | Strategic Resource and other product dependencies where applicable. |
| `industry_scale_configuration` | Rules and references required to initialise and calculate Scale. |
| `permitted_production_configurations` | References to eligible production configurations. |
| `operational_requirements` | Configured conditions required for operation. |
| `system_interfaces` | Required dependencies and authoritative data sources. |

Any additional required fields introduced by the implementation must be documented in the schema and must not contradict the system ownership rules.

### 12.2 Explicitly Undefined Values

The Industry Type catalogue establishes the initial 16 categories but does not, by itself, establish a complete numerical configuration for each type.

The following values must be defined through canonical configuration before the affected mechanics can be fully operational:

- Industry Establishment Capital Requirement for each Industry Type.
- Opening Company Capital requirement for each Industry Type or establishment profile.
- Workforce requirements by Industry Type and Scale.
- Base wage data for relevant employment categories.
- Industry Scale calculation parameters.
- Industry-specific geographic eligibility.
- Required technology identifiers.
- Industry-specific resource dependencies.
- Initial permitted production configurations.
- Any Industry-specific establishment or operating restrictions.

An implementation must not silently replace missing required values with arbitrary defaults.

A required value that has not been configured must be treated as a validation failure or an explicit unresolved configuration error, according to the applicable initialisation rules.

### 12.3 Establishment Profiles

Where multiple establishment arrangements are required for a single Industry Type, the configuration may define separate establishment profiles.

For example, an Industry Type may support different establishment arrangements depending on its intended activity or geographic eligibility.

Each profile must explicitly define the applicable:

- Establishment expenditure.
- Opening Company Capital.
- Mandatory prerequisites.
- Geographic requirements.
- Initial Scale rules.
- Required production configuration references.
- Other applicable establishment conditions.

Profiles must not be used to conceal differences in financial treatment or bypass validation.

### 12.4 Configuration Versioning

Canonical Industry Type configuration must be versioned.

A change to establishment costs, workforce requirements, geographic restrictions or Scale parameters may affect newly established Industries and existing Industries differently.

The relevant migration or update rule must explicitly define which instances are affected.

Changing a configuration must not retroactively create establishment payments, change Company Capital or create physical facilities without an explicit rule.

---

## 13. Starting-World Data

### 13.1 Starting Date

The default starting date for Project Sovereign is 1 January 1955.

Starting-world Industry data must initialise a coherent economic world at that date.

The initialisation process must distinguish the starting state from economic events that occur after the simulation begins.

### 13.2 Starting-World Requirements

Every starting Industry instance must have:

- A unique Industry ID.
- A valid Industry Type.
- A valid associated Company.
- A valid Nation association.
- A valid Industry Area or approved geographic representation.
- An initial Industry Scale.
- A defined establishment state.
- A defined operational state.
- Valid references to required Industry configuration.
- The required workforce state or a valid initialisation procedure.
- Valid technology and resource references where required.
- Valid production configuration references where required.
- Valid inventory location references where required.

Where a starting Industry depends on a system that has not yet initialised, world initialisation must follow an explicit dependency order.

### 13.3 Starting Company State

Every starting Industry's associated Company must exist in the starting data or be created through an explicitly defined initialisation procedure.

The Company must have a valid Company Capital balance and the other financial state required by the scenario.

Starting Company Capital is an initial condition, not an establishment payment.

It must not be added to the Nation Treasury as well as to the Company balance unless a separate, explicitly defined financial relationship justifies both entries.

### 13.4 Starting Industry Scale

Every starting Industry must have an explicitly defined initial Industry Scale or a valid procedure for calculating it.

The initial Scale must be consistent with the relevant Industry configuration and initial economic state.

The starting-world loader must not assume that every Industry begins at the same Scale.

Where the initial Scale is provided directly by scenario data, the scenario must identify the value and its permitted representation.

Where the initial Scale is calculated, the calculation must use the canonical Scale rules.

### 13.5 Starting Operational State

A starting Industry may be operational, constrained, suspended or otherwise classified according to the starting scenario.

The initial state must be consistent with the available workforce, Company state, physical facilities, production configuration and other mandatory conditions.

A starting Industry must not automatically be marked fully operational solely because its entity definition exists.

### 13.6 Starting-World Industry Catalogue

The starting world must support instances of any of the 16 canonical Industry Types where the scenario data defines them.

The Industry Type catalogue is not a list of Industries that must all exist in every Nation.

The presence, absence, Scale and operating state of Industry instances must be determined by the starting scenario.

No Industry instance should be created merely because its Industry Type is available.

### 13.7 Starting Resource Associations

Where a starting Industry depends on a Strategic Resource deposit, the relevant deposit and access relationship must be valid in the starting world.

Starting resource quantities and extracted stock must be initialised through the authoritative resource and Inventory models.

The starting-world loader must not treat an unextracted deposit as an available physical stockpile.

### 13.8 Starting Financial Consistency

Starting-world initialisation must preserve the distinction between:

- Nation Treasury.
- Company Capital.
- Revenue.
- Expenditure.
- Physical inventory.
- Industry Establishment Capital Requirement.
- Opening Company Capital.

Existing starting Industries must not incur new establishment charges merely because they are loaded.

The starting data must define the initial financial balances required by the scenario without generating duplicate or retroactive transactions.

### 13.9 Initialisation Order

Starting-world initialisation must respect dependencies between the relevant systems.

The logical dependency order is:

1. Load canonical Industry Type configuration.
2. Initialise Nations and Government-owned Treasury state.
3. Initialise Companies and their financial state.
4. Initialise the required geographic world and Industry Areas.
5. Initialise relevant Population and workforce state.
6. Initialise Research and technology state.
7. Initialise resource deposits and authoritative Inventory state.
8. Create Industry instances and resolve their mandatory references.
9. Initialise or calculate Industry Scale.
10. Resolve the Industry's permitted production configuration and system interfaces.
11. Validate initial operating state against the relevant prerequisites.
12. Commit the completed starting-world state.

The implementation may use a different technical loading sequence where dependencies are resolved safely, but it must preserve the logical requirement that all authoritative references and initial conditions are valid before the world becomes available for simulation.

### 13.10 Starting-World Validation

The starting world must fail validation if it contains:

- Duplicate Industry IDs.
- Unknown Industry Type IDs.
- Missing required Company references.
- Missing Nation references.
- Invalid geographic associations.
- Missing mandatory Industry configuration.
- Invalid Industry Scale values.
- Invalid Company Capital state.
- Invalid technology references.
- Invalid mandatory resource dependencies.
- Invalid required production configuration references.
- Invalid inventory location references.
- Contradictory establishment or operational states.
- Financial state inconsistent with the canonical accounting rules.

Errors must identify the affected entity and the failed validation condition.

The simulation must not silently repair invalid economic state by creating missing Companies, inventing Industry Types or assigning arbitrary financial balances.

---

## 14. Validation and Invariants

### 14.1 Industry Identity

Every committed Industry instance must have a unique, stable Industry ID.

Every Industry instance must reference exactly one valid Industry Type.

Every Industry instance must reference exactly one associated Company and one relevant Nation.

Every required geographic reference must resolve.

### 14.2 Industry Type Integrity

Only the 16 canonical Industry Types may be used by the initial Industry taxonomy.

Scenario data may create any number of instances of those types, subject to the relevant simulation rules.

A new Industry Type must not be introduced by inventing an identifier in scenario data.

### 14.3 Financial Integrity

Company Capital and Nation Treasury must remain separate authoritative financial accounts.

Industry Establishment Capital Requirement and Opening Company Capital must remain separate values.

Every establishment-related financial movement must identify its purpose and accounting treatment.

Establishment must not deduct funds more than once or create an unrecorded transfer.

Failed establishment must not alter Treasury balances or Company Capital.

Starting-world initialisation must not trigger retroactive establishment payments.

### 14.4 Geographic Integrity

Every committed Industry instance must have a valid geographic association.

The geographic representation must conform to the Terrain System's Area Mapping Primitive and relevant spatial validation rules.

Industry Area geometry must not be treated as a substitute for Industry Scale or physical production capacity.

### 14.5 Workforce Integrity

Workforce requirements must reference valid Population Types or canonical employment categories.

Labour fulfilment must use a defined calculation.

An Industry must not receive labour allocations that violate the applicable Population System rules.

Educational attainment and qualification information must come from the authoritative owning systems.

### 14.6 Resource Integrity

Resource dependencies must reference valid resource or product identifiers.

A required resource-access condition must be validated against its authoritative source.

The existence of a deposit must not be treated as proof that physical stock is available.

Industry Data must not independently decrement deposits or inventory.

### 14.7 Production Integrity

Industry Data may reference production configurations, but it must not independently calculate physical production capacity or actual output.

Industry Scale must not be used as a substitute for physical production capacity.

Production inputs and outputs must reference canonical Goods and Services where applicable.

### 14.8 State Integrity

Establishment state, operational state, Industry Scale, workforce fulfilment, Company financial state, physical facility condition and inventory availability must remain distinct.

State transitions must follow explicit rules.

An Industry must not be considered fully operational merely because it has an established entity record.

### 14.9 Referential Integrity

All required references must resolve before the corresponding Industry state is committed.

Deleting or changing a referenced entity must not silently leave an invalid Industry.

Any removal, transfer or migration must use the relevant authorised mechanism and update dependent references consistently.

### 14.10 Deterministic Validation

Validation must produce consistent results for the same canonical configuration, entity state and simulation inputs.

Validation must not depend on incidental file ordering, display names or non-deterministic iteration.

Where several validation failures exist, the implementation should provide a stable and useful diagnostic order.

---

## 15. Required Data Schemas

### 15.1 Schema Purpose

The following schemas define the logical fields required by Industry Data.

They are illustrative data contracts rather than a commitment to a specific programming language, serialisation format or database implementation.

The final implementation schema must preserve the required fields, constraints, references and ownership boundaries.

### 15.2 Industry Type Definition

```yaml
industry_type_id: steelmaking
display_name: Steelmaking
description: Production of steel and associated steel products.

industry_establishment_capital_requirement:
  amount: null
  currency: Dollar

opening_company_capital_requirement:
  amount: null
  currency: Dollar

establishment_requirements: []

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
  - inventory
  - population
  - government
  - research
  - terrain
```

The `null` amounts and configuration references above are explicit unresolved configuration fields, not valid final operating values. The canonical Industry Type definition must supply the required values or an explicit, valid no-requirement configuration before the affected operation is permitted.

### 15.3 Industry Instance

```yaml
industry_id: industry_example_001
industry_type_id: steelmaking

company_id: company_example_001
nation_id: nation_example_001
industry_area_id: industry_area_example_001

industry_configuration_id: steelmaking_standard

establishment_state: established
operational_state: constrained

industry_scale: null

production_configuration_ids: []
facility_ids: []
inventory_location_ids: []
resource_deposit_ids: []

created_at_simulation_date: null
```

This example illustrates the logical entity structure. It is not starting-world data and does not establish a canonical Industry Scale or operational state.

A committed instance must contain a valid Industry Scale and all required references. Optional lists may be empty only where the relevant configuration permits them.

### 15.4 Industry Establishment Configuration

```yaml
establishment_profile_id: steelmaking_standard

industry_type_id: steelmaking

industry_establishment_capital_requirement:
  amount: null
  currency: Dollar
  recipient_account_id: null

opening_company_capital:
  required: true
  amount: null
  currency: Dollar

mandatory_requirements: []

optional_requirements: []

geographic_requirements: []

technology_requirements: []

resource_access_requirements: []

workforce_requirements: []

initial_scale_configuration_id: null

permitted_production_configuration_ids: []
```

All required amounts, recipient-account rules, prerequisites and configuration references must be defined before this establishment profile can be used.

### 15.5 Workforce Requirement Configuration

```yaml
workforce_requirement_id: steelmaking_workforce

industry_type_id: steelmaking

scale_relationship:
  configuration_id: null

employment_categories: []

education_requirements: []

qualification_requirements: []

base_wage_configuration_id: null

labour_fulfilment_configuration_id: null
```

Employment categories must reference canonical Population Types or approved employment categories.

The Scale relationship, wage configuration and fulfilment rules must be explicitly defined by their authoritative specifications.

### 15.6 Industry Area Reference

```yaml
industry_area_id: industry_area_example_001

terrain_area_reference_id: terrain_area_example_001

industry_id: industry_example_001

geographic_requirements_configuration_id: null
```

The Terrain System remains authoritative for geographic geometry and its applicable spatial attributes.

The Industry Area reference must not duplicate the authoritative geographic representation.

---

## 16. Industry Establishment Workflow

### 16.1 Request

An authorised establishment request identifies:

- The requested Industry Type.
- The relevant Nation.
- The associated Company or the permitted Company-creation arrangement.
- The proposed Industry Area.
- The applicable establishment profile.
- Any required configuration choices.
- The requested initial Scale or the rule used to determine it.

The request must be validated against the current authoritative world state.

### 16.2 Prerequisite Validation

The establishment process resolves the canonical Industry Type and establishment profile.

It validates all mandatory financial, geographic, legal, technological, workforce and resource requirements.

Any requirement supplied by another system must be checked against that system's authoritative state.

### 16.3 Financial Validation

The establishment process calculates the complete required Treasury outlay from the configured establishment expenditure and Opening Company Capital allocation, including any additional explicit establishment payments.

It verifies that the Nation Treasury can fund all required payments.

No funds are deducted during validation.

### 16.4 Entity Preparation

The establishment process prepares the Industry instance and any required associated Company or Industry Area entities.

All required identifiers and references must be valid.

The initial Industry Scale must be supplied or calculated through the configured rules.

The operation must not create physical production facilities merely because an Industry is established. Any required facility creation must occur through the explicitly defined Production or construction interface.

### 16.5 Commit

If all validation succeeds, the establishment process commits the entity changes and financial movements as one atomic logical operation.

The resulting Industry must have valid identity, ownership, jurisdiction, geographic association, configuration and initial economic state.

### 16.6 Rejection

If any mandatory validation fails, the establishment request is rejected.

The operation must not leave partial entity creation or financial changes.

The failure must identify the relevant validation condition so that the requesting system or Player can understand why establishment was rejected.

### 16.7 Post-Establishment Operation

Following successful establishment, the Industry becomes eligible for operating-state evaluation.

Its ability to operate depends on the relevant economic, workforce, legal, geographic, technological, inventory and physical production conditions.

Successful establishment must not be interpreted as proof of full production readiness.

---

## 17. Data Lifecycle and Change Management

### 17.1 Industry Creation

Industry creation must follow the establishment workflow or an authorised starting-world initialisation procedure.

Direct creation of an Industry instance outside these mechanisms is not permitted in authoritative simulation state.

### 17.2 Industry Configuration Changes

Changes to an Industry's configuration must identify the affected fields and the rules governing their update.

A configuration change must not automatically create financial transactions, alter physical facilities or change inventory unless an explicit rule requires those effects.

### 17.3 Industry Transfer

Where an Industry may be transferred to another Company or Nation, the transfer must use an explicit authorised mechanism.

The transfer must define the resulting Company association, jurisdiction, financial treatment, legal permissions and geographic relationships.

A transfer must not silently move Company Capital, inventory, facilities or liabilities unless the relevant rules explicitly require it.

### 17.4 Industry Expansion

Industry expansion may change Industry Scale or other economic requirements.

Any required physical expansion must be resolved through the Production and relevant construction systems.

Additional establishment expenditure, capital allocation, facility construction or input acquisition must be separately defined.

An increase in Industry Scale must not automatically create physical production capacity.

### 17.5 Industry Contraction

Industry contraction may reduce intended economic activity and its associated requirements.

The resulting treatment of employment, financial commitments, facilities, inventory and production processes must follow the relevant system rules.

A reduction in Scale must not automatically destroy facilities, eliminate inventory or close the Industry.

### 17.6 Industry Closure

Industry closure must use an explicit authorised mechanism.

Closure must define the resulting establishment and operational state, as well as the treatment of any relevant Company relationships and outstanding obligations.

Closure must not automatically destroy the associated Company or erase its financial and historical records.

Physical facilities and inventory must be handled through their respective owning systems.

### 17.7 Industry Reopening

If reopening is supported, its requirements must be explicitly defined.

The reopening process must validate the Industry's current configuration, legal eligibility, Company relationship and any relevant workforce, technology, geographic and physical operating conditions.

Reopening must not automatically recreate destroyed facilities or replenish depleted inventory.

### 17.8 Historical Records

Where historical Industry events are recorded, the record must preserve stable Industry identity and the relevant event details.

Changes to display names or configuration must not invalidate historical references.

The precise historical-record model belongs in the relevant Economy and Technical Specifications.

---

## 18. Implementation Requirements

### 18.1 Canonical Configuration

The initial Industry taxonomy must be stored as canonical configuration data.

The implementation must not duplicate the 16 Industry Type definitions across unrelated systems.

Other systems must reference the canonical Industry Type identifiers.

### 18.2 Data Validation

The implementation must validate Industry Type configuration and Industry instances at load time and at the relevant state transitions.

Invalid mandatory configuration must produce explicit errors.

The implementation must not silently substitute arbitrary establishment costs, Scale values, workforce requirements or resource prerequisites.

### 18.3 Authoritative State

The implementation must preserve the authoritative ownership of economic, geographic, population, research, production and inventory data.

References and cached derived values may be used for performance, but they must not become competing authoritative state.

Any cache invalidation and state synchronisation rules belong in the Technical Specifications.

### 18.4 Deterministic State Transitions

Industry establishment, configuration changes and other authoritative transitions must produce deterministic results.

Financial movements and entity changes must be committed consistently.

A failed operation must not leave partial authoritative state.

### 18.5 Save and Load

The saved world must preserve the Industry identities, configuration references and state required to resume simulation.

Loading a saved Industry must not repeat its original establishment payments or recreate its original entities.

The loader must validate the saved state against the relevant schema and migration rules.

### 18.6 Error Reporting

Industry configuration and instance validation errors must identify:

- The affected Industry Type or Industry ID.
- The field or reference that failed validation.
- The relevant validation condition.
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
- Industry closure and transfer rules where implemented.
- Save and load consistency.
- Deterministic state transitions.

---

## 19. Acceptance Criteria

Industry Data is considered complete for implementation when the following conditions are satisfied.

### 19.1 Industry Taxonomy

- [ ] All 16 canonical Industry Types are defined.
- [ ] Every Industry Type has a stable identifier.
- [ ] Industry Types are distinct from individual Industry instances.
- [ ] Product definitions are referenced rather than duplicated.
- [ ] Industry-specific requirements are explicitly configured.

### 19.2 Industry Instances

- [ ] Every Industry instance has a unique ID.
- [ ] Every Industry references one valid Industry Type.
- [ ] Every Industry references one associated Company.
- [ ] Every Industry references its relevant Nation.
- [ ] Every Industry has a valid geographic representation.
- [ ] Required configuration and system references resolve.

### 19.3 Industry Scale

- [ ] Industry Scale is distinct from physical production capacity.
- [ ] The Scale configuration defines its inputs and calculation rules.
- [ ] Initial Scale is supplied or calculated through an explicit rule.
- [ ] Scale changes do not automatically create physical facilities.
- [ ] Scale-related economic requirements are defined through the relevant Economy rules.

### 19.4 Establishment and Financing

- [ ] Every Industry Type has an explicit establishment configuration.
- [ ] Establishment expenditure is distinct from Opening Company Capital.
- [ ] All required financial movements identify their purpose and destination.
- [ ] Mandatory prerequisites are validated before commit.
- [ ] Failed establishment produces no partial entity or financial changes.
- [ ] Starting Industries incur no retroactive establishment payments.

### 19.5 Geographic and Workforce Requirements

- [ ] Industry Areas reference the Terrain System's geographic representation.
- [ ] Geographic requirements are explicitly configured.
- [ ] Workforce requirements reference canonical Population data.
- [ ] Education policy and educational attainment remain under their respective owning systems.
- [ ] Labour fulfilment is calculated through explicit rules.

### 19.6 Cross-System Interfaces

- [ ] Production owns physical production capacity and actual output.
- [ ] Production Processes and Recipes own input-to-output transformations.
- [ ] Inventory owns physical stock, reservations, transfers, delivery and consumption.
- [ ] The Good and Service Catalogue owns product definitions.
- [ ] Government owns Treasury state, education policy and public financial authority.
- [ ] Research owns technological knowledge and discoveries.
- [ ] Population owns demographics, labour-force state and educational attainment.
- [ ] Terrain owns geographic geometry.
- [ ] Economic and physical state cannot silently diverge through duplicate ownership.

### 19.7 Starting-World Integrity

- [ ] The default starting date is 1 January 1955.
- [ ] Starting Industry instances have valid Company and Nation references.
- [ ] Starting financial balances are consistent with the canonical accounting rules.
- [ ] Resource deposits are distinct from extracted physical stock.
- [ ] Industry operating state is consistent with its mandatory prerequisites.
- [ ] Invalid starting data produces explicit validation errors.

---

## 20. Dependencies and Next Specifications

Industry Data depends on the Economy System and the authoritative interfaces supplied by Population, Government, Research, Terrain and the other referenced systems.

The following documents complete the industrial production and physical stock model in the agreed order.

### 20.1 Next: Production System

**Path:** `docs/systems/production/production-system.md`

The Production System must define:

- Physical production capacity.
- Physical production facilities and equipment.
- Production availability and operating constraints.
- Actual output calculation.
- Physical input consumption and output generation.
- Production interruptions and physical damage.
- Resource extraction and depletion.
- The relationship between Industry Scale and physical production.
- Interfaces with Industry Data and Inventory.

The Production System must use the Industry Type and Industry instance model defined here rather than creating a competing industrial entity model.

### 20.2 Production Processes and Recipes

**Path:** `docs/systems/production/production-processes-and-recipes.md`

This document must define the specific processes that transform inputs into outputs, including the relevant transformation rules, input requirements, outputs and process-level constraints.

It must reference canonical Goods and Services and remain distinct from Industry Type configuration.

### 20.3 Inventory System

**Path:** `docs/systems/inventory/inventory-system.md`

The Inventory System must establish authoritative physical stock, reservations, transfers, delivery and consumption.

It must define the physical stock state used by Industries and production processes without transferring inventory ownership to Industry Data or the Economy System.

### 20.4 Good and Service Catalogue

**Path:** `docs/reference/economy/good-and-service-catalogue.md`

The Good and Service Catalogue must formalise the product definitions referenced by the Economy, Production and Inventory Systems.

Industry Data and production configurations must reference its canonical identifiers.

---

## 21. Final Specification Rules

The following rules are authoritative for Industry Data.

1. Project Sovereign begins with 16 canonical Industry Types.
2. An Industry Type is a reusable definition. An Industry is an individual economic establishment.
3. Every Industry instance is associated with exactly one Company and one relevant Nation.
4. Every Industry has a valid geographic representation through the Terrain System.
5. Industry Scale represents intended economic magnitude, not physical production capacity or actual output.
6. Industry Establishment Capital Requirement and Opening Company Capital are distinct financial values and movements.
7. The Nation Treasury and Company Capital remain separate authoritative accounts.
8. Industry establishment requires explicit prerequisite validation and atomic commitment of required state changes.
9. Starting-world Industries do not incur retroactive establishment charges.
10. Strategic Resource deposits are not equivalent to extracted, available or delivered physical stock.
11. Industry Data defines workforce requirements; Population owns demographic, labour-force and educational-attainment state.
12. Government owns education policy, public education funding and institutional governance.
13. Research owns scientific research, technological knowledge and discoveries.
14. Production owns physical production capacity and actual production.
15. Production Processes and Recipes own the definitions of input-to-output transformations.
16. Inventory owns authoritative physical stock, reservations, transfers, delivery and consumption.
17. The Good and Service Catalogue owns canonical product definitions.
18. Industry Data must not duplicate authoritative state belonging to another system.
19. Missing mandatory configuration must produce an explicit validation error rather than an invented default.
20. All Industry interfaces must preserve deterministic simulation behaviour and consistent state transitions.

**End of Industry Data specification.**
