
# Inventory System

**Canonical Path:** `docs/systems/inventory/inventory-system.md`  
**System:** Inventory  
**Document Type:** Systems Specification  
**Status:** Foundational Specification  
**Starting World Date:** 1955

---

## 1. Purpose

The Inventory System maintains the authoritative record of physical goods held throughout the world.

It determines the quantities of goods represented in recognised inventory holdings, their locations, ownership and custody, their condition, and whether they are available, reserved, committed, in transit, restricted, or otherwise unavailable for a particular use.

Inventory is a shared system used by industrial companies, commercial enterprises, governments, military organisations, infrastructure operators, and other entities that hold physical goods.

It supports the movement of resources through the economy, from extraction and manufacturing to storage, distribution, consumption, military supply, and final use.

The system must support simple inventory operations and complex supply chains involving multiple locations, organisations, transport networks, intermediate goods, and competing demands for scarce resources.

### 1.1 Core Principle

**Inventory is the authoritative record of physical stock. No system may independently create, consume, transfer, or destroy physical stock outside the authorised Inventory transaction interfaces.**

Production may generate goods, Logistics may move goods, Economy may purchase or sell goods, and Military may request or consume supplies. Inventory records the corresponding physical stock changes and ensures that quantities remain consistent across the simulation.

An economic transaction does not, by itself, move physical stock. A production order does not, by itself, establish that its outputs exist. A transport order does not, by itself, establish that goods have arrived. A reservation does not, by itself, consume goods.

Each operation must be reflected through the appropriate authoritative systems and interfaces.

---

## 2. System Boundaries

Inventory owns authoritative physical stock records and the rules governing stock reservations, inventory transactions, and stock adjustments.

It does not independently determine the economic, productive, or logistical decisions that lead to those transactions.

| System | Authoritative responsibility |
|---|---|
| Economy System | Financial balances, purchases and sales, prices, expenditure, revenue, financial ownership records, and economic decisions |
| Industry Data | Industry Types, Industry instances, Industry Scale, establishment configuration, and Industry Areas |
| Production System | Production facilities, production orders, scheduling, execution, and coordination of input consumption and output registration |
| Production Processes and Recipes | Transformation definitions, input requirements, expected outputs, yields, by-products, and process losses |
| Good and Service Catalogue | Canonical product identifiers, classifications, attributes, and measurement conventions |
| Logistics and Transportation | Transport capacity, route planning, physical movement execution, and delivery operations |
| Terrain and Geography | Physical locations, geographic features, and infrastructure geography |
| Government System | Legal restrictions, seizures, requisitions, embargoes, and other authorised interventions |
| Military System | Military supply requirements, allocation priorities, acceptance criteria, and authorised consumption requests |
| Workforce and Population | Population and workforce records, including household-held or consumed goods where those activities are modelled |
| Research System | Technologies and capabilities that may affect production, storage, or handling requirements |

The interfaces described in this document define system responsibilities. They do not require every dependent system to be fully implemented before basic inventory operations can function.

### 2.1 Ownership Rules

1. Inventory owns the authoritative quantity and state of each physical stock holding.
2. Inventory records reference goods through canonical identifiers defined by the Good and Service Catalogue.
3. Inventory does not independently define product identity, specifications, or canonical characteristics.
4. Inventory does not determine prices, market values, or financial asset valuations.
5. Inventory does not independently purchase goods, transfer money, or settle financial obligations.
6. Inventory does not schedule production or calculate recipe outputs.
7. Inventory does not plan or simulate transportation routes.
8. Inventory does not independently determine which organisation or objective should receive scarce resources.
9. Inventory must not permit stock to be consumed, reserved, or transferred beyond the quantity eligible for the requested operation.
10. Every committed stock-changing transaction must be traceable and reproducible.
11. Services and other non-physical economic outputs must not be represented as physical stock unless a separate, explicitly defined physical good is produced.
12. No system may record the same physical stock change independently of Inventory.

---

## 3. Core Concepts

### 3.1 Good

A Good is a catalogue-defined physical product or material that may be held in inventory.

Examples include iron ore, coal, crude oil, steel, timber, grain, fuel, machinery, electrical components, vehicles, ammunition, medical supplies, and finished consumer products.

Each good must reference a canonical identifier in the Good and Service Catalogue.

A good's identity must not be duplicated merely because it is held by a different organisation, stored in a different location, or used for a different purpose.

### 3.2 Inventory Holding

An Inventory Holding is a logical record of stock associated with a particular good and a defined combination of ownership, location, and other relevant attributes.

A holding may contain one or more stock lots.

Inventory holdings provide the aggregate view of stock. Underlying stock lots preserve distinctions in condition, origin, quality, restrictions, or other attributes that affect subsequent operations.

### 3.3 Stock Lot

A Stock Lot is a distinguishable quantity of a good held under a shared set of relevant attributes.

A lot may track:

- Quantity.
- Quality or grade.
- Condition.
- Production or acquisition origin.
- Production or acquisition date.
- Expiry date, where applicable.
- Ownership.
- Custody.
- Restrictions.
- Provenance.
- Other supported attributes.

Lots allow the simulation to distinguish goods that share a catalogue identity but are not necessarily interchangeable for every purpose.

The initial implementation may aggregate stock where these distinctions do not affect simulation behaviour. It must not aggregate away an attribute that changes eligibility, availability, quality, or the outcome of a later operation.

### 3.4 Inventory Location

An Inventory Location is a recognised place at which stock can be held or through which stock passes.

Examples include warehouses, factories, production facilities, mines, farms, ports, railway depots, distribution centres, shops, military depots, military units, construction sites, and households where household inventory is modelled.

An inventory location must reference the appropriate authoritative location or entity in the relevant system.

Inventory does not independently create geographic locations. Locations must originate from Terrain and Geography, Industry Data, Military, or another authoritative location-owning system.

### 3.5 Owner

The Owner is the entity that holds the recognised ownership interest in a stock holding.

Ownership may belong to a company, government, military organisation, household, or another supported entity.

Ownership is distinct from physical location and custody. Goods may be stored by a third-party warehouse, held by a distributor, or transported by a carrier without ownership necessarily changing.

### 3.6 Custodian

The Custodian is the entity responsible for physically holding or handling stock on behalf of an owner, where custody is relevant to the simulation.

Custody may affect access, handling, and operational responsibility.

Custody does not automatically imply ownership or authority to sell, consume, or transfer stock.

### 3.7 Reservation

A Reservation is a commitment of available stock to a defined purpose, order, or authorised recipient.

Reserved stock remains part of physical inventory but is no longer generally available for competing uses.

Examples include stock reserved for a production order, military supply requirement, customer order, construction project, scheduled shipment, or authorised government requirement.

A reservation does not itself consume stock or transfer ownership.

### 3.8 Inventory Transaction

An Inventory Transaction is an authorised, recorded operation that changes stock quantity, location, availability, condition, ownership, or another inventory-controlled attribute.

Transactions may include:

