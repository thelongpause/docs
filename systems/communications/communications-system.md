# Communications System

**Project:** Project Sovereign  
**Canonical Path:** `systems/communications/communications-system.md`  
**System:** Communications and Information Delivery  
**Document Type:** Canonical Behavioural Specification  
**Document ID:** COMM-001  
**Status:** Canonical initial specification; technology catalogues, schemas, signal/network models and detailed calibration remain open  
**Starting World Date:** 1 January 1955  
**Technology Horizon:** At least 2026, extensible beyond

---

## 1. Purpose

The Communications System governs the transmission, routing, delivery, delay, disruption, acknowledgement and availability of messages between people, headquarters, units, platforms, installations, government institutions, companies, sensors, intelligence services and other authorised entities.

Communications connects decision and observation systems without becoming the owner of the information contained in every message. Command owns orders and their authority; Sensors owns observations and tracks; Intelligence owns assessments and dissemination decisions; Military owns units and appointments; Logistics owns shipments; Communications governs whether, when and through which permitted communication process a message or report is delivered.

**A message existing is not the same as a message being received, understood or acted upon.** Sending an order does not mean the recipient has received it. Delivery does not guarantee correct interpretation. Acknowledgement does not mean the order has been executed. A communication failure must not silently create perfect knowledge or erase a command's prior authority.

The system must support historically appropriate communication methods from 1955 onward, including couriers, field telephone, wired networks, radio, naval and aviation communications, strategic communications infrastructure, satellite and digital networks as they become available. The simulation may abstract technical detail where appropriate, but it must preserve the causal distinctions between sender, channel, delay, disruption, recipient knowledge and subsequent action.

## 2. Design Principles

1. **Content ownership remains with the source domain:** Communications transports messages; it does not own the truth of orders, observations, intelligence assessments or economic transactions.
2. **Send and receive are distinct states:** a message cannot be treated as delivered until the delivery process confirms it.
3. **Delivery, comprehension and execution are distinct:** the receiving system owns interpretation and action.
4. **Communication capability is physical and institutional:** range, infrastructure, equipment, power, operators, protocols, security and operating conditions may constrain service.
5. **Latency is meaningful:** processing, queuing, encoding, transmission, relay, retransmission, travel and handling may create delay where modelled.
6. **Information access is explicit:** a recipient must be authorised and technically able to receive the message.
7. **Disruption has causal inputs:** jamming, damage, power loss, interception, congestion, network failure, distance and other effects apply through defined rules.
8. **No universal radio assumption:** the model must distinguish communication media and technologies, and historical availability must be respected.
9. **No perfect AI communications:** AI actors use the same delivery rules and information boundaries as the Player.
10. **Deterministic persistence:** messages, retries, acknowledgements and delivery events must be save/load safe and idempotent.
11. **Security is not invisibility by default:** confidentiality, authentication, interception risk, deception and cryptographic protection are distinct properties.
12. **Graceful degradation:** when a channel is unavailable, a system may use an authorised alternative, delay, queue, relay or operate under standing instructions; it must not silently bypass the failure.

## 3. Scope and Ownership

### 3.1 Communications Owns

- Message transport and delivery lifecycle.
- Sender, intended recipient(s), message routing and delivery status.
- Channel, link, relay and network service state where defined by the Communications model.
- Communication latency, queueing, capacity, retries and acknowledgement.
- Channel availability, disruption and supported degradation behaviours.
- Technical eligibility of sender/recipient/channel combinations.
- Communication technology and protocol references.
- Signal transmission/propagation effects at the abstraction level configured by the simulation.
- Message integrity/authentication status where modelled.
- Interception or exposure events as communication outcomes, while Intelligence owns the resulting assessment and Combat/EW own relevant physical effects.
- Delivery records and provenance sufficient for audit, replay and actor-knowledge updates.
- Communication-related information access checks, in cooperation with the source and recipient domains.

### 3.2 Owned Elsewhere

