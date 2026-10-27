# Government System

**Project:** Project Sovereign  
**Canonical Path:** `systems/government/government-system.md`  
**Document ID:** GOV-001  
**Document Type:** Canonical behavioural system specification  
**Status:** Initial canonical specification; institutional catalogues, detailed political models, Treasury schemas and calibrated rules remain open  
**Historical Baseline:** 1 January 1955  
**Technology Horizon:** At least 2026, extensible beyond

---

## 1. Purpose

The Government System models public authority and the institutions through which a nation establishes policy, exercises lawful power, allocates public resources, administers public services, regulates activity and authorises collective action.

It connects political authority to the systems that implement policy. Government decisions may change permissions, incentives, public budgets, mobilisation status, legal restrictions and policy priorities. They do not directly create economic transactions, physical goods, trained personnel, infrastructure, research results or military readiness. Those outcomes must be resolved by their owning systems.

The simulation begins on 1 January 1955. Government institutions, laws, administrative capacity and policy instruments must be scenario-configurable and historically/technologically extensible through at least the 2026 state. No one real-world constitution or institutional model is universal.

## 2. Core design principles

1. **Authority is explicit.** Every consequential public decision identifies the institution, office or actor empowered to make it and the applicable authority.
2. **Policy is not execution.** Government authorises, regulates, funds or prioritises; the relevant domain system resolves the resulting state change.
3. **Institutions are data-driven.** Constitutional arrangements, offices, jurisdiction, appointment rules and decision procedures are configured, not hard-coded to one country.
4. **Legal authority and practical capacity differ.** A government may be legally entitled to act but lack funding, staff, information, industrial capacity or effective territorial control.
5. **Decisions have scope and time.** Policy and authority have effective dates, expiry/review conditions, jurisdiction and versioned provenance.
6. **Budget authority is not cash movement.** Appropriation, spending authority, commitment, payment and physical delivery remain distinct.
7. **Emergency powers are bounded.** Emergency status does not grant unrestricted authority unless the applicable constitutional/legal configuration explicitly says so.
8. **Public and private actors remain distinct.** Government ownership, regulation, subsidy, contract and direct public provision are different relationships.
9. **Consequences propagate through owners.** Economy, Population, Production, Infrastructure, Military, Research, Diplomacy and other systems apply their own rules.
10. **Player and AI parity.** The same laws, institutional powers, information limits, budgets and review requirements apply to every actor.
11. **Deterministic and auditable.** Decisions, appeals, revisions and resulting events must be replay-safe and explainable.
12. **No assumed omniscience.** Government actors decide using the information available to them, including uncertainty, reporting delay and institutional bias where modelled.

## 3. Scope and ownership

### 3.1 Government owns

- Constitutional and institutional configuration for public authority.
- Government institutions, offices, jurisdiction and authority relationships.
- Public law, regulations, licences, prohibitions, sanctions implementation and legal status where assigned to Government rather than Diplomacy or another authority system.
- Political executive, legislative and administrative decision processes as configured for a nation.
- Public policy proposals, enactment, amendment, suspension, repeal and review.
- Public budget preparation, appropriation, allocation, spending authority and fiscal-policy instructions.
- Public procurement authority, spending delegation and authorisation of public contracts.
- Mobilisation authority, civil emergency policy and public strategic-reserve policy.
- Public-service mandates, funding decisions and government programme objectives.
- Government administrative capacity, compliance/enforcement mandates and institutional performance, where supported by data.
- Public decisions and records of their authority, scope, effective time, conditions and outcomes.

### 3.2 Government does not own

| State or process | Authoritative owner |
|---|---|
| Company revenue, costs, commercial contracts, transaction prices, payments and settlement | Economy |
| Physical stock, reservations, receipts, transfers, issue, consumption and loss | Inventory |
| Production recipes, capacity, work scheduling and manufacturing execution | Production |
| Population identity, demographics, household composition and labour characteristics | Population |
| Military organisation, operational orders, combat and force readiness | Military |
| Research projects, scientific knowledge and technology unlocks | Research |
| Physical geography, infrastructure geometry and territorial spatial queries | World and Infrastructure |
| Actual carrier traversal, route progress and physical arrival | Movement and Transportation |
| Diplomatic relations, treaty negotiation and foreign-policy relationship state | Diplomacy, with Government authority interfaces |
| Election mechanics and public opinion measurement | The designated Political/Elections and Population interfaces; Government consumes their validated outcomes |
| Judicial case processing, if modelled as a distinct system | Justice/Judiciary domain; Government supplies legal framework but cannot silently decide individual cases |
| Actual compliance, production, service delivery or enforcement effects | The domain system responsible for the affected state |

