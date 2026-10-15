# Project Sovereign: Population Data Schema

**Status:** Draft logical schema for review  
**Authority:** Population System behavioural specification  
**Implementation target:** PostgreSQL logical model; not a migration script  
**Canonical system specification:** [Population System: Population Centres and Demographics](../../systems/population/population-centres-system.md)

## 1. Purpose and Scope

This document translates the Population System's behavioural contracts into a logical data model. It defines authoritative population state, aggregate demographic distributions, population assignments and flows, composition, interests, spatial distribution, and the interfaces through which Population exchanges information with Economy, Government, Political Systems, Logistics, Combat and Weapons, and Military Systems.

The default representation is aggregate counts and distributions, not one database row per civilian. The schema must support local settlement simulation and national aggregation without requiring the player to manage each Population Centre or social group individually.

This document does not define scenario-specific fertility rates, wages, interest-axis values, political platforms, need quantities, wellbeing weights, migration coefficients, final SQL migrations, or a final spatial-density storage technology. Those remain versioned reference data or implementation decisions.

The schema is subordinate to the Population System specification. If a behavioural rule changes, update that specification first and then reconcile this document.

## 2. Design Principles

1. **One authoritative owner per fact.** Population owns civilian population assignments and demographic state. Other domains own their respective authoritative state.
2. **Aggregate-first simulation.** Counts and distributions are the default; individual civilian records are not required.
3. **Stable identity.** Persistent entities and committed events use stable IDs. Names, coordinates and row order are not identity.
4. **Explicit time.** Simulation-effective time is distinct from database audit time. State transitions and reference definitions are effective-dated where historical interpretation requires it.
5. **No double counting.** Every resident, in-transit aggregate and population-changing event has an unambiguous accounting role.
6. **Explicit denominator and unit.** Every rate, quantity, count and distribution identifies its population denominator, unit, interval and scope.
7. **Composition dimensions declare their semantics.** Mutually exclusive categories reconcile to a denominator; overlapping dimensions do not.
8. **Derived values are not competing truth.** National totals, age bands, dominant culture, labour indicators and interest aggregates should be derived from authoritative state where practical.
9. **No cross-domain shadow ledgers.** Population may receive or expose derived economic indicators but does not become authoritative for money, employment contracts, production, stock or military personnel records.
10. **Deterministic replay.** Retried events, saved games and replayed intervals must not double-apply population changes or produce nondeterministic rounding.
11. **Historical reproducibility.** Committed events reference the applicable rule or reference-data version where it affects the outcome.
12. **Core relationships are relational.** JSONB may hold versioned extension payloads but must not hide core counts, relationships, statuses or integrity-critical fields.

## 3. Shared Identifier and Time Conventions

Use the repository's shared identifier types and simulation-time conventions from [Data Model Foundations](data-model-foundations.md). The same identifier must use the same representation wherever referenced. Logical names below are descriptive; they do not mandate a specific UUID or integer type before the shared identifier decision is finalised.

Every effective-dated record uses the shared temporal representation. If a record has both a valid simulation interval and operational audit timestamps, they remain distinct. A mutable display name must never be used as a foreign key.

All population counts represent integer persons. Expected demographic flows may be fractional during calculation, but committed flows are integer persons and must preserve deterministic remainder state under the configured rounding convention.

## 4. Domain Ownership and Boundaries

| Fact or entity | Authoritative owner | Population schema treatment |
|---|---|---|
| Population Centre geometry and stable geographic feature ID | World Map | Reference the canonical Area feature; do not duplicate geometry |
| Civilian resident and in-transit assignment | Population | Authoritative aggregate assignment records |
| Age cohorts, births and demographic deaths | Population | Authoritative counts and committed events |
| Migration and displacement transitions | Population, using route/access inputs from owning systems | Authoritative population transition and assignment records |
| Citizenship and nationality policy | Government / political domain | Store distribution only where Population requires it; reference canonical identifiers |
| Sovereignty, claims and effective territorial control | National Territory and Control | Reference/query authoritative records; do not duplicate control |
| Administrative jurisdiction | Government / administrative geography | Effective-dated references or imported authoritative assignments |
| Job capacity, vacancies, job contracts and hiring transactions | Economy / Employment domain | Consume committed employment snapshots/events; store only derived labour classification where required |
| Company, household or personal financial balances; wages, taxes, transfers, income, wealth | Economy | Consume derived values through an interface; do not create Population-owned accounts |
| Production, physical inventory and delivery execution | Production / Inventory / Logistics | Consume delivery/access/consumption outcomes; do not duplicate stock or shipment state |
| Catalogue entries and units | Good and Service Catalogue | Reference canonical Catalogue ID and unit definitions |
| Military service status, personnel records and assignments | Military Systems | Cross-system contract only; never create duplicate military personnel |
| Weapon effects and casualty calculation | Combat and Weapons | Consume committed civilian-harm events and apply demographic consequences once |
| Political parties, organised movements, platforms, elections and political authority | Political Systems | Population publishes interest distributions; Political Systems own organisations, platforms and political outcomes |
| Population-group composition, cultural/religious distributions and interest axes | Population, with shared taxonomies where appropriate | Authoritative aggregate distributions and derived interest profiles |
| National policy and causal responses to population indicators | Government / receiving system | Population publishes indicators; it does not assume every indicator automatically changes behaviour |