- Order identity, authority, intent, precedence, execution and cancellation: Command and Operations.
- Sensor observations, detections, contacts and tracks: Sensors and Detection.
- Intelligence requirements, assessments, source evaluation, confidence, dissemination policy and intelligence products: Intelligence.
- Character identity, appointments and authorised roles: Character and domain systems.
- Unit identity, command hierarchy and military operational state: Military Organisation.
- Equipment identity, configuration, physical condition, installed radios, antennas, power systems and communications equipment capability: Equipment and Materiel.
- Electronic attack, jamming, spoofing and electronic protection effects: Electronic Warfare, with Communications applying the resulting channel/service effects.
- World geometry, distance, terrain and spatial queries: World Map and Terrain.
- Weather/environmental state: Weather and Environment, once specified.
- Commercial network ownership, service contracts, prices and payments: Economy and relevant infrastructure operators.
- Infrastructure identity, shared facility capacity and physical condition: Installations and Facilities and the relevant infrastructure owners.
- Legal authority, classification policy, diplomacy and access rights: Government, Political, Intelligence and relevant domain systems.

## 4. Core Concepts

### 4.1 Message

A Message is a typed communication payload created by an authorised source system or actor. It must have a stable message ID, source, intended recipient or recipient group, creation time, priority, message type, payload reference, security/access requirements and a link to the originating event or entity where applicable.

The payload may be an order, report, acknowledgement, request, status update, intelligence product, warning, administrative communication or other configured message type. Communications transports the payload or a valid reference to it; it does not independently rewrite its domain meaning.

### 4.2 Sender and Recipient

The sender and recipient may be people, headquarters, units, platforms, installations, agencies or other entities with a valid communications endpoint. A person may act through an institutional role, but the message must preserve both the originating actor and the authority context when relevant.

A recipient group may resolve to multiple individual recipients. Delivery to one recipient must not imply delivery to every member of the group. A command post, HQ, relay, or network endpoint is not equivalent to every subordinate unit receiving the same message.

### 4.3 Communication Endpoint

An endpoint is a configured ability to send, receive, relay or process messages. It may be associated with a person, unit, platform, headquarters, installation, civilian network or institution.

Endpoint availability may depend on equipment, operators, power, physical condition, channel configuration, authentication, protocol, access, range, network connection and current disruption. An entity's nominal endpoint does not guarantee that it is operational.

### 4.4 Channel and Link

A Channel represents a communication method or service, such as courier, wired telephone, radio, relay network, satellite link or digital network. A Link is a current or potential connection between endpoints or relays using a compatible channel.

A channel definition describes supported technical and historical properties. A link state represents current feasibility and quality. The model must not assume a permanent direct link merely because both endpoints possess compatible equipment.

### 4.5 Relay and Network

A message may pass through one or more relays, repeaters, headquarters, dispatch offices, exchanges or other nodes. Each relay may impose capacity, delay, access, routing, failure and exposure constraints.

Network topology and route are separate from the message payload. A route may change or fail while the message remains queued or in transit. Relay delivery must be traceable without creating multiple authoritative copies of the same message.

### 4.6 Delivery State

A message lifecycle must distinguish at least:

- Created.
- Validated.
- Queued.
- Transmitting or dispatched.
- In transit or relaying.
- Delivered to endpoint.
- Acknowledged, where required.
- Failed, rejected, expired, cancelled or unresolved.

The exact state path depends on the channel. A courier may physically travel before delivery; a radio message may be transmitted immediately but fail reception; a digital message may be queued pending network access. State transitions must be explicit and time-stamped.

### 4.7 Acknowledgement

An acknowledgement is a separate message or delivery event indicating a defined receipt condition. It must specify whether it acknowledges endpoint receipt, successful integrity check, human/operator acknowledgement, or another defined stage.

An acknowledgement is not proof that the recipient understood the content or executed the requested action. A sender may remain uncertain if acknowledgement is unavailable or delayed.

### 4.8 Communication Latency

Latency is the elapsed time between defined message stages. It may include source processing, queueing, encoding, transmission, relay handling, physical courier travel, retransmission and destination processing where modelled.

Latency must not be conflated with command decision delay, time to interpret a report, or time to execute a physical order. Command owns decision/order-processing rules; Communications provides the actual delivery timeline and relevant channel delays.

### 4.9 Interception and Exposure

A message may be exposed to interception, direction finding, traffic analysis, capture or other observation where supported by the channel and EW/Intelligence rules. Interception does not automatically mean the adversary has understood the content.

