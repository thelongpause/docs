# Character System

**Project:** Project Sovereign  
**Canonical Path:** `systems/characters/character-system.md`  
**System:** Characters and People  
**Document Type:** Canonical Behavioural Specification  
**Document ID:** CHAR-001  
**Status:** Canonical initial specification; general attribute catalogue, schemas and domain-specific progression contracts remain open  
**Starting World Date:** 1 January 1955  
**Technology Horizon:** At least 2026, extensible beyond

---

## 1. Purpose

The Character System governs persistent individual people represented in Project Sovereign. It provides the shared identity, general attributes, personality, relationships, biography, life history and global character lifecycle that other systems extend or consume.

Characters may participate in civilian society, government, industry, research, diplomacy, military service, crime, education, healthcare, religion, culture, or other supported domains. A character may occupy multiple roles over a lifetime, move between institutions, acquire new skills and relationships, change beliefs, retire, become incapacitated, or die. The same person must remain the same persistent entity as their roles and circumstances change.

**Core principle: a Character is a persistent person, not a role-specific stat bundle.** Military officers, political leaders, scientists, executives, workers and other role-bearing people reference the same canonical Character identity. Domain systems own the specialised records, appointments, qualifications, authority and behaviour that arise from those roles.

The Character System must support simulation depth without requiring every person in the world to receive equal individual-level computational detail. The simulation may represent some populations statistically and promote particular people to persistent individual characters when required by scenario design, social importance, institutional roles or simulation events.

## 2. Design Principles

1. **Persistent identity:** each individual Character has one stable identity for their lifetime and across role changes.
2. **General attributes are shared:** general character statistics belong here, not in parallel copies inside Military, Government, Industry or other domains.
3. **Role specialisation belongs to domains:** Military owns military careers and command traits; Government owns political/administrative appointments; Research owns research-specific role records; other domains follow the same principle.
4. **Current state and history are distinct:** current attributes and status must be distinguishable from dated events, prior values and historical roles.
5. **Attributes are not destiny:** attributes influence tendencies and capability within domain rules; they must not directly guarantee outcomes or bypass authority, knowledge, resources, technology or physical constraints.
6. **Personality is not omniscience:** characters act on information available to them, with uncertainty, bias and bounded interpretation.
7. **Progression has causes:** training, experience, health, age, life events, work, relationships and other defined causes may change character state. Time alone must not arbitrarily improve every attribute.
8. **Identity and demographic state are distinct:** a character may have demographic attributes, but Population owns aggregate population/demographic state and the authoritative social facts defined by its specification.
9. **Privacy and knowledge are scoped:** personal facts, beliefs, reports and relationships may be known to some actors and hidden from others. Actual state and actor knowledge must not be conflated.
10. **Historical and cultural context matters:** names, attributes, careers, institutions, languages, education, opportunities and life events depend on scenario configuration and historical context.
11. **No arbitrary domain multipliers:** domain systems must define how relevant character attributes influence their own decisions and outcomes, with explainable causal links.
12. **Player and AI parity:** characters controlled by the Player, AI, or autonomous simulation use the same state rules and domain constraints.
13. **Scalable fidelity:** the simulation may use different levels of detail for named characters, role-bearing characters and aggregated populations, while preserving stable identities and valid promotion/demotion of simulation detail.
14. **Deterministic persistence:** character lifecycle and attribute changes must be traceable, save/load safe and protected against duplicate event application.

## 3. Scope and Ownership

### 3.1 Character System Owns

- Stable individual Character identity and lifecycle.
- General biographical and life-history record.
- General character statistics and attributes defined by the shared Character model.
- General personality and psychological disposition fields, where implemented.
- General skills or competencies that are not exclusively owned by a specialised domain.
- General relationships and personal connections, with relationship type and temporal validity.
- Character-level status such as alive, deceased, missing, incapacitated or otherwise configured lifecycle states.
- General age and life-stage calculations from authoritative birth/date information where represented.
- Character-level event history and attribute-change provenance.
- Shared interfaces for domain-specific role extensions.
- General eligibility and consistency rules for character records.
- Character generation and identity persistence for scenarios that require named individuals.