These boundaries do not require separate database servers. Logical ownership and transaction contracts still apply if domains share a PostgreSQL database.

## 5. Entity Relationship Overview

The core relationship is: World Map Area feature → Population Centre → population assignment → age-cohort state. Composition and derived social state attach to a declared population scope and denominator. Events create auditable changes to assignments and distributions. Spatial partitions, when enabled, reconcile to centre-level totals. National values are derived by grouping canonical assignments under a declared aggregation dimension.

Core entity groups:

- Geographic identity: population centre.
- Assignment and accounting: Population Aggregate Group, population assignment, population event, event effect, population movement.
- Demographics: cohort state, demographic rate schedule and immutable schedule versions.
- Composition: population dimension, category, composition state.
- Group classifications: population group definition and group distribution.
- Spatial distribution: spatial partition and spatial distribution.
- Labour and education: labour snapshot and education distribution.
- Needs and wellbeing: need definition reference, need assessment, fulfilment record, indicator definition and indicator observation.
- Interests and dynamics: interest axis and version, population interest profile, interest observation, transition-rule version.
- External integration: stable domain-event references and immutable source/version identifiers.

## 6. Population Centre and Assignment

### 6.1 Population Centre

Represents Population-owned demographic state associated with a stable World Map Area feature.

Fields:
- Stable Population Centre ID.
- Canonical World Map feature ID.
- Settlement-class reference.
- Simulation-effective creation and optional retirement times.
- Optional scenario provenance reference.

Constraints:
- The referenced feature must have Area geometry.
- A Population Centre must not own or duplicate its Area geometry.
- A centre may be depopulated and remain geographically present.
- Settlement class is descriptive reference data, not a hard-coded population threshold or political authority.
- Names, label positions, localisation and historical names are not identity.

### 6.2 Population Aggregate Group and Assignment

A Population Aggregate Group is a stable identity for an aggregate set of civilians that the simulation can track through residence changes, movement, displacement and demographic events without creating one record per person. It is not a socioeconomic Population Type. A group may be split into child groups when only part of its count moves or experiences an event; the split/merge relationship and counts must be recorded explicitly. Groups may be merged only when their relevant demographic and composition state can be reconciled under a declared rule. The record contains a stable group ID, creation/provenance reference and optional parent group reference for a split. It does not require individual identities.

A Population Assignment represents that aggregate group assigned to one accounting location/state at a given simulation time. Assignment is the authoritative answer to where the group's counted population currently resides for Population accounting.

Fields:
- Stable assignment ID.
- Stable Population Aggregate Group ID.
- Assignment kind: resident, in transit, unassigned, or another explicitly modelled kind.
- Population Centre ID, required for resident assignments and otherwise nullable only where permitted by the assignment kind.
- Movement ID, required for in-transit assignments.
- Optional prior assignment ID for history.
- Optional intended destination, which does not itself make people residents there.
- Effective interval, controlled status and source event ID.

Assignments should be effective-dated or retained as immutable transition history, not silently overwritten. A current-state projection is permitted if it can be reconstructed from committed transitions.

Constraints:
- A population quantity is counted in one authoritative assignment at a time.
- Each active aggregate group has one authoritative assignment at a time, except during an atomic split/merge transition whose effects reconcile exactly.
- A group cannot be resident at origin and simultaneously counted in transit or at destination.
- In-transit assignment requires a valid movement record.
- Assignment intervals for the same aggregate group must not overlap incompatibly.
- Unassigned populations require a stable scope and explicit inclusion rules for each report.
- Interrupted movement or return to origin requires a committed transition.