Communications records the relevant transmission/exposure event. EW applies electronic effects; Sensors may detect emissions; Intelligence assesses information obtained and its confidence. Access to message content depends on the applicable security, encryption, authentication and interpretation rules.

## 5. Message Lifecycle

A generic message may follow these stages:

1. **Created:** source domain produces a message or message reference.
2. **Validated:** source identity, recipient, authority, message type and required fields are checked.
3. **Authorised:** required access, classification, communication and transmission permissions are validated by their owners.
4. **Routed:** a feasible channel, link or relay path is selected.
5. **Queued/dispatched:** the message enters the selected delivery process.
6. **Transmitting/in transit:** channel-specific progress and delay are resolved.
7. **Delivered or failed:** the receiving endpoint confirms receipt or the delivery process reports a failure.
8. **Integrity/authentication check:** where supported, the recipient endpoint verifies the message according to channel and protocol rules.
9. **Acknowledged:** if required and supported, the recipient sends a separate acknowledgement.
10. **Consumed by recipient system:** Command, Sensors, Intelligence or another domain interprets the message under its own rules.
11. **Closed or retained:** delivery history is preserved under the applicable persistence and audit rules.

Not every channel uses every stage. A missing stage must be justified by the channel model. Delivery failure must not delete the source message or falsely notify the recipient.

## 6. Channel Selection and Feasibility

### 6.1 Inputs

Channel feasibility may depend on:
- Sender and recipient endpoint capabilities.
- Available equipment, power and operator staffing.
- Channel technology, frequency/band, protocol and historical availability.
- Distance, terrain, line of sight, propagation and relay availability where modelled.
- Infrastructure connectivity and physical condition.
- Weather/environmental conditions where relevant.
- Jamming, interference, spoofing and electronic protection.
- Channel capacity, congestion and priority queues.
- Security classification, encryption, authentication and access rights.
- Movement of the sender, recipient or relay.
- Whether the sender and recipient are connected to the same command, institutional or civilian network.
- Current state of damage, repair, maintenance and network operations.

### 6.2 Channel Choice

The sender or source domain may select a channel based on priority, latency, reliability, security, capacity, cost, exposure and available alternatives. The system must preserve hard constraints and must not choose a channel the endpoints cannot use.

A preferred channel may be unavailable. The message may be queued, rerouted, sent through an authorised fallback or fail according to policy. Fallback must not grant capabilities or permissions that the alternative channel does not support.

### 6.3 Historical Availability

Communication methods and capabilities must be data-driven and historically available. A nation must not receive satellite communications, modern digital encryption or other later capability merely because the simulation date advances. Research, procurement, production, deployment, staffing and operating readiness remain separate conditions where the model requires them.

## 7. Latency, Capacity and Reliability

### 7.1 Queueing and Priority

Channels and relays may have finite throughput and concurrent-message limits. Priority may affect queue order, pre-emption or retransmission policy only according to explicit rules. A high-priority message does not bypass hard technical or legal constraints.

Queueing must preserve messages and their order/priority semantics. Reordering is allowed only where the protocol and source domain permit it.

### 7.2 Reliability and Retry

Delivery reliability depends on the channel, current conditions, equipment, network topology, interference and configured protocol. Retries may improve the chance of delivery but consume time and capacity and may increase exposure.

Retrying the same message must not duplicate its domain effect. The recipient domain should use the stable message ID and originating event ID to detect duplicates and apply idempotency. A retransmitted order must not be executed twice merely because it was received twice.

### 7.3 Message Loss and Corruption

The model may support lost, corrupted, incomplete, delayed or misattributed messages where the channel and fidelity justify it. These outcomes must be explicit and bounded. A corrupted message must not silently mutate into a different valid order or report unless a defined deception/spoofing process produces that outcome.

### 7.4 Congestion and Outage

Congestion, network outages, relay loss, power failure, damaged equipment, channel interference or other disruptions may delay or prevent delivery. The system must expose the reason category and any retry or alternative route. Recovery must not retroactively mark failed messages as delivered unless a new valid delivery event occurs.

## 8. Security, Authentication and Access

### 8.1 Access Control