### 3.2 Character System Does Not Own

- Military branch, rank, military service number, appointment, command authority, military qualifications, military-specific traits or military service record: Military.
- Political office, ministerial authority, party role, government appointment and legal powers: Government/Political systems.
- Company employment role, executive appointment, ownership, compensation and company authority: Economy/Company/Employment systems.
- Research appointment, research-team role, field-specific research progression and research output: Research.
- General population totals, demographic cohorts, labour supply, aggregate education and population distributions: Population.
- Medical diagnosis, injury state, treatment, nutrition/hydration consequences or other health details where a Medical/Health system owns them. Character may reference relevant status or summary fields only under a defined contract.
- Legal guilt, criminal cases, arrest authority, custody and judicial outcomes: relevant Law/Security/Government systems. Character records may reference events affecting personal status.
- Intelligence reports, source reliability and actor-specific intelligence dissemination: Intelligence.
- Unit identity, command chain, orders and operational authority: Military Organisation and Command.
- Any domain-specific authoritative role state that should evolve under that domain's rules.

### 3.3 General Attributes Versus Domain Extensions

The Character System owns attributes intended to be meaningful across multiple domains. A domain-specific extension may store its own traits, qualifications, role history, authority and progression. It may also derive domain-specific assessments from general Character attributes.

For example, a military command assessment may consume general intelligence, judgement, temperament or stress-response attributes where those attributes are part of the approved Character model. Military then combines them with military education, command experience, doctrine familiarity, branch-specific traits, staff capacity, communications, mission complexity and information quality. Military must not create its own duplicate intelligence or personality score merely because it needs those values for command decisions.

A domain may cache a derived value for performance or audit purposes only if it records the source values/version and can rebuild the derived value. A cache is not a second authoritative Character attribute.

## 4. Character Identity and Lifecycle

### 4.1 Stable Character Identity

Every persistent Character must have a stable, unique Character ID. Names, titles, aliases, service numbers, office titles and social identifiers are not substitutes for this ID.

Names may change or have multiple representations across languages, cultures and historical periods. The Character record must support a canonical identity reference plus configured display names, aliases and name-history where appropriate. Name changes must not create a new person or break existing domain references.

Character identity should remain stable through:
- Name or title changes.
- Career changes.
- Promotion, appointment, transfer or retirement.
- Marriage, partnership or relationship changes where represented.
- Changes in citizenship, residence, political allegiance or institutional affiliation.
- Injury, incapacity or other status changes.
- Capture, detention or other custody changes.
- Death and posthumous historical records.

### 4.2 Identity Resolution

Scenario initialisation may load known historical characters, generate fictional characters or promote an existing represented individual into a persistent Character record. Identity resolution must prevent duplicate people where the scenario has enough evidence to recognise the same person.

Uncertain identity matches must remain uncertain until resolved by explicit scenario data or a defined identity-resolution process. Similar names alone are not sufficient proof of identity.

### 4.3 Character Lifecycle States

The system must support configurable lifecycle states, at minimum enough to distinguish active/living, deceased, missing or presumed missing, and other scenario-relevant states such as incapacitated. Exact status taxonomy is a schema decision.

A lifecycle transition must identify the effective time, cause/source event and relevant evidence or authority. Death does not delete the Character record. Deceased characters may remain referenced by historical events, prior appointments, relationships and institutional history.

### 4.4 Birth, Age and Death

Where birth date or age is represented, age is derived relative to the simulation date using the configured calendar. Age-related eligibility and progression rules belong to the relevant domain or shared policy, not as hidden universal assumptions in every consumer.

The Character System must not invent a death merely because an age threshold is reached unless a configured life-expectancy or mortality model explicitly applies. Health, injury, combat, disease, accidents and other mortality causes belong to the responsible systems, which submit validated lifecycle events.

### 4.5 Missing, Captured and Unavailable Characters

A character may be alive but unavailable to a role because of absence, detention, capture, injury, travel, communications isolation or other defined conditions. These conditions must be distinguished from death and from the end of a role appointment.

