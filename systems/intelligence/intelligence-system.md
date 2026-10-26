# Intelligence System

**Project:** Project Sovereign  
**Canonical Path:** `systems/intelligence/intelligence-system.md`  
**System:** Intelligence and Assessment  
**Document Type:** Canonical Behavioural Specification  
**Document ID:** INTEL-001  
**Status:** Canonical initial specification; collection catalogues, schemas, assessment calibration and detailed intelligence operations remain open  
**Starting World Date:** 1 January 1955  
**Technology Horizon:** At least 2026, extensible beyond

---

## 1. Purpose

The Intelligence System governs how actors transform observations, reports, sources and prior knowledge into intelligence requirements, assessments, estimates, warnings and actor-specific knowledge. It provides a disciplined distinction between what is actually true in the simulated world, what has been observed, what has been reported, what an actor believes, and how confident that actor should be.

The system must support military intelligence, national intelligence, counterintelligence, strategic warning, operational assessment, reconnaissance reporting, signals intelligence, human sources, imagery and other collection disciplines as they become historically available. It must support deception, stale reports, source bias, uncertainty, incomplete coverage and contradictory information without making every assessment either perfectly accurate or randomly wrong.

**Core principle: Intelligence is an assessment of information, not a second world-state database.** Sensors owns sensor observations and tracks. Other systems own their observations and events. Communications owns message delivery. Intelligence owns source evaluation, analytical assessments, intelligence requirements, dissemination decisions and the resulting knowledge state of authorised actors.

The Intelligence System must not grant the Player or AI perfect access to hidden enemy state. It must also avoid making AI actors artificially ignorant of information they have legitimately collected and received.

## 2. Design Principles

1. **Actual world truth remains elsewhere:** Intelligence references authoritative facts and observations but does not overwrite world, unit, equipment, political or economic truth.
2. **Observation, report, assessment and belief are distinct:** a detected contact is not an identified platform; a report is not a confirmed fact; an assessment is not certainty.
3. **Provenance matters:** every important assessment traces to its source reports, observations, assumptions and analysis.
4. **Confidence is explicit:** confidence must reflect source reliability, freshness, corroboration, ambiguity, collection conditions and analytical uncertainty where modelled.
5. **Knowledge is actor-specific:** a national intelligence service, military HQ, individual officer and platform may hold different knowledge states.
6. **Information must be delivered:** a report not received through the authorised communication process must not silently become available to a remote actor.
7. **Deception is causal:** adversaries may create misleading evidence, conceal activity or exploit collection limits, but false information must enter through defined events or sources rather than arbitrary AI cheating.
8. **Stale information remains historically true as a report:** updates may supersede an assessment but must not rewrite what was known at an earlier time.
9. **No perfect omniscience:** AI assessments use the same collection and information boundaries as the Player.
10. **Domain-specific action remains elsewhere:** Intelligence may recommend or warn; Command, Government, Combat, Logistics and other systems decide and execute actions under their own authority.
11. **Historical progression:** collection disciplines, sensors, institutions, analytical methods and dissemination capabilities depend on technology and institutional configuration at the simulated date.
12. **Deterministic and auditable:** source/report/assessment state changes must be save/load safe and resistant to duplicate events.

## 3. Scope and Ownership

### 3.1 Intelligence Owns

- Intelligence requirements, collection priorities and tasking requests.
- Source records and source-access relationships where modelled.
- Source reliability and reporting history.
- Intelligence report records, including provenance and time.
- Analytical assessments, estimates, warnings, hypotheses and confidence.
- Correlation and fusion of permitted reports from multiple sources.
- Contradiction detection, uncertainty tracking and assessment revision.
- Actor- or institution-specific intelligence knowledge state.
- Dissemination decisions and recipient eligibility, subject to authority/classification rules.
- Intelligence products and their version/history.
- Deception assessment and counterintelligence analysis within the configured scope.
- Intelligence-driven estimates of adversary capability, intent, disposition, readiness, logistics and other hidden state.
- Feedback and evaluation of prior assessments where outcomes later provide evidence.

### 3.2 Owned Elsewhere

