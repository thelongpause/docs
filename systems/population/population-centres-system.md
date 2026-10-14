# Population System: Population Centres and Demographics

**Canonical Path:** `systems/population/population-centres-system.md`  
**System:** Population  
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

The Population System owns civilian population totals, demographic cohorts, births, deaths, migration, residence assignments, labour-force availability, dependants, education, population-group distributions, religion, cultural affiliation, needs and other population state defined by its specification. Active military personnel, personnel records and service assignments belong to Military Systems. Recruitment or conscription must affect civilian labour availability and population reporting through an explicit cross-system contract and must never duplicate the same people across civilian and military records. The convention for whether serving personnel remain included in a settlement's displayed resident count must be resolved with the Military System architecture before its data schema is frozen. Employment demand, job capacity, wage payments and financial wealth remain with the systems that own those facts; Population may hold derived employment or wellbeing indicators only under an explicit interface.

### 2.3 Economy Ownership

The Economy System owns financial records and economic transactions, including prices, revenue, expenditure, taxes, wages, income, wealth accounting and GDP calculation where defined by its rules.

A Population Centre may expose economic indicators calculated by Economy, but must not maintain an independent competing GDP or financial ledger.

### 2.4 Government and Nation Ownership

Government owns legislation, policy and administrative rules. National Territory and Control owns legal sovereignty, territorial claims and effective control; Government and other political systems own national identity, citizenship policy and administrative jurisdiction. A Population Centre's location, resident population, citizenship distribution, legal sovereign, effective controller and administrative jurisdiction are distinct relationships. A Population Centre may have a capital or administrative designation, but that designation does not itself grant authority.

### 2.5 Industry, Production and Inventory Ownership

Industry Data owns Industry Type definitions and establishment profiles. Production owns facilities, capacity, orders and execution. Inventory owns physical stock. Population Centres may contain, neighbour or be served by these entities, but do not own their production, inventory or financial state.

## 3. Geographic Representation

Each Population Centre must have:

- a stable Population Centre ID;
- a name and any localisation or historical names;
- an Area geometry;
- a settlement class;
- zero or more administrative-jurisdiction references where applicable, with effective dates;
- any required political-geography references resolved through National Territory and Control rather than embedded as an independent sovereignty or control value;
- a defined initial scenario state;
- references to associated features and administrative entities where applicable.

The geometry represents the settlement footprint used by the World Map and relevant simulation systems. It does not imply that every point inside the Area is built-up or inhabited at equal density.

Population density may be uniform, zonal, rasterised or represented through finer settlement subareas depending on the implementation. The chosen representation must be explicit and must support meaningful differences between a dense urban core, low-density outskirts, industrial districts and undeveloped land where the simulation models those distinctions.

### 3.1 Population Distribution Within a Centre

Population density and built-up intensity are separate attributes. Built-up intensity describes the presence or concentration of buildings and urban fabric; population density describes the number and distribution of residents. Neither may be inferred solely from the Population Centre's total population or polygon area.

The World Map and infrastructure layers provide the geometry and physical settlement features. Population owns the distribution of residents across any modelled zones or spatial density representation. The representation may use authored subareas, a raster or tiled density field, or another validated aggregate model, but it must support location-sensitive civilian exposure and demographic sampling where those features are enabled.

The sum of population assigned to spatial subareas or density partitions must reconcile to the Population Centre's authoritative cohort counts. If detailed distribution data is unavailable, the system must use a declared fallback model and preserve the fact that the result is estimated; it must not silently assume uniform density when that assumption materially changes civilian exposure or other spatial calculations.

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

Age distribution must be represented at sufficient resolution to support cohort ageing, labour eligibility, education, demographic projections and later manpower interfaces. The canonical simulation representation should use single-year age cohorts from age 0 through age 99, plus an open-ended 100+ cohort. Broader reporting bands, including 0–14, 15–64 and 65+, are derived aggregations and must not replace the underlying cohorts.

Age cohort definitions are data-model conventions, not assumptions about legal working age, retirement age, conscription eligibility or other policy. Those rules are owned by the relevant Labour, Government and Military Systems. A change in policy must not rewrite the underlying age distribution.

The sum of mutually exclusive age cohorts must reconcile to Population Total, subject only to an explicit and consistently handled unknown-age cohort. If unknown ages are represented, they must not silently disappear from totals or be treated as eligible for an age-dependent process.

### 6.2 Births and Deaths

Births and deaths are flows that change Population Total over a simulation interval. Each rate or probability must specify its applicable population subgroup or age cohort, denominator, time basis, causal inputs and valid range. Rates may be expressed as events per 1,000 relevant persons per year or another explicitly defined unit; an undefined percentage is not sufficient.