The owning domain determines how an unavailable character affects their appointments or responsibilities. Military may appoint an acting commander when a commander is captured, but Character identity remains persistent and the Character System does not itself choose military succession.

## 5. General Attributes and Statistics

### 5.1 Attribute Model

A Character Attribute is a general, versioned property or assessment of a person. Attributes must have a clear definition, unit or scale, valid range, uncertainty/quality where relevant, provenance and progression rules.

The canonical Character model may support multiple attribute families, but the final attribute catalogue and numerical scales must be defined in a supporting schema/reference document rather than improvised independently by consuming systems.

Potential attribute families include:

- Cognitive capacity and information processing.
- Judgement and decision quality.
- Learning and adaptation.
- Temperament and emotional regulation.
- Stress response and resilience.
- Initiative and persistence.
- Social aptitude, communication and interpersonal influence.
- Integrity, reliability and trustworthiness.
- Risk tolerance and uncertainty preference.
- Physical capability or general fitness where represented.
- General competencies or learned skills shared across domains.

This list is a candidate taxonomy, not a declaration that every attribute is already implemented. Attribute definitions must be reviewed for overlap, measurable meaning, historical applicability, computational cost and domain relevance.

### 5.2 Attribute Scales

Each attribute must define its own scale semantics. If multiple attributes use a shared numerical range, the range must not imply that their meanings are interchangeable. A value of 70 in one attribute is not necessarily comparable to 70 in another unless the model explicitly defines such comparability.

Attribute scales must avoid false precision. The UI may present qualitative descriptions or bands while preserving the underlying numeric or categorical representation. Calibration and generation rules should be tested for plausible distributions and meaningful differentiation.

### 5.3 Personality and Disposition

Personality fields represent relatively persistent tendencies, not guaranteed actions. They may influence how a character interprets options, responds to uncertainty, manages conflict, trusts others, accepts risk or reacts to pressure.

Personality must not bypass formal authority, hard constraints, information access, available resources or domain rules. A bold character cannot make an impossible route traversable; a highly persuasive character cannot automatically compel a government decision; a capable commander cannot create ammunition or perfect intelligence.

Personality models should be interpretable enough to explain broad tendencies without claiming deterministic prediction of every decision. The system may represent uncertainty, contextual variation and change through explicit life events or progression.

### 5.4 General Skills and Competencies

A general skill may be stored in Character when it is reusable across domains and has a shared meaning. Domain-specific qualifications and role eligibility remain with the relevant domain.

For example, general language proficiency or broad administrative competence may be shared Character data if the schema defines them. Military staff-college completion, naval watchkeeping qualification or artillery command certification belong to Military education/qualification records. A general skill must not automatically grant a regulated or domain-specific qualification.

### 5.5 Attribute Provenance and Confidence

Attributes may be generated, observed, assessed, reported or inferred. The model should distinguish actual state from an observer's estimate when the difference matters to gameplay.

A character's actual competence may differ from an employer's assessment or an intelligence service's belief. The Character System owns the authoritative attribute under the model, while the relevant information system owns reports, source quality, confidence and who knows what.

Where attribute estimates are stored, they must reference the assessed Character and attribute, the observer/source, time, confidence or uncertainty, and any permitted disclosure rules. These estimates must not overwrite the underlying actual value.

## 6. Attribute Progression and Change

### 6.1 Explicit Causes

Attributes may change through defined events or processes such as education, training, practice, experience, age-related change, injury, illness, stress exposure, rehabilitation, major life events or other configured causes.

A change must identify:
- Character and attribute.
- Prior and resulting value or state.
- Effective simulation time.
- Cause/event and source domain.
- Rule/configuration version.
- Whether the change is permanent, temporary, decaying, uncertain or subject to recovery.
- Provenance sufficient for audit and replay.

The Character System may apply shared progression rules for general attributes, but domain systems remain responsible for specialised training outcomes and qualifications.

### 6.2 Training and Experience

Training does not automatically produce a fixed attribute increase. Outcomes may depend on baseline ability, instruction quality, time, effort, prior knowledge, resources, health, motivation and assessment rules where those factors are explicitly modelled.