- Actual world, unit, equipment, production, economy, population, weather and political state: each authoritative domain.
- Raw sensor observations, detections, contacts, tracks and sensor uncertainty: Sensors and Detection.
- Electronic emissions, jamming, spoofing, electronic attack/protection effects: Electronic Warfare.
- Message transmission, delivery, latency, interception opportunity and acknowledgement: Communications.
- Military units, reconnaissance formations, command appointments and operational orders: Military Organisation and Command.
- Physical reconnaissance movement, platform positions and route execution: Movement and Transportation.
- Combat engagement and damage outcomes: Combat.
- Legal authority, diplomatic policy, national strategy and institutional budgets: Government/Political and Economy.
- General Character identity and attributes: Character System.
- Officer career, qualifications and military-specific traits: Military.
- Research progress and technology unlocks: Research.
- Physical stock and inventory transactions: Inventory.

## 4. Core Concepts

### 4.1 Intelligence Requirement

An Intelligence Requirement describes a question or uncertainty an actor needs to resolve for a decision. It should specify the question, requester, decision context, priority, required-by time, geographic/temporal scope, permitted collection methods, security/access level and the decision that the answer may inform.

A requirement is not proof that the information is obtainable. Collection may be impossible, delayed, denied, incomplete or too uncertain to resolve the question.

### 4.2 Collection Task

A Collection Task requests information gathering through an authorised source or collection system. It may task reconnaissance, sensors, human sources, signals collection, imagery, open sources, technical collection or other supported methods.

Intelligence may request collection but does not itself move a unit, turn on a sensor, intercept a signal or force a source to report. The responsible domain validates feasibility, executes the task and returns observations or collection outcomes.

### 4.3 Source

A Source is the origin or pathway of information. Sources may include a sensor/platform, human source, captured document, intercepted communication, open publication, diplomatic report, commercial record, local authority, prisoner debriefing or other configured collection channel.

Source identity, access, reliability, past performance, possible bias, access to the subject and risk of deception may affect assessment. Source reliability is not identical to the truth of each report.

### 4.4 Observation

An Observation is a recorded result produced by a source system. Sensors owns its observations and tracks. Other systems may publish their own observations/events under their contracts. Intelligence references observations rather than replacing their authoritative records.

An observation may include time, location/area, subject or contact reference, measured properties, confidence/uncertainty and source provenance. An observation may be ambiguous, incomplete, misclassified or later contradicted.

### 4.5 Report

A Report is information submitted by a source or intermediary to an intelligence consumer. It may summarise or interpret observations, but its content, source and transmission history must be preserved. Communications governs delivery; Intelligence records the report's analytical status once received by an authorised intelligence endpoint.

A report that has not been delivered to an actor is not part of that actor's knowledge merely because the simulation has stored it globally.

### 4.6 Intelligence Assessment

An Intelligence Assessment is an analytical judgement about a subject, event, capability, disposition, intention or other question, based on available reports and assumptions. It must identify the assessment scope, time basis, confidence/uncertainty, supporting and contradicting evidence, key assumptions, author/analytical process and validity or review condition where relevant.

An assessment is not a direct write to actual world state. It may be correct, partially correct, wrong, incomplete or obsolete.

### 4.7 Knowledge State

A Knowledge State represents what a particular actor or institution has received, assessed, retained or currently believes about a subject. It is distinct from actual world truth and from another actor's knowledge.

The model should distinguish, where relevant:
- Not observed or not known.
- Suspected.
- Reported but unassessed.
- Assessed with low/moderate/high confidence according to the defined confidence model.
- Confirmed to a stated standard.
- Contradicted or disputed.
- Stale/outdated.
- Explicitly discredited or superseded.

These labels require precise definitions in the reference/schema layer. The model must not collapse confidence, identification certainty, source reliability and report freshness into one unexplained score.

### 4.8 Intelligence Product

An Intelligence Product is a packaged assessment, estimate, warning, briefing or report distributed to permitted recipients. It may include multiple assessments and source references. Product creation and dissemination are distinct from collection and delivery.

### 4.9 Deception and Denial

Deception is an intentional attempt to create, distort or conceal evidence to influence an adversary's assessment. Denial reduces an adversary's ability to collect or interpret information. These effects must enter through actual actions and systems such as camouflage, emissions control, decoys, false reports, operational security, communications security or EW.

