# Project Sovereign: Implementation Data Schema Foundation

**Status:** Draft for implementation review  
**Scope:** Canonical relational data model and schema-generation rules  
**Applies to:** Shared domain schemas, including the Good and Service Catalogue, Industry Data, Production Processes and Recipes, Production, Inventory, Economy, Population, and their system interfaces

## 1. Purpose

This document translates the existing simulation specifications into a consistent relational data model. It establishes the shared rules that domain schemas must follow before database migrations or application models are implemented.

This is a schema contract, not a replacement for the owning simulation specifications. When a domain rule changes, its authoritative specification must be updated first, then the affected schema and validation rules must be reconciled.

The initial implementation target is a relational PostgreSQL database. The design remains database-oriented rather than tying simulation rules to a particular ORM, server framework, or user interface.

## 2. Governing Principles

1. **One authoritative owner per state.** A table may reference another domain's state but must not become a second authoritative copy of it.
2. **Stable identifiers.** Persistent entities use stable identifiers. Relationships use identifiers, never display names, array positions, or coordinates alone.
3. **Explicit relationships.** Many-to-many relationships and repeating input/output sets are represented as related records, not packed into comma-separated strings or parallel arrays.
4. **Foreign-key integrity.** References to entities with a persistent identity must use foreign keys where the entities share the database boundary. Cross-service references must have an explicitly defined validation contract.
5. **Catalogue identity is canonical.** Every product reference resolves to the Good and Service Catalogue. Schema columns use `catalogue_id` for this relationship. Legacy or conceptual field names such as `catalogue_entry_id` must map to the same canonical identity rather than create a second identifier system.
6. **Physical stock is restricted.** Inventory records only eligible physical products. Services and other non-physical deliverables are not stored as stock.
7. **Units are explicit.** Every stored quantity has a unit or a clearly defined unit inherited from a referenced, versioned definition. Unit compatibility and conversions follow the shared measurement contract. A consumption interval is not a physical unit.
8. **Derived values are not competing truth.** Values such as available stock, effective capacity, remaining order quantity, and utilisation should be derived unless there is a documented performance reason to cache them. Cached values must be reconcilable with authoritative records.
9. **Historical outcomes are reproducible.** Executions reference exact recipe versions and preserve the inputs, outputs, and configuration references needed to explain the result. Updating current definitions must not silently rewrite history.
10. **Transactions are auditable and idempotent.** Retried commands must not duplicate stock changes, financial movements, establishment costs, or production outputs.
11. **Flexible metadata is not a substitute for schema.** JSONB may hold explicitly extensible descriptive attributes or versioned configuration payloads. It must not hide core relationships, quantities, statuses, ownership, or fields used for critical integrity checks.
12. **Simulation rules are deterministic.** Schema constraints must not rely on incidental database row order, wall-clock timestamps, random identifiers as simulation ordering, or unspecified rounding.

## 3. Identifier and Temporal Conventions

### 3.1 Identifiers

Use a stable identifier for each persistent entity and retain it for the entity's lifetime. Canonical Catalogue identifiers are globally unique within the Catalogue. Recipe versions have identifiers distinct from the logical recipe identifier. Production Orders, Production Executions, Inventory Transactions, Reservations, and Stock Lots each have their own stable identifiers.

An identifier must not be derived solely from a mutable name, owner, location, Industry Scale, or current status.

Schema implementations must define identifier type and generation centrally. The same identifier must use a compatible type in every foreign-key reference. Do not mix integer, UUID, and text representations for the same identity across tables.

### 3.2 Simulation Time

Simulation dates and intervals must follow Project Sovereign's calendar rules: 365 days per simulation year, with no leap years. The schema must distinguish simulation time from real-world audit timestamps.

- **Simulation time** determines when a state or event is valid in the game world.
- **Audit time** records when a database operation was recorded or changed, if operational auditing requires it.

Real-world timestamps must not determine simulation outcomes. The shared technical specification must define the canonical representation of simulation dates, intervals, and ordering before database migrations encode it.

### 3.3 Units and Quantities

A quantity is incomplete without its measurement convention. Tables that store quantities must either store a unit reference alongside the quantity or explicitly inherit a unit from a referenced recipe line or Catalogue entry. Where a quantity can be expressed in multiple compatible units, conversion must use the shared measurement contract.

The schema must not treat a consumer Good Unit or consumption interval as the physical unit used to store or transport stock.

## 4. Domain Ownership and Schema Boundaries