### 6.3 Population Cohort State

Stores integer civilian counts by assignment and canonical age cohort.

Fields:
- Population assignment ID.
- Canonical age-cohort reference.
- Non-negative integer person count.
- Effective simulation time.
- Optional source event ID.

Constraints:
- Canonical cohorts are ages 0 through 99, plus an open-ended 100+ cohort. Unknown age is permitted only if explicitly enabled.
- Unknown-age population must not silently disappear from totals or be treated as eligible for age-dependent rules.
- Cohort counts reconcile exactly to the assignment total at the same effective time.
- Cohort ageing changes the distribution, not the assignment total.
- Historical state is not rewritten by later policy or taxonomy changes.

### 6.4 Derived Population Total

Population Total is a derived value for a declared assignment scope and effective time. It should normally be computed from authoritative cohort counts or assignment effects, not stored as independent competing truth.

A cached total is permitted for performance only if it is transactionally or deterministically reconcilable to cohort and event records. A mismatch is a validation failure, not a reason to silently overwrite source counts.

## 7. Population Events and Demographic Flows

### 7.1 Population Event

An immutable committed event that changes or classifies population state.

Fields:
- Stable event ID.
- Controlled event type: birth, demographic death, departure, arrival, displacement, reallocation, casualty consequence, cohort transition, composition transition, authorised adjustment, or another defined type.
- Effective simulation time.
- Optional operational audit time, which must not determine simulation outcomes.
- Authoritative source domain and stable upstream event ID.
- Idempotency key with centrally defined uniqueness scope.
- Applicable demographic or transition-rule version.
- Controlled event status, reason code and provenance reference.

Constraints:
- Only committed events change authoritative Population state.
- Retrying the same source event/idempotency key must not apply its effect twice.
- A committed event is immutable. Corrections use a compensating event with provenance.
- Population does not recalculate upstream combat effects, employment contracts or delivery execution.
- Late events follow the shared scheduler's replay policy; they must not be applied according to database arrival order.

### 7.2 Population Event Effect

Stores one or more typed effects of an event.

Fields:
- Parent event ID and stable effect sequence.
- Affected assignment and aggregate-group ID and, where applicable, age cohort.
- Controlled effect kind: addition, removal, transfer, reclassification or another defined effect.
- Signed integer person-count delta where applicable.
- Destination assignment for transfers where applicable.
- Composition-transfer reference or deterministic transfer rule where required.
- Source-record reference.

Constraints:
- Effects reconcile to the event's declared totals.
- Reclassification and ageing effects conserve total population.
- An event may affect multiple cohorts or assignments without concealing an unexplained discrepancy.

### 7.3 Population Movement

Represents the movement lifecycle of an aggregate civilian group. Logistics owns routes, transport assets and physical movement execution.

Fields:
- Stable movement ID.
- Movement class: internal migration, immigration, emigration, evacuation, forced displacement, refugee movement, return, or another configured class.
- Origin assignment and intended destination where known.
- Current authoritative assignment.
- Departure and arrival event references.
- Controlled status: planned, departing, in transit, arrived, interrupted, returned, cancelled, or another defined state.
- Cause code, optional Logistics movement reference, and effective interval.

Constraints:
- Planned movement does not change residence totals.
- Departure removes the quantity from the origin resident assignment and creates one in-transit assignment.
- Arrival removes it from transit and assigns it to the destination or an explicitly modelled unassigned pool.
- Internal movements conserve world population.
- Cross-world-boundary movement changes world totals only where the scenario explicitly models that boundary.
- Displacement is a status/classification, not an additional population count.
- Interruption and return require explicit committed transitions.

### 7.4 Demographic Rate Reference Data

Use versioned reference tables rather than hard-coded rates in Population Centre records.

- **Demographic rate schedule:** stable identity for fertility, mortality or another demographic flow.
- **Demographic rate schedule version:** immutable version, validity and provenance.
- **Demographic rate rule:** one row per applicable cohort/group and interval rule.

Each rate rule identifies its schedule version, applicable age cohort and any required demographic subgroup only where explicitly modelled, numerator meaning, denominator population, rate value and unit, time basis, exposure convention, causal-input references, bounded response-function reference, rounding/remainder convention, effective interval and provenance.

Rates must declare denominator, time basis, valid range, eligible subgroup and causal inputs. A rate without these fields is not implementation-ready. Historical events retain the exact schedule/rule version used.