Intelligence assesses evidence of deception and denial. It must not assume every adversary report is false or apply a hidden global accuracy penalty without a causal basis.

## 5. Intelligence Lifecycle

A typical intelligence cycle may follow these stages:

1. **Requirement identified:** a decision-maker defines an information need.
2. **Prioritised:** intelligence authority allocates collection/analysis effort.
3. **Collection planned:** feasible sources and collection methods are identified.
4. **Tasked:** the relevant source or domain receives an authorised collection task.
5. **Observed/collected:** the source system produces a result or reports failure.
6. **Reported:** information is assembled into a report with provenance.
7. **Delivered:** Communications confirms delivery to the intended intelligence endpoint.
8. **Evaluated:** source access, reliability, freshness, credibility and possible bias are considered.
9. **Correlated/fused:** reports are compared with existing knowledge and other evidence.
10. **Assessed:** a judgement, estimate, warning or unresolved question is produced.
11. **Disseminated:** the product is delivered to permitted actors.
12. **Used in decision-making:** the receiving domain consumes the assessment under its own authority and decision rules.
13. **Reviewed/updated:** new evidence, contradiction, time passage or outcome feedback revises the assessment.
14. **Archived/superseded:** prior versions remain available as historical knowledge.

Not every report requires every stage. A direct observation may be consumed by a local unit under an explicitly configured doctrine without national intelligence analysis. The resulting knowledge must still respect the relevant source and recipient contract.

## 6. Source Evaluation and Confidence

### 6.1 Separate Dimensions

Where modelled, source evaluation should distinguish:
- Source access to the subject or event.
- Source reliability based on history and incentives.
- Report credibility and internal consistency.
- Evidence quality and directness.
- Corroboration by independent sources.
- Contradictory evidence.
- Freshness and relevance to the current decision.
- Collection conditions and known limitations.
- Possible deception, manipulation or bias.
- Analytical uncertainty and assumptions.

The implementation may use qualitative categories, numerical probabilities, confidence intervals or a combination. It must define the semantics and avoid false precision.

### 6.2 Independence and Corroboration

Multiple reports do not necessarily constitute independent confirmation. Reports may derive from the same original source, copied publications, shared sensor tracks, common intercepts or coordinated deception.

The system should preserve source relationships and avoid multiplying confidence merely because one claim has been repeated. Independent corroboration must be supported by provenance, not inferred from the count of report records alone.

### 6.3 Freshness and Staleness

An assessment must state its time basis. Information about a mobile unit, stockpile, order, readiness or political intention can become stale as the world changes.

Staleness should depend on subject type, rate of change, observation time and decision context. A historical report remains part of the record, but the current assessment may lower confidence, widen uncertainty or mark the report outdated.

### 6.4 Contradictory Evidence

Contradictory reports must be retained and analysed. The system may revise an assessment, maintain competing hypotheses or mark a question unresolved. It must not silently discard inconvenient evidence or overwrite earlier assessments without history.

### 6.5 Confidence Calibration

Confidence must have consistent semantics across sources and consumers. If numerical probabilities are used, they should be calibrated and tested against outcomes where possible. Qualitative categories must map to documented uncertainty ranges or clear interpretations if comparisons across domains are required.

A high-confidence assessment can still be wrong. A low-confidence assessment is not necessarily false. The model must avoid treating confidence as a direct multiplier of actual combat or world outcomes.

## 7. Actor-Specific Knowledge and Dissemination

### 7.1 Knowledge Is Not Global

The simulation may store actual state and all reports centrally for persistence, but each actor's usable knowledge must be scoped. An actor may only act on a report if it has been delivered, independently observed, obtained through another valid source or made available by an explicit rule.

Different actors may possess different assessments of the same subject. A national intelligence service may hold a broad strategic estimate; a field HQ may have local contact reports; a platform may have only its sensor tracks; an officer may also have personal experience or relationships.

### 7.2 Dissemination Authority

Intelligence products may be restricted by classification, role, need-to-know, national policy, coalition caveats, source protection, operational security or other configured rules. Intelligence determines permitted dissemination under the relevant authority. Communications delivers the approved product.