A completed domain course may update a general skill only if its approved outcome contract says so. A qualification itself remains a domain-owned record. The Character System must not infer qualifications from experience alone.

### 6.3 Temporary State Versus Persistent Traits

The data model must distinguish persistent attributes and traits from temporary conditions. Fatigue, acute stress, injury, intoxication, fear, temporary morale or similar state should be owned by the appropriate domain when that domain has a canonical model.

A character's general resilience may influence the response to stress; it is not the same thing as the current stress state. A character's general fitness is not the same thing as current fatigue or injury. A military-specific command trait is not the same thing as a general personality attribute.

### 6.4 Change Limits and Reversibility

Progression rules should define plausible limits, rates, persistence and recovery. The system must not permit unbounded growth through repeated event replay or duplicate training completion.

Reversible and irreversible changes must be distinguished. A temporary condition may decay or recover. A completed education credential or historical appointment remains part of history even if the character later loses current eligibility.

## 7. Relationships and Social Connections

### 7.1 Relationship Records

Where relationships are simulated, they should be represented as explicit records between stable Character IDs or between a Character and another supported entity. Relationship records may include type, directionality, strength/quality where modelled, status, effective period, source and confidence.

Relationship types may include family, partnership, friendship, mentorship, rivalry, professional trust, political alliance, patronage, hostility or other scenario-defined connections. The catalogue of relationship types must remain extensible.

A relationship is not necessarily reciprocal in the same form or intensity. One character may trust another who does not reciprocate. The data model must allow asymmetric beliefs or sentiments when relevant.

### 7.2 Relationship State and Knowledge

Actual relationship state, each character's belief about the relationship, and third-party knowledge are distinct concepts. The simulation must not assume every actor knows every private relationship.

Relationships may change through interactions, shared events, conflict, institutional dependence, betrayal, time and other defined causes. A domain may use relevant relationship information in decision-making, but must respect what the decision-maker knows and what the domain rules permit.

### 7.3 Domain-Owned Relationships

Military command familiarity, unit cohesion and institutional trust may depend on relationships and shared history, but Military owns the relevant military evaluation and operational effect. The Character System provides the relevant person/relationship data and history; it does not calculate unit cohesion or command effectiveness.

Political alliances, company reporting relationships, criminal networks and diplomatic relations may be represented by domain-owned institutional entities or records. They should reference Character IDs where a person-to-person relationship is genuinely relevant rather than forcing all institutional relationships into the Character System.

## 8. Biography, History and Personal Record

### 8.1 Biography

A Character may have a biography assembled from birth/life events, education, employment, appointments, relationships, achievements, failures, awards, legal events and other domain-owned records.

The biography is a presentation/derived view of authoritative events and records. It must not become a second source of truth or overwrite domain records. Historical statements may have uncertainty or conflicting sources where the simulation models contested accounts.

### 8.2 Event History

Important changes should be recorded as events or history entries with stable identity, effective time, source domain, provenance and the state transition they caused. Current values should be queryable without requiring consumers to replay the entire biography.

The system may retain a full event history, versioned snapshots or a hybrid approach, provided that authoritative history remains auditable and save/load safe.

### 8.3 Public and Private Information

Not every biographical fact is public. The simulation should distinguish facts visible to all, known to a character or institution, confidential records, classified information and hidden state according to the relevant information and legal systems.

The Character System stores authoritative identity and attributes under its ownership contract. Communications and Intelligence determine dissemination and actor knowledge. Presentation must not leak hidden data simply because the UI can query a Character record.

## 9. Domain Extension Contract

### 9.1 Extension Identity

A domain-specific record references the stable Character ID and contains only fields whose semantics and lifecycle are owned by that domain. A Character may have zero, one or multiple domain extensions at a time, subject to each domain's rules.

An extension should define its own identity, effective period, current status, history, eligibility and event contract. It must not duplicate the shared Character ID or copy general attributes into a competing authoritative record.

### 9.2 Military Officer Extension

The Military extension is expected to include, as appropriate:

- Officer status and military identifier.
- Branch affiliation and branch-specific career history.
- Rank and rank history.
- Current and historical appointments.
- Command authority and succession/acting appointment state.
- Military education, courses and qualifications.
- Military-specific traits and professional competencies.
- Command experience, operational record and doctrine familiarity.
- Decorations and military-specific reputation.
- Promotion, assignment, relief, dismissal, retirement and other military service transitions.
- Links to units, headquarters, academies and military institutions.

These fields are owned by Military, not Character. Military defines how shared general Character attributes and its own military-specific records influence judgement, planning, delegation, command latency and execution.

### 9.3 Other Domain Extensions

Government, Economy, Research, Population, Security, Medical and other systems should follow the same pattern. A person's government appointment is not a Character attribute; employment status is not a Character attribute; a research qualification is not a general personality trait.

A domain may publish validated events that affect general Character state, but those changes must use the Character System's authorised transition contract and provenance.

## 10. Character Generation and Simulation Fidelity

### 10.1 Named Characters

Named characters may be provided by scenario data or generated through explicit generation rules. Generation must respect configured historical context, eligibility, institution needs, age/life-stage constraints, and any required consistency rules.

Random generation must be reproducible under the simulation's random-seed and event model. It must not create duplicate identities on save/load or when an event is retried.

### 10.2 Persistent Role-Bearing Characters

Characters who hold consequential roles, command units, lead institutions or participate in important decisions should persist as individual entities for as long as the simulation needs their identity and history.

A role vacancy is not itself a person. A position may exist without a current appointee, and a person may exist without holding any position.

### 10.3 Aggregated People and Promotion to Detail

The simulation may represent ordinary population groups through aggregate Population records rather than a persistent Character record for every person. A character may be instantiated/promoted when a scenario, event or system requires individual identity.

Promotion to individual detail must preserve consistent population accounting and must not duplicate the same person in both an aggregate cohort and an individual record without an explicit accounting convention. The Population System owns aggregate demographic and labour state; Character owns the persistent individual identity once created.

### 10.4 Performance and Fidelity

The implementation may use cached or reduced-resolution character assessments for distant or low-importance people. It must preserve stable identity, relevant outcomes and authoritative role state. Fidelity changes must not create hidden advantages for AI nations or break deterministic replay.

## 11. AI and Decision Interfaces

### 11.1 Character as Decision Participant

Characters may make decisions through the relevant domain's AI or decision process. Character provides general attributes, personality and relevant relationships; the domain defines the decision options, constraints, authority, information, costs, risks and outcome resolution.

Character does not own one universal decision engine that overrides every domain. Officer Character decisions are integrated with Military Command and the shared decision architecture. Political leaders act through Government/Political authority; executives act through company/market authority; researchers act through Research institutions.

### 11.2 Bounded Influence

General attributes should influence decisions through explainable mechanisms such as learning rate, attention, risk preference, persistence, interpretation, conflict handling or communication effectiveness where explicitly modelled.

They must not:
- Create unavailable resources.
- Override legal or command authority.
- Grant perfect information.
- Make physically impossible actions succeed.
- Guarantee success or failure independently of the domain process.
- Apply an unexplained global bonus to unrelated systems.
- Duplicate a domain-owned trait or state.

### 11.3 Player and AI Parity

Player-controlled, AI-controlled and non-player characters use the same authoritative Character records and domain rules. AI may have different decision quality or planning methods, but cannot access hidden attributes or private relationships unless permitted by its information state.

## 12. Interfaces and State Ownership