The Population System owns demographic calculations. War, disease, deprivation, living conditions and access to services may be causal inputs where those systems define them. Combat damage and civilian casualties must be resolved by Combat and Weapons, then reported to Population as a committed demographic event to avoid double-counting deaths.

### 6.3 Immigration, Emigration and Internal Migration

Immigration is population entering a Nation from outside that Nation. Emigration is population leaving a Nation. Internal migration is movement between Population Centres within a Nation. Refugee and forced-displacement movements require explicit classifications so they are not counted again as ordinary migration.

Each movement must have a stable event or movement ID, origin, destination where known, quantity by applicable age cohort, cause or movement class, effective time, and resolution status. Where other demographic distributions are modelled, the event must either carry the relevant composition or identify an explicit deterministic selection rule for deriving it. Population totals and cohort counts must be debited and credited consistently, and the same committed movement must not be applied more than once. Cross-border movement must not create or erase population merely because the receiving or originating centre is unloaded or outside the active viewport.

### 6.4 Refugees and Displacement

Refugees and internally displaced people are people in a displacement state, not an additional population category to be added on top of the Population Total. The Population System must define whether displaced people remain assigned to an origin centre, are assigned to a destination centre on arrival, or are tracked through a separate in-transit record. The chosen model must avoid counting the same people in both places simultaneously.

A displaced population may move to a nearby safe Population Centre, another region or another Nation. Destination selection must consider route accessibility, safety, capacity, border policy and other explicitly modelled factors. “Nearest settlement” must not be the only rule unless the simulation intentionally abstracts away those constraints.

### 6.5 Population Representation and Accounting

The Population System uses aggregate demographic counts as its default simulation representation. It does not require one persistent entity per civilian. Aggregate counts must retain enough demographic resolution for the processes enabled by the simulation, and derived summaries must be reproducible from the authoritative distributions.

For each Population Centre, the Population Total is the count of people currently assigned as resident civilians under the Population System's assignment rules. The total must reconcile to its mutually exclusive age cohorts and any explicitly modelled unknown-age cohort. Cultural affiliation, religion, social/economic group, political interest, education and wellbeing dimensions may overlap; their counts must not be added together as if they were mutually exclusive components of Population Total unless a specific taxonomy explicitly defines them that way.

Population counts are integer persons. Rate-based demographic calculations may produce fractional expected flows internally; those fractions must be accumulated using a deterministic method and converted to integer persons under a documented rounding rule. The same sub-person remainder must not be independently rounded in multiple systems.

For each Population Centre and accounting interval:

`Closing Residents = Opening Residents + Births + Arrivals - Deaths - Departures + Authorised Adjustments`

Cohort ageing and reclassification change the distribution of residents but do not, by themselves, change Population Total. Authorised adjustments must identify their cause, effective time, source and quantity; they must not be used to conceal an unreconciled flow.

At the simulation-wide level, internal movements between centres cancel out. The total population changes through births, deaths, explicitly modelled migration across the simulation world's boundary, and authorised scenario adjustments. People in transit must be represented in a single in-transit or displacement assignment after departure and before arrival. They must not remain counted as resident at the origin and also be counted at the destination or in transit.

A committed casualty, birth, death, arrival, departure or other population-changing event must be applied exactly once. Replaying or retrying the same event must not duplicate its effect. A destination that is unloaded, outside the viewport or not currently simulated in detail remains part of the authoritative accounting model.

### 6.6 Cohort Ageing and Eligibility

At the annual demographic boundary, each age cohort advances according to a documented deterministic transition rule. People reaching age 100 enter the open-ended 100+ cohort. Births enter the age-0 cohort at their effective time. The implementation must document how partial-year timing is handled and must apply the same convention consistently across all centres.

Age cohort is a demographic fact, not a determination of employment, legal adulthood, military eligibility or conscription status. Those are derived from age, health or capability where modelled, and the applicable law or military rules. A policy change may change eligibility without changing a person's age cohort.

The single-year cohort model is an aggregate approximation. It does not imply that the simulation stores each person's exact birthday or individual identity. If a later subsystem requires finer-grained individual-level representation, it must define an explicit interface and reconciliation rules rather than silently replacing aggregate counts.

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

A population need is not itself an Inventory reservation or proof of consumption. Consumption is resolved through the relevant systems and must be recorded once. A need requirement, requested quantity, delivered quantity, confirmed consumption and unmet shortfall are separate values. Physical Goods require the relevant delivery and consumption events; Services require an explicit service-delivery or access record. Population must not infer consumption merely from production, purchase, shipment, or stock existing somewhere in the economy.