A product being approved for dissemination does not mean it has reached every recipient. A delivery failure leaves the recipient without the new product.

### 7.3 Recipient Knowledge Updates

On delivery, the recipient's knowledge state is updated through an explicit event. The system must record what information was received, when, from whom, with what caveats and confidence. The recipient's assessment may differ from the sender's assessment if the recipient uses different evidence, competence, bias or context, where the model supports such behaviour.

### 7.4 Information Boundaries for AI

AI actors must use the same information boundaries as the Player. The national AI may have access to national intelligence products that are not available to individual units; a local commander may use only delivered reports and local observations. AI must not query hidden enemy inventory, exact unit positions, private orders or actual character attributes unless a valid information path grants that knowledge.

## 8. Collection and Tasking Interfaces

### 8.1 Collection Feasibility

Collection feasibility is determined by the source domain. Relevant constraints may include sensor capability, range, geometry, platform availability, source access, communications, training, weather, security, adversary countermeasures, time, budget and political permission.

Intelligence may prioritise or request a collection task but cannot manufacture a successful observation. The source system returns success, partial result, no result, denial, compromise, loss, delay or another explicit outcome.

### 8.2 Sensors and Detection

Sensors owns detection, classification, identification, tracking, measurement and track uncertainty. Intelligence consumes authorised sensor outputs and may combine them with other reports to produce an assessment.

A sensor track does not automatically prove the identity, mission, readiness, doctrine or intent of the tracked entity. Those conclusions require explicit inference and confidence.

### 8.3 Human Sources and Reporting

Human sources may have access, reliability, motives, risk and exposure state where the simulation models them. Their reports may be delayed, incomplete, mistaken, biased, coerced or deceptive. The relevant source-management model determines access and collection outcomes; Intelligence evaluates the received report.

### 8.4 Signals, Imagery and Technical Collection

Technical collection depends on available equipment, signatures, geometry, technology, processing and relevant EW effects. Intelligence consumes outputs from the authoritative collection system. It must not duplicate the physical detection or signal-processing model inside the analytical layer.

### 8.5 Collection Priorities

Collection priorities should follow the requesting authority's decision context and the value of reducing uncertainty. Collection has opportunity costs: a platform, analyst, source or communications resource assigned to one task may not be available for another.

National, operational and tactical priorities may conflict. The responsible authority resolves those conflicts; Intelligence should expose trade-offs and gaps rather than silently override national or command priorities.

## 9. Assessment and Analytical Reasoning

### 9.1 Assessment Structure

An assessment should identify:
- Assessment ID and version.
- Subject/question and relevant time/geographic scope.
- Authoring intelligence organisation or process.
- Supporting and contradicting report references.
- Assumptions and alternative hypotheses where applicable.
- Confidence/uncertainty and the basis for it.
- Key unknowns and collection gaps.
- Validity horizon or review trigger.
- Dissemination restrictions.
- Current status: active, revised, superseded, disputed or withdrawn.
- Causal links to prior assessments and relevant outcomes.

### 9.2 Inference and Prediction

Intelligence may infer likely state or future action from observed evidence and known patterns. Such estimates must remain distinct from actual state and should carry confidence and assumptions.

Prediction should not be used to create the predicted event. A forecast that an enemy will attack does not cause the attack; a prediction of a fuel shortage does not remove fuel from Inventory.

### 9.3 Alternative Hypotheses

Where evidence supports multiple plausible interpretations, the assessment process should preserve alternatives rather than forcing false certainty. For example, a reduction in radio traffic may indicate movement, communications discipline, jamming, equipment failure, deception or routine inactivity. The system should allow the evidence to remain ambiguous until further information resolves it.

### 9.4 Outcome Feedback

Later events may reveal that an assessment was accurate, incomplete or wrong. The system may use that feedback to evaluate sources and analytical calibration, but must not rewrite what the actor believed at the time. Source reliability and analyst performance may change only through explicit rules and recorded evidence.

## 10. Military Intelligence Interfaces

### 10.1 Command and Operations

Commanders use intelligence assessments to plan operations, prioritise objectives, evaluate risk and adapt orders. Intelligence provides the available assessments and uncertainty; Command owns the decision, order and operational plan.

