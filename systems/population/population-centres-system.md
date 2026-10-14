# Population System: Population Centres and Demographics

**Canonical Path:** `systems/population/population-centres-system.md`  
**System:** Population  
**Document Type:** Systems Specification  
**Status:** Behavioural Specification; scenario reference data and implementation schema remain separate  
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

Refugees and internally displaced people carry a displacement status; they are not an additional population category to be added on top of Population Total. The canonical accounting transition is: people remain resident at the origin until departure is committed; after departure they are assigned to one in-transit movement record; after arrival is committed they are assigned to the destination centre or an explicitly modelled unassigned population pool. They must not be counted as resident at the origin while also in transit or resident at the destination. Displacement status may persist after arrival and is not a separate population count.

A displaced population may move to a nearby safe Population Centre, another region or another Nation. Destination selection must consider route accessibility, safety, capacity, border policy and other explicitly modelled factors. “Nearest settlement” must not be the only rule unless the simulation intentionally abstracts away those constraints. A failed or interrupted movement must resolve to one authoritative assignment state, with any return to origin represented by a committed movement transition.

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

### 7.4 Labour Supply Accounting and Employment Interface

Population publishes a labour-supply snapshot for a defined location, period and labour classification. At minimum, the interface distinguishes working-age or legally eligible population, labour-force participants, available job-seekers, people unavailable for work under the current model, employed people sourced from committed employment assignments, and people outside the labour force. These are distinct counts. The labour force is the sum of employed people and unemployed job-seekers under the chosen statistical definition; it is not the same as the eligible population. The system must document whether temporary absence, seasonal work, underemployment, multiple jobs and informal work are represented.

Population calculates demographic eligibility and participation inputs. The system owning employment assignments commits hiring, separation and job allocation. Population consumes those events or snapshots to update derived employment classifications. A job offer, vacancy, labour demand value or wage payment alone does not establish that a person is employed.

Where the simulation uses aggregate labour rather than identified workers, allocation must still reconcile: the same labour quantity cannot be assigned to incompatible jobs, and committed employment must not exceed the eligible workforce after applying defined availability rules. Any fractional or probabilistic allocation must use deterministic accumulation and integer reconciliation. Workforce skills and education profiles must be available as explicit distributions when required; a centre-wide average must not be treated as proof that every worker has a qualification.

A recruitment or conscription event transfers or reclassifies people through an explicit interface. It must reduce civilian labour availability where appropriate, must not create duplicate persons, and must preserve Population accounting. Military Systems own service status, personnel records and assignment. The convention for settlement resident display remains a cross-system decision and must not alter the no-duplication invariant.

### 7.5 Education Progression

Education is represented as a distribution by defined education level and, where needed for progression, by age cohort. A transition from one level to another must identify the eligible cohort, transition rule, effective interval, source of eligibility and destination level. Education progression may depend on school access, capacity, attendance, policy and scenario parameters, but the existence of a school or education service does not prove attendance or completion.

The transition process must conserve the affected population: progression changes education classification, not Population Total. Migration carries education composition under an explicit composition-transfer rule. If a detailed cohort-by-education cross-tab is not enabled, any derived allocation between age and education groups must declare its method and uncertainty; it must not imply unsupported individual-level precision.

Education levels are ordered categories, not inherently equal numeric intervals. Any displayed average must document its scoring and weighting convention and remain a derived indicator. Changes to education definitions must be versioned so historical simulation state remains interpretable.

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

### 8.1 Need Fulfilment and Demand Interface

For each need definition and assessment interval, Population calculates a requirement from the applicable population exposure, cohort or group and versioned per-person requirement. The interface must distinguish required quantity, requested quantity, quantity allocated or reserved, quantity dispatched, quantity delivered, quantity accessible, quantity confirmed consumed or service received, and unmet shortfall. Only the owning system may confirm the state it controls.

For physical Goods, delivered quantities must be associated with the receiving population or a declared distribution area before they count as locally accessible. Delivery to a warehouse, port or market is not automatically delivery to every resident. Confirmed consumption reduces the outstanding requirement only once. For Services, access or service delivery must be recorded using the Catalogue's defined service unit and an explicit recipient or coverage model.