| Data or behaviour | Authoritative owner | Character System interface |
|---|---|---|
| Persistent person identity, general attributes, personality, biography and global lifecycle | Character System | Own and expose validated Character records according to access policy |
| Military service, branch, rank, appointments, qualifications and command traits | Military | Extend Character ID; publish role/career events and consume shared attributes |
| Political/government office and authority | Government/Political systems | Reference Character ID and provide domain appointment/status |
| Employment, company role, compensation and corporate authority | Economy/Company/Employment systems | Reference Character ID and provide domain employment records |
| Research appointment and domain-specific expertise/qualifications | Research | Reference Character ID and own research role/progression |
| Population totals, demographic cohorts and labour availability | Population | Maintain individual-to-aggregate accounting contract where individuals are represented |
| Health, injury, treatment, nutrition and hydration state | Medical/Health system when specified | Expose only agreed status/summary references; do not duplicate health authority |
| Legal case, arrest, detention and custody authority | Legal/Security/Government systems | Record validated life/status event and relevant external reference |
| Observation, intelligence assessment and actor knowledge | Sensors / Intelligence / Communications | Keep actual attributes separate from reports, beliefs and disclosed facts |
| Unit identity, command chain, orders and operational authority | Military Organisation / Command | Reference Character ID for commanders and appointments |
| General event identity, time, provenance and replay conventions | Shared data-model foundations | Follow shared identity, history and transaction conventions |

### 12.1 Required Interfaces

The Character System must provide stable interfaces for:
- Create/resolve a persistent Character identity.
- Retrieve an authorised Character record or permitted attribute subset.
- Apply a validated attribute change with provenance.
- Record lifecycle changes such as death or status change.
- Record general skills/training outcomes where Character owns them.
- Create, update and close a general personal relationship record where supported.
- Query character history and current state at a simulation time.
- Attach or query domain-specific extension references without transferring ownership.
- Resolve whether a character exists, is active/alive, or is unavailable under the current state.
- Support actor-specific disclosure through the relevant information/communications layer.

Domain-specific events must not directly mutate Character records outside the authorised Character transition contract.

## 13. Time, Persistence and Determinism

### 13.1 Simulation Time

Character age, life stages, appointments, relationships and attribute changes use the shared simulation calendar and event time. The Character System must not use a separate incompatible clock.

### 13.2 Event Ordering and Concurrency

Concurrent events may affect the same Character, such as death and promotion, capture and reassignment, or training completion and dismissal. The simulation must define deterministic precedence or explicit conflict resolution. A deceased character must not receive a new active appointment after the death event unless the scenario is resolving a historical correction under an explicit administrative process.

### 13.3 Save/Load and Replay

Persist stable identity, current authoritative attributes/status, relevant relationships, domain-extension references and unresolved events needed to resume the simulation. History must remain auditable and derived views rebuildable.

Repeated generation, promotion to individual detail, attribute-change or lifecycle events must be idempotent. Save/load must not duplicate a Character, training outcome, death event, relationship or appointment transition.

### 13.4 Versioning

Attribute definitions, scales, generation rules and progression models must be versioned. A catalogue update must not silently reinterpret old saved values. Migrations must be explicit, traceable and preserve historical meaning.

## 14. Failure Modes and Edge Cases

The system must handle at least:

- Duplicate name but distinct person.
- Name change or alias update without identity change.
- Uncertain identity match.
- Character with no current appointment.
- Position with no current appointee.
- Character holding multiple compatible domain roles.
- Conflicting or overlapping appointment validity periods.
- Death while holding multiple roles.
- Missing, captured, detained or incapacitated person.
- Attribute change received twice after event retry.
- Training or experience event that lacks valid authority/provenance.
- General attribute and domain-specific trait with overlapping names but different meanings.
- Character promotion to persistent detail from an aggregate population record.
- Relationship whose knowledge differs among actors.
- Hidden attribute or private relationship accidentally exposed through UI/AI queries.
- Save/load while a lifecycle or appointment transition is unresolved.
- Character schema update that changes an attribute's scale or meaning.
- AI decision request that attempts to bypass domain authority by directly modifying Character state.

Each failure must return an explicit rejection, unresolved state, or defined correction process. The system must not silently create duplicate people or rewrite historical identity.

## 15. Minimum Acceptance Tests