An intelligence estimate of enemy strength is not actual enemy strength. An order based on a false estimate may fail through the normal combat and operational rules, with subsequent reports updating the assessment.

### 10.2 Combat and Targeting

Combat consumes authorised target/contact information and applicable confidence/identification state. Intelligence may support target assessment, order of battle, threat evaluation and battle-damage assessment. Combat owns engagement eligibility, firing, damage and actual outcome.

An intelligence assessment must not bypass rules of engagement, identification requirements, weapon constraints or the actual physical engagement resolution.

### 10.3 Logistics and Sustainment

Intelligence may estimate enemy supply routes, depots, readiness, logistics activity or interdiction risk. Logistics planning uses only the information available to the planning actor. Actual friendly cargo and inventory state remain authoritative in Inventory and Logistics.

A high-confidence report of a suspected enemy depot does not create a physical depot at that location. A stale report may cause a poor route decision, but the movement/combat systems resolve the actual outcome against world truth.

### 10.4 Officer Characters

Officer Characters may consume intelligence reports delivered to their headquarters or personal role. Their general Character attributes, military experience, doctrine familiarity and branch-specific traits may influence interpretation and planning through explicit Military/AI rules.

An officer must not automatically receive every intelligence product held by the national service. Access, dissemination, communications, appointment and need-to-know constraints apply.

### 10.5 National AI and Government

National AI uses intelligence assessments to support strategic priorities, diplomacy, mobilisation, procurement and military planning. It must represent uncertainty and conflicting evidence, and it must not directly query hidden enemy state to improve its decisions.

Government/Political systems own policy, legal authority and diplomatic consequences. Intelligence may inform decisions but cannot independently declare war, change a treaty or authorise procurement.

## 11. Deception, Counterintelligence and Denial

### 11.1 Deception Events

Deception must arise from explicit actions or conditions, such as false communications, decoys, camouflage, dummy installations, misleading movements, controlled leaks, fabricated documents or source manipulation where those mechanics are supported.

The relevant source/domain creates the misleading evidence. Communications, Sensors, EW or other systems resolve its delivery or observation. Intelligence assesses the evidence and may or may not detect deception.

### 11.2 Counterintelligence

Counterintelligence may investigate source compromise, hostile penetration, leaks, sabotage, deception and information security. Its authority, investigation, arrest and legal consequences belong to the relevant intelligence, security, government and legal systems.

An intelligence organisation must not automatically know the identity of a hostile source or the existence of a mole without a valid evidence and assessment process.

### 11.3 Denial and Concealment

Operational security, emissions control, camouflage, decoys, movement discipline and other measures may reduce collection opportunities or increase uncertainty. Their effects are applied by the owning domain systems. Intelligence records the resulting collection gaps and confidence changes; it does not independently grant invisibility.

### 11.4 Deception Against AI

AI may be deceived when it receives plausible misleading evidence through a valid channel. Deception outcomes should depend on evidence, source history, analysis capability and available corroboration. Difficulty settings may alter planning quality or analytical effort, but must not grant hidden truth to AI or make deception always succeed against the Player.

## 12. State Ownership and Interface Matrix

| Data or behaviour | Authoritative owner | Intelligence responsibility |
|---|---|---|
| Actual world/unit/equipment/economic state | Respective domain owner | Assess from permitted evidence; do not overwrite |
| Sensor observations, detections, tracks and sensor uncertainty | Sensors and Detection | Consume as source evidence |
| Raw message delivery and interception opportunity | Communications | Consume delivered messages and authorised exposure events |
| Intelligence requirement, source evaluation, assessment, confidence and actor knowledge | Intelligence | Authoritative |
| Military unit identity, appointments and orders | Military Organisation / Command | Reference entities; inform assessments through evidence |
| Electronic attack, jamming, spoofing and protection effects | Electronic Warfare | Evaluate resulting evidence and collection limitations |
| Movement and physical reconnaissance execution | Movement and Transportation | Request/interpret reports; do not move assets |
| Combat, damage and battle outcomes | Combat | Consume after-action evidence; perform assessment/BDA |
| Character identity and general attributes | Character System | Use only when legitimately available and relevant |
| Government authority and national policy | Government/Political systems | Provide assessments and warnings; do not exercise policy authority |
| Inventory and supply state | Inventory / Logistics | Estimate adversary state from evidence; use actual friendly state only through authorised interfaces |
| Report/product dissemination decision | Intelligence, under policy/authority | Decide permitted recipients and version |
| Actual message delivery | Communications | Request dissemination; do not claim delivery before confirmation |