- Receipt.
- Production output.
- Consumption.
- Transfer.
- Dispatch.
- Receipt at destination.
- Reservation.
- Reservation release.
- Loss.
- Destruction.
- Adjustment.
- Reclassification.
- Authorised ownership transfer.

Each transaction must identify the relevant stock, quantities, source and destination where applicable, reason, and authoritative initiating operation.

### 3.9 In-Transit Stock

In-Transit Stock is physical stock that has departed its origin but has not yet been received at its destination.

In-transit stock remains part of the world's physical inventory unless an authorised operation records a loss or another change.

It must not simultaneously be counted as available stock at the origin and destination.

Logistics owns the movement operation. Inventory owns the authoritative stock record throughout that movement.

---

## 4. Inventory Data Model

The following fields define the conceptual data contract. The implementation may use relational tables, structured objects, event records, or another suitable representation, provided the required invariants remain enforceable.

### 4.1 Inventory Holding

An Inventory Holding should contain:

| Field | Description |
|---|---|
| `inventory_holding_id` | Stable, unique holding identifier |
| `catalogue_id` | Canonical Good and Service Catalogue identifier |
| `location_id` | Authoritative location identifier |
| `owner_id` | Entity that owns the stock, where applicable |
| `custodian_id` | Entity responsible for custody, where applicable |
| `quantity_on_hand` | Total physical quantity represented by the holding |
| `quantity_reserved` | Quantity committed to active reservations |
| `quantity_available` | Quantity available for eligible new commitments |
| `unit` | Canonical or compatible unit of measurement |
| `status` | Operational status of the holding |
| `created_at` | Simulation time at which the holding was created |
| `updated_at` | Simulation time of the latest relevant update |

Aggregate quantity fields may be derived from stock lots and reservations rather than independently stored.

If a value is stored redundantly for performance, the system must maintain consistency with the authoritative underlying records.

An inventory holding represents physical stock. It must not be used as a substitute for a financial asset record or a service-delivery record.

### 4.2 Stock Lot

A Stock Lot should contain:

| Field | Description |
|---|---|
| `stock_lot_id` | Stable, unique lot identifier |
| `catalogue_id` | Canonical Good and Service Catalogue identifier |
| `quantity` | Physical quantity represented by the lot |
| `unit` | Unit of measurement |
| `location_id` | Current inventory location |
| `owner_id` | Current owner, where applicable |
| `custodian_id` | Current custodian, where applicable |
| `condition` | Current supported condition classification |
| `quality_attributes` | Relevant grade, purity, specification, or quality information |
| `origin_type` | Production, purchase, extraction, transfer, or another supported origin |
| `origin_reference` | Reference to the authoritative originating operation |
| `created_at` | Simulation time of lot creation |
| `expiry_at` | Optional expiry time or date |
| `restrictions` | Applicable handling, legal, quality, or allocation restrictions |
| `metadata` | Optional additional supported attributes |

A lot may be split into multiple lots when a transaction separates quantities with different destinations, owners, conditions, or restrictions.

Where lots are merged, all attributes that affect future behaviour must remain consistent. Incompatible lots must not be merged in a way that destroys material distinctions.

### 4.3 Reservation Record

A Reservation should contain:

| Field | Description |
|---|---|
| `reservation_id` | Stable, unique reservation identifier |
| `catalogue_id` | Canonical good identifier |
| `stock_lot_id` | Referenced lot, where reservation is lot-specific |
| `inventory_holding_id` | Referenced holding |
| `quantity_reserved` | Reserved quantity |
| `requesting_entity_id` | Entity requesting the reservation |
| `purpose_type` | Production, shipment, military supply, customer order, or another supported purpose |
| `purpose_reference` | Reference to the initiating order or requirement |
| `priority` | Explicit reservation priority, where applicable |
| `status` | Active, partially fulfilled, fulfilled, released, expired, or cancelled |
| `created_at` | Reservation creation time |
| `expires_at` | Optional expiry time |
| `fulfilled_quantity` | Quantity fulfilled against the reservation, where applicable |

Reservations must be traceable to their purpose and the authority that created them.

`quantity_reserved` and `fulfilled_quantity` use the stock's canonical Catalogue unit unless an explicit compatible unit conversion is recorded. A reservation must not silently mix units or product identities. Where a reservation is allocated across multiple Production recipe input lines, Production owns the line-level allocation records; Inventory remains authoritative for the reservation's total quantity, eligible stock and fulfilment state.

A reservation does not replace the physical stock record or the initiating system's order record.

### 4.4 Inventory Transaction Record

Every committed stock-changing transaction must produce an auditable record.

The conceptual transaction record should include:

| Field | Description |
|---|---|
| `transaction_id` | Stable, unique transaction identifier |
| `transaction_type` | Receipt, consumption, transfer, output, loss, adjustment, or another supported type |
| `catalogue_id` | Canonical good identifier |
| `quantity` | Transaction quantity |
| `unit` | Unit of measurement |
| `source_location_id` | Source location, where applicable |
| `destination_location_id` | Destination location, where applicable |
| `source_owner_id` | Source owner, where applicable |
| `destination_owner_id` | Destination owner, where applicable |
| `stock_lot_id` | Relevant lot or lots |
| `reservation_id` | Related reservation, where applicable |
| `initiating_system` | System authorised to initiate the transaction |
| `initiating_reference` | Production order, shipment, purchase, or other originating reference |
| `simulation_time` | Simulation time at which the transaction was committed |
| `reason_code` | Structured reason for the operation |
| `idempotency_key` | Identifier used to prevent duplicate execution |
| `metadata` | Additional relevant transaction information |

A transaction may affect more than one holding or lot. In such cases, the transaction must record the complete set of stock movements needed to represent the operation consistently.

The implementation may separate the transaction header from individual stock movements.

---

## 5. Quantity Semantics

Inventory must distinguish between physical stock, reserved stock, and stock available for additional commitments.

### 5.1 Quantity on Hand

Quantity on hand represents the physical quantity currently held in an inventory holding.

It includes stock that is reserved, provided the stock remains physically held and has not been consumed, lost, destroyed, or transferred out.

It excludes stock that has departed the holding and is recorded as in transit or held elsewhere.

### 5.2 Quantity Reserved

Quantity reserved represents the portion of stock committed to active reservations.

Reserved stock is not automatically available for another reservation, sale, consumption request, or shipment.

A reservation does not reduce quantity on hand.

### 5.3 Quantity Available

For stock governed by the basic reservation model:

\[
Q_{\text{available}} =
Q_{\text{on hand}} -
Q_{\text{reserved}}
\]

Where:

- \(Q_{\text{available}}\) is the quantity available for additional commitments.
- \(Q_{\text{on hand}}\) is the physical quantity currently held.
- \(Q_{\text{reserved}}\) is the quantity committed to active reservations.

The calculation applies only to compatible units and the relevant holding or stock scope.

