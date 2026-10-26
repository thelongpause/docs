# World and Command Interface Contracts

**Project:** Project Sovereign  
**Canonical Path:** technical/data-schemas/world-and-command-interface-contracts.md  
**Document ID:** SCHEMA-WORLD-CMD-001  
**Document Type:** Implementation-facing logical schema and interface contract  
**Status:** Implementation baseline for review; numerical calibration and executable conformance tests remain open  
**Historical Baseline:** 1 January 1955  
**Technology Horizon:** At least 2026, extensible beyond

## 1. Purpose and authority

This document freezes the minimum logical record shapes and state-transition boundaries for Movement and Transportation, Character, Communications, Intelligence, and Weather and Environment. It complements, and does not replace, each canonical behavioural specification.

The contracts are expressed in logical field names and types rather than database-specific DDL. The implementation must map these types to the repository's selected physical database and runtime types without changing their semantics.

### 1.1 Shared conventions

- Every persistent entity has an immutable, globally unique ID using the repository's selected ID primitive. Foreign references use the same primitive.
- Every consequential state transition has an immutable event ID, effective simulation time, schema version, source system, source entity ID where applicable, and idempotency key.
- Effective simulation time uses the simulation calendar and deterministic ordering convention. Wall-clock/audit time must never decide simulation outcomes.
- Physical quantities include a numeric value and canonical unit ID, or reference a versioned definition that supplies the unit unambiguously.
- Spatial records reference canonical World Map geometry/location IDs or store a continuous coordinate tuple using the World Map convention. Do not persist the 100 m Movement Grid as an alternative position truth.
- Unknown, unavailable, stale, unsupported, denied, and failed are distinct states. Missing data must not silently become zero, success, perfect reliability, clear weather, or unrestricted access.
- Cross-system writes are requests or commands to the owning system. A consumer may keep a rebuildable projection, but not a second authoritative state.
- Definitions that affect outcomes are versioned and referenced by completed events so historical results remain explainable.
- These are logical schemas. This document does not claim runtime code or executable tests already exist.

## 2. Movement and Transportation

### 2.1 Movement task

The movement task is the execution record for a requested physical movement.

| Field | Type | Required | Rule |
|---|---|---:|---|
| id | ID | yes | Stable task identity |
| requesting_system | enum/string | yes | Domain requesting movement |
| request_id | ID | yes | Source order, shipment or movement request |
| moving_entity_type | enum/string | yes | Unit, vehicle, vessel, aircraft, train, person, convoy or supported carrier |
| moving_entity_id | ID | yes | Stable entity identity |
| movement_mode_id/version | ID/version | yes | Versioned mode/capability definition |
| origin_location_id | ID | yes | Origin location |
| destination_location_id | ID | yes | Destination location |
| route_id | ID/null | no | Accepted route plan |
| access_decision_refs | list of ID | yes | Authoritative access/permission decisions |
| planned_departure_time | simulation time/null | no | Plan only, not proof of departure |
| actual_departure_time | simulation time/null | no | Set by accepted departure transition |
| current_position | canonical spatial tuple/reference | yes | Movement system's authoritative position |
| progress_state | enum | yes | requested, validating, planned, ready, moving, interrupted, suspended, arrived, aborted, failed |
| progress_measure | typed quantity/null | no | Distance or route progress with explicit unit |
| estimated_arrival_time | simulation time/null | no | Derived estimate with model/version provenance |
| actual_arrival_time | simulation time/null | no | Set only after arrival is resolved |
| interruption_reason | enum/string/null | no | Required for interrupted, suspended, aborted or failed states |
| last_transition_event_id | ID | yes | Idempotent transition history |
| definition_version_refs | list/map | yes | Movement, route, terrain/environment and capability versions used |

Arrived means the carrier/entity reached its destination. It does not mean cargo was unloaded, stock was received, a passenger disembarked, or a recipient accepted delivery.

### 2.2 Route and segment

A route is a versioned proposal or accepted traversal sequence, not a second map.

- Route fields: ID, origin and destination references, ordered segment IDs, planning time, validity expiry, feasibility status, cost estimates with units, constraint references, source map version and movement model version.
- Route-segment fields: ID, route ID, sequence index, geometry/path representation, movement mode ID, expected distance and unit, expected duration and simulation-time unit, constraint references, segment status.
- Feasibility results use reason-coded states: feasible, conditionally feasible, blocked, permission denied, capability incompatible, unknown, unsupported.
- A route may be invalidated by changed terrain, infrastructure, control/access, weather, damage or carrier capability. Revalidation must not rewrite historical completed movement events.