A message can be sent or delivered only where the sender has authority to originate it and the recipient is permitted to receive it. Command authority, institutional roles, classification and legal permissions are checked by their owning domains; Communications enforces the required transport/access constraints.

The existence of a communications endpoint does not grant authority to issue orders or access classified content.

### 8.2 Authentication and Integrity

Where modelled, authentication verifies the sender or message origin according to the applicable technology and procedures. Integrity checks establish whether the payload arrived intact. Neither guarantees that the message is truthful, wise or authorised unless the relevant authority check succeeds.

Spoofing, impersonation, interception, cryptographic compromise and other deception methods must be resolved through explicit EW, Intelligence, Security and technology rules.

### 8.3 Confidentiality and Interception

Confidentiality depends on the channel, encryption/security technology, key availability, procedures and adversary capability. Encryption does not necessarily conceal the existence, timing, source or volume of traffic.

An intercepted message may provide metadata without content, content without reliable source identification, or no usable information. Intelligence owns the resulting assessment and confidence.

### 8.4 Information Boundaries

The actual message content and delivery history may be stored by the simulation, but only permitted recipients should learn its content through the normal information model. AI actors must not query hidden message payloads or enemy command traffic unless a valid interception/intelligence event grants that knowledge.

## 9. Military Command and Operations Interface

Command owns order identity, authority, precedence, validity, intent and execution lifecycle. Communications provides delivery and acknowledgement.

A command order must distinguish:
- Created/issued by the authority.
- Accepted by the command system.
- Sent for delivery.
- Delivered to the intended HQ/unit/platform.
- Acknowledged where required.
- Understood or interpreted according to Command rules.
- Planned and executed by the recipient.
- Completed, failed, superseded or cancelled.

The command system may have standing orders or delegated autonomy that remain active during communications loss. Loss of communication does not automatically cancel all prior orders, nor does it grant unrestricted independent authority. The recipient's allowed behaviour is defined by command doctrine, prior orders, current situation and applicable decision rules.

A subordinate cannot be assumed to have received a new order merely because a superior issued it. A superior cannot assume execution merely because delivery was acknowledged.

## 10. Sensors, Intelligence and EW Interfaces

### 10.1 Sensors and Reports

Sensors owns observations, detections, tracks and their uncertainty. Communications transports sensor reports, contact reports, warnings and other sensor-derived messages to authorised recipients. Communications delay may make a report stale before it is received.

A sensor detection is not automatically shared with every friendly unit. Reporting policies, network capability, channel availability and recipient authority determine whether the observation is delivered.

### 10.2 Intelligence Dissemination

Intelligence owns assessment, source evaluation, confidence, dissemination rules and the recipient's intelligence knowledge state. Communications executes the permitted delivery of intelligence products and reports.

A report that is created but not delivered must not become available to a remote commander or AI actor. A delivered report does not become certain fact merely because it arrived.

### 10.3 Electronic Warfare

EW owns jamming, electronic attack, spoofing and electronic protection effects. Communications applies the resulting channel/service degradation, delay, loss, misdirection or other outcomes through defined interfaces.

Communications must not independently implement a second EW resolution model. An EW event should identify affected channels, links, endpoints or message flows and the effective time; Communications resolves delivery consequences once.

## 11. Logistics and Civilian/Economic Interfaces

Logistics may send shipment status, route changes, procurement requests, delivery acknowledgements and resupply reports through authorised communications endpoints. Communications transports those messages but does not own cargo or inventory.

Civilian transport, industrial networks, government administration, diplomacy and other systems may use the same communications principles. Their domain-specific authority and message content remain with the source system.

Where commercial communications services are modelled, Economy owns service contracts, price and payment. Infrastructure and facility owners retain physical network and site state. Communications resolves actual message delivery using the available service.

## 12. State Ownership and Interface Matrix