Additional restrictions may reduce the quantity eligible for a particular use. Damaged goods, for example, may exist physically but be unsuitable for a production recipe.

The system must not treat every unreserved unit as universally usable.

### 5.4 Quantity In Transit

Quantity in transit represents stock that has departed its source but has not yet been received at its destination.

It must be represented separately from available stock at either endpoint.

Dispatch reduces the source holding by the quantity dispatched. The stock remains part of the world's physical inventory and is associated with the authorised movement until it is received, lost, or otherwise resolved.

### 5.5 Non-Negative Quantities

Physical quantities must not become negative through ordinary inventory transactions.

A stock-consuming transaction must be rejected or constrained if the eligible quantity is insufficient.

Any exceptional correction that reveals an earlier accounting error must be handled through an explicit, auditable correction process. It must not silently bypass quantity invariants.

---

## 6. Inventory Availability and Restrictions

Physical presence does not guarantee that stock is available for every operation.

Inventory must retain the attributes and commitments needed to determine whether a quantity is eligible for a requested transaction.

### 6.1 Availability Conditions

Stock may be unavailable because it is:

- Reserved for another purpose.
- In transit.
- Damaged or unusable.
- Expired.
- Restricted by law or policy.
- Incompatible with the required quality specification.
- Held at a location inaccessible to the requesting operation.
- Subject to an ownership or custody restriction.
- Otherwise ineligible under an authoritative system's rules.

Restrictions must be represented explicitly where the simulation models them.

Inventory provides stock and restriction information to authorised requesting systems. It does not independently determine legal policy, facility eligibility, route accessibility, or military priority.

### 6.2 Stock Condition

Where condition affects usability, inventory must distinguish between relevant states.

Potential condition classifications include:

- `serviceable`
- `damaged`
- `unserviceable`
- `contaminated`
- `expired`
- `under_inspection`
- `unknown`

The applicable set may vary by good category and implementation scope.

Condition classifications must have defined meanings and must not be assigned arbitrary operational effects.

The applicable production or consumption system determines the acceptance criteria for a particular use.

### 6.3 Quality and Specification

Inventory may hold multiple lots of the same good with different quality attributes.

A production recipe may require a particular grade or purity. Inventory must provide relevant lot information so Production can determine which quantities satisfy that requirement.

Inventory must not redefine the meaning of a material grade. The Good and Service Catalogue and applicable domain specifications own the relevant definitions.

### 6.4 Ownership and Custody Restrictions

Stock held by one organisation must not automatically become available to another organisation merely because both entities can access the same location.

Access and authority must be validated through the applicable ownership, custody, contractual, and legal rules.

A warehouse holding a company's stock may have physical custody without having the right to sell, consume, or transfer ownership of it.

### 6.5 Location Eligibility

A request for stock must identify the relevant location or authorised stock scope.

Inventory must not assume that stock at a remote warehouse, factory, port, or military depot is immediately available at the requesting location.

If stock must be moved, the operation must be coordinated through Logistics and recorded through Inventory.

---

## 7. Reservations

Reservations allow multiple systems to commit stock to future operations without prematurely consuming it.

### 7.1 Reservation Creation

A reservation request must specify:

- The good or goods required.
- The requested quantity and compatible unit.
- The eligible stock scope or location.
- The requesting entity.
- The purpose and originating order.
- Quality, condition, ownership, or other applicable requirements.
- The applicable priority or allocation rule.
- Any reservation expiry or release conditions.

Inventory validates the request against available stock and applicable restrictions.

A reservation may be accepted in full, accepted in part where explicitly permitted, or rejected.

The system must not reserve more stock than exists or double-commit the same quantity to competing reservations.

### 7.2 Reservation Scope

Reservations may be made against a specific stock lot or a defined holding.

Lot-specific reservations are required when the requesting operation depends on lot attributes, quality, condition, origin, or other restrictions.

Holding-level reservations may be used when the relevant stock is interchangeable and no lot-specific constraint applies.

The reservation must preserve enough information to determine which stock is committed and prevent incompatible stock from being substituted without authorisation.

### 7.3 Partial Reservation

If the requested quantity exceeds the eligible available quantity, the system may return a partial reservation only if the requesting operation permits partial fulfilment.

The accepted quantity must be explicit.

Production or the requesting system determines whether the partial quantity is sufficient to proceed. Inventory must not independently decide whether a partially supplied production order should run.

### 7.4 Reservation Fulfilment

A reservation is fulfilled when the associated authorised operation consumes, transfers, or otherwise commits the reserved quantity according to its purpose.

Fulfilment must reconcile the reserved quantity with the actual quantity processed.

For example, if a production order reserves 100 tonnes of material but executes using 80 tonnes, the resulting inventory transaction must reflect the actual consumption and the disposition of the remaining reservation.

The unused portion must remain reserved, be released, or be handled according to the authorised order and reservation rules. It must not disappear implicitly.

### 7.5 Reservation Release

A reservation may be released when:

- Its initiating order is cancelled.
- The requesting system no longer requires the stock.
- The reservation expires under an explicit rule.
- The operation completes and unused stock is no longer required.
- An authorised system cancels the commitment.

Releasing a reservation does not create stock. It returns the eligible unconsumed quantity to availability.

### 7.6 Reservation Priority

Inventory may support priority values supplied by the requesting system or determined through an authorised allocation policy.

Examples include production urgency, contractual commitments, military requirements, or government directives.

Inventory must not invent a universal priority hierarchy or decide which economic or military objective should take precedence.

Where scarce stock requires allocation, the authoritative requesting or policy system must supply a deterministic allocation rule.

### 7.7 Reservation Expiry

Reservations may expire if their purpose is time-limited.

Expiry must be governed by an explicit simulation-time rule.

An expired reservation must not remain an active commitment indefinitely, but its release must be recorded so that availability can be reconstructed.

Expiry must not automatically cancel the originating production order, purchase, or shipment unless the relevant system explicitly defines that consequence.

---

## 8. Inventory Transactions

Inventory transactions provide the controlled means by which the physical stock record changes.

### 8.1 Transaction Principles

Every committed transaction must:

1. Be initiated by an authorised system or operation.
2. Reference valid goods and relevant stock holdings.
3. Use compatible quantities and units.
4. Validate the stock available for the requested operation.
5. Preserve ownership, custody, location, and condition rules.
6. Record the resulting stock changes.
7. Record the originating operation and reason.
8. Prevent duplicate commitment.
9. Preserve an auditable history.
10. Produce a deterministic result under the same relevant state and inputs.

A transaction must not be committed if its required validations fail.

### 8.2 Receipt

A receipt adds physical stock to an inventory holding.

A receipt may originate from:

- An authorised purchase delivery.
- Completed production.
- Resource extraction.
- An authorised transfer.
- An authorised government or military operation.
- Another explicitly supported source.

The receipt must identify the origin of the goods and the location at which they are received.

A purchase order alone does not create a physical receipt. Stock is recorded when the relevant receipt event is authorised under the applicable procurement and delivery rules.