### 2.3 Transport assignment and cargo manifest

Transport assignment fields: ID, shipment ID, carrier entity type/ID, capacity definition ID/version, assignment time, assignment state and movement task ID.

Cargo manifest line fields: ID, shipment ID, canonical catalogue ID, quantity and unit ID, Inventory holding/lot references, reserved quantity, loaded quantity, delivered quantity, lost quantity, damaged quantity and line state.

All manifest quantities reconcile against Inventory transactions. A manifest is a Logistics/Movement view, not an Inventory ledger. Loaded cargo remains accounted for in Inventory as an explicit in-transit holding/custody state or equivalent transaction-backed representation.

### 2.4 Movement events

Required event types: MovementRequested, MovementValidated, RouteAccepted, MovementDeparted, MovementProgressed, MovementInterrupted, MovementResumed, MovementArrived, MovementAborted, TransportLoaded, TransportUnloaded, EntityEmbarked and EntityDisembarked.

Each event includes the shared event envelope, task/entity IDs, before/after state references or values, effective simulation time, reason codes where applicable, and idempotency key. Events apply once; replay reconstructs the same state.

## 3. Character System

### 3.1 Character record

Character owns persistent person identity and general attributes only.

| Field | Type | Required | Rule |
|---|---|---:|---|
| id | ID | yes | Stable person identity |
| identity_state | enum | yes | provisional, confirmed, contested, merged, deceased or approved lifecycle value |
| display_name_ref | ID/string reference | no | Presentation value, not identity |
| birth_date_or_interval | date/interval/null | no | Uncertainty supported; do not fabricate exact dates |
| origin_refs | list | yes | Historical/scenario provenance |
| general_attribute_values | versioned attribute records | yes | Only attributes in approved Character Attribute Catalogue |
| personality_profile_ref | ID/version/null | no | Approved personality representation |
| relationship_refs | list of relationship IDs | yes | Explicit relationship records |
| lifecycle_state | enum | yes | Global person lifecycle |
| effective_from | simulation time | yes | State validity |
| last_transition_event_id | ID | yes | Lifecycle/attribute history |
| record_version | integer | yes | Concurrency/version control |

Character attribute value fields: character ID, attribute definition ID/version, typed value, optional unit, effective interval, provenance/source references, confidence/uncertainty representation, and last change event ID. Scales and allowed values come from the approved catalogue; this schema does not invent a universal 0–100 scale.

Character relationship fields: ID, source and target Character IDs, relationship type ID/version, directionality, strength/state as defined by type, effective interval, knowledge/belief state if applicable, provenance and transition event ID.

### 3.2 Military Officer extension

Military-specific facts live in a separate extension keyed by Character ID: service episodes, branch, rank, appointments, qualifications, command assignments, military-specific traits, service record and military career lifecycle. The Military extension must not duplicate general identity, personality or general attributes.

An appointment/command assignment is a time-bounded relationship between an Officer Character and an organisational position/unit. It includes effective start/end times, authority scope, assignment source and transition events. A general attribute change does not automatically change rank, qualification or appointment.

### 3.3 Character events

Required event types: CharacterCreated, IdentityResolved, CharacterAttributeChanged, RelationshipChanged, CharacterLifecycleChanged, OfficerServiceStarted, OfficerPromoted, OfficerAppointed, OfficerRelieved, OfficerQualificationGranted, OfficerTraitChanged and OfficerServiceEnded.

Every event is idempotent and preserves before/after history. Conflicting identity merges require explicit resolution; no automatic destructive merge is permitted.

## 4. Communications System

### 4.1 Message

A message records a communication attempt and payload reference, not the payload's domain truth.

| Field | Type | Required | Rule |
|---|---|---:|---|
| id | ID | yes | Stable message identity |
| originator_actor_id | ID | yes | Sender/authorised originating actor |
| source_system | enum/string | yes | Payload-owning domain |
| payload_type | enum/string | yes | Versioned payload type |
| payload_ref | ID/reference | yes | Domain-owned payload; do not duplicate authoritative state |
| origin_endpoint_id | ID | yes | Origin endpoint |
| recipient_endpoint_refs | list of ID | yes | Explicit recipient endpoints |
| channel_id/version | ID/version | yes | Channel/protocol definition |
| priority_class_id/version | ID/version | yes | Priority policy |
| security_label_id/version | ID/version | yes | Classification/access policy |
| created_sim_time | simulation time | yes | Creation time |
| expiry_sim_time | simulation time/null | no | Source domain decides actionability after expiry |
| delivery_state | enum | yes | queued, in transit, delivered, partially delivered, failed, expired, cancelled, blocked |
| delivery_attempt_refs | list of ID | yes | Per-recipient/attempt history |
| idempotency_key | string | yes | Prevent duplicate delivery effects |
| payload_integrity_ref | ID/hash/null | no | Detect payload mutation if implemented |
| last_transition_event_id | ID | yes | Latest transition |

