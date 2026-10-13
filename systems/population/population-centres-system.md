# Population Centres System

**Canonical Path:** `systems/population/population-centres-system.md`  
**System:** Population Centres and Demographics  
**Document Type:** Systems Specification  
**Status:** Foundational Specification  
**Starting World Date:** 1 January 1955

---

## 1. Purpose

Population Centres represent inhabited settlements and significant concentrations of civilian population within the World. They provide the geographic settlement framework through which the Population System simulates civilian inhabitants, demographics, labour, needs, living conditions, cultural composition and local social development.

A Population Centre represents the settlement and its geographic footprint, not merely a numeric population value. Initial settlement classes are:

- Village
- Town
- City
- Capital City

These classes are descriptive settlement types. They do not by themselves determine population, legal status, administrative powers, economic output or military importance. Such properties are defined by authoritative data and the systems that own them.

Population Centres are Area features in the World Map. They may overlap or intersect Terrain Objects such as roads, railways, rivers, vegetation and ground. Their geographic footprint and the terrain features within it remain separately identifiable.

## 2. Ownership and Domain Boundaries

Population Centres require explicit separation between settlement geography and the simulated population.

### 2.1 World Map Ownership

The World Map System owns the shared coordinate model, spatial geometry, spatial queries and geographic feature identity.

### 2.2 Population System Ownership

The Population System owns population totals, demographic cohorts, births, deaths, migration, labour-force availability, dependants, education, employment, needs, population groups, religion, cultural affiliation and other population state defined by its specification.

### 2.3 Economy Ownership

The Economy System owns financial records and economic transactions, including prices, revenue, expenditure, taxes, wages, income, wealth accounting and GDP calculation where defined by its rules.

A Population Centre may expose economic indicators calculated by Economy, but must not maintain an independent competing GDP or financial ledger.

### 2.4 Government and Nation Ownership

Government owns legislation, policy and administrative rules. Nation-level political organisation, national policy and legal status are owned by the relevant Government and Political Systems. A Population Centre may have a capital or administrative designation, but that designation does not itself grant authority.

### 2.5 Industry, Production and Inventory Ownership

Industry Data owns Industry Type definitions and establishment profiles. Production owns facilities, capacity, orders and execution. Inventory owns physical stock. Population Centres may contain, neighbour or be served by these entities, but do not own their production, inventory or financial state.

## 3. Geographic Representation

Each Population Centre must have:

- a stable Population Centre ID;
- a name and any localisation or historical names;
- an Area geometry;
- a settlement class;
- an associated Nation or jurisdiction reference where applicable;
- a defined initial scenario state;
- references to associated features and administrative entities where applicable.

The geometry represents the settlement footprint used by the World Map and relevant simulation systems. It does not imply that every point inside the Area is built-up or inhabited at equal density.

Population density may be uniform, zonal, rasterised or represented through finer settlement subareas depending on the implementation. The chosen representation must be explicit and must support meaningful differences between a dense urban core, low-density outskirts, industrial districts and undeveloped land where the simulation models those distinctions.

The Population Centre is not required to be a single point. A centre point may be derived for labels, routing or user-interface purposes, but the Area geometry remains authoritative for spatial containment and area-based effects.

## 4. Settlement Class

Settlement class describes the broad scale and role of a settlement.

- **Village:** a small settlement, commonly with limited built-up area and a relatively small population.
- **Town:** a settlement serving a local area with a more developed service and commercial base.
- **City:** a major settlement with substantial population, infrastructure, services and economic activity.
- **Capital City:** a settlement designated as a capital by the relevant political authority or scenario definition.

These descriptions are not hard-coded numeric thresholds. If population thresholds are later adopted, they must be configured in reference data and must account for historical and regional differences. A capital designation is a political or administrative status and may coexist with an underlying settlement scale classification. If the data model needs to express both, store them as separate attributes rather than forcing them into one exclusive enum.

## 5. Geographic Attributes and Military Effects

Population Centres may have geographic and physical attributes relevant to simulation, including:

- terrain altitude and local elevation profile;
- settlement density or built-up intensity;
- building or infrastructure density where modelled;
- movement and access characteristics;
- visibility and line-of-sight characteristics;
- cover and protection characteristics;
- damage and destruction state for represented settlement structures or zones.

Population Centres must not blindly duplicate every Terrain Object attribute as a second independent truth. Terrain Objects remain the source for terrain features. Settlement-specific effects should be represented as settlement attributes or explicit linked features.