| Domain | Authoritative entities and state | Must not own |
| World Map | Geographic feature identity, geometry, spatial queries and spatial reference | Population totals, demographic state, economic or political control state |
| Population | Civilian assignments, demographic cohorts and flows, composition, needs/fulfilment state, derived labour availability, wellbeing and interest profiles | Geometry, financial ledgers, employment contracts, physical stock, military personnel, political organisations/platforms |
|---|---|---|
| Good and Service Catalogue | Product identities, classification, canonical units, product attributes, lifecycle validity | Market prices, stock quantities, production outcomes |
| Industry Data | Industry Types, reusable Industry configurations, establishment profiles and prerequisites | Live Company finances, physical capacity, actual production, stock |
| Economy | Companies' financial state, economic transactions, prices, obligations, Industry Scale | Nation Treasury authority, physical stock quantities, production execution |
| Government | Nation Treasury, policy, permissions, establishment authorisation | Company Capital, physical stock, recipe calculations |
| Production Processes and Recipes | Process definitions, recipe identities and immutable versions, quantitative transformation rules | Stock mutations, financial postings, facility capacity |
| Production | Facilities, production lines, capacity allocation, Production Orders, Production Executions and outcomes | Physical stock ledger, canonical product definitions, Company accounts |
| Inventory | Stock holdings, lots, reservations, stock transactions and in-transit stock representation | Recipe calculations, production scheduling, financial balances |
| Logistics and Transportation | Transport capacity, route and movement execution, shipment and delivery state | Product identity, Company accounts, duplicate stock ledger |
| Shared simulation engine | Deterministic phase order, command execution coordination, save/load and simulation-time progression | Domain-owned rules or duplicate domain state |

These are ownership boundaries, not requirements for separate database servers. The first implementation may store multiple domains in one PostgreSQL database while preserving logical ownership through schema namespaces, foreign keys, service interfaces, and transaction contracts.

## 5. Core Entity Map

The following is the initial logical entity inventory. It is not yet a complete SQL migration. Field-level schemas and constraints are defined in the domain schema documents that follow.

### 5.1 Catalogue

- `catalogue_entry`: canonical identity, name, description, physical form, primary economic class, category, lifecycle state, unit reference, validity dates, optional parent entry, and supported metadata.
- `catalogue_alias`: alternate names, uniquely constrained according to the Catalogue's alias rules.
- `catalogue_classification`: additional controlled classifications.
- `catalogue_designation`: additional designations, including Strategic Resource.
- `catalogue_product_attributes` or class-specific extension tables: typed attributes for Goods, Services, Designed Components, and Armaments.
- `measurement_unit` and `unit_conversion`: canonical units, dimensions, compatible conversions, precision, and conversion provenance, subject to the shared measurement specification.

Catalogue entries remain the canonical identity for every product reference. The four primary economic classes remain mutually exclusive; Strategic Resource is an additional designation rather than a primary class.

### 5.2 Industry Data and Economy

- `industry_type`: the 16 canonical Industry Types and their stable identifiers.
- `industry_configuration`: reusable rules and permitted activities for a type or configuration version.
- `industry_establishment_profile`: establishment expenditure, Opening Company Capital requirement, and applicable prerequisites as separate concepts.
- `industry_instance`: the established Industry identity and references to its type, Company, Nation, geographic area, and configuration.
- `company`: the economic entity that owns Company Capital and financial records.
- `company_capital_account` or equivalent Economy-owned account records: Company Capital and its auditable movements.
- `nation_treasury_account` or equivalent Government-owned account records: public funds and authorised Treasury movements.
- `financial_transaction`: balanced or otherwise explicitly validated monetary postings, with transaction type, simulation date, currency convention, references, and idempotency control.
- `financial_obligation`: an outstanding contractual or economic obligation, distinct from physical fulfilment and settlement.
- `price_observation` or authoritative price-state tables: Base Price and other explicitly modelled prices, with a canonical Catalogue reference and denominator unit.

Industry Scale belongs to the live economic state of an Industry and must not be represented as Production Capacity. Establishment expenditure and Opening Company Capital must remain distinct, auditable movements. Government authority over Treasury must not make Treasury a Company-owned account.

### 5.3 Production Processes and Recipes

- `production_process`: stable transformation identity, capabilities, process category, applicable Industry Types, and technical constraints.
- `recipe`: stable logical identity for a recipe.
- `recipe_version`: immutable, versioned quantitative and technical definition linked to one process.
- `recipe_input`: one row per input requirement in a specific recipe version, including Catalogue reference, quantity, unit, role, consumption mode, and constraints.
- `recipe_output`: one row per output in a specific recipe version, including Catalogue reference where applicable, output role, quantity/yield definition, unit, and applicable conditions.
- `recipe_requirement`: normalised technology, capability, equipment, or operating requirements where these require independent identity or relationships.