Delivery-attempt fields: ID, message ID, recipient endpoint ID, attempt number, queued/sent/delivery/acknowledgement times, delivery state, failure reason, link/relay references, latency model version and security decision reference.

Endpoint fields: ID, owner entity type/ID, endpoint type, location reference, supported channel references, availability state, security capability references and effective interval.

Link fields: ID, endpoint A/B, channel/protocol reference, capacity definition, latency/reliability model reference, availability state, disruption references, effective interval and historical validity. Physical propagation and abstract service links must be explicitly distinguished.

Delivery is not comprehension, acknowledgement, authority validation or execution. The receiving domain validates payload against current authority and lifecycle rules. One recipient's delivery does not imply delivery to other recipients.

### 4.2 Communications events

Required event types: MessageCreated, MessageQueued, MessageAttempted, MessageDelivered, MessagePartiallyDelivered, MessageAcknowledged, MessageBlocked, MessageFailed, MessageExpired, MessageCancelled and CommunicationsLinkStateChanged.

A retry may create a new attempt but must not create a new domain action for the same payload/idempotency key.

## 5. Intelligence System

### 5.1 Report, evidence and assessment

An intelligence report is an information product and provenance record, not the authoritative sensor observation, contact/track, or source-domain fact.

Report fields: ID, report type ID/version, originating organisation reference, creation time, subject references, evidence references, source references, analyst/process reference, classification reference, dissemination policy reference, freshness/validity policy reference, report version, superseded report ID and event ID.

Evidence-link fields: ID, assessment ID, evidence type, evidence reference, source reference, observed/reported time, received time, source reliability reference, independence-group reference, support/contradiction classification and provenance-chain references. Evidence is referenced rather than copied as new truth.

Assessment fields: ID, subject references, hypothesis references, judgement text/code, confidence representation reference, evidence-link references, alternative explanations, deception considerations, creation time, validity/review trigger, assessment version, superseded assessment ID, analyst/process reference, classification reference, dissemination policy reference and event ID.

The confidence representation must point to a configured, versioned qualitative or quantitative model. Do not assume all assessments share a universal probability scale. If a numeric probability is used, record its calibration/model version and preserve source evidence; confidence is not source reliability.

### 5.2 Actor knowledge and dissemination

Actor-knowledge fields: ID, actor ID, subject reference, knowledge type, source report/message references, received simulation time, access decision reference, validity state, staleness state, confidence/belief reference and last update event ID.

Knowledge is actor-specific. An assessment existing nationally does not mean every unit, commander or AI process knows it. Intelligence owns assessment and knowledge records; Communications owns whether and when a message is delivered; access policy determines whether the recipient may receive content.

### 5.3 Intelligence events

Required event types: IntelligenceReportCreated, EvidenceLinked, AssessmentCreated, AssessmentUpdated, AssessmentSuperseded, AssessmentExpired, DisseminationAuthorised, ActorKnowledgeUpdated, SourceReliabilityUpdated and DeceptionSuspected.

Assessments are append-versioned. New evidence must not silently rewrite the historical assessment that informed an earlier decision.

## 6. Weather and Environment

### 6.1 Environmental state

Environmental state represents actual conditions for a defined spatial scope and time interval.

| Field | Type | Required | Rule |
|---|---|---:|---|
| id | ID | yes | Stable state-slice identity |
| spatial_scope_type | enum | yes | Point, cell, region, altitude layer, maritime area or approved scope |
| spatial_scope_ref | ID/reference | yes | World Map geometry/cell/region reference |
| vertical_scope | typed interval/null | no | Required for altitude-dependent variables |
| effective_start | simulation time | yes | Start of validity |
| effective_end | simulation time/null | no | End of validity |
| variable_values | typed value records | yes | Each variable has catalogue ID/version, value, canonical unit and quality state |
| source_or_model_ref | ID/version | yes | Source or simulation model |
| uncertainty_ref | ID/version/value | yes | Required where the variable/model represents uncertainty |
| state_quality | enum | yes | modelled, observed, interpolated, estimated, unknown, stale |
| event_id | ID | yes | State creation/change event |