The Movement System determines movement through built-up areas. The Visibility and Detection System determines observation through settlement geometry and structures. Combat and Weapons determine weapon effects, civilian harm and structural damage. Engineering and Infrastructure determine repair and reconstruction.

Population Centre population density may influence exposure or expected civilian presence, but it must not be used as a substitute for a weapon's actual effects, blast model, impact location or damage calculation.

## 6. Population State

Population is dynamic simulation state, not a static map label. A Population Centre's population may increase, decrease, become displaced or be redistributed through time.

The Population System should represent changes through explicit demographic and movement processes, including births, deaths, immigration, emigration, internal migration, displacement and refugee movement. Each process must define its unit, simulation interval, causal inputs and accounting rules.

A Population Centre may become partially evacuated or depopulated while retaining its geographic identity. The system must not delete and recreate the settlement solely because its inhabitants have moved.

### 6.1 Population Total and Cohorts

Population total is the count of civilian inhabitants assigned to the Population Centre under the canonical Population System rules.

Age distribution should be represented as population counts or shares across explicitly defined cohorts. The initial broad cohorts proposed for scenario configuration are:

- 0–14 years;
- 15–64 years;
- 65 years and over.

These bands are a starting model, not a universal demographic truth. They must be reconciled with the Labour and Legislation rules. If laws alter legal working age or retirement age, the underlying age distribution must remain unchanged; labour eligibility is derived from age cohorts plus applicable policy and capability rules.

The sum of mutually exclusive age cohorts must reconcile to the Population Total, subject only to a documented treatment of unknown or unclassified ages.

### 6.2 Births and Deaths

Births and deaths are flows that change Population Total over a simulation interval. Rates must specify their denominator and time basis, such as events per 1,000 inhabitants per year, rather than using an undefined percentage.

The Population System owns demographic calculations. War, disease, deprivation, living conditions and access to services may be causal inputs where those systems define them. Combat damage and civilian casualties must be resolved by Combat and Weapons, then reported to Population as a committed demographic event to avoid double-counting deaths.

### 6.3 Immigration, Emigration and Internal Migration

Immigration is population entering a Nation from outside that Nation. Emigration is population leaving a Nation. Internal migration is movement between Population Centres within a Nation. Refugee and forced-displacement movements require explicit classifications so they are not counted again as ordinary migration.

Each movement must have an origin, destination where known, quantity, cause or movement class, time, and resolution status. Population totals must be debited and credited consistently. Cross-border movement must not create or erase population merely because the receiving or originating centre is unloaded or outside the active viewport.

### 6.4 Refugees and Displacement

Refugees and internally displaced people are people in a displacement state, not an additional population category to be added on top of the Population Total. The Population System must define whether displaced people remain assigned to an origin centre, are assigned to a destination centre on arrival, or are tracked through a separate in-transit record. The chosen model must avoid counting the same people in both places simultaneously.

A displaced population may move to a nearby safe Population Centre, another region or another Nation. Destination selection must consider route accessibility, safety, capacity, border policy and other explicitly modelled factors. “Nearest settlement” must not be the only rule unless the simulation intentionally abstracts away those constraints.

## 7. Labour, Dependants and Education

### 7.1 Labour Force

The labour force is derived from the population eligible and available to work under the Population, Labour and Legislation rules. It is not automatically equal to the entire 15–64 age cohort.

Eligibility and availability may depend on age, legislation, health, education, conscription, caregiving, participation decisions and other defined factors. Child labour or later retirement, if enabled by law, changes labour eligibility without changing the underlying age cohort.

The Population System determines demographic eligibility and availability. Employment assignments and workforce demand must be coordinated with Industry Data, Production and Economy. A worker must not be counted as simultaneously employed in incompatible full-time assignments unless the workforce model explicitly supports that arrangement.

### 7.2 Dependants

Dependants are people not currently counted as available labour under the defined model, including children, retirees and others outside the labour force. Dependants are a derived or classified population state and must not be added again to Population Total.

### 7.3 Education

Average Education describes the educational profile of the population or labour force, depending on the field's explicit definition. These must be distinct fields if both are required.

A proposed initial education scale is:

- No Formal Education
- Primary Education
- Secondary Education
- Higher Education
- Advanced Research Education

Education should ideally be represented as cohort counts or shares by level, with a derived average or summary value for the interface. A single average can conceal whether a centre has a balanced education profile or a small highly educated group alongside a large uneducated population.