A recipe version is immutable once used by a committed execution. A change to simulation-relevant transformation behaviour creates a new version. Repeating inputs or outputs must be represented by related rows rather than embedded arrays.

### 5.4 Production

- `production_facility`: Industry association, facility type, location, ownership/operator references, operational state, and nominal capacity definition.
- `production_line`: optional facility substructure where the simulation models production lines.
- `facility_process_capability`: many-to-many relationship between facilities and supported processes/capabilities.
- `production_order`: requesting Industry, selected process and recipe version, target output or production basis, requested quantity and unit, priority, state, schedule constraints, and progress fields.
- `production_order_reservation`: relationship between a Production Order and Inventory Reservations. Do not store an unvalidated list of reservation IDs as the only relationship.
- `production_execution`: one committed or attempted execution, including stable execution identifier, order, facility, exact recipe version, simulation interval, outcome, and deterministic input/configuration references.
- `production_execution_input`: the execution's resolved input quantities and links to authoritative Inventory transactions.
- `production_execution_output`: one row per output, with its own `catalogue_id`, unit, quantity, output role, and execution relationship; includes the Inventory transaction reference when a physical output is registered.
- `production_execution_commit` or equivalent idempotency record: execution key, payload fingerprint, transaction state, and committed result reference.

For a multi-output recipe, the order's requested quantity applies to its explicitly identified target output or production basis. It does not mean every output is produced in that same quantity. The execution output records preserve the actual quantities of all outputs.

### 5.5 Inventory

- `inventory_holding`: a product's physical stock context at a location and under ownership/custody.
- `stock_lot`: a quantity of a Catalogue product with shared location, ownership, condition, quality, origin, and restrictions.
- `inventory_reservation`: a claim against eligible stock with purpose, quantity, status, and lifecycle.
- `inventory_transaction`: auditable transaction header, including transaction type, simulation time, initiating authority/reference, idempotency key, and commit state.
- `inventory_transaction_line`: one row per product/lot/quantity/unit movement within a transaction, allowing multi-input and multi-output production commits.
- `inventory_transit` or equivalent movement state: explicit in-transit representation where a shipment is not yet received at its destination.

Inventory fields use `catalogue_id`, not `good_id`. Only eligible physical products may be held as stock. Services and non-physical deliverables must use appropriate service-delivery records instead. On-hand, reserved, and available quantities must be derivable and reconcilable from authoritative lots and active reservations.

### 5.6 Shared Execution and Events

- `simulation_command` or equivalent command record: an authorised requested state transition, with deterministic ordering and idempotency where required.
- `domain_event` or equivalent event record: committed outcomes published for dependent systems.
- `transaction_idempotency`: stable key, request/payload fingerprint, outcome, and reference to the committed result where a domain needs an explicit deduplication ledger.

These entities must support deterministic processing and recovery without becoming a second authoritative copy of domain state. Exact scheduler, event delivery, and cross-domain transaction implementation remain subject to the shared Technical Specification.

## 6. Required Relationships

At minimum, the relational model must enforce or validate these relationships:

1. Each `recipe_version` references exactly one logical `recipe` and one `production_process`.
2. Each recipe input/output line references a valid Catalogue entry when it represents a catalogue-defined product or service, and carries a unit compatible with that entry and the recipe basis.
3. Each Production Order references a valid process and exact recipe version. Its target output or production basis and requested unit must be valid for that version.
4. Each Production Execution references its originating order, exact recipe version, and the facility/line used where applicable.
5. Each execution input/output row belongs to exactly one execution. Each physical output row references the Catalogue entry and unit used, and links to the authoritative Inventory transaction that registers it.
6. Each Inventory Reservation references a holding and/or lot, a Catalogue identity consistent with that stock, a quantity and unit, and its requesting purpose.
7. Each Inventory Transaction Line references a valid Catalogue entry and eligible physical stock item for stock-changing movements. Service delivery cannot masquerade as physical stock.
8. Each Industry references a canonical Industry Type and a Company, with valid Nation and geographic references.
9. Financial transactions reference the appropriate Economy-owned accounts and do not directly mutate Inventory.
10. Treasury postings require Government-authorised operations. Company Capital postings remain Economy-owned.
11. Industry establishment is committed atomically with its required financial movements and authorisation; scenario-loaded starting Industries are not charged establishment costs again.
12. Every idempotent transaction key is unique within its declared scope, and a repeated key with a different payload is rejected.