## 13. AI, Player Parity and Decision Use

The Player and AI use the same observation, report, assessment and dissemination rules. AI may differ in analytical capacity, collection prioritisation, source networks, memory, planning, risk tolerance and ability to combine evidence, provided those differences are explicitly modelled and historically plausible.

AI decisions should be based on actor-specific knowledge and uncertainty. They may make mistakes, maintain competing hypotheses, delay decisions for more information or act despite uncertainty. They must not read actual hidden state through a backdoor.

Intelligence outputs should be useful to the UI and decision systems, showing the assessed conclusion, confidence, time basis, key evidence, major contradictions and information gaps where appropriate. The UI must not imply that an estimate is a confirmed world fact.

## 14. Time, Persistence and Determinism

### 14.1 Time and Validity

Reports, assessments and knowledge updates use the shared simulation clock. Every assessment should identify its effective time and relevant evidence cut-off. A current assessment may be revised, but prior versions remain part of the history.

### 14.2 Event Ordering

Collection, report delivery, assessment creation and dissemination are separate transitions. A report received after an assessment's cutoff must not retroactively become evidence used in the earlier version. Concurrent updates require deterministic ordering or explicit version conflict resolution.

### 14.3 Save/Load and Replay

Persist active intelligence requirements, source/task state where owned, received reports, assessment versions, actor knowledge state, dissemination records and unresolved events. On load, reconcile with Sensors, Communications, Military, Government and other source systems.

Duplicate report delivery must not create duplicated source evidence or repeated confidence gains. Replaying an assessment event must not create multiple active versions with inconsistent histories.

### 14.4 Audit and Provenance

Every important assessment must be traceable to source evidence, assumptions, authoring process, time and version. A later analyst must be able to distinguish what the actor knew at the time from what the simulation later revealed to be true.

## 15. Failure Modes and Edge Cases

The system must handle at least:

- No collection source can answer the requirement.
- Collection is tasked but the source is unavailable or incapable.
- Observation exists but is not delivered to the intelligence endpoint.
- Report is delayed and becomes stale before arrival.
- Several reports derive from the same original source and are not independent.
- Sources contradict each other.
- Source is unreliable, compromised, biased or deliberately deceptive.
- Assessment confidence is too low to support a strong conclusion.
- A high-confidence assessment later proves wrong.
- A report is received by one actor but not another.
- Intelligence product is not authorised for a requested recipient.
- A recipient acts on an old or superseded assessment.
- Communications failure prevents dissemination.
- EW reduces collection or distorts technical evidence.
- An adversary uses deception or operational security.
- A local commander has observations that are not yet known to national HQ.
- National AI attempts to read hidden enemy state directly.
- A report or assessment event is duplicated after retry/save-load.
- New evidence arrives after an earlier assessment has been published.
- A predicted event does not occur.
- A battle-damage assessment is confused with actual damage state.
- Source reliability is incorrectly equated with report truth.
- A UI presents an estimate as certain fact.
- Intelligence actions exceed institutional/legal authority.

Each case must produce explicit uncertainty, status, rejection or reassessment rather than silently manufacturing certainty.

## 16. Minimum Acceptance Tests