Education changes through schooling, cohort ageing, migration and policy-driven access. Standard of Living, legislation and access to education may influence education outcomes, but education should not automatically increase every simulation tick without a defined rate, cohort process and time horizon.

Education may affect workforce suitability, productivity, research staffing, political participation, social mobility, migration and happiness through explicit rules in the relevant systems. An Industry's education requirements are defined by Industry Data and Production configuration, not by a hard-coded exception for a named industry.

## 8. Needs, Living Conditions and Wellbeing

Population needs describe desired or required access to goods and services. They must reference canonical entries in the Good and Service Catalogue using stable `catalogue_id` values and explicit quantities and units.

The Catalogue defines what a Good or Service is and its canonical measurement or delivery unit. Population defines the requirement, demand profile and consumption interval. Economy defines price, affordability and financial settlement. Production determines whether domestic production occurs. Inventory records eligible physical stock. Logistics governs transport and delivery. These are separate responsibilities.

A need definition should specify:

- catalogue entry;
- required or desired quantity;
- unit of measure;
- consumption or assessment interval;
- applicable population cohort or group;
- priority or criticality;
- substitution rules, if supported;
- source and version of the need definition.

A population need is not itself an Inventory reservation or proof of consumption. Consumption is resolved through the relevant systems and must be recorded once.

Standard of Living is a derived or composite indicator whose inputs, weights, scale and update interval must be explicitly defined. It should not be treated as a single authoritative cause of every other attribute. Access to necessities, housing, healthcare, education, income, security and other factors may contribute according to the eventual model.

Population Happiness is a separate wellbeing or satisfaction measure. It must have a documented scale, inputs and update rules. It is not interchangeable with wealth, Standard of Living, political support or migration attraction.

Wealth Distribution describes how wealth is distributed across households or population groups. Economy owns the financial quantities; Population may store derived distributions or classifications for demographic and political calculations. The same wealth must not be independently created in Population.

## 9. Population Types, Culture and Religion

Population Centres may contain multiple social, cultural, religious and political-interest groups. These are not necessarily exclusive and should not be compressed into one label.

### 9.1 Cultural Affiliation

Culture represents shared identities and practices defined by the Culture and Society reference model. A Population Centre may contain multiple cultural groups, each with a population count or share. Primary Culture is a derived designation for display or reporting, not a replacement for the full distribution.

Cultural traits may influence identity, social norms, migration preferences, political interests or cohesion where the Culture and Politics systems define those effects. Traits must be data-defined and should not be assigned solely from income, industry mix or constitutional form.

### 9.2 Religion

Religion should be represented as a distribution across defined religious affiliations and, where required, unaffiliated populations. A single Religion field is insufficient where pluralism is modelled.

Religious affiliation may influence social institutions, political interests, cultural practice and migration where those relationships are defined. Religion must not automatically determine political ideology or behaviour.

### 9.3 Population Interest Groups

Population Types in the original draft are better modelled as **Population Interest Groups**: social or economic groupings with shared material interests, occupational position, education, identity or policy preferences. They are not necessarily innate population classes, and a person may belong to several dimensions at once.

Illustrative mid-twentieth-century or Cold War-era group labels may include:

- Agricultural Workers and Smallholders
- Industrial Workers
- Skilled Trades and Technicians
- Urban Service Workers
- Small Business Owners and Shopkeepers
- Industrialists and Large Business Owners
- Landowners and Rural Elites
- Civil Servants and Public Administrators
- Teachers and Education Professionals
- Scientists and Technical Professionals
- University Students
- Professional and Managerial Classes
- Religious Communities and Institutions
- Veterans and Military Families
- Organised Labour
- Rural Traditionalists
- Urban Reformists
- Social Democrats
- Communists and Revolutionary Socialists
- Nationalists
- Liberal Constitutionalists
- Conservative Traditionalists

These are candidate classifications, not a mandatory flat enum. Some are economic or occupational groups, while others are political tendencies. The implementation should separate **social/economic groups**, **cultural affiliations** and **political ideologies** into different taxonomies, then model relationships between them. Names and political alignments must be configurable and context-sensitive.

A population group's size, interests and political effects should be calculated from defined membership and influence rules. Do not assume that all wealthy, educated or urban inhabitants share the same ideology or cultural traits.

## 10. Local Economic Indicators

Population Centres may display local economic indicators where a canonical system can calculate them. Examples include employment, unemployment, local income, consumption, business activity, tax contribution and local or attributed GDP.

Every indicator must declare its owning system and calculation method.