## 7. Critical Integrity Invariants

### 7.1 Inventory

- Quantities cannot be negative unless a specific, explicitly documented domain mechanic permits it.
- Reserved quantity cannot exceed eligible on-hand quantity.
- Available quantity is consistent with on-hand quantity minus active reservations, subject to explicitly defined exclusions.
- A physical quantity cannot be simultaneously available at its origin and destination during transit.
- A sale, transfer, shipment, or receipt changes physical stock only at the authoritative lifecycle point and exactly once.
- A committed Production Execution cannot consume inputs or register outputs twice.

### 7.2 Production

- A committed execution references the immutable recipe version actually used.
- Input and output quantities are stored with compatible, explicit units.
- Each output has an independent Catalogue identity, quantity, unit, and execution relationship.
- The order's completion quantity is measured against its target output or explicit production basis, not the sum of unrelated co-products.
- Capacity and Industry Scale remain distinct.
- A failed or pending execution cannot be reported as successfully committed.

### 7.3 Economy

- Monetary transactions and physical stock movements remain separate but may reference the same commercial or operational cause.
- Company Capital and Nation Treasury remain separate.
- Establishment expenditure and Opening Company Capital remain separate.
- Monetary amounts must identify the applicable currency convention. Project Sovereign's initial global currency is Dollar (`DOLLAR`); no foreign-exchange subsystem is implied.
- Outstanding obligations, payments, deliveries, and receipts must not be treated as interchangeable states.

### 7.4 Historical Reproducibility

- Recipe version, relevant process configuration, simulation interval, and resolved execution inputs/outputs must be retained for committed production.
- Changes to Catalogue definitions, recipe versions, facility capabilities, or industry configuration must not silently rewrite committed historical outcomes.
- Save/load must preserve pending transaction state and deduplication data needed to avoid duplicate commits after reload.

## 8. Shared Production Execution Transaction

Production and Inventory share one logical Production Execution transaction for committing physical production outcomes.

The transaction must:

1. Validate the stable execution identifier and idempotency key.
2. Resolve the exact order, immutable recipe version, target output/production basis, and deterministic execution inputs.
3. Validate all required input reservations or explicitly supported allocation records.
4. Calculate and validate the complete set of input quantities, physical outputs, by-products, recoveries, tracked waste, and losses.
5. Commit authorised input consumption and all physical output registrations as one atomic logical operation.
6. Record execution input/output details and references to authoritative Inventory transactions.
7. Update order progress consistently with the committed execution.
8. Publish success events only after commitment.

A retry with the same key and same payload resolves to the same result. A retry with the same key and a different payload is rejected. If the underlying implementation cannot use a single database transaction, it must provide an equivalent durable prepare/commit/recovery mechanism and must not report success while stock state is unresolved.

This contract defines required outcomes, not a prescribed database transaction technology.

## 9. Schema Implementation Sequence

Build and validate schemas in dependency order, respecting the agreed project workflow:

1. **Shared schema conventions and world references.** Confirm identifiers, simulation time, units, canonical World Map feature references and spatial identity.
2. **Population schema.** Assignments, cohort state, event ledger, migration/displacement, composition, spatial distribution, labour/education interfaces, needs/indicators and interest-profile contracts. Validate its boundaries against the Population System specification.
3. **Shared measurement and Catalogue schema.** Canonical product identity, class constraints, unit references, conversions, product attributes and lifecycle validity.
4. **Industry and Company identity schema.** Industry Types/configuration, Companies, Industry instances, establishment profiles, and the separation between Company Capital and Treasury.
5. **Process and recipe schema.** Processes, immutable recipe versions, input/output lines, units, yields and constraints.
6. **Production schema.** Facilities, lines, capabilities, orders, target output/basis, executions and per-output execution records.
7. **Inventory schema.** Holdings, lots, reservations, transaction headers and multi-line stock movements.
8. **Shared Production Execution transaction contract.** Idempotency, atomic commit, failure recovery, output registration and execution linkage across Production and Inventory.
9. **Economy transaction schema.** Accounts, financial transactions, prices, obligations, establishment postings, and links to commercial/operational references.
10. **Cross-domain integrity and migration tests.** Referential integrity, unit compatibility, duplicate retries, save/load recovery, multi-output production, starting-world initialisation, and deterministic replay.

The detailed Population schema is maintained in [population-schema.md](population-schema.md). The sequence above records the current project dependency order; economic/resource schema reconciliation remains subsequent to Population review.

Each schema stage must include constraints, indexes, lifecycle rules, and validation tests before it is treated as implementation-ready.