| Data or behaviour | Authoritative owner | Communications responsibility |
|---|---|---|
| Message content and meaning of an order | Source domain, e.g. Command, Sensors or Intelligence | Transport payload/reference without changing domain semantics |
| Message delivery state, latency, queueing, relay and acknowledgement | Communications | Authoritative |
| Order authority, lifecycle, interpretation and execution | Command / Operations | Deliver order and report delivery outcome |
| Sensor observation, detection and track | Sensors and Detection | Deliver reports to permitted recipients |
| Intelligence assessment, confidence and dissemination decision | Intelligence | Transport approved intelligence products |
| EW/jamming/spoofing effect | Electronic Warfare | Apply resulting channel/link/message consequences |
| Endpoint hardware, installed equipment and physical condition | Equipment and Materiel | Query capabilities and operating state |
| Geometry, terrain and spatial relation | World Map / Terrain | Query relevant propagation/route context |
| Weather/environmental state | Weather/Environment, when specified | Consume relevant conditions |
| Institutional role, access policy and legal authority | Owning domain / Government | Enforce returned permission/access requirements |
| Commercial service contract and financial state | Economy | Reference valid service arrangements |
| Facility/network physical state and capacity definitions | Infrastructure / Facilities | Query and reflect actual service capability |
| Character identity and role | Character / domain systems | Reference stable endpoint identity |
| Message and delivery audit events | Communications, with source-domain correlation | Preserve IDs, time, provenance and idempotency |

## 13. AI and Player Parity

The Player and AI use the same message creation, routing, access, latency, delivery and acknowledgement rules. AI may differ in channel choice, message priority, network design, operational security and contingency planning, but it cannot bypass the communication process.

AI must reason from information actually received or otherwise legitimately available. An enemy order issued but not intercepted is not known to the AI. A friendly message sent but not delivered is not known to the remote recipient. An AI may estimate that a message was likely received only if its decision model represents that uncertainty.

## 14. Time, Persistence and Idempotency

### 14.1 Time-Based Processing

Message creation, queueing, transmission, relay, courier travel, delivery and acknowledgement use the shared simulation clock. Different channels may use different processing cadence, but the timing contract must remain consistent across save/load and replay.

### 14.2 Stable Identity and Duplicate Handling

Each message must have a stable message ID and originating event/correlation reference. Retries and relays preserve the logical message identity while recording separate delivery attempts as needed. Duplicate delivery must be distinguishable from a new message.

Recipient domains must prevent duplicate application of consequential messages using stable IDs or equivalent idempotency. Communications must not decide that a repeated order is a new order merely because a retransmission occurred.

### 14.3 Save/Load

Persist queued/in-transit messages, route/relay progress, delivery state, acknowledgements, relevant link state and unresolved outcomes needed to resume delivery. On load, reconcile with source and recipient domains. Do not repeat a completed domain action because a delivery event was replayed.

### 14.4 Audit Trail

Important messages should retain sender, recipient, type, priority, source event, creation/sent/delivery times, channel/route, delivery outcome, acknowledgement and relevant security/access results. Access to audit records must follow classification and information rules.

## 15. Failure Modes and Edge Cases

The system must handle at least:

- Sender has no valid endpoint.
- Recipient endpoint is absent, disabled or inaccessible.
- No compatible channel exists.
- Channel is available but no feasible link or relay route exists.
- Message exceeds payload/capacity constraints.
- Channel or relay becomes unavailable while the message is queued or in transit.
- Sender or recipient moves during delivery.
- Message arrives after the originating order/request has expired or been superseded.
- Duplicate transmission or duplicate delivery event.
- Acknowledgement is lost even though the message was delivered.
- Message is delivered but not acknowledged or interpreted.
- Corrupted or unauthenticated message.
- Intercepted traffic metadata without content access.
- Jamming or interference changes channel quality.
- Relay/endpoint is destroyed, damaged, powered down or disconnected.
- Recipient lacks authority to act on the message.
- An actor attempts to read hidden message content through AI or UI.
- Save/load occurs during queueing, relay, delivery or acknowledgement.
- Historical technology is unavailable at the current simulation date.
- Fallback channel is incompatible, insecure or not authorised.
- Communications is unavailable and a unit must continue under standing orders or delegated autonomy.

Every failure must produce an explicit status and reason category. No failure may silently mark a message delivered or grant the recipient knowledge.

## 16. Minimum Acceptance Tests