- **GDP:** Economy owns the calculation and the treatment of production, geography, ownership and value added. A Population Centre may display an attributed local GDP measure, but it must not independently sum Industry Scale or gross sales and label the result GDP.
- **Employment and Unemployment:** Population and Labour rules determine the relevant labour denominator; Economy and Industry/Production provide employment or job data. Definitions must distinguish labour force, employed persons, unemployed persons and people outside the labour force.
- **Tax:** Government and Economy own assessment, liability, payment and receipts. A Population Centre may display taxes attributable to residents or local activity only when the attribution rule is explicit.
- **Industries:** A centre may link to nearby or contained Industry establishments. The link is geographic or administrative and does not imply ownership of the Industry.
- **Standard of Living and Wealth:** These are calculated from defined economic and demographic inputs and must not become independent financial ledgers.

Missing data must be represented explicitly. The absence of a local indicator must not be interpreted as zero economic activity.

## 11. Damage, Conflict and Civilian Harm

Population Centres may be affected by combat, occupation, infrastructure damage, shortages, disease, displacement and other events. The settlement's geographic identity persists while population and physical conditions change.

Combat and Weapons determine weapon impact, damage and casualties under their rules. Population consumes committed casualty and displacement events to update demographic state. Economy, Infrastructure and Logistics consume the relevant disruptions through their own rules.

Civilian casualties must not be calculated solely from a generic circular error probable (CEP) value. CEP describes a weapon delivery error characteristic, not by itself the full distribution of effects or casualty count. The relevant weapon model must resolve impact uncertainty, munition effects, blast or fragmentation where applicable, protected or exposed populations, structures, timing and other defined inputs. Population then records the resulting civilian losses without applying the same event a second time.

Damage to a settlement may be represented at the Population Centre level, through linked buildings or infrastructure objects, or through finer urban subareas. The selected model must define how damage affects housing, services, capacity, accessibility and recovery without double-counting the same destroyed asset across multiple layers.

## 12. Nation-Level Aggregation

Population Centre data contributes to national demographic summaries. National totals must be derived from a consistent set of centre-level assignments plus any explicitly modelled unassigned, nomadic, maritime, displaced or in-transit populations.

Aggregation must avoid double-counting people who are represented in transit, displaced or linked to multiple administrative regions. If a population belongs to a Nation politically but resides in another Nation, the system must distinguish residence, citizenship and jurisdiction where those concepts are modelled.

A centre's Culture, Religion, Population Interest Groups and demographic composition may differ substantially from national averages. Nation-level summaries should aggregate distributions, not simply copy the characteristics of the capital or the most populous centre.

## 13. Time, Update Order and Persistence

Population values are simulation state and must persist when a region is outside the active viewport. Demographic processes use the global simulation scheduler and canonical simulation-time representation.

The Population System must define the update cadence and ordering for births, deaths, migration, labour availability, education, needs and derived indicators. Systems should exchange committed events or explicitly ordered inputs rather than independently modifying the same population total.

Save/load must preserve stable Population Centre identity, population assignments, demographic distributions, in-transit movement records and state version. Scenario authoring may set initial values; the running simulation owns subsequent changes.

## 14. Data Validation

Validation must cover:

- stable and unique Population Centre IDs;
- valid Area geometry and coordinate reference;
- valid settlement class and separate capital designation where used;
- valid Nation and jurisdiction references;
- age-cohort reconciliation with Population Total;
- valid demographic rates with denominators and time bases;
- non-negative population counts and defined rounding rules;
- valid migration origins, destinations and movement states;
- no duplicate counting of refugees or displaced people;
- labour-force calculations consistent with cohort and policy rules;
- canonical Catalogue IDs and explicit units for needs;
- separation of financial values from demographic state;
- declared ownership for GDP, tax, employment and other indicators;
- valid cultural, religious and interest-group taxonomies;
- persistent references for damaged, evacuated or depopulated centres.

## 15. Acceptance Criteria

The Population Centres System is conformant when:

1. Settlement geography is distinct from the population living there.
2. Population Centres are Area features in the shared World Map.
3. Population and demographic values are dynamic and persistent.
4. Population, labour, dependants and economic indicators have explicit ownership and do not duplicate one another.
5. Needs reference canonical Catalogue entries and explicit units.
6. Culture, religion, social groups and political ideologies are represented as distinct concepts.
7. Combat casualty events update Population once and are not recalculated independently.
8. National aggregation reconciles with local assignments and explicitly modelled unassigned or in-transit populations.
9. Terrain, rendering and settlement class do not silently determine demographic or political outcomes.