### 8.3 Consumption

Consumption removes physical stock from inventory for an authorised purpose.

Examples include inputs consumed by production, fuel used in an operating process, supplies consumed by a military unit, materials used in construction, goods consumed by households where modelled, and goods irreversibly used.

The transaction must identify the quantity consumed and the initiating operation.

Consumption must not be inferred merely from a reservation or an order being scheduled.

### 8.4 Transfer

A transfer changes the location, ownership, custody, or another authorised attribute of stock.

The transaction must explicitly identify which attributes change.

A location transfer does not automatically imply a change in ownership. A change in custody does not automatically imply a sale.

Where a transfer involves physical movement, Logistics coordinates the movement and Inventory records the corresponding stock state changes.

### 8.5 Loss

A loss transaction records a reduction in physical stock that is not represented by ordinary consumption or a completed transfer.

Examples include spoilage, accidental destruction, transport damage that renders stock unusable, theft where modelled, environmental degradation, and other explicitly modelled losses.

The reason for the loss must be recorded.

The system responsible for detecting or simulating the event supplies the authorised loss operation. Inventory records the physical consequence.

### 8.6 Adjustment

An adjustment corrects an inventory discrepancy or changes a stock attribute through an authorised process.

Adjustments must include:

- The affected stock.
- The quantity or attribute changed.
- The reason.
- The initiating authority.
- The simulation time.
- A reference to any relevant audit or investigation record.

Adjustments must not be used as an undocumented substitute for production, purchases, transfers, or consumption.

### 8.7 Destruction

Destruction is an authorised operation that removes physical stock from usable inventory because the stock has been destroyed or irreversibly rendered unavailable.

Destruction must be distinguished from ordinary consumption where that distinction affects the simulation.

If the destruction process generates recoverable materials, those materials must be recorded as outputs through an authorised operation rather than silently restored to inventory.

### 8.8 Reclassification

Reclassification changes how existing stock is represented when an authorised process changes its recognised identity, condition, or specification.

For example, an inspection may establish that a lot does not meet its originally recorded grade.

A reclassification must preserve the relationship to the original stock and must not create duplicate physical quantities.

If a transformation physically converts one good into another, it is normally a production process rather than a simple inventory reclassification.

---

## 9. Production Integration

Production and Inventory must operate as separate but coordinated systems.

Production determines what is executed and the actual quantities resulting from execution. Production Processes and Recipes defines the quantitative transformation rules. Inventory records the physical input and output consequences.

### 9.1 Production Inputs

Before a production order executes, Production must obtain the required stock through the Inventory interface.

The recipe defines expected input requirements. Production determines the requested execution quantity. Inventory validates availability and records any authorised reservation.

The presence of a valid recipe does not guarantee that the required materials exist.

### 9.2 Input Consumption

When production executes, the actual quantity of inputs consumed must be recorded through an authorised Inventory transaction.

Consumption must correspond to the quantity actually processed and the applicable recipe execution rules.

If production is interrupted or completes partially, Inventory must not consume the full planned quantity unless the actual execution and its defined losses justify that result.

### 9.3 Production Outputs

When Production reports a completed execution or authorised partial execution, Inventory records the resulting physical outputs.

Outputs may include primary products, co-products, by-products, recoverable materials, and physical waste that the simulation tracks.

Recorded quantities must correspond to the authorised execution result, not merely the recipe's theoretical output for a fully completed order.

### 9.4 Shared Production Execution Transaction and Atomicity

Production and Inventory must use one shared Production Execution transaction to commit the physical stock effects of each production execution. Production orchestrates the execution and owns its operational execution record. Inventory alone owns the authoritative physical stock quantities, reservations, stock changes and inventory transaction records.

The transaction must use a stable Production Execution identifier and idempotency key. Before commitment, Inventory must validate all relevant input reservations or supported allocation records, product eligibility, compatible units, available quantities, and the complete set of physical output changes against authoritative state.

Where the execution is valid, the shared transaction must commit all authorised input consumption and all physical outputs as one atomic logical operation. This includes applicable primary outputs, co-products, by-products, recoverable materials and tracked waste. Every output must retain its own `catalogue_id`, unit, quantity and relationship to the Production Execution. Non-physical services or deliverables must not be represented as physical stock.

The transaction must also preserve references linking the Production Execution record to the authoritative Inventory transactions. The Production Execution must not be reported as committed before its physical stock effects are committed, and Inventory must not infer production completion from a stock transaction alone.

If validation or commitment fails, no unintended partial stock change may remain. Repeating a request with the same idempotency key must not duplicate consumption, reservations or outputs. Reuse of the same key with a different execution payload must be rejected. If a prior request committed successfully but its response was lost, a retry must resolve to that committed result rather than applying the stock changes again.

The implementation may use database transactions, a transactional event/commit protocol, or an equivalent mechanism. The specification defines the consistency and recovery guarantees, not the technical implementation.

Long-running or multi-stage production may consist of multiple separately identified Production Executions. Each committed stage must have a defined, auditable effect and must not imply that an uncommitted later stage has completed.

### 9.5 By-products and Losses

Production must report the actual by-products, recoveries, and losses supported by its execution model.

Inventory records physical outputs and stock reductions where applicable.

A non-material process loss must not be invented as a physical inventory item merely to balance quantities.

### 9.6 No Direct Stock Creation

Production must not bypass Inventory when recording completed output.

Similarly, Inventory must not independently calculate recipe outputs or decide that a production order has completed.

The systems must coordinate through an explicit execution result and authorised transaction.

---

## 10. Economy Integration

Inventory records physical quantities, not monetary value.

Economy remains authoritative for financial balances, prices, payments, revenue, expenditure, Company Capital, and other economic records.

### 10.1 Purchasing

A purchase may create a financial commitment or payment without immediately creating a physical receipt.

Inventory receives goods when the authorised delivery or receipt event occurs.

The purchase record and physical receipt must be linked where the procurement system supports that relationship.

### 10.2 Sales

A sale does not automatically remove stock from inventory when a commercial agreement is created.

The applicable order, fulfilment, and delivery rules determine when stock is reserved, dispatched, and transferred.

Inventory records the physical consequences of fulfilment. Economy records the corresponding financial consequences according to its own transaction rules.

A sale must not be deducted once as a sale and again as a physical transfer. Each physical quantity change must be represented exactly once.

### 10.3 Ownership Changes

Ownership may change without stock changing physical location.

Where an authorised sale or other transaction transfers ownership, Inventory updates the relevant ownership records without inventing a physical movement.

The associated financial transfer remains Economy's responsibility.

### 10.4 Stock Valuation

Inventory must not independently determine market prices, replacement costs, accounting valuations, or company asset values.

If a valuation view is needed, it must use applicable valuation rules supplied by Economy or another authorised financial system.

Physical quantity and financial value must remain distinct concepts.

### 10.5 Financial and Physical Reconciliation

Where required, the simulation may compare financial records with physical inventory records.