1. A sensor observation is not automatically a national intelligence assessment.
2. A report not delivered through Communications is unavailable to the remote recipient.
3. Two actors may hold different knowledge states about the same enemy unit.
4. A report preserves source, time, provenance and uncertainty.
5. Two reports copied from one source do not count as independent corroboration.
6. Contradictory reports remain traceable and can produce an unresolved or competing-hypothesis assessment.
7. A stale assessment is distinguishable from a current assessment.
8. New evidence creates a revised assessment version without rewriting the earlier actor knowledge.
9. A high-confidence assessment can be wrong without changing actual world truth.
10. A predicted attack does not cause an attack.
11. A suspected depot assessment does not create a depot or inventory stock.
12. A commander receives only reports authorised and delivered to their role/endpoint.
13. An AI cannot query hidden enemy positions or stock except through a valid information path.
14. EW effects alter the evidence available through the relevant collection/communications chain, not by directly changing Intelligence confidence arbitrarily.
15. Deception enters through explicit source/domain events and may be detected or missed according to the evidence and analytical process.
16. Intelligence cannot override ROE, command authority or Government policy.
17. Duplicate report delivery does not increase confidence or create duplicate source evidence.
18. Save/load preserves active assessments, source links, delivery status and actor-specific knowledge.
19. Combat outcomes remain authoritative even when intelligence assessments are wrong.
20. Player and AI use the same evidence, confidence, dissemination and information-boundary rules.

## 17. Open Decisions

- Canonical source/reliability taxonomy and confidence scale.
- Numerical probability model versus qualitative confidence bands.
- Assessment data schema, hypothesis structure and source-provenance graph.
- Intelligence organisation and national/operational/tactical authority model.
- Collection requirement/task schema and interfaces to Sensors, Human Sources, EW and other collection systems.
- Dissemination policy, classification, need-to-know and coalition caveats.
- Actor-specific knowledge-state schema and memory/forgetting rules.
- Signals, imagery, human-source and open-source collection mechanics by historical period.
- Deception and counterintelligence mechanics, including source compromise.
- Assessment freshness/decay rules by subject type.
- Battle-damage assessment and intelligence feedback.
- Interface with Government, diplomacy, Research, Character and AI decision architecture.
- Shared event envelope, schema versioning, persistence and idempotency.

Until these are resolved, the system must keep uncertainty explicit and avoid a universal confidence score that silently controls all domains.

## 18. Related Specifications

- [Sensors and Detection](../military/sensors-and-detection.md) — authoritative observations, detections, contacts and tracks.
- [Electronic Warfare](../military/electronic-warfare.md) — electronic effects and collection/dissemination disruption.
- [Communications System](../communications/communications-system.md) — message delivery, latency, disruption and acknowledgement.
- [Military Branches and Unit Organisation](../military/military-branches-and-unit-organisation.md) — reconnaissance/support units, commanders and organisational references.
- [Doctrine, Command and Operations](../military/doctrine-command-and-operations.md) — operational decisions and orders.
- [Combat and Engagement Systems](../military/combat-and-engagement-systems.md) — actual engagements and outcomes.
- [Movement and Transportation System](../world/movement-and-transportation-system.md) — physical movement and reconnaissance execution.
- [Character System](../characters/character-system.md) — persistent person identity and general attributes.
- [World and Command Interface Contracts](../../technical/data-schemas/world-and-command-interface-contracts.md) — report, evidence, assessment, actor-knowledge and event contracts.
- [Simulation Integration and AI Decision Architecture](../integration/simulation-integration-and-ai-decision-architecture.md) — actor-specific knowledge and AI decision boundaries.
- [Military Interface Audit and End-to-End Vertical Slice](../../technical/architecture/military-interface-audit-and-vertical-slice.md) — cross-system interface and acceptance plan.

---

## 19. Completion Criteria

The Intelligence System is ready for implementation when:

1. Source, report, assessment, confidence, actor knowledge and dissemination schemas are stable.
2. Sensors, Communications, EW, Command and AI interfaces agree on ownership and event semantics.
3. Provenance, staleness, contradictory evidence and source independence are represented and testable.
4. Actor-specific information access is enforced.
5. Deception and collection denial operate through explicit source/domain events.
6. Save/load and repeated report delivery preserve assessment history and confidence state.
7. At least one sensor-to-assessment-to-command decision scenario passes with realistic uncertainty.
8. Player and AI use the same intelligence rules and information boundaries.

The logical report, evidence, assessment and actor-knowledge contracts are defined in SCHEMA-WORLD-CMD-001. Source taxonomy, confidence/reliability calibration, physical schema constraints and executable provenance/access tests remain open. The existence of this specification does not prove that intelligence simulation has been implemented.