## 8. Composition, Population Types and Group Distributions

### 8.1 Population Dimension and Category

A dimension defines a controlled way to classify population, such as cultural affiliation, religion, education level, occupation/economic class, political interest or another scenario-supported classification.

Each dimension declares:
- Stable identity and version.
- Semantic owner.
- Whether categories are mutually exclusive, overlapping or probabilistic.
- Whether categories are exhaustive.
- Applicable population denominator/scope.
- Whether unknown/unclassified is allowed.
- Whether the dimension is descriptive, a state classification or a derived indicator.
- Effective dates and taxonomy-migration rules.

Each category has a stable ID, dimension ID/version, label keys, optional parent category and effective validity. Display labels are not identifiers.

### 8.2 Population Composition State

Stores aggregate distribution values for a dimension/category at a declared scope and time.

Fields:
- Stable state ID or versioned state key.
- Scope kind and stable scope ID.
- Dimension and category references.
- Integer population count where the dimension is count-based.
- Optional derived share, not an independent authority where count and denominator exist.
- Effective simulation time.
- Estimation method where an intersection or allocation is estimated.
- Optional source event ID.

Constraints:
- Mutually exclusive, exhaustive dimensions reconcile to the relevant denominator, including unknown/unclassified values if enabled.
- Overlapping dimensions are not summed as if mutually exclusive.
- If only shares are stored, they reconcile under declared semantics and precision; prefer integer counts where available.
- Derived national distributions do not become a second authority independent of local distributions.
- Taxonomy changes require explicit mapping/transition rules.
- Unsupported cross-tab queries use a declared allocation method and are labelled estimated.

### 8.3 Population Group Definition

Represents a reusable Population Type or social/economic group such as industrial labourers, professional class, students, agricultural workers or capital owners, when enabled by scenario data.

A group is not assumed to be a universal, natural or mutually exclusive category. Its definition declares stable identity/version, name keys, membership rule and source dimensions, exclusivity/overlap semantics, valid scope and effective interval, whether it is authored/derived/scenario-initialised, and mapping rules for definition changes.

Group membership is normally represented as an aggregate distribution, not an individual membership row for every civilian.

### 8.4 Population Group Distribution

Stores the number/share assigned to a Population Type within a declared scope and time. It declares denominator, exclusivity semantics, derivation method and uncertainty where membership is inferred from aggregate source dimensions.

If the model cannot support an exact intersection of age, education, occupation and culture, it must not present a synthetic cross-tab as exact data.

### 8.5 Interest Axes and Profiles

Population Interest Axes are bounded, versioned dimensions for aggregate preferences on questions relevant to public life and policy. The axis catalogue should be extensible rather than enumerate every conceivable ideology. Possible axes include economic regulation, welfare, labour protection, immigration, trade, taxation, military posture, social change, religion in the state, political authority, nationalism, civil liberties, industrialisation and environmental regulation. These examples are conceptual, not canonical reference data.

Each interest-axis version defines:
- Stable axis identity and version.
- Neutral semantic definition and intended interpretation.
- Lower/upper bound, scale precision and direction.
- Low/high endpoint meanings or reference category IDs.
- Whether the axis is continuous, ordinal or categorical.
- Whether higher values mean more of a stated construct.
- Valid scope, effective interval and provenance.

A numeric value is meaningless without its axis definition. Do not assume all axes share a 0–100 scale or that every axis is a single continuum. Inherently multidimensional or categorical concepts must not be forced onto a misleading line.

**Population Interest Profile** defines axis values calculated or initialised for a population group/scope under a profile version. It identifies the scope/group, axis version, value or category distribution, effective time, derivation method/model version, and whether the value is authored, observed, estimated or derived.

**Population Interest Observation** stores a time-stamped value/distribution at a declared scope. Historical observations are retained so shifts can be inspected and attributed to explicit conditions and transition rules. Current interest values are not directly player-editable unless an explicit scenario or policy mechanic permits it.

**Interest Transition Rule Version** describes how inputs may change an axis over time. It identifies eligible group/scope, input indicators and owning domains, functional form or bounded transition table, direction, lag and interval, output/rate-of-change bounds, rule priority/conflict resolution, estimation behaviour and model/reference-data version.

Do not hard-code stereotypes such as an automatic ideology for an occupation. Economic insecurity, immigration, wages, safety, cultural conflict, education and other conditions may influence interests only through explicit, versioned and testable rules. A condition must not deterministically label an entire group unless the rule actually defines that outcome.