Differences may arise from timing, goods in transit, damage, unfulfilled orders, or other legitimate circumstances.

Reconciliation identifies and resolves discrepancies through authorised procedures. It must not silently alter physical stock merely to make financial totals agree.

---

## 11. Logistics and Transportation Integration

Logistics determines how goods move through the transportation network. Inventory maintains the authoritative stock record before, during, and after that movement.

### 11.1 Shipment Preparation

A shipment may require goods to be reserved before dispatch.

Inventory validates stock, quantity, location, ownership, and other applicable constraints.

Logistics validates the transport operation, including route and capacity constraints.

Neither system may assume that the other has completed its part without an authoritative result.

### 11.2 Dispatch

When goods are dispatched, Inventory records their departure from the source holding and associates them with the authorised movement.

The dispatched quantity must no longer be available for use at the source.

It must not be added to the destination's available stock before receipt.

### 11.3 In-Transit State

While a shipment is in transit, its stock must remain accounted for.

The in-transit record must preserve its association with the originating stock, shipment, and expected destination.

Inventory must not duplicate goods across source, transit, and destination holdings.

### 11.4 Arrival and Receipt

When Logistics confirms arrival and the applicable receiving conditions are satisfied, Inventory records receipt at the destination.

The received quantity may differ from the dispatched quantity if the simulation explicitly records losses, damage, or discrepancies.

The system must preserve a traceable relationship between dispatch and receipt.

### 11.5 Interrupted Movement

If a shipment is delayed, diverted, damaged, captured, destroyed, or otherwise interrupted, the relevant system must report the event.

Inventory updates the stock's recorded state only through an authorised transaction.

The handling of cargo at intermediate locations, transfers between carriers, or cargo awaiting inspection must be defined by the Logistics and Inventory contracts.

### 11.6 No Instantaneous Remote Availability

Stock at one location must not be treated as immediately available at another location merely because both locations exist in the simulation or are connected by a route.

Any operation that requires physical relocation must respect applicable movement and delivery rules.

---

## 12. Storage, Deterioration, and Expiry

Inventory must be capable of representing stock whose condition changes over time.

Detailed spoilage, degradation, and storage physics may be implemented incrementally, but the core data model must support their introduction.

### 12.1 Storage Requirements

A good may require particular storage conditions or handling capabilities.

Examples include temperature control, moisture protection, ventilation, hazardous-material containment, secure storage, special handling, and protection against contamination.

Where the simulation models these requirements, the Catalogue or relevant technical specification defines them, and the applicable storage or facility system determines whether they are satisfied.

Inventory records the relevant stock attributes and authorised resulting changes.

### 12.2 Expiry

A stock lot may have an expiry date or time.

Expiry must be defined for the applicable good or lot where relevant.

An expired lot must not be silently deleted. It remains physically present unless a separate authorised process consumes, disposes of, destroys, or otherwise changes the stock.

The system must distinguish between physical existence and eligibility for use.

### 12.3 Deterioration

If stock deteriorates over time, the simulation must use explicit deterioration rules.

Those rules may affect quantity, condition, quality, usability, recoverability, or permitted destinations.

Deterioration must be deterministic under the simulation's time and state rules unless an explicitly defined stochastic process applies.

### 12.4 Initial Implementation

The foundational implementation should support stock condition, quality attributes, and expiry metadata even if detailed deterioration calculations are deferred.

Where deterioration is not yet simulated, the system must not invent automatic losses or quality changes.

---

## 13. Ownership, Custody, and Government Intervention

Inventory must support situations in which physical stock is held, controlled, or transferred under different legal and organisational arrangements.

### 13.1 Ownership and Location

The stock owner and physical location must be represented independently.

A company may own goods held in a third-party warehouse. A government may own goods stored at a contractor's facility. A military organisation may hold supplies acquired from a commercial supplier.

The physical stock must not be duplicated because multiple entities have a relationship to it.

### 13.2 Custody and Access

Custody may determine which entity physically controls or handles goods.

Access to stock must be validated against applicable permissions and ownership rules.

Possession does not automatically confer a right to sell, consume, or transfer ownership.

### 13.3 Government Restrictions

Government actions may restrict, requisition, seize, release, or otherwise affect stock when authorised under the simulation's legal and political systems.

Inventory records the resulting restrictions or changes.

The Government System determines whether the intervention is authorised and defines its scope. Inventory must not independently invent a seizure, embargo, requisition, or other government action.

### 13.4 Military Stock

Military organisations may maintain dedicated inventory holdings, depots, unit supplies, ammunition, fuel, spare parts, and other physical resources.

Military allocation rules determine which supplies are required and where they should be assigned.

Inventory validates and records reservations, transfers, receipts, and consumption.

Military requirements must not permit stock to become available beyond the quantity recorded in the authoritative inventory state.

---

## 14. Inventory Transfers and Ownership Changes

Stock movements and changes in ownership must be recorded without duplicating physical goods.

### 14.1 Internal Transfer

An internal transfer moves stock between locations or holdings while preserving ownership, unless the operation explicitly changes ownership.

The transaction must reduce the source holding and establish the corresponding stock state at the destination or in transit.

### 14.2 Inter-Organisation Transfer

An inter-organisation transfer may involve a change in custody, ownership, or both.

The operation must specify which attributes change and identify the relevant authorisation.

A change of owner without a change of location must not be represented as a physical shipment.

### 14.3 Split and Merge

A stock lot may be split when a portion of its quantity is assigned to a different location, owner, condition, or restriction.

Lots may be merged only when the resulting record preserves all distinctions required by the simulation.

Splitting and merging must preserve total physical quantity unless the operation explicitly includes a separately recorded loss, conversion, or other stock-changing effect.

### 14.4 Transfer Validation

Before a transfer is committed, Inventory must validate:

- The source stock exists.
- The quantity is eligible for transfer.
- The operation is authorised.
- Source and destination references are valid.
- Units and stock attributes are compatible.
- Active reservations are respected.
- The operation does not duplicate stock.
- Applicable restrictions are satisfied.

Where physical transport is required, Logistics must additionally validate and execute the movement.

---

## 15. Losses, Waste, and Disposal

The simulation must distinguish between goods that remain physically present but unusable and goods that no longer exist as usable physical stock.

### 15.1 Unusable Stock

Damaged, contaminated, expired, or otherwise unusable goods may remain in physical inventory.

Their quantity remains recorded, but their availability for specific operations may be restricted.

A later repair, inspection, treatment, or recovery process may change their status through an authorised operation.

### 15.2 Waste

Waste may be recorded as a physical good if the Good and Service Catalogue defines it and the simulation needs to track its quantity or handling.

Where waste is tracked, it may require storage, transport, treatment, recycling, or disposal.

Waste must not be silently discarded if its handling is part of the simulated economy or environment.

### 15.3 Disposal

Disposal removes stock from usable inventory through an authorised operation.

If disposal represents physical destruction or removal from the simulated world, the relevant quantity change must be recorded.