Need fulfilment is evaluated against the applicable population count, cohort or group, requirement quantity and assessment interval. Where the model supports substitution, the substituted Catalogue entry and conversion rule must be explicit. A shortfall may contribute to wellbeing, health, migration or other outcomes only through defined causal rules in the owning systems.

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

The Population System must define update cadence and ordering for births, deaths, cohort ageing, migration, labour availability, education, needs and derived indicators. Demographic rates must declare their denominator and time basis and be prorated against elapsed simulation time where the scheduler interval differs from that basis. Cohort ageing occurs on a defined annual demographic boundary; event-driven changes such as births, deaths, recruitment transfers where modelled, casualties and migration are applied at their committed effective time.

Systems must exchange committed events or explicitly ordered inputs rather than independently modifying the same population total. Population-changing events require stable identifiers and idempotent application so retries, save/load, or event replay cannot double-count people. Fractional expected flows from rate-based calculations must use a documented deterministic accumulation and rounding method rather than being silently discarded each update.

Save/load must preserve stable Population Centre identity, cohort counts, residence and displacement assignments, in-transit movement records, applied-event identity or equivalent replay protection, and state version. Scenario authoring may set initial values; the running simulation owns subsequent changes.

## 14. Data Validation

Validation must cover:

- stable and unique Population Centre IDs;
- valid Area geometry and coordinate reference;
- valid settlement class and separate capital designation where used;
- valid Nation and jurisdiction references;
- age-cohort reconciliation with Population Total, including the explicit handling of unknown ages;
- population stock-flow reconciliation across births, deaths, arrivals, departures, displacement and authorised adjustments;
- unique application of population-changing events and deterministic fractional-flow rounding;
- distinct residence, citizenship, legal sovereignty, effective control and administrative jurisdiction references;
- no duplicate representation of the same people in resident, displaced, in-transit or military-personnel records;
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

## 15. Required Reference Data and Open Decisions

The Population System defines the ownership boundaries, accounting invariants and minimum behavioural contracts. Scenario values and model parameters must be supplied through versioned reference data or explicit system rules; they must not be guessed by the implementation.

The following items require explicit values or a documented model before Population can be considered implementation-ready:

- Baseline fertility and mortality schedules, including age-specific rates, scenario variation, time basis and any supported effects from health, deprivation, conflict or access to services.
- The deterministic calculation interval for rate-based demographic flows, fractional remainder handling, and the precise annual cohort-ageing convention.
- The default and scenario-authored model for spatial population distribution within settlements, including uncertainty and fallback behaviour when detailed density data is absent.
- Labour-force participation, eligibility, availability, retirement, caregiving and education progression parameters, with clear separation between demographic facts and policy.
- Per-capita or cohort-specific need requirements, assessment intervals, priority rules, substitution eligibility, and the interface for recording confirmed delivery, access and consumption.
- The definitions, input variables, weights, scales and update intervals for Standard of Living and Population Happiness. Neither indicator may become an unexplained catch-all modifier.
- The reference taxonomies and membership rules for cultural affiliation, religion, social/economic groups and political ideologies, including whether each distribution is exclusive or overlapping.
- The rules by which migration responds to safety, employment, living conditions, policy, routes, capacity, family or community ties, and border restrictions.
- The representation of unassigned, nomadic, maritime, displaced and in-transit civilian populations, and how each contributes to local and national reporting.
- The spatial and temporal exposure model connecting population distribution to committed civilian-harm events, without duplicating Combat and Weapons calculations.
- The reporting convention for serving military personnel with respect to settlement resident counts, to be agreed with the Military Systems architecture before military data schemas are frozen.
- The rules for changes to Population Centre geometry, administrative jurisdiction, names and classifications over time without silently reallocating residents or changing demographic totals.

Each item must have an owning system, explicit inputs and outputs, a time basis where relevant, and validation rules. An unresolved parameter must be represented as missing or unconfigured, not silently treated as zero or a universal default.

## 16. Acceptance Criteria

The Population System is conformant when:

1. Settlement geography is distinct from the population living there.
2. Population Centres are Area features in the shared World Map.
3. Population and demographic values are dynamic and persistent.
4. Population Total reconciles to mutually exclusive age cohorts and committed stock-flow events.
5. Residence, citizenship, sovereignty, territorial control and administrative jurisdiction remain distinct.
6. Population-changing events are idempotent and cannot double-count or silently erase people.
7. Population, labour, dependants and economic indicators have explicit ownership and do not duplicate one another.
8. Needs reference canonical Catalogue entries and explicit units.
9. Culture, religion, social groups and political ideologies are represented as distinct concepts.
10. Combat casualty events update Population once and are not recalculated independently.
11. National aggregation reconciles with local assignments and explicitly modelled unassigned or in-transit populations.
12. Terrain, rendering and settlement class do not silently determine demographic or political outcomes.