### 8.6 Political Groups and Support: Ownership Boundary

Population provides aggregate interest profiles and, where useful, population-group distributions. Political Systems own political organisations, party/group identities, platforms, membership rules, elections, mobilisation, political pressure and resulting support.

A Political Systems schema may later represent support by population group and Population Centre/nation using compatibility between an interest profile and a versioned platform. That support is not a Population-owned fact and must not be stored here as an independent authority. Population exposes input distributions through a stable interface; it does not own party platforms or assume an interest profile automatically changes party support.

## 9. Spatial Population Distribution

### 9.1 Population Spatial Partition

Defines a spatial partition or zone inside a Population Centre, linked to canonical World Map geometry or a supported spatial tile. It does not replace centre-level total or age-cohort state.

Fields include stable partition ID, Population Centre ID, spatial feature/tile reference, partition method/version, effective interval and provenance.

### 9.2 Population Spatial Distribution

Stores aggregate population counts or density estimates by partition and any demographic resolution supported by the model.

Fields include partition ID, effective time, population count/density, optional age-cohort reference, estimation method/version, source and explicit unit/spatial support (area, cell, zone or density tile).

Constraints:
- Partition counts reconcile to Population Centre assignment/cohort totals at the same effective time and resolution.
- Density is not interchangeable with built-up intensity.
- Missing spatial detail uses a declared fallback and is marked estimated where it materially affects exposure or other outcomes.
- A raster/tile implementation supports sparse storage and does not require a record for every 100 m Movement Grid location.
- The schema does not mandate one row per Movement Grid cell.

## 10. Labour, Dependants and Education

### 10.1 Population Labour Snapshot

A derived aggregate interface record, not an employment ledger. It represents a declared scope, population group, time and labour classification.

At minimum distinguish age/legal eligibility, labour-force participants, employed population sourced from committed employment assignments, unemployed job-seekers under the selected definition, people unavailable for work, people outside the labour force, and optional skill/qualification distributions.

Fields identify scope, effective interval, group/dimension references, counts, source snapshot/event references, derivation version and whether values are exact or estimated.

Constraints:
- Eligible population is not the same as labour-force participants.
- Employment is sourced from committed employment assignments, not inferred from wages, vacancies or job offers.
- The same aggregate labour quantity cannot be assigned to incompatible jobs or exceed the available eligible workforce.
- Recruitment/conscription uses an explicit Military Systems interface, reduces civilian labour availability where applicable and does not duplicate people.
- The snapshot is derived and reconcilable; it is not an independent hiring or payroll ledger.

### 10.2 Dependants

Dependent status may be represented as a composition dimension or a derived count from age, caregiving, household/family structure and other explicitly modelled criteria. It must not be assumed that all children or older people are dependants, or that every working-age person is economically independent.

If household units are not simulated, dependant ratios are aggregate estimates with explicit denominators and estimation rules. Household identity must not be fabricated solely to create an apparently precise dependency ratio.

### 10.3 Population Education Distribution

Stores population counts by education level and supported scope/cohort resolution. Education levels reference a versioned ordered taxonomy but are not assumed to have equal numeric spacing.

Progression events conserve population totals. School existence, capacity or service availability is not proof of attendance, attainment or completion. If education is not stored as a complete cross-tab with age cohorts, inferred intersections are estimated and use a declared method.

## 11. Needs, Fulfilment and Wellbeing

### 11.1 Population Need Definition Reference

Population needs reference versioned need definitions and, for physical goods/services, the canonical Good and Service Catalogue using its canonical Catalogue ID and unit contract.

The definition/version specifies the population denominator or eligible group, requirement interval, required quantity per person or applicable group, permitted substitutions, prioritisation rules and whether the need is goods-based, service-based or another explicit class. Scenario-specific quantities are reference data, not constants in a Population Centre record.

### 11.2 Population Need Assessment

Represents the need requirement for a declared scope and interval.

Fields include stable assessment ID, scope and population denominator, need definition/version, interval start/end, required quantity/unit, requested quantity/unit, calculated shortfall, estimation method, effective state and rule version. Where the model tracks multiple fulfilment stages, each stage must retain its own quantity and source reference rather than overwriting the preceding stage's quantity.