If disposal generates recoverable materials or other outputs, those outputs must be recorded through an authorised process.

### 15.4 Environmental Consequences

Inventory does not independently calculate pollution, emissions, land contamination, or environmental damage.

Where disposal or destruction has environmental consequences, the responsible system must calculate them and report the resulting effects through the appropriate interface.

---

## 16. Auditing and Historical Records

Inventory must maintain a traceable history of committed stock changes.

This is necessary for reliable simulation, debugging, economic reconciliation, military supply tracking, and historical analysis.

### 16.1 Transaction History

Every committed stock-changing operation must be attributable to an originating action or authorised system.

The history should make it possible to determine:

- What good changed.
- What quantity changed.
- Which holding or lot was affected.
- Where the stock originated.
- Where it went, where applicable.
- Who owned or controlled it.
- Which system initiated the operation.
- Why the operation occurred.
- When it occurred in simulation time.

### 16.2 Corrections

Historical transactions must not be silently overwritten when a correction is required.

A correction must preserve an audit trail connecting the original record to the corrective action.

The implementation may use compensating transactions, explicit correction records, or another method that preserves the history of the change.

### 16.3 Historical Reconstruction

The inventory architecture should support reconstructing stock quantities at a prior simulation time from an authoritative baseline and the relevant committed transactions.

The initial implementation need not expose a full historical-query interface, but its transaction model must not make historical reconstruction impossible.

### 16.4 Provenance

Where provenance matters, stock should retain its relationship to the operation that created or introduced it.

Examples include a production order, extraction operation, purchase receipt, or originating transfer.

Provenance must survive ordinary transfers and lot splits. Merging stock from multiple origins must not erase provenance where that information remains relevant to the simulation.

---

## 17. Determinism, Atomicity, and Idempotency

Inventory is a foundational simulation system. Inconsistent stock records would propagate errors into production, trade, logistics, military operations, and economic outcomes.

### 17.1 Determinism

Given the same authoritative stock state, transaction request, permissions, and applicable rules, Inventory must produce the same result.

Reservation allocation, lot selection, quantity calculations, and other operations that can affect outcomes must use explicit deterministic rules.

If several eligible lots could satisfy a request, the selection rule must be defined rather than relying on incidental database order or execution timing.

### 17.2 Atomicity

A committed transaction must leave inventory state internally consistent.

For a transfer between two holdings, the source reduction and destination or in-transit representation must form one logically consistent operation.

For production execution, the relationship between input consumption and output registration must follow the shared Production and Inventory transaction contract.

The implementation may use database transactions, event-based commits, or another mechanism, provided the resulting state remains consistent.

### 17.3 Idempotency

A retried request must not create a duplicate stock change.

For example, if a production output transaction has already been committed, retrying the same authorised operation must not create the same output twice.

The system must use a stable transaction or idempotency identifier to recognise repeated requests.

### 17.4 Concurrency

If multiple operations attempt to reserve or consume the same stock concurrently, the system must ensure that the combined committed quantity does not exceed the eligible quantity.

The implementation must validate against an authoritative state at the point of commitment.

Concurrent requests must not independently observe the same unreserved quantity and both commit reservations that exceed it.

### 17.5 Save and Load

Saving and reloading the simulation must preserve:

- Stock quantities.
- Locations.
- Owners and custodians.
- Lot attributes.
- Active reservations.
- In-transit stock.
- Pending transaction states where supported.
- Historical transaction references.
- Idempotency information required to prevent duplicate commits.

Reloading a save must not automatically replay already committed stock changes.

---

## 18. Failure Handling

Inventory operations must return explicit outcomes when they cannot be completed.

A rejected operation must not partially mutate stock unless partial commitment is explicitly supported and reported.

### 18.1 Proposed Failure Codes

The following codes are illustrative and should be aligned with the project's shared error-code conventions.

**Stock and quantity errors**

- `INVENTORY_HOLDING_NOT_FOUND`
- `STOCK_LOT_NOT_FOUND`
- `GOOD_NOT_FOUND`
- `INSUFFICIENT_STOCK`
- `INSUFFICIENT_AVAILABLE_STOCK`
- `INVALID_QUANTITY`
- `INCOMPATIBLE_UNIT`
- `UNDEFINED_UNIT_CONVERSION`
- `NEGATIVE_STOCK_PROHIBITED`

**Reservation errors**

- `RESERVATION_NOT_FOUND`
- `RESERVATION_ALREADY_FULFILLED`
- `RESERVATION_EXPIRED`
- `RESERVATION_QUANTITY_EXCEEDED`
- `STOCK_ALREADY_RESERVED`
- `PARTIAL_RESERVATION_NOT_PERMITTED`

**Transfer and permission errors**

- `INVALID_SOURCE_LOCATION`
- `INVALID_DESTINATION_LOCATION`
- `TRANSFER_NOT_AUTHORISED`
- `OWNERSHIP_RESTRICTION`
- `CUSTODY_RESTRICTION`
- `STOCK_RESTRICTED`
- `INCOMPATIBLE_STOCK_ATTRIBUTES`

**Transaction errors**

- `TRANSACTION_ALREADY_COMMITTED`
- `TRANSACTION_STATE_CONFLICT`
- `INVALID_TRANSACTION_REFERENCE`
- `INVALID_TRANSACTION_TYPE`
- `IDEMPOTENCY_CONFLICT`
- `INVENTORY_INVARIANT_VIOLATION`

### 18.2 Failure Behaviour

When a transaction fails:

1. The system must return a structured failure result.
2. The result must identify the reason for rejection.
3. The stock state must remain unchanged unless a defined partial-commit contract applies.
4. The originating system must receive enough information to decide whether to retry, modify, or cancel the request.
5. The failure may be logged for auditing and debugging.
6. A retry must not duplicate any previously committed transaction.

Inventory must not silently satisfy an invalid request by creating additional stock, consuming reserved stock, or changing ownership without authorisation.

---

## 19. System Interfaces

Inventory should expose a stable set of logical interfaces to other simulation systems.

These interfaces describe required behaviour, not mandatory programming-language function names.

### 19.1 Query Stock

**Purpose:** Retrieve the current authoritative inventory state for a defined stock scope.

**Inputs:**
- Good identifier or set of goods.
- Location or permitted location scope.
- Optional owner or custodian.
- Optional quality, condition, or restriction filters.

**Outputs:**
- Quantity on hand.
- Quantity reserved.
- Quantity available for the requested scope.
- Relevant lot attributes.
- Applicable restrictions.
- In-transit quantities where explicitly requested.

The response must distinguish physical stock from reserved and in-transit quantities.

### 19.2 Reserve Stock

**Purpose:** Commit eligible stock to an authorised purpose.

**Inputs:**
- Good identifier.
- Requested quantity and unit.
- Eligible stock scope.
- Requesting entity.
- Purpose and originating reference.
- Applicable quality and restriction requirements.
- Partial-fulfilment permission.
- Applicable priority or allocation rule.