Government may retain references, decision records and derived policy assessments. It must not create a competing authoritative copy of an external system's state.

## 4. Government institutions and authority

### 4.1 Institutional model

A nation may define institutions such as executive offices, cabinet or council, legislature, ministries, treasury, civil service, regulatory bodies, local government and other public authorities. The model must not assume every nation has all of them.

Each institution or office definition should identify:

- Stable ID and version.
- Parent institution and jurisdiction.
- Institutional type and functions.
- Legal/constitutional source of authority.
- Appointment, election, succession or delegation method where applicable.
- Permitted decision types and scope.
- Required co-signatures, votes, consultations or approvals.
- Budget and administrative capacity constraints.
- Effective start/end time and historical provenance.
- Succession and continuity rules.

An institution's existence does not mean it is competent in every policy area. Jurisdiction, delegated power and current legal status must be checked for each decision.

### 4.2 Authority resolution

Before accepting a consequential decision, the Government System resolves:

1. Who or what institution initiated the proposal?
2. Does the actor currently hold the required office or delegation?
3. Does the institution have jurisdiction over the subject and territory?
4. Is the decision type permitted under the active constitutional/legal framework?
5. Are required procedures, votes, signatures, consultation periods or notices complete?
6. Are budget, time, capacity and policy constraints satisfied?
7. Is the authority active at the decision's effective simulation time?
8. Does the decision conflict with a superior authority or active legal constraint?

Outcomes must be explicit: authorised, authorised with conditions, deferred, rejected, expired, revoked, procedurally invalid or unresolved. A missing authority record must not default to approval.

### 4.3 Authority versus control

Legal sovereignty, recognised jurisdiction, effective territorial control and administrative reach are separate concepts. Government policy may apply legally to a territory without being enforceable there. Military occupation or presence does not automatically change legal sovereignty. Government consumes the authoritative territorial/control state and emits policy or enforcement requests; it does not rewrite World or Diplomacy state by declaration alone.

## 5. Policy lifecycle

A policy is a versioned public decision with a defined scope and implementation pathway.

### 5.1 Policy record

A logical Policy record should include:

- Stable policy ID and version.
- Policy type and subject domain.
- Proposing actor/institution.
- Legal/constitutional authority references.
- Jurisdiction and affected population/entity classes.
- Objectives and measurable intended outcomes.
- Operative rules, permissions, prohibitions, incentives or allocations.
- Required approvals and procedural evidence.
- Effective time, expiry, review date and repeal/suspension conditions.
- Funding and administrative-capacity requirements where applicable.
- Implementing system/institution and implementation status.
- Impact assumptions, uncertainty and source provenance.
- Supersession/revision relationships and decision event references.

### 5.2 Policy states

The policy lifecycle must distinguish at least: proposed, under review, procedurally approved, enacted, scheduled, active, partially implemented, suspended, expired, repealed, rejected and unresolved. A configured policy may use a subset where appropriate, but transitions must remain explicit.

Enactment does not prove implementation. For example, a subsidy policy does not itself transfer money; an industrial priority does not itself produce goods; a mobilisation declaration does not itself create trained personnel or equipment.

### 5.3 Conflicting policies

When policies overlap or conflict, the system resolves them through explicit precedence rules grounded in the active constitutional/legal configuration, jurisdiction, specificity, effective time and emergency provisions. It must not rely on file order, database insertion order or arbitrary AI preference. If no valid precedence rule resolves a conflict, return an explicit unresolved/contested result and trigger review.

## 6. Treasury, budgets and fiscal authority

Government owns public budget authority and fiscal policy; Economy owns company and market financial state and records commercial transactions and payments.

### 6.1 Budget concepts

The Government model must distinguish:

- **Budget proposal:** requested or forecast allocation, not authority to spend.
- **Appropriation/allocation:** legally authorised budget scope for a defined purpose and period.
- **Spending authority:** permission for a named institution or delegate to commit funds within specified limits.
- **Commitment reservation:** budget amount reserved against an approved proposed commitment, according to configured rules.
- **Commercial commitment:** an Economy-owned contract/order obligation.
- **Payment instruction:** authorised request to settle an obligation.
- **Payment/settlement:** confirmed Economy-owned financial event.
- **Actual expenditure:** derived or recorded value under the approved fiscal accounting model, reconciled to financial events.
- **Forecast/contingency reserve:** planning value that is not itself a completed transaction or unrestricted spending authority.

These concepts must not be collapsed into a single “budget spent” number.

### 6.2 Budget record

A logical Budget Allocation should include stable ID/version, public institution/nation, fiscal period, programme/purpose, currency/unit convention, authorised amount, committed amount reference, available authority derived from owned commitments, restrictions, approval source, effective interval and audit events.

The canonical currency and monetary precision follow the Economy System. Government must not create a second financial ledger. Exact accounting treatment, rollover, deficit/borrowing, debt instruments and monetary-policy mechanisms require explicit sub-specifications before implementation.

### 6.3 Budget validation

A procurement or programme request must be checked against the relevant allocation, delegated authority, restrictions, commitments and applicable emergency rules. If budget data is missing or stale, the result is unresolved/deferred rather than unlimited spending.

The Government System may authorise a valid request despite a physical supply shortfall, but the downstream transaction must expose the shortfall. Conversely, available stock or a supplier offer does not itself create public spending authority.

## 7. Procurement and public purchasing

The existing [Government Procurement and Mobilisation Interface](../technical/architecture/government-procurement-and-mobilisation-interface.md) remains the minimum integration contract for Military Logistics. This system specification makes Government the authoritative owner of public procurement authority, delegation, policy and budget permission.

A procurement decision must identify the originating request, requesting institution, authorised buyer, canonical product/service references, approved quantity/value/scope, permitted supplier or selection constraints where relevant, delivery/acceptance conditions, budget and legal references, effective time, expiry/review conditions, outcome and reason.

After authorisation:
- Economy validates and records the commercial contract/order, price, payment obligation and settlement.
- Production determines feasible manufacture and executes production.
- Inventory confirms physical stock and stock-changing transactions.
- Movement and Transportation resolves physical carrier traversal.
- Logistics coordinates demand, shipment and delivery reconciliation.
- The requesting domain confirms acceptance against the originating request.

Government approval is not a purchase, production result, stock reservation, shipment or receipt. Emergency procurement uses explicit delegated powers and does not bypass physical, technical, legal or commercial constraints unless the relevant authority rule explicitly changes a constraint owned by that domain.

## 8. Mobilisation, civil emergencies and strategic reserves

### 8.1 Mobilisation

Mobilisation is a set of versioned Government decisions that may authorise defined changes in public priorities, call-up or requisition procedures, industrial allocation, procurement delegation, civil protection measures, reserve release and administrative coordination.

A mobilisation record must define scope, jurisdiction, authority, activation criteria, affected institutions and entities, permitted actions, limits, effective time, review/expiry conditions and demobilisation rules.

Mobilisation does not automatically:
- Create military units, personnel, skills, equipment or ammunition.
- Complete training or make unserviceable equipment operational.
- Increase production capacity or create inputs.
- Reserve or transfer physical stock without Inventory confirmation.
- Move carriers or guarantee route feasibility.
- Override population, labour, legal, diplomatic or economic constraints without explicit configured authority and downstream resolution.

Military determines force-generation and operational requirements. Population resolves demographic/labour consequences; Industry and Production resolve feasible capacity and output; Economy resolves transactions; Inventory and Logistics resolve physical stock and delivery.

### 8.2 Strategic reserves

Government defines reserve policy: protected categories, target levels, release conditions, eligible authorities, replenishment priorities and review rules. Inventory owns actual holdings and reservations. A reserve target is not stock. Protected stock must not be counted as freely available to routine demand while release conditions remain unmet.

Reserve release is an authorised request followed by a confirmed Inventory transaction. Repeated requests and replayed events must not release the same stock twice.

### 8.3 Emergency powers