1. A Character retains the same ID after name change, promotion, career change and retirement.
2. A military Officer Character references a Character ID and does not create a second general-stat record.
3. Military can update rank, appointment, military qualifications and military-specific traits without overwriting Character-owned personality or general attributes.
4. A Character may exist without an appointment, and a position may exist without a Character assigned.
5. A character's general attribute changes only through an authorised, traceable event.
6. Replaying the same attribute-change event does not apply it twice.
7. Death preserves identity and historical references while preventing invalid active roles according to domain rules.
8. Character System does not independently determine military succession, political authority or company employment outcomes.
9. A general attribute influences a domain decision only through an explicit, testable domain rule.
10. A character cannot use a hidden attribute or private relationship in AI decision-making unless permitted by its knowledge state.
11. Character generation is reproducible under the configured seed and does not duplicate records after save/load.
12. Promotion from an aggregate Population cohort to an individual Character preserves population accounting.
13. Domain-specific qualifications are not granted merely because a general skill or attribute exceeds a threshold unless the owning domain explicitly defines that rule.
14. A temporary condition is not stored as a permanent personality trait without an explicit transition.
15. Character age derives from the shared simulation calendar.
16. Conflicting same-time lifecycle events resolve deterministically.
17. Player and AI use the same Character record and authoritative attribute semantics.
18. Historical attribute scales and definitions remain interpretable after catalogue version changes.
19. An unauthorised request to directly alter Character state is rejected.
20. Character biographies and UI views are derived from authoritative records and do not leak undisclosed information.

## 16. Open Decisions

The following remain to be resolved through the shared data-schema and implementation process:

- Final canonical list of general Character attributes and their definitions.
- Attribute scale, units, generation distributions, confidence/provenance and progression rules.
- Which general skills are shared Character attributes versus domain-owned qualifications.
- Exact personality representation and its relationship to bounded decision-making.
- Relationship type catalogue, directionality and knowledge/belief model.
- Character generation rules for historical and fictional scenarios.
- Population cohort accounting when individual Characters are created/promoted.
- Health/medical ownership and the minimum public interface to injuries, hydration, nutrition and mortality.
- Exact privacy/disclosure permissions and actor-specific attribute queries.
- Event envelope, lifecycle enum, attribute history and persistence schemas.
- Research, Government, Economy and other domain-extension contracts.
- Performance policy for individual versus aggregated character simulation.
- Historical data provenance and handling of uncertain or contested biographical records.

Until resolved, implementations must not invent incompatible general attributes or create domain-specific copies of shared state. Unknown attributes or unsupported domain extensions must be explicit.

## 17. Related Specifications

- [Military Branches and Unit Organisation](../military/military-branches-and-unit-organisation.md) — Officer Character extensions, academies, appointments, military careers and command behaviour.
- [Doctrine, Command and Operations](../military/doctrine-command-and-operations.md) — military command authority, order lifecycle and bounded decision-making.
- [Combat and Engagement Systems](../military/combat-and-engagement-systems.md) — combat events that may affect character status through authorised interfaces.
- [Population Centres System](../population/population-centres-system.md) — population aggregates, demographics and labour availability.
- [Inventory System](../inventory/inventory-system.md) — physical stock and material state; Character does not own inventory.
- [Simulation Integration and AI Decision Architecture](../integration/simulation-integration-and-ai-decision-architecture.md) — bounded Officer Character and AI decision model.
- [Military Interface Audit and End-to-End Vertical Slice](../../technical/architecture/military-interface-audit-and-vertical-slice.md) — cross-system contracts and remaining dependencies.
- [Data Model Foundations](../../technical/data-schemas/data-model-foundations.md) — shared identity, history, time and validation conventions.

---

## 18. Completion Criteria

The Character System is ready for implementation when:

1. The canonical general-attribute catalogue and scale semantics are approved.
2. Stable Character identity, lifecycle, history and attribute-change schemas are defined.
3. Military Organisation and other domain extensions reference Character IDs without duplicating general state.
4. Attribute progression, relationship state and event handling are deterministic and auditable.
5. Actor-specific information disclosure is connected to Intelligence/Communications rules.
6. Individual-to-aggregate population accounting is defined.
7. Save/load, duplicate-event and lifecycle-conflict acceptance tests pass.
8. The Military Officer Character integration tests pass without a parallel military general-stat system.

This specification establishes the shared Character ownership boundary. It does not claim that Character runtime simulation or all domain extensions have been implemented.