**Outputs:**
- Reservation identifier.
- Accepted quantity.
- Relevant stock or lot references.
- Reservation status.
- Structured rejection or partial-fulfilment information, where applicable.

### 19.3 Release Reservation

**Purpose:** Release unused or cancelled stock commitments.

**Inputs:**
- Reservation identifier.
- Quantity to release, where partial release is supported.
- Authorised initiating reference.

**Outputs:**
- Updated reservation status.
- Quantity released.
- Resulting stock availability.

A release must not alter physical stock quantity.

### 19.4 Commit Consumption

**Purpose:** Record the authorised consumption of physical stock.

**Inputs:**
- Good and stock references.
- Quantity and unit.
- Relevant reservation, if applicable.
- Initiating operation.
- Idempotency identifier.

**Outputs:**
- Committed transaction identifier.
- Actual quantity consumed.
- Updated stock quantities.
- Structured failure information, where applicable.

### 19.5 Record Receipt or Production Output

**Purpose:** Add physical stock resulting from an authorised operation.

**Inputs:**
- Good identifier.
- Quantity and unit.
- Destination location.
- Owner and custodian, where applicable.
- Relevant lot attributes.
- Originating operation.
- Idempotency identifier.

**Outputs:**
- Transaction identifier.
- Stock lot or holding references.
- Recorded quantity.
- Updated inventory state.

### 19.6 Dispatch Stock

**Purpose:** Record stock leaving a source holding for an authorised physical movement.

**Inputs:**
- Stock and reservation references, where applicable.
- Quantity and unit.
- Source location.
- Shipment or movement reference.
- Destination reference, where known.
- Idempotency identifier.

**Outputs:**
- Dispatch transaction identifier.
- Quantity dispatched.
- In-transit stock reference.
- Updated source availability.

### 19.7 Receive In-Transit Stock

**Purpose:** Record goods arriving at a destination after an authorised movement.

**Inputs:**
- In-transit stock reference.
- Shipment or movement reference.
- Receiving location.
- Actual received quantity.
- Relevant condition or quality findings.
- Idempotency identifier.

**Outputs:**
- Receipt transaction identifier.
- Received stock references.
- Recorded received quantity.
- Recorded discrepancy or loss information, where applicable.

### 19.8 Record Loss or Adjustment

**Purpose:** Record an authorised physical loss, correction, or stock adjustment.

**Inputs:**
- Affected stock.
- Quantity or attribute change.
- Reason code.
- Authorised initiating system.
- Originating event or audit reference.
- Idempotency identifier.

**Outputs:**
- Transaction identifier.
- Resulting stock state.
- Structured validation or failure information.

### 19.9 Events

Inventory may publish events such as:

- `inventory_stock_received`
- `inventory_stock_consumed`
- `inventory_stock_reserved`
- `inventory_reservation_released`
- `inventory_stock_dispatched`
- `inventory_stock_received_at_destination`
- `inventory_stock_lost`
- `inventory_stock_adjusted`
- `inventory_stock_reclassified`

These names are illustrative and should follow shared event naming and versioning conventions.

The initiating system remains responsible for the decision that caused the event. Inventory reports the authoritative stock consequence.

---

## 20. Simulation Timing and Daily Processing

Inventory must participate in the simulation's authoritative processing sequence without independently deciding the order of production, economic, or logistical activities.

The exact daily phase order is a shared simulation architecture decision. Inventory must follow the relevant system contracts and ensure that each committed operation observes a consistent state.

A typical interaction may proceed as follows:

1. An authoritative system requests an inventory query or transaction.
2. Inventory validates the request, references, quantities, permissions, and current stock state.
3. If the operation involves a reservation, Inventory commits it under the applicable allocation rules.
4. If the operation requires physical movement, Logistics coordinates dispatch and transportation.
5. Inventory records dispatch and associates the stock with the authorised movement.
6. Upon arrival, the relevant system confirms the actual receipt.
7. Inventory records the received quantity, location, condition, and applicable ownership.
8. The initiating system receives the authoritative transaction result.
9. Relevant dependent systems receive the appropriate events or updated records.

This sequence is illustrative rather than universal. A production receipt, local consumption, or ownership change may require a different sequence.

All operations must respect the same underlying invariants.

### 20.1 No Unauthorised Daily Stock Changes

Inventory must not create stock merely because a new day begins.

Time-dependent changes, including expiry, deterioration, or other losses, may occur only through explicit rules implemented by the responsible system and recorded through authorised inventory operations.

### 20.2 Stable Transaction Ordering

Where transaction ordering affects outcomes, the authoritative simulation architecture must provide a deterministic ordering rule.

Inventory must not rely on nondeterministic request arrival order to decide which competing operation receives scarce stock.

Where priority-based allocation is required, the responsible system must provide the relevant rule and tie-breaking behaviour.

---

## 21. Initial Implementation Scope

The initial implementation should prioritise correctness, traceability, and integration with Production before introducing advanced warehousing or logistics simulation.

### 21.1 Required Initial Capabilities

The foundational implementation should support:

- Canonical good references.
- Inventory holdings and stock lots.
- Physical quantities and compatible units.
- Locations and ownership records.
- Optional custody records.
- Stock condition and quality attributes.
- Quantity on hand, reserved, and available.
- Stock reservations and releases.
- Authorised consumption.
- Authorised receipts and production outputs.
- Internal transfers.
- Dispatch and in-transit stock.
- Receipt at a destination.
- Explicit losses and adjustments.
- Transaction provenance and audit history.
- Deterministic stock selection and allocation.
- Atomic transaction commitment.
- Idempotent transaction handling.
- Save-and-load preservation.
- Structured errors and validation.
- Production, Economy, and Logistics interfaces.

### 21.2 Deferred Capabilities

The following may be introduced as the simulation develops:

- Detailed warehouse layouts and storage slots.
- Pallet, container, and packaging-level tracking.
- Batch-specific expiry and spoilage simulation.
- Advanced stock rotation policies.
- Temperature and environmental storage simulation.
- Hazardous-material handling.
- Automated inventory replenishment.
- Dynamic inventory optimisation.
- Complex multi-echelon distribution.
- Detailed household inventories.
- Advanced ownership and consignment arrangements.
- Sophisticated stocktaking and reconciliation.
- Detailed theft, damage, and cargo-loss simulation.
- Automated allocation across competing production and military demands.

The initial model must remain extensible enough to support these features without abandoning the authoritative stock and transaction model.

---

## 22. Acceptance Criteria

The Inventory System is ready for foundational integration when the following conditions are met.

### 22.1 Stock Representation

- [ ] Physical stock can be recorded against a canonical good and valid location.
- [ ] Stock can be associated with an owner and, where applicable, a custodian.
- [ ] Multiple lots of the same good can be represented.
- [ ] Relevant quality, condition, and restriction attributes can be retained.
- [ ] Quantity on hand, reserved, and available are distinguishable.
- [ ] In-transit stock remains accounted for without being duplicated at either endpoint.
- [ ] Non-physical services are not represented as physical stock.