Where the model uses aggregate allocation, priorities and tie-breaking must be deterministic. Fulfilment must not exceed eligible available quantity after reservations, losses and competing claims. A substitute product may fulfil a need only when the versioned need rule explicitly permits it and defines unit conversion and equivalence. Shortfall must remain visible even where a substitute partially fulfils the requirement.

Population publishes demand and receives fulfilment outcomes; it does not create physical stock, commit inventory, route shipments, settle purchases or assert consumption from economic activity alone. Each exchanged record must carry a stable reference, Catalogue ID, unit, covered population or area, effective interval, source and state.

### 8.2 Wellbeing Indicators and Causal Boundaries

Standard of Living and Population Happiness are separate derived indicators. Each configured indicator must define its scale, input measures, directionality, weights or aggregation rule, missing-data behaviour, update interval and version. Missing input is not zero unless the indicator definition explicitly establishes that interpretation.

The calculation must retain component measures where available. A composite score must not erase critical shortages, inequality, insecurity or regional variation behind a single average. National averages must be population-weighted where the underlying measure is an individual-level quantity; simple averaging of settlement averages is invalid unless explicitly intended.

A wellbeing indicator is descriptive unless a receiving system defines a causal rule that consumes it. Migration, political support, fertility, mortality, productivity and unrest must not all respond automatically to a single score. Any causal response must name the source indicator, affected population, direction, lag or assessment interval, response function, bounds and owning system. Avoid circular calculations in which wellbeing depends on migration while migration is resolved from the same newly calculated wellbeing value without a declared update order.

Wealth Distribution is a demographic or social description derived from Economy-owned financial quantities and a defined population grouping. Population may classify or aggregate wealth distribution, but must not create household balances, income, wages or assets. If household-level finance is not simulated, the approximation from economic aggregates to population groups must be explicitly marked as estimated.

## 9. Population Composition, Culture and Religion

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

### 9.4 Composition Accounting and Change

Every composition dimension must declare whether its categories are mutually exclusive, overlapping or probabilistic. Counts within a mutually exclusive and exhaustive dimension must reconcile to the applicable population denominator, including an explicit unknown or unclassified category where required. Overlapping dimensions, such as occupational identity, political interest or multiple cultural affiliations, must not be expected to sum to Population Total.

The system must distinguish a population distribution from a settlement's dominant label. Primary Culture, majority religion and largest interest group are derived reporting values. They must not replace distributions or trigger automatic political, legal or behavioural changes.

Composition changes through explicit events or versioned transition rules, including migration, education, employment change, conversion where modelled, cultural assimilation where modelled, and changes in political affiliation. Each transition must define its eligible population, time basis, inputs and accounting effect. Changes to a taxonomy must use an explicit migration or mapping rule; renaming or merging categories must not silently erase or duplicate counts.

Where demographic cohorts and composition dimensions are not stored as a full cross-tab, queries that require their intersection must use a declared allocation method and report the result as estimated. The system must not assume every age cohort has the same religious, cultural or political distribution unless scenario data or the model explicitly says so.

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

### 11.1 Civilian-Harm Event Contract

Combat and Weapons are authoritative for weapon effects and the resulting casualty and damage event. A committed civilian-harm event supplied to Population must have a stable event ID, effective time, affected geographic area or population assignment, source and event version, and explicit counts or a defined aggregate distribution for fatalities, injuries and displacement. If demographic composition of casualties is not directly resolved by the owning combat model, the event must state the deterministic sampling or allocation rule used to derive cohort impacts.

Population applies the event once. Fatalities reduce the appropriate resident or in-transit population and update age-cohort and composition counts to the resolution supported by the event. Injuries do not reduce Population Total unless a later, separate death event is committed. Displacement changes residence or assignment state but does not itself change world population. A single event may produce fatalities, injuries and displacement, but the same person must not be counted twice within a mutually exclusive outcome for the same event unless the event model explicitly represents sequential outcomes and their timing.