Requirement, request, reservation/allocation, dispatch, delivery, access, confirmed consumption/service receipt and unmet shortfall are distinct measures or lifecycle outcomes. They must not be collapsed into one fulfilled boolean or treated as additive quantities: the same units may appear at successive stages of a delivery lifecycle.

The assessment must declare how quantities are aggregated across its covered population, area, cohort or group, and must preserve the applicable Catalogue unit and assessment interval. If a group or area is only partially covered, the covered denominator must be explicit so that a delivery to one subset is not reported as fulfilment for the whole assessment.

### 11.3 Population Fulfilment Record

References externally owned allocation, Inventory, Logistics, service-delivery or consumption records. It may store integration status needed to reconcile Population demand, but it does not create stock or become the authority for physical movement.

A fulfilment record identifies its stable ID, source domain and event/reference, need assessment, Catalogue ID where applicable, canonical unit, quantity, covered population/area, effective time or interval, lifecycle stage/status, and provenance. Each confirmed stage must be traceable to the system that owns that fact. A current-state projection may be maintained for efficient queries, but it must be reconcilable to the source events and must not erase the history needed to distinguish dispatch, delivery, access and consumption.

The model may represent lifecycle milestones as separate immutable records or as an event history plus a derived current-state projection. That storage choice remains open. In either case, records must distinguish at least requested, allocated/reserved, dispatched, delivered, accessible, confirmed consumed/service received, failed and cancelled where those stages are enabled. Stage quantities must not be summed as if each represented additional fulfilled units.

Constraints:
- Delivery to a warehouse or market is not automatically delivery/access for all residents.
- Confirmed consumption or service receipt is sourced from the owning interface and applied once.
- Fulfilment cannot exceed eligible quantity after reservations, losses and competing claims, using a declared rule for partial coverage and competing claims.
- Substitution is allowed only by a versioned need rule with explicit equivalence and unit conversion.
- Population does not infer consumption from production, purchase, shipment or stock existing somewhere in the economy.
- Repeated source events cannot advance the same quantity through a lifecycle stage twice.
- A correction to a committed fulfilment milestone must preserve provenance and follow the source domain's correction/reversal contract.

### 11.4 Population Indicator Definition and Observation

These represent derived descriptive indicators such as Standard of Living and Population Happiness.

The versioned definition identifies scale, direction, input measures, weights or aggregation rule, missing-data treatment, assessment interval and provenance. Observations identify scope, effective time, value, contributing definition version and source snapshot references.

Constraints:
- Missing data is not zero unless the definition explicitly says so.
- National averages of individual-level quantities are population-weighted.
- Composite indicators must not conceal important shortages, inequality or regional variation; component measures remain available where possible.
- An indicator has no automatic causal effect on migration, fertility, mortality, productivity, political support or unrest. A receiving system defines the causal rule and update order.
- Wealth, earnings, savings, wages, taxes, transfers and disposable income are Economy-owned. Population may consume derived group-level economic statistics through a versioned interface; it must not create financial balances or duplicate the Economy ledger.

## 12. Population Interest Dynamics and Economic Feedback

The intended feedback loop is:

**World and policy conditions → population exposure/conditions → aggregate interest profiles → Political Systems' support and pressure calculations → Government decisions → changed conditions.**

Interest axes and group profiles are Population-owned aggregate preference state where modelled. Political organisations, platforms and political support outcomes remain owned by Political Systems. Economic conditions such as earnings, savings, employment, prices, taxes and transfers remain Economy-owned facts. Population can consume those conditions as inputs to explicit transition rules and publish derived interest profiles, but must not own the financial values.

A transition rule specifies source measure, population group, exposure denominator, time interval, direction, response function, lag, bounds, version and missing-data behaviour. For example, declining real earnings might contribute to a defined economic-security interest over time, but must not automatically and universally produce a particular view on immigration or labour policy. Each causal link is explicit, testable and versioned.

National interest values are derived from local/group profiles using declared weights and scope. The aggregation method specifies whether weights are population counts, eligible group counts or another relevant denominator. Missing or estimated local values are handled explicitly and do not silently disappear from the national result.

## 13. Civilian Harm and Population Consequences

### 13.1 External Civilian-Harm Event Interface

Combat and Weapons own weapon effects and casualty calculation. Population consumes a committed event with stable source event ID, effective time, affected spatial area or assignments, fatalities, injuries, displacement effects, source/model version and any required cohort/composition allocation method.