1. An issued command order is not considered delivered until the recipient endpoint confirms delivery.
2. A delivered order is not automatically marked executed.
3. Acknowledgement is a distinct state and may be lost independently of the original delivery.
4. A unit without a valid endpoint cannot receive a remote order through that endpoint.
5. A channel outage delays or prevents delivery and preserves the message's unresolved state.
6. A message rerouted through a relay records the route and relevant delays.
7. Replaying the same message delivery does not execute the domain action twice.
8. A sensor report is not available to a remote HQ before delivery.
9. A delivered intelligence report preserves source, confidence and uncertainty; delivery does not increase its truth automatically.
10. EW jamming changes delivery outcomes through one authoritative effect path and does not duplicate the EW calculation.
11. An AI cannot inspect enemy messages unless a valid interception/intelligence process provides access.
12. A character's appointment or endpoint role does not by itself grant authority to issue any message type.
13. A destroyed or disabled relay interrupts or reroutes affected messages without teleporting them to the next node.
14. A historical communication technology cannot be used before its configured availability and deployment prerequisites.
15. Save/load during message transmission does not duplicate delivery, acknowledgement or domain effects.
16. A fallback channel is used only if the sender/recipient have compatible equipment, access and authority.
17. Congestion or priority handling follows configured policy and does not create infinite channel capacity.
18. Message payload content remains owned by its source domain and cannot be altered by the Communications system.
19. A message arriving after its command/request expired is handled by the source domain's validity rules rather than automatically executed.
20. Player and AI actors use the same delivery and information-access constraints.

## 17. Open Decisions

- Canonical channel and protocol catalogue, including historical availability.
- Exact representation of endpoints, links, relay networks and network capacity.
- Which channels model physical propagation versus abstract service delivery.
- Detailed latency, reliability, queueing, retransmission and congestion calibration.
- Security classification, encryption, authentication, key management and compromise model.
- Interface between EW events and communications link/message degradation.
- Courier and physical dispatch representation for historical and low-technology contexts.
- How command message delivery interacts with standing orders and delegated autonomy.
- Which civilian and commercial communications networks are simulated in the initial implementation.
- Event envelope, schema versioning, persistence and idempotency details.
- Actor-specific access rules for message history and intelligence content.

These decisions must be resolved in data schemas, reference catalogues or explicit domain rules. Unknown capability must not be treated as perfect or instantaneous communication.

## 18. Related Specifications

- [Military Branches and Unit Organisation](../military/military-branches-and-unit-organisation.md) — command hierarchy, unit identity and communications requirements.
- [Doctrine, Command and Operations](../military/doctrine-command-and-operations.md) — order authority, lifecycle and execution.
- [Sensors and Detection](../military/sensors-and-detection.md) — observations, contacts, tracks and reporting.
- [Electronic Warfare](../military/electronic-warfare.md) — jamming, spoofing, electronic attack and protection.
- [Intelligence System](../intelligence/intelligence-system.md) — assessment and dissemination contract, to be authored.
- [Character System](../characters/character-system.md) — stable person identity and domain extension boundary.
- [Movement and Transportation System](../world/movement-and-transportation-system.md) — physical movement and relay/courier travel where applicable.
- [World and Command Interface Contracts](../../technical/data-schemas/world-and-command-interface-contracts.md) — message, endpoint, link, delivery-attempt and event contracts.
- [Simulation Integration and AI Decision Architecture](../integration/simulation-integration-and-ai-decision-architecture.md) — shared decision and information boundaries.
- [Military Interface Audit and End-to-End Vertical Slice](../../technical/architecture/military-interface-audit-and-vertical-slice.md) — cross-system acceptance plan.

---

## 19. Completion Criteria

The Communications System is ready for implementation when:

1. Message, endpoint, channel/link, delivery and acknowledgement schemas are stable.
2. Command, Sensors, Intelligence and EW interfaces agree on ownership and event semantics.
3. Latency, disruption, access and historical availability rules are data-driven and testable.
4. Delivery retry and save/load do not duplicate domain effects.
5. Actor-specific information access is enforced.
6. At least one command-delivery scenario and one sensor/intelligence-report delivery scenario pass.
7. Player and AI use the same communication rules and information boundaries.

The logical message, endpoint, link and delivery contracts are defined in SCHEMA-WORLD-CMD-001. Channel taxonomy, historical availability, numerical latency/reliability calibration, physical schema constraints and executable delivery tests remain open. The existence of this specification does not prove that communications simulation has been implemented.