Events affecting an area with estimated spatial density must retain that exposure uncertainty. Population reports only the committed civilian demographic consequences; it must not recalculate weapon probability, blast effects, structural protection or casualties from CEP or another delivery-error measure alone. Missing casualty composition must not be fabricated as exact demographic knowledge.

## 12. Nation-Level Aggregation

Population Centre data contributes to national demographic summaries. National totals must be derived from a consistent set of centre-level assignments plus any explicitly modelled unassigned, nomadic, maritime, displaced or in-transit populations.

Aggregation must avoid double-counting people who are represented in transit, displaced or linked to multiple administrative regions. If a population belongs to a Nation politically but resides in another Nation, the system must distinguish residence, citizenship and jurisdiction where those concepts are modelled.

A centre's Culture, Religion, Population Interest Groups and demographic composition may differ substantially from national averages. Nation-level summaries should aggregate distributions, not simply copy the characteristics of the capital or the most populous centre.

### 12.1 Aggregation Dimensions and Reconciliation

National reporting must declare the dimension being aggregated. At minimum, distinguish residence, citizenship or nationality, administrative jurisdiction, legal sovereignty and effective control, displacement status, and in-transit status. Residence describes where a person is currently assigned for population accounting; citizenship describes political membership where modelled; jurisdiction describes administrative authority under effective-dated rules; sovereignty and effective control come from National Territory and Control. Displacement is a status, not an additional population to sum. In-transit status is a temporary accounting assignment between origin departure and destination arrival.

A national resident total uses the chosen residence definition and includes each resident assignment once. A citizenship total may include citizens resident abroad and exclude non-citizen residents, so it must not be substituted for a resident total. Territorial control or occupation does not automatically change citizenship or legal sovereignty.

For internal migration, origin departure and destination arrival must reconcile at world scale. During transit, people occupy one in-transit assignment and are not simultaneously resident at the origin or destination. Cross-border movement changes residence assignment and, where relevant, citizenship/residence distribution; it does not by itself change citizenship. Any unassigned or special population pool must have a stable identity, an explicit inclusion rule for each report and a reconciliation rule.

If administrative boundaries or Population Centre geometries change, historical values must remain interpretable. A boundary change may alter geographic attribution only through an explicit effective-dated reallocation or reporting crosswalk; it must not create births, deaths or migration merely because a polygon changed.

## 13. Time, Update Order and Persistence

Population values are simulation state and must persist when a region is outside the active viewport. Demographic processes use the global simulation scheduler and canonical simulation-time representation.

The Population System must define update cadence and ordering for births, deaths, cohort ageing, migration, labour availability, education, needs and derived indicators. Demographic rates must declare their denominator and time basis and be prorated against elapsed simulation time where the scheduler interval differs from that basis. Cohort ageing occurs on a defined annual demographic boundary; event-driven changes such as births, deaths, recruitment transfers where modelled, casualties and migration are applied at their committed effective time.

Systems must exchange committed events or explicitly ordered inputs rather than independently modifying the same population total. Population-changing events require stable identifiers and idempotent application so retries, save/load, or event replay cannot double-count people. Fractional expected flows from rate-based calculations must use a documented deterministic accumulation and rounding method rather than being silently discarded each update.

Save/load must preserve stable Population Centre identity, cohort counts, residence and displacement assignments, in-transit movement records, applied-event identity or equivalent replay protection, and state version. Scenario authoring may set initial values; the running simulation owns subsequent changes.

### 13.1 Population Update Contract

Population follows the shared simulation scheduler and does not establish an independent global clock. Each process declares its required input state, effective time, cadence, output state and whether it is event-driven or interval-driven.

Within a Population update, the logical dependency order is:

1. Ingest and validate committed external events effective at or before the update boundary, including births or deaths reported by authoritative sources, arrivals, departures, displacement, recruitment transfers, employment changes, service delivery and civilian-harm events.
2. Apply each event idempotently to the relevant population assignment and demographic distributions.
3. Resolve any due annual cohort-ageing transition using the canonical boundary convention, without changing Population Total.
4. Calculate rate-based births and deaths for the elapsed interval from the defined exposure population and versioned schedules. Accumulate fractional expected flows deterministically and commit integer outcomes as events.
5. Resolve interval-driven migration and displacement decisions from the explicitly ordered snapshot of safety, routes, capacity, policy, employment and living conditions. Commit departures, in-transit assignments and arrivals as separate state transitions.
6. Update labour eligibility and availability from the resulting demographics, education, policy and committed service/employment status.
7. Update education progression and composition transitions due in the interval, preserving their population denominators.
8. Evaluate need requirements and reconcile delivery, access and consumption records for the applicable assessment interval.
9. Calculate derived wellbeing and reporting indicators from committed inputs; publish outputs for receiving systems.

This is a Population-local logical order, not a replacement for the global scheduler. If an event is produced after the relevant phase has passed, it is applied at the next eligible boundary or at its effective time through the scheduler's event contract. Systems must not mutate Population-owned totals directly.

Rate-based calculations must define whether the exposure population is measured at interval start, interval end or another explicitly ordered point. The same people must not be used as exposure for incompatible calculations because of an accidental double update. Annual transitions, event processing and rate calculations must be deterministic across save/load and replay.

## 14. Data Validation

Validation must cover:

- stable and unique Population Centre IDs;
- valid Area geometry and coordinate reference;
- valid settlement class and separate capital designation where used;
- valid administrative-jurisdiction references and effective-date intervals; valid citizenship or nationality references where those distributions are modelled;
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
- Versioned scenario reference values for fertility and mortality schedules, plus the configured scheduler interval and annual cohort-ageing boundary. The behavioural contract requires deterministic fractional remainder handling and one canonical convention; these are not optional implementation behaviours.
- The default and scenario-authored model for spatial population distribution within settlements, including uncertainty and fallback behaviour when detailed density data is absent.
- Versioned scenario and policy parameters for labour-force participation, eligibility, availability, retirement, caregiving and education progression. Rules remain separate from demographic facts and use the labour and education accounting contracts in Section 7.
- Versioned need definitions, per-capita or cohort-specific requirements, assessment intervals, priority rules and substitution eligibility. Cross-system records must implement the requirement/fulfilment distinctions and stable references in Section 8.
- The configured definitions, input variables, weights, scales and update intervals for Standard of Living and Population Happiness. The causal boundary in Section 8.2 is mandatory regardless of selected values.
- The reference taxonomies and membership rules for cultural affiliation, religion, social/economic groups and political ideologies, including whether each distribution is exclusive or overlapping.
- The rules by which migration responds to safety, employment, living conditions, policy, routes, capacity, family or community ties, and border restrictions.
- Scenario-specific initial representation for unassigned, nomadic, maritime, displaced and in-transit civilian populations. Their mutually exclusive accounting and reporting rules are defined in Sections 6 and 12.
- The configured spatial and temporal exposure model connecting population distribution to committed civilian-harm events. The event ownership and idempotency contract in Section 11.1 is mandatory and must not duplicate Combat and Weapons calculations.
- The reporting convention for serving military personnel with respect to settlement resident counts, to be agreed with the Military Systems architecture before military data schemas are frozen.
- Scenario and authoring rules for changes to Population Centre geometry, administrative jurisdiction, names and classifications over time. Such changes require effective dates and explicit reporting crosswalks or reallocation events; they must not silently change demographic totals.

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
13. Labour supply distinguishes eligibility, participation, availability, unemployment and committed employment without assigning the same workforce twice.
14. Education transitions conserve population and do not assume that service availability proves attendance or completion.
15. Need requirement, request, allocation, delivery, access and consumption are distinct states with explicit units and idempotent records.
16. Wellbeing indicators retain their configured inputs and do not act as undocumented universal modifiers.
17. Cultural, religious, social/economic and political dimensions declare their overlap and reconciliation rules.
18. Civilian-harm events update demographic state once; injuries and displacement do not automatically reduce world population.
19. National reporting distinguishes residence, citizenship, jurisdiction, sovereignty, effective control and displacement/transit assignment.
20. The Population-local update contract is deterministic and compatible with the shared scheduler.