Population applies the event once. Fatalities reduce the appropriate assignment and cohort counts. Injuries do not reduce Population Total unless a later death event is committed. Displacement changes assignment, not world population. If exact demographic composition is not resolved by the source system, the event identifies its deterministic allocation method and preserves estimation uncertainty.

### 13.2 Required Invariants

- Event idempotency prevents casualty effects being applied twice.
- Cohort and composition counts reconcile to affected population at their supported resolution.
- The same group cannot be counted in mutually exclusive outcomes for the same event unless sequential outcomes and timing are explicitly represented.
- Population does not recalculate weapon probability, blast effects, protection or casualties from weapon delivery-error data alone.

## 14. National and Local Aggregation

National totals and indicators should normally be derived views or queries over authoritative assignments and composition state.

Every report declares whether it aggregates by current residence, citizenship/nationality, administrative jurisdiction, legal sovereign, effective territorial controller, displacement status, in-transit/unassigned population, or another explicitly defined dimension.

These dimensions are not interchangeable. Occupation or territorial control does not automatically change legal sovereignty or citizenship. A citizenship total may include people living abroad and exclude resident non-citizens; it must not be substituted for resident population.

Internal migration reconciles at world scale. In-transit people appear once in the in-transit assignment and not as residents at origin or destination. A geographic or administrative boundary change requires effective-dated attribution, an explicit crosswalk or committed reallocation; it must not generate births, deaths or migration as a side effect.

Derived national interest profiles, labour statistics, demographic rates and wellbeing indicators document weighting and missing-data rules. Cached aggregates are permitted only if reconcilable to source records.

## 15. Update Ordering and Transactions

Population follows the shared simulation scheduler and does not create an independent global clock.

The Population-local logical order is:

1. Validate committed external events effective at the boundary.
2. Apply each event once to assignments, cohorts and applicable composition state.
3. Apply due annual cohort ageing under the canonical convention.
4. Calculate rate-based births and deaths using the declared exposure snapshot and versioned schedules.
5. Resolve migration/displacement using an explicitly ordered snapshot of safety, routes, capacity, policy, employment and living conditions; commit departures, transit and arrivals as separate transitions.
6. Update labour eligibility and derived availability from demographics, education, policy and committed employment/service state.
7. Apply education and composition transitions due in the interval.
8. Calculate need requirements and reconcile fulfilment outcomes.
9. Calculate derived wellbeing and interest observations from committed inputs.
10. Publish versioned outputs for consuming systems.

This is a logical dependency order within Population, not a substitute for the global phase scheduler. The global scheduler defines when upstream inputs are available and how late events are replayed.

### 15.1 Atomicity and Idempotency

- A population-changing event and its count/cohort effects commit atomically.
- A movement transition atomically debits one assignment and creates/updates the next authoritative assignment.
- A retry with the same idempotency key returns the prior committed result or a defined conflict; it does not reapply the effect.
- A failed transaction leaves no partial cohort, total, composition or assignment update.
- A committed historical event is corrected through a compensating event, not silently rewritten.
- Rate-based fractional remainders are persisted or deterministically reproducible so save/load and replay produce the same integer flows.

## 16. Data Validation and Integrity Rules

At minimum, schema/application validation enforces:

1. Every Population Centre references a canonical World Map Area feature.
2. Population counts are non-negative integer persons.
3. Cohort counts reconcile to assignment totals at the same effective time.
4. A group is in exactly one authoritative population assignment at a time.
5. Internal movements conserve total population; boundary crossings alter world totals only under explicit scenario rules.
6. Every committed population-changing event is idempotent and has provenance.
7. Event effects reconcile to declared event quantities.
8. Cohort ageing and education/composition reclassification conserve totals.
9. Mutually exclusive composition dimensions reconcile to declared denominators; overlapping dimensions do not use that constraint.
10. Spatial partitions reconcile to centre totals at the represented resolution.
11. Estimated intersections and spatial fallbacks retain estimation method/provenance.
12. Labour classifications reconcile and do not double-allocate the same aggregate workforce.
13. Employment status is derived from committed employment assignments, not inferred solely from wages or vacancies.
14. Need quantities use canonical Catalogue IDs and compatible units.
15. Fulfilment references authoritative external events and is not counted twice.
16. Economic financial state and military personnel state are not duplicated in Population.
17. Interest values reference a valid axis version and respect its declared scale/type.
18. Derived national aggregates state their aggregation dimension and weighting rule.
19. Effective-dated taxonomy, policy and rule changes preserve historical interpretation.
20. Results are deterministic under save/load, replay and event retry.