Emergency powers must identify the legal basis, activating authority, territorial and subject scope, allowed derogations, prohibited actions, spending/quantity limits, duration, review/renewal procedure and termination conditions. An emergency label alone confers no additional permissions.

## 9. Regulation, compliance and public services

Government may define regulatory requirements, licences, standards, taxes, subsidies, public-service mandates and enforcement priorities. Each rule must state the regulated entities, jurisdiction, effective interval, compliance conditions, exceptions, enforcement authority and responsible implementation system.

The rule's existence is not proof of compliance or enforcement. The owning system records actual economic, population, industrial, infrastructure or other consequences. If a dedicated regulator or judiciary system is later introduced, its authoritative case and enforcement records must be linked rather than duplicated.

Public services must distinguish policy mandate, funding, provider capability, service capacity, delivery event and observed outcome. Funding a hospital, school or transport programme does not create the facility, staff, supplies or service output. Infrastructure, Population, Production, Economy and other relevant systems resolve those states.

## 10. Government decisions, events and persistence

Every consequential decision/event must follow the shared event envelope and data-model foundations. At minimum it must preserve:

- Stable decision/event ID and version.
- Decision type and outcome/reason code.
- Initiating actor and authoritative institution.
- Authority, policy and jurisdiction references.
- Effective simulation time and recorded/audit time where distinct.
- Approved scope, conditions and applicable budget/policy versions.
- Causation/correlation references to the request and downstream events.
- Idempotency key and supersession/revocation links.
- Provenance and procedure evidence where required.

Retries must not duplicate approvals, spending authority, budget reservations, reserve releases, mobilisation effects or policy enactments. A changed decision is a new version or linked event, not an invisible rewrite of history. Save/load must preserve active policies, pending proposals, decision procedures, approvals, commitments references, emergency expiry/review times and outstanding implementation requests.

## 11. AI decision behaviour

Government AI evaluates policy options within the same institutional authority, legal constraints, information boundaries, budgets and administrative capacity as the Player.

AI may propose policies, prioritise requests, estimate impacts and recommend decisions. It cannot bypass a required approval, invent budget authority, read unavailable information, create downstream physical/economic effects directly or mark an implementation successful without confirmation from the owning system.

Decision evaluation should account for the configured national objectives, public welfare, institutional stability, economic constraints, military/security requirements, time horizon and uncertainty. No universal weighting or single political ideology is hard-coded. Scenario data and policy rules determine the objectives and their relative authority.

AI decisions must be explainable through the selected policy/authority references, input state available at decision time, constraints, forecast/configuration versions and outcome.

## 12. Cross-system interfaces

| Consumer/source | Government interface | Other authoritative owner |
|---|---|---|
| Economy | Tax/subsidy/fiscal policy, public budget authority, procurement decision references | Economy records commercial and financial events |
| Population | Public-service policy, labour/civil emergency requests, demographic impact requests | Population records demographic and household state |
| Industry and Production | Industrial policy, mobilisation priorities, public purchase authorisation | Industry Data/Production own establishments, capacity and execution |
| Inventory and Logistics | Reserve policy/release authority, procurement authorisation | Inventory owns stock; Logistics coordinates demand and delivery |
| Military | Procurement, mobilisation, strategic reserve, defence priority and emergency authority | Military owns force structure, readiness, orders and operations |
| Research | Public research priorities, funding authority and permitted programmes | Research owns research projects, progress and knowledge |
| Infrastructure and World | Public works mandate, access/regulatory decisions, emergency response authority | Infrastructure/World own facilities and physical/spatial state |
| Diplomacy | Government policy position, authorised treaty/foreign-policy decisions | Diplomacy owns diplomatic relationship and treaty state as specified |
| Character/Political actors | Office authority, appointment/delegation and institutional participation references | Character owns person identity; Political/Elections systems own their respective records |

All interfaces use stable IDs and versioned requests/results. An interface must return explicit unsupported, denied, deferred, rejected or unresolved outcomes rather than silently defaulting to success.

## 13. Minimum acceptance tests