Environmental variable definition fields: ID, version, name, physical quantity, canonical unit ID, allowed bounds, spatial/temporal resolution constraints, altitude applicability, interpolation policy, uncertainty policy, historical availability and source provenance. Every variable must have units and semantics.

### 6.2 Forecast and observation

Forecast fields: ID, issuing organisation/model reference, issued simulation time, target spatial scope, valid start/end, forecast horizon, variable values, uncertainty reference, model version, historical capability reference and superseded forecast ID.

Observation fields: ID, observer/sensor reference, observed time, received time, spatial scope, variable values, measurement uncertainty reference, instrument/method reference and provenance reference. Observations do not directly overwrite actual environmental state; the environment process may assimilate them under an explicit rule.

### 6.3 Environmental event and terrain handoff

Environmental-event fields: ID, event type ID/version, spatial scope, effective interval, severity/measure, driver references, model version, uncertainty reference and transition event ID.

Weather may issue a terrain-transition request/event with causal variables and thresholds, but Terrain Objects remains authoritative for persistent terrain state and transition history. Movement, Sensors, Equipment, Combat and Logistics query environmental conditions and calculate their own effects. No universal weather penalty is stored here.

Required event types: EnvironmentStateUpdated, ForecastIssued, ObservationRecorded, EnvironmentEventStarted, EnvironmentEventChanged, EnvironmentEventEnded, TerrainTransitionRequested and EnvironmentModelVersionChanged.

A model update must not silently recompute historical outcomes unless the simulation explicitly supports versioned historical replay.

## 7. Shared event and transaction semantics

Every event uses the event envelope in Military Cross-System Interface Contracts and includes at least event ID, event type, schema version, source system, source entity ID where applicable, effective simulation time, causation event ID or null, correlation ID, idempotency key, payload and definition-version references.

Event handlers are idempotent. Repeating the same event identity and payload is a no-op after first successful application. Reusing an idempotency key with a materially different payload is a conflict and must be rejected or quarantined.

Cross-system workflows use explicit request/response or event acknowledgement. A timeout is an unknown outcome until reconciled; it is not permission to create a duplicate entity, movement, message, stock movement or intelligence effect.

## 8. Required conformance tests

1. Replaying MovementArrived does not create duplicate arrival consequences or cargo receipt.
2. A route cannot bypass a denied territorial/access decision.
3. Changing a route estimate does not rewrite authoritative current position.
4. A Character attribute update does not overwrite Military rank, qualification or appointment.
5. A conflicting Character identity merge requires explicit resolution and retains history.
6. A multi-recipient message delivered to one recipient remains undelivered to other recipients until their own delivery transition.
7. Retrying a delivered command message does not execute the command twice.
8. Message delivery after order expiry does not automatically execute the expired order.
9. An Intelligence assessment references source evidence and preserves prior versions after new evidence arrives.
10. An actor without access does not gain an assessment merely because it exists.
11. A low-confidence assessment is not treated as actual environmental or sensor truth.
12. A forecast is not substituted for actual environmental state without an explicit consumer policy.
13. An observation does not directly overwrite authoritative environment state.
14. A weather-driven terrain change is committed by Terrain Objects, not Environment.
15. Missing weather data returns unknown/unsupported rather than clear weather or zero impact.
16. Time-dependent queries use simulation time and deterministic event ordering.
17. Save/load/replay preserves current position, active message state, actor knowledge, active environmental state and event history.
18. Player and AI receive the same authoritative outcomes for equivalent inputs and permissions.

## 9. Explicitly not frozen by this schema

This contract does not invent or freeze the physical database ID type; exact simulation-time encoding; Character attribute scales; relationship taxonomy; communications channel taxonomy and numerical latency/reliability calibration; Intelligence source taxonomy/confidence calibration; environment variable catalogue and weather model; route/movement equations; or historical source datasets. These must be approved in the relevant canonical catalogue/model configuration. Implementations must expose supported states and reject unsupported calculations rather than fabricate defaults.

## 10. Related specifications

- Data Model Foundations: data-model-foundations.md
- Military Cross-System Interface Contracts: military-interface-contracts.md
- Movement and Transportation: ../../systems/world/movement-and-transportation-system.md
- Character System: ../../systems/characters/character-system.md
- Communications System: ../../systems/communications/communications-system.md
- Intelligence System: ../../systems/intelligence/intelligence-system.md
- Weather and Environment: ../../systems/world/weather-and-environment-system.md
- Military Interface Audit and End-to-End Vertical Slice: ../architecture/military-interface-audit-and-vertical-slice.md