## 17. Implementation Sequence

1. **Geographic references and assignments:** Population Centre reference, assignment, age cohorts and current-state projections.
2. **Event ledger and movement:** committed events, effects, movement lifecycle, idempotency and reconciliation tests.
3. **Demographic reference data:** cohort taxonomy, versioned fertility/mortality schedules, deterministic flow accumulation and ageing.
4. **Composition and spatial distribution:** dimension/category taxonomy, group distributions, spatial partitions and estimated fallbacks.
5. **Labour and education interfaces:** derived labour snapshots, education distributions, eligibility and conservation rules.
6. **Needs and indicators:** need assessments, fulfilment references, wellbeing definitions/observations and Catalogue-unit integration.
7. **Interest dynamics:** versioned axes, profiles, observations and transition-rule interfaces; integrate with Political Systems only through explicit contracts.
8. **National reporting and validation:** residence/citizenship/control distinctions, cached aggregation reconciliation, save/load/replay and migration tests.

Do not implement all optional cross-tabs or per-cell population records by default. Add them only where an enabled simulation feature requires them and the cost/accuracy trade-off is documented.

## 18. Open Decisions Before Database Migration

These decisions concern implementation or scenario configuration, not permission to weaken behavioural invariants:

- Final shared identifier types and simulation-time storage convention.
- Whether current state is stored directly, reconstructed from event history, or represented by an event ledger plus current-state projection.
- Canonical event envelope, idempotency-key scope and late-event/replay handling shared with the simulation engine.
- Exact rate-accumulation and rounding implementation, including persisted fractional remainder.
- Whether demographic subgroup dimensions such as sex are required for the initial scenario, based on actual simulation needs and available reference data.
- Spatial partition/tile format, resolution, sparse storage and index strategy.
- Which Population Type/group taxonomies are enabled in the initial scenario and whether each is exclusive, overlapping or probabilistic.
- Initial Interest Axis catalogue, scales, endpoint meanings, reference profiles and transition rules. Conceptual examples are not canonical data.
- Which economic group statistics Economy exposes, their units, intervals, denominators and whether they are exact or estimated.
- Which wellbeing indicators are enabled and their versioned formulas.
- Need definitions, requirements, substitutions, priority rules and service-delivery interfaces.
- Fulfilment lifecycle storage strategy: immutable milestone/event rows versus an event history plus current-state projection. The behavioural requirement to distinguish each stage, preserve source authority and prevent duplicate advancement is fixed regardless of the storage choice.
- Exact Military Systems contract for recruitment/conscription, personnel transfers and settlement resident-count presentation.
- Exact Political Systems interface for platforms, group support, political pressure and government action.
- Handling of unassigned, nomadic, maritime and other special civilian population pools in scenario data and national reports.

No migration should encode these as silent defaults. Record each resolved decision in the relevant canonical specification or reference-data definition before depending on it.

## 19. Acceptance Criteria

The Population schema is conformant when:

1. It preserves ownership boundaries in the Population System and shared schema foundations.
2. It models aggregate population without requiring one record per civilian.
3. It provides unambiguous assignments for residents, transit and unassigned groups.
4. It reconciles totals, age cohorts, events and movements.
5. It prevents duplicate event application and duplicate population assignment.
6. It represents composition dimensions according to declared overlap semantics.
7. It supports estimated spatial/group intersections without presenting them as exact.
8. It distinguishes labour eligibility, participation, availability and committed employment.
9. It preserves education and composition totals during reclassification.
10. It integrates need fulfilment without duplicating Catalogue, Inventory, Logistics or Economy authority.
11. It supports versioned, bounded interest axes and explicit condition-response rules without hard-coded stereotypes.
12. It keeps political organisations, platforms and support outcomes within Political Systems.
13. It keeps earnings, savings, wages, taxes, transfers, wealth and financial transactions within Economy.
14. It receives civilian-harm events from Combat and Weapons and applies demographic consequences once.
15. It supports national aggregation by explicitly declared dimensions.
16. It preserves historical validity and deterministic replay.
17. It can be implemented incrementally without requiring dense 100 m population data or unsupported cross-tab precision.

## Governing Principle

**Population schema records where civilian population is assigned, how its demographic and social distributions change, and which derived conditions and interests it exposes. It does not duplicate geographic, financial, employment, logistical, military or political authority.**