### 22.2 Transactions

- [ ] Authorised receipts create stock exactly once.
- [ ] Authorised consumption reduces stock by the committed quantity.
- [ ] Transfers preserve stock consistency between source, transit, and destination.
- [ ] Ownership changes do not create duplicate physical stock.
- [ ] Reservations prevent the same stock from being committed twice.
- [ ] Reservation release restores appropriate availability without creating stock.
- [ ] Losses and adjustments are explicitly recorded.
- [ ] Invalid or unauthorised transactions are rejected.

### 22.3 Production Integration

- [ ] Production can query eligible input stock.
- [ ] Production can reserve required inputs.
- [ ] Actual consumption is recorded through Inventory.
- [ ] Production outputs are added only through authorised receipt transactions.
- [ ] Partial production creates only the corresponding authorised output.
- [ ] Failed or retried production commits cannot duplicate consumption or output.
- [ ] Inventory does not calculate recipe outputs independently.

### 22.4 Economy and Logistics Integration

- [ ] Purchasing does not create physical stock before an authorised receipt.
- [ ] Sales do not remove stock merely because a commercial agreement exists.
- [ ] Stock ownership can change independently of physical location.
- [ ] Dispatch removes stock from source availability.
- [ ] In-transit stock is not available at the destination before receipt.
- [ ] Arrival records the actual received quantity.
- [ ] Financial valuation remains outside Inventory's authority.
- [ ] A physical outflow is not deducted twice because it is associated with both a sale and a transfer.

### 22.5 Determinism and Integrity

- [ ] Identical transaction requests against identical authoritative state produce identical results.
- [ ] Competing reservations cannot exceed eligible stock.
- [ ] Physical stock quantities cannot become negative through ordinary transactions.
- [ ] Retried requests cannot duplicate committed stock changes.
- [ ] Every committed stock-changing operation has an auditable origin.
- [ ] Saving and reloading preserves holdings, reservations, in-transit stock, and transaction integrity.
- [ ] Corrections preserve a traceable history rather than silently overwriting committed transactions.

---

## 23. Unresolved Design Decisions

The following decisions must be resolved through shared contracts or implementation testing rather than assumed by this specification.

1. **Canonical measurement system:** Finalise the authoritative unit registry, conversion rules, and treatment of material-dependent conversions.
2. **Quantity precision:** Define numerical precision, rounding behaviour, and fractional quantities for discrete and continuous goods.
3. **Stock-lot granularity:** Determine which goods require lot-level tracking and which may use aggregate holdings.
4. **Ownership model:** Finalise supported owner and custodian entity types, including consignment and third-party storage.
5. **Reservation allocation:** Define the shared priority, tie-breaking, and partial-allocation rules used when stock is scarce.
6. **Reservation expiry:** Establish standard expiry behaviour and the interaction between expired reservations and their originating orders.
7. **Transaction architecture:** Select the implementation model for atomic commits, transaction records, and idempotency.
8. **Production transaction boundaries:** Finalise the contract for input reservations, consumption, output recording, interrupted production, and partial completion.
9. **Logistics integration:** Define the state model for dispatched cargo, intermediate stops, diversions, damage, losses, and final receipt.
10. **Condition and quality:** Define the Catalogue and domain contracts for quality attributes, grades, condition, inspection, and usability.
11. **Deterioration:** Determine which goods deteriorate over time and how expiry, spoilage, and degradation are simulated.
12. **Waste and disposal:** Define the treatment of physical waste, recycling, destruction, and environmental consequences.
13. **Historical reconstruction:** Determine the implementation strategy for stock history, transaction retention, and historical queries.
14. **Save compatibility:** Define how inventory state and transaction records are migrated across simulation version changes.
15. **Starting inventory:** Specify the initial stock holdings, ownership, locations, quantities, and provenance for the 1955 starting world.
16. **Good and Service Catalogue:** Finalise canonical identifiers, physical versus non-physical classifications, unit definitions, and relevant stock attributes in the Catalogue specification.
17. **Household inventory:** Determine whether consumer goods held by individual households are represented as detailed inventory holdings or through an aggregate population-consumption model.
18. **In-transit accounting:** Finalise the canonical representation of stock in transit and its relationship to Logistics movement records.
19. **Inventory valuation interfaces:** Define any read-only interface through which Economy can obtain quantities and provenance for valuation without transferring valuation authority to Inventory.
20. **Event contracts:** Establish shared event names, payload schemas, versioning, retry behaviour, and failure handling.

Until these decisions are resolved, implementation must preserve the ownership boundaries and invariants defined here.

---

## 23A. Resource Extraction and Source-to-Stock Transaction Contract

A natural resource source or deposit and extracted Inventory stock are distinct authoritative states.

- The owning resource-source model defines source identity, resource type, remaining quantity where finite, regeneration where applicable, extraction constraints, environmental conditions and validity/versioning. Inventory must not maintain a second authoritative deposit balance.
- Production authorises and executes an extraction operation against a valid source, process/recipe, facility capability and operating constraints. A deposit's existence does not create an Industry, extraction capability, delivered input or Inventory stock.
- Inventory records extracted material only when the extraction result is committed as an authorised physical receipt or stock-creation transaction. The transaction must identify the source, extraction operation/execution, Catalogue identity, quantity, unit, destination holding/location and provenance.
- Source depletion and Inventory receipt must commit as one atomic logical operation, or through a durable prepare/commit/recovery protocol that prevents either duplicate stock or unexplained source depletion. A retry with the same idempotency key and payload returns the original result; reuse of that key with a different payload is rejected.
- If extraction fails before commit, neither source depletion nor usable output stock may be committed. If a failure occurs during a multi-step implementation, recovery must resolve the operation deterministically before reporting success.
- Finite depletion, renewable regeneration, extraction efficiency, environmental impact and the exact source-state schema remain owned by the resource-source and Production specifications. This contract deliberately does not invent their algorithms or parameters.
- The resulting stock is ordinary Inventory stock subject to stock-lot, quality, ownership, location, reservation, transfer and consumption rules. The source deposit is not an Inventory holding, and extracted stock must not remain available at the source and in Inventory simultaneously.

## 24. Governing Principle

Inventory represents the physical reality of the simulated economy.

A company may purchase goods without having received them. A factory may possess a valid recipe without having the necessary materials. A military unit may have a supply requirement without having received the required stock. A shipment may have departed without having arrived. A reservation may commit stock without consuming it.

These are distinct states and must remain distinct in the simulation.

Economy records financial consequences. Industry Data defines industry configuration. Production executes transformations. Production Processes and Recipes define the quantitative rules of those transformations. Logistics moves goods. Inventory records authoritative physical stock and its state throughout the process. The Good and Service Catalogue defines the goods and services referenced by these systems.

**No purchase, production order, shipment, reservation, or requirement may be treated as physical stock unless the corresponding inventory state has been authoritatively recorded.**