1. An actor without the required office, delegation or jurisdiction cannot enact a policy.
2. A policy proposal or enactment does not itself create a downstream economic, physical or personnel effect.
3. Budget allocation and spending authority are distinct from Economy's commercial commitment and settlement.
4. Procurement approval does not create a purchase, production output, Inventory stock or delivery.
5. Partial approval constrains downstream quantity/value/scope to the authorised limit.
6. Expired or revoked emergency authority cannot approve a new request.
7. Mobilisation does not conjure personnel, equipment, industrial capacity or supply.
8. Strategic reserve release requires authority and an idempotent Inventory transaction.
9. A company Industry Type does not prove product availability or supplier capability.
10. A policy conflict is resolved by configured authority/precedence or returned as unresolved, not by arbitrary ordering.
11. A government may be legally authorised but practically unable to implement policy; capability/territorial-control constraints remain visible.
12. Replayed decision events do not duplicate approvals, spending authority, reserve release or downstream requests.
13. Save/load preserves pending procedures, active policies, expiry/review dates and decision history.
14. Player and AI follow the same authority, budget, information and policy constraints.
15. Public-service funding does not itself create facility capacity, staff or completed service delivery.
16. Government procurement in the sustainment vertical slice traces approval, Economy transaction, Production/Inventory availability, Movement/Logistics delivery and destination receipt as distinct transitions.
17. Fiscal and policy consequences affecting households or civilian demand are applied by the appropriate Economy/Population owner and are not duplicated inside Government.

## 14. Open decisions and staged implementation

The following remain explicit work items, not implicit assumptions:

- Constitutional and institutional catalogue: office types, jurisdiction, appointment/succession and decision procedures.
- Public law and policy taxonomy, hierarchy, conflict-resolution and review/appeal rules.
- Treasury/budget schema, fiscal periods, accounting basis, deficit/debt/borrowing and rollover.
- Delegated spending limits and procurement workflows by institution and level.
- Tax, subsidy, transfer and public-service mechanisms, including which belong in the initial playable slice.
- Mobilisation categories, conscription/requisition interfaces, emergency powers and demobilisation.
- Strategic reserve categories, targets, release rules and protected-stock accounting.
- Sanctions, export controls, licensing, treaties and relationship to Diplomacy.
- Election, public opinion, political legitimacy and government-transition interfaces.
- Government administrative capacity, corruption, compliance and enforcement modelling.
- Physical schema types, migrations, policy versioning, event ordering and executable integration tests.
- Historical and country-specific calibration, sourced through versioned reference data.

Do not invent universal institutional forms, legal powers, budget values or political weights to fill these gaps. Unsupported mechanics must be explicitly marked unresolved until their owner and reference data are approved.

## 15. Completion criteria

The Government System is behaviourally specified when ownership, authority resolution, policy lifecycle, Treasury concepts, procurement, mobilisation and cross-system responsibilities are agreed.

It is schema-ready when institutional, policy, budget, decision and event records have stable IDs, versioned enums, constraints, transition rules and migration/replay semantics.

It is integration-complete only when:
- Institution and policy catalogues needed by the initial playable scenario are configured.
- Authority and budget validation tests pass.
- Government-to-Economy procurement and payment boundaries pass.
- Mobilisation and reserve-release flows pass without bypassing Population, Military, Production, Inventory or Movement constraints.
- Save/load, replay/idempotency and Player/AI parity pass.
- The company-to-unit sustainment scenario passes through Government approval without treating approval as physical fulfilment.

A documentation-only change cannot satisfy runtime gates. If the simulation runtime is not yet available, report implementation-dependent criteria as blocked, not passed.

## 16. Related specifications

- [Government Procurement and Mobilisation Interface](../../technical/architecture/government-procurement-and-mobilisation-interface.md)
- [Research and Technology Progression Interface](../../technical/architecture/research-technology-progression-interface.md)
- [Economy System](../economy/economy-system.md)
- [Industry Data](../economy/industry-data.md)
- [Population Centres](../population/population-centres-system.md)
- [Military Logistics and Sustainment](../military/logistics-and-sustainment.md)
- [Inventory System](../inventory/inventory-system.md)
- [Production System](../production/production-system.md)
- [World and Command Interface Contracts](../../technical/data-schemas/world-and-command-interface-contracts.md)
- [Military Cross-System Interface Contracts](../../technical/data-schemas/military-interface-contracts.md)
- [Canonical Specification Register](../../technical/architecture/canonical-specification-register.md)