## 10. Open Decisions That Must Not Be Guessed in Migrations

The following need explicit decisions in the Technical Specification or relevant shared contracts before final SQL migrations:

- The canonical storage representation for simulation dates and intervals.
- The canonical measurement-unit and conversion registry, including precision and rounding.
- Identifier type and generation strategy, applied consistently across all tables.
- Whether domain modules share one PostgreSQL transaction boundary or use a durable cross-domain commit protocol.
- Exact event ordering, simulation scheduler phases, and command idempotency scope.
- Which flexible attributes require typed relational columns versus validated JSONB.
- The final account model and double-entry requirements for Economy, if applicable.
- The canonical representation of in-transit stock and partial delivery.
- The policy for retaining historical Catalogue and configuration versions.

An unresolved decision must remain explicitly unresolved. It must not be hidden in an arbitrary column type, default value, implicit conversion, or application assumption.

## 11. Acceptance Criteria

The schema foundation is acceptable when:

- Every authoritative entity has one owning domain.
- Every cross-domain relationship uses a stable identifier and explicit validation.
- Product references consistently use `catalogue_id`.
- Physical stock cannot represent a non-physical Service.
- Recipe inputs and outputs support multiple rows and explicit units.
- Multi-output execution can persist each output independently and link all outputs to one Production Execution.
- Production/Inventory retries cannot duplicate committed stock changes.
- Company Capital and Nation Treasury cannot be conflated.
- Industry Scale cannot be mistaken for physical capacity.
- Simulation date rules and canonical units are explicit before migrations depend on them.
- Historical production outcomes remain reproducible after definition changes.
- All constraints can be tested without relying on presentation-layer behaviour.

## Governing Principle

**The database must encode the ownership and invariants of the simulation, not replace them with duplicated state.** Each domain owns its truth, each relationship is explicit, each quantity has a defined unit, and each committed physical or financial change is auditable and idempotent.

## 12. World and Spatial Schema Boundary

The world-data schema is defined in `technical/data-schemas/world-data-schema.md`. It establishes the logical model for canonical world geometry, Terrain Objects and their state references, the 100 m Movement Grid as a derived analysis layer, political boundaries and territorial state, Population Centre geometry references, installations, facilities, and visual-asset references.

The world-data schema must be implemented alongside, but not conflated with, the Catalogue, Production, Inventory, and Economy schemas. Its principal cross-domain rules are:

- World Map owns coordinates, canonical geometry, spatial indexes, and geometry versioning.
- Terrain Objects owns terrain definitions and supported state semantics.
- National Territory and Control owns political meaning, claims, effective territorial control, occupation, and frontline derivation.
- Population owns population and demographic state, while geographic records own only spatial identity and geometry references.
- Installations and Facilities owns site composition, capabilities, capacity definitions, condition, and ownership/operator references.
- Economy and corporate domains own legal economic ownership and financial truth. Military Systems own military unit identity, organisation, equipment/personnel state, orders, and readiness according to explicit subsystem boundaries. Movement owns authoritative continuous positions and route traversal. Inventory owns physical stock; Logistics owns transport and delivery.
- The renderer owns no authoritative simulation state. Labels, textures, flags, frontlines, spatial indexes, and Movement Grid values may be derived or cached only when they can be rebuilt from authoritative records.

The Movement Grid must not become a discrete movement model or require one persistent database record per 100 m cell without performance evidence. Unit positions remain continuous geographic positions.

Political sovereignty, national claims, effective control, occupation, and frontlines must be separately represented. A frontline can move without a national border changing; occupation does not automatically transfer sovereignty; and capturing a facility does not automatically transfer its company ownership, stock, or serviceability.

### 12.1 Revised Implementation Dependency Order

The implementation sequence must respect domain dependencies rather than assume all schemas can be built in isolation:

1. Shared identifier, time, unit, and historical-validity conventions.
2. Canonical Catalogue definitions and unit conventions.
3. World geometry and spatial-query foundations.
4. Terrain definitions and runtime terrain-state references.
5. Nation, boundary, territorial claim, control, occupation, and frontline data contracts.
6. Installation, facility, capability, capacity, and ownership/operator relationships.
7. Population-centre geometry references and the separate Population state schema.
8. Production, Inventory, Economy, and Logistics contracts that reference catalogue entries, facilities, and locations.
9. Derived Movement Grid, spatial indexes, frontline geometry, label placement, and rendering caches.
10. Runtime persistence, event history, validation, and integration tests.

This is a dependency order, not a requirement to complete every system before prototyping. Temporary adapters are permitted, but must not establish duplicate authoritative state.
