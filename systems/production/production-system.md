
# Production System

## 1. Purpose

The Production System defines how industries use physical facilities, productive capacity, available inputs, labour, energy, technology and infrastructure to execute production activities and generate physical outputs.

It establishes the operational rules governing production facilities, production lines, productive capacity, production orders, scheduling, operating constraints, execution records and the results of production.

Production operates against the Industry model established in `docs/systems/economy/industry-data.md`. It does not independently define Industry Types, Industry instances, Company relationships, Industry Scale or Industry establishment requirements.

Production is a physical simulation. An Industry may possess substantial economic importance, financing or intended output without possessing the facilities, inputs, workforce or infrastructure required to produce at that level.

The system must distinguish economic intent from physical capability, physical capability from actual output, and production completion from the subsequent storage, transportation, sale or delivery of outputs.

### 1.1 Design Principles

1. **Physical capability governs output.** Production is constrained by facilities, installed capacity, available inputs, workforce, utilities, technology and operating conditions.
2. **Industry and production are distinct.** An Industry is an economic entity. A Production Facility is a physical asset or operational installation through which production occurs.
3. **Capacity is not output.** Nominal capacity describes potential throughput under defined operating conditions, not guaranteed production.
4. **Production consumes real inputs.** Required physical inputs must be available and validly allocated before they can be consumed.
5. **Inventory owns physical stock.** Production requests authorised stock changes through Inventory and does not maintain a competing quantity ledger.
6. **Transformation rules are externalised.** Production executes a validated process and recipe version defined by Production Processes and Recipes.
7. **Ownership remains explicit.** Ownership, custody, location and operational control are distinct relationships.
8. **Location matters.** Facilities operate at physical locations and are subject to applicable infrastructure, terrain, accessibility, environmental and legal constraints.
9. **Technology modifies capability through explicit rules.** Research availability does not automatically install or activate technology at a facility.
10. **Production is deterministic.** Identical authoritative state, orders, configuration versions and deterministic inputs must produce identical outcomes.
11. **Financial and physical outcomes are distinct.** Production results may inform Economy, but do not independently determine prices, revenue, expenditure or solvency.
12. **Execution is auditable and atomic.** A production execution must not leave unintended partial consumption or duplicated output after a failure or retry.

---

## 2. System Boundaries

The Production System is authoritative for the physical execution of production.

It is not the authority for every economic, geographic, logistical or organisational activity associated with production.

| System | Responsibility | Relationship to Production |
|---|---|---|
| Economy | Economic entities, financial resources, expenditure, revenue, transactions and economic decisions | Provides economic intent and records financial consequences |
| Industry Data | Industry Types, Industry instances, Company relationships, Industry Scale, establishment configuration, Industry Areas, workforce requirements and starting-world Industry data | Defines the Industries for which productive activity may be configured |
| Production | Production-specific facility capability mappings, process throughput, production lines, capacity calculations and allocation, production orders, scheduling, operating constraints and execution | References canonical Facility identity and shared physical/capacity state; determines and executes production |
| Production Processes and Recipes | Transformation definitions, recipe versions, input/output quantities, yields, by-products, waste and process-level requirements | Defines the transformation executed by Production |
| Inventory | Physical stock, quantities, locations, condition, ownership, reservations, transfers, delivery and consumption | Supplies inputs and records resulting physical stock changes |
| Good and Service Catalogue | Canonical product identities, classifications, attributes and units | Defines the Goods and Services referenced by Production |
| Research | Technologies, designs, research progression and technology availability | Defines technological prerequisites and available knowledge |
| Terrain and Geography | Physical locations, terrain, elevation, infrastructure geography and spatial relationships | Supplies location and environmental context |
| Government and Law | Permissions, policy, regulation, restrictions and mandates | Determines whether activities are legally permitted |
| Workforce and Population | Population, labour availability, skills, employment and labour allocation, where implemented | Supplies workforce availability and constraints |
| Logistics and Transportation | Goods movement, transportation capacity, routes, dispatch and delivery | Determines whether inputs and outputs can be transported |
| Military and Organisations | Military requirements, procurement, acceptance and operational demand | May originate production requirements or receive outputs |
| Construction and Infrastructure | Construction projects, infrastructure assets, commissioning and upgrades | Supplies the rules for creating or modifying physical productive assets |

Where a supporting system is not fully specified, Production must use a defined interface or an explicitly documented placeholder. It must not create a competing authoritative implementation of that system.

### 2.1 Authoritative Ownership Rules

The following distinctions are mandatory:

- Industry Scale does not automatically determine physical Production Capacity.
- Production Capacity does not represent the quantity of Goods currently in Inventory.
- A production order does not prove that its required inputs have been delivered.
- A production facility does not establish ownership of the Goods stored within it.
- Production completion does not automatically establish delivery to a customer.
- A Company's financial ability to purchase inputs does not guarantee that those inputs physically exist or are accessible.
- A research discovery does not automatically install or activate a production capability.
- A facility's location does not automatically confer legal permission, infrastructure access or territorial control.
- Planned production is not completed production.
- Economic demand is not equivalent to a valid, executable production order.
- A recipe calculation does not itself consume stock or create Inventory.
- A successful production execution does not itself establish a sale, payment or delivery.
- Production must not directly alter Company Capital, Nation Treasury or other financial balances.
- Inventory must not independently infer production merely because a production order is completed.

These rules prevent one system's intent or derived calculations from being mistaken for an authoritative physical or financial event.

---

## 3. Core Concepts

### 3.1 Industry

An Industry is an economic entity defined by Industry Data.

An Industry may be associated with a Company, have a defined Industry Type, operate at a particular Industry Scale, employ a workforce, possess financial relationships and maintain one or more Industry Areas.

Production references Industry instances using stable identifiers.

Production must not redefine Industry identity, Industry Type, Company relationships or Industry Scale.

### 3.2 Production Facility

A Production Facility, in Production terminology, is a canonical Facility that has a Production-owned capability extension enabling one or more production activities. The Installations and Facilities System owns the underlying Facility identity, type, name, location, shared condition, ownership/operator/control relationships and common facility capabilities/capacity definitions. Production must reference that identity rather than create a competing facility record.

Production owns the mapping from a Facility to supported production processes, production-specific throughput bases and calculations, line-level production configuration, capacity allocation, orders and execution state. A process-specific throughput value is not a duplicate of generic facility capacity. Production eligibility is distinct from the Facility's shared physical condition and lifecycle state.

Examples include:

- Farms and agricultural installations.
- Mines and extraction installations.
- Oil wells and refineries.
- Power stations.
- Sawmills and timber-processing plants.
- Textile mills.
- Machine shops and engineering works.
- Steelworks and foundries.
- Chemical plants.
- Vehicle assembly plants.
- Shipyards.
- Aircraft factories.
- Workshops and repair installations.

A Production Facility reference must resolve to a canonical Facility with a stable identifier and physical location. Production queries its authoritative lifecycle and physical condition and associates its own defined productive capability with that identity.

A facility may contain several production lines or process-capable units. Conversely, a process may require several cooperating facilities where its configuration explicitly supports that arrangement.

One Industry does not necessarily correspond to one facility. A facility must not be assumed to represent an entire Industry.

### 3.3 Production Line

A Production Line is an operational unit within a facility that executes a defined production process or compatible family of processes.

A line has its own applicable capacity, operating state, process compatibility, scheduling constraints and utilisation.

A line may be unavailable while its parent facility remains operational. Examples include maintenance, equipment failure, missing specialist labour or a lack of a required utility.

Where line-level simulation is unnecessary, a facility may expose one implicit production line. This simplification must preserve the distinction between facility capacity and process-level throughput.

### 3.4 Production Process

A Production Process defines a category of transformation from one or more inputs into one or more outputs.

The quantitative transformation is defined by `docs/systems/production/production-processes-and-recipes.md`.

Production executes a validated process and recipe version but does not independently author, redefine or calculate an alternative recipe.

### 3.5 Production Order

A Production Order is an authorised instruction to produce specified outputs through an eligible process at a designated facility or production line, or through a valid facility-selection rule.

It defines the requested activity and operational parameters.

A production order is not itself a financial transaction, inventory transfer or completed production event.

Orders must reference valid Industries, facilities, processes and output definitions.

### 3.6 Productive Capacity

Productive Capacity represents the maximum throughput a facility, production line or other explicitly defined productive unit can achieve over a specified interval under stated operating conditions.

Capacity must be associated with:

- A measurable output or process basis.
- A canonical unit or compatible unit definition.
- A time interval or rate basis.
- A productive facility or line.
- Applicable operating conditions.

For example, a facility may have a nominal capacity of 100 tonnes of a specified output per day. This does not imply that 100 tonnes will actually be produced on any given day.

### 3.7 Industry Area

An Industry Area is the spatial extent or operational area associated with an Industry as defined by Industry Data.

An Industry Area and a Production Facility are distinct concepts.

An Industry Area may represent an agricultural region, mining concession, industrial district, group of extraction sites or another geographically defined economic area.

A facility may be situated inside an Industry Area, serve several Industry Areas or operate outside an Industry Area where the Industry configuration permits it.

An Industry Area does not automatically constitute a production facility or confer productive capacity.

### 3.8 Production Execution

A Production Execution is a discrete, identifiable attempt to execute an authorised production order against a validated process and recipe version.

It records the physical transformation attempt, its inputs, outputs, losses, capacity usage, execution interval and outcome.

Each execution must have a stable identifier to support auditability, recovery and duplicate-execution prevention.

### 3.9 Physical Output and Non-Physical Results

Physical Goods produced by a process must be registered through Inventory.

A Service or other non-physical result must be handled through the relevant service, delivery or operational interface. It must not be inserted into physical Inventory merely because it is an output of a process.

The Good and Service Catalogue determines product classification and physical form.

---

## 4. Production System Data Model

All persistent production entities must have stable identifiers. Relationships must use identifiers rather than display names.

The following is the logical data model. Exact storage formats, programming-language types and serialisation structures are implementation decisions unless otherwise specified.

### 4.1 Production Facility Capability Extension

The canonical Facility record is owned by the Installations and Facilities System. Production maintains a capability extension keyed by the same `facility_id`, which is both the extension's primary key and a foreign key to the canonical Facility. Production must not duplicate the Facility's type, name, location, ownership, operator, shared physical condition, lifecycle state or generic capacity definitions.

The Production-owned extension and related capability associations may reference:

| Field or association | Purpose |
|---|---|
| `facility_id` | Canonical Facility ID; primary/foreign key, not a separately generated identity |
| `industry_id` or normalised Industry association | Economic/operational Industry relationship where applicable; use a many-to-many association if needed |
| `industry_area_id` | Optional Industry Area reference |
| `production_eligibility_state` | Production-specific eligibility, distinct from shared Facility condition |
| `facility_process_capability` | Supported Production Processes, configuration state, validity and process-specific nominal throughput basis |
| `production_requirements` | References to technology, workforce, utilities, infrastructure and operating prerequisites |
| `created_simulation_time` | Simulation time at which the Production extension was created |
| `last_updated_simulation_time` | Simulation time of the latest Production-owned change |

A process-specific throughput definition must identify the process or compatible process family, output/process basis, canonical unit, time basis, applicable conditions and configuration version. It is not generic Facility capacity, Effective Throughput, Allocated Throughput or Actual Output. Production calculates effective throughput from the configured capability and authoritative constraints. If independently scheduled production lines are modelled, line-specific throughput and eligibility are represented on the Production Line record.

A canonical Facility may serve more than one Industry. If that relationship is supported, represent it as a normalised association rather than overwriting a single authoritative Industry reference.

### 4.2 Production Line Record

A Production Line should support:

- `production_line_id`
- `facility_id` (canonical Facility reference)
- `production_eligibility_state`
- `supported_processes`
- `configured_nominal_throughput` and its process/output basis, canonical unit and time basis, only where line-level throughput is independently modelled
- `current_process_id`, if assigned
- `current_order_id`, if assigned
- `maintenance_state`
- `effective_throughput`
- `last_updated_at`

Effective throughput is a derived value determined by process-specific nominal throughput and applicable operating constraints. If stored for performance, it must be treated as a derived value and kept consistent with its authoritative inputs.

### 4.3 Production Order Record

A Production Order must support:

| Field | Purpose |
|---|---|
| `production_order_id` | Unique, stable order identifier |
| `industry_id` | Industry requesting or responsible for production |
| `facility_id` | Assigned facility, if selected |
| `production_line_id` | Optional assigned production line |
| `process_id` | Process to execute |
| `recipe_id` | Selected recipe |
| `recipe_version_id` | Exact recipe version selected for execution |
| `target_output_catalogue_id` | Canonical Catalogue identifier of the output whose quantity defines order completion, when the order targets a specific output |
| `production_basis` | Explicit basis for the requested quantity, such as a target output or a recipe-defined input/throughput basis |
| `output_specification` | Required output specification, including the target output or production basis and any applicable constraints; it does not replace the recipe's complete output calculation |
| `requested_quantity` | Requested quantity expressed in the unit belonging to the explicitly identified target output or production basis |
| `requested_quantity_unit` | Canonical unit compatible with the Catalogue entry for the target output or the defined production basis |
| `priority` | Order priority under the applicable scheduling policy |
| `order_state` | Current execution state |
| `created_at` | Simulation time at which the order was created |
| `earliest_start_time` | Earliest permitted execution time |
| `deadline` | Optional requested completion time |
| `input_reservation_ids` | References to valid Inventory reservations, where applicable |
| `allocated_throughput` | Capacity assigned to the order |
| `completed_quantity` | Quantity successfully produced |
| `rejected_quantity` | Quantity rejected or recorded as unusable, where quality simulation is supported |
| `cancelled_quantity` | Outstanding quantity cancelled |
| `failure_reason` | Structured reason for a blocked, failed or cancelled order |
| `last_updated_at` | Simulation time of the most recent order update |

The order must explicitly identify the basis to which `requested_quantity` applies. Where the order targets a specific output, it must identify that output by its canonical Catalogue identifier and compatible unit. Where the order instead uses a recipe-defined input or throughput basis, that basis and its unit must be explicit and defined by the selected recipe.

Requested, completed, rejected and cancelled quantities must use compatible units for the same target output or production basis. Quantities belonging to different outputs must never be compared or added as if they shared a unit or completion basis.

The selected recipe version and Production Execution determine the quantities of all associated outputs. Each output record must retain its own canonical `catalogue_id`, unit, quantity, output role where applicable, and relationship to the Production Execution that produced it. Co-products, by-products, recoveries, waste and losses must not be inferred from the requested target quantity alone.

### 4.4 Production Execution Record

A Production Execution must record:

- A stable execution identifier.
- The originating production order.
- The executing facility and production line, where applicable.
- The process definition and exact recipe version used.
- The simulation interval during which execution occurred.
- The effective production rate and capacity allocated.
- The input quantities requested for consumption.
- The authoritative Inventory consumption transaction references.
- The outputs calculated by the process.
- The authoritative Inventory output-registration transaction references.
- Any by-products, waste, losses or rejected outputs.
- The execution result.
- Any operational constraints that affected the result.
- The deterministic inputs or configuration references required to reproduce the outcome.

Execution records must allow the simulation to distinguish an order being scheduled from an order actually being executed.

### 4.5 Authoritative and Derived Fields

Persistent records may store derived values for performance, but must identify their authoritative source.

Examples include Effective Throughput, utilisation, remaining order quantity and derived production statistics.

A derived value must not become a competing source of truth. Recalculation must follow the owning specification's rules and must not change completed historical outcomes.

---

## 5. Production Capability Eligibility Lifecycle

The Installations and Facilities System owns the canonical Facility lifecycle and shared physical condition. Production must consume those authoritative states and must not create a second facility lifecycle. This section governs the eligibility of a configured Production capability, not the existence or physical state of the Facility itself.

### 5.1 Production Capability States

| State | Meaning |
|---|---|
| `configured` | A production capability has been defined, but current eligibility has not necessarily been established |
| `eligible` | The capability may be scheduled, subject to order, input, capacity and other checks |
| `restricted` | The capability may execute only under explicit constraints |
| `suspended` | Production through this capability is temporarily prohibited |
| `retired` | The capability configuration is no longer available for new orders; historical executions remain valid |

Transitions must be validated against the current capability state and the rules governing the transition. These states do not replace shared Facility condition, construction/commissioning state, damage, maintenance or decommissioning state.

A production capability cannot execute merely because its record exists. Production must validate the canonical Facility's lifecycle and condition, the configured capability's state, and all applicable prerequisites before scheduling or committing execution.

### 5.2 Establishment and Commissioning

Industry establishment and financing are governed by Industry Data and Economy.

Production consumes the resulting Industry and facility configuration.

Where construction is simulated, a facility must not become operational until required construction, installation, commissioning and other prerequisites have been satisfied.

Construction may require materials, equipment, labour, capital expenditure and access to a construction site. These are distinct from the facility's subsequent production requirements.

A construction project may itself require production orders for construction materials or equipment. Authorising construction does not prove that those Goods physically exist.

Detailed construction costs, construction material consumption, commissioning and facility upgrades belong to the relevant construction and economic specifications.

### 5.3 Suspension and Decommissioning

A facility may be suspended or constrained because of:

- Missing or insufficient inputs.
- Insufficient workforce.
- Equipment failure or maintenance.
- Utility interruption.
- Infrastructure failure.
- Environmental conditions.
- Legal or governmental restrictions.
- Damage caused by accidents, disasters or military action.
- Loss of required access.
- A lack of valid production orders.

These conditions may reduce output without requiring a permanent lifecycle transition.

A temporary shortage of inputs should normally block or reduce execution rather than automatically decommission the facility.

Decommissioning must not automatically delete historical execution records, financial records or inventory. The relevant systems retain authority over their own records.

---

## 6. Capacity Model

### 6.1 Capacity Definitions

The Production System distinguishes between the following measures. Capacity in this section means production-specific throughput for a declared process or compatible process family and a declared output/process basis. It does not redefine or duplicate the shared generic capacity definitions owned by the Installations and Facilities System.

**Configured Nominal Throughput:** The nominal production rate configured for a particular process/output basis at a canonical Facility or Production Line, expressed in a compatible unit over an explicit interval. This is the Production-owned capacity input, not a universal capacity value for the whole Facility.

**Available Throughput:** The portion of configured nominal throughput that remains usable after applying authoritative Facility/line condition, maintenance and other applicable availability constraints.

**Effective Throughput:** The productive rate usable for the selected process and simulation interval after accounting for process compatibility, workforce, utilities, technology and applicable operating constraints.

**Allocated Throughput:** The portion of effective throughput assigned to a production order for the relevant interval.

**Actual Output:** The quantity physically produced by a successful execution, classified according to the process's output definitions and execution results.

These measures must not be treated as interchangeable. Nominal, available, effective and allocated throughput must retain their process/output basis, unit, time basis and relevant configuration version.

### 6.2 Effective Throughput

Effective throughput is derived from the configured process-specific nominal throughput and all applicable constraints.

Conceptually:

`Effective Throughput = Configured Nominal Throughput × Availability × Applicable Modifiers`

This expression illustrates the relationship rather than prescribing a universal numerical algorithm. The precise treatment of simultaneous constraints, non-linear bottlenecks and process-specific operating limits must be defined by the applicable facility and process rules.

Capacity modifiers may include:

- Maintenance downtime.
- Equipment damage.
- Workforce availability and qualification.
- Power and fuel availability.
- Water or other utility availability.
- Environmental conditions.
- Technology and equipment efficiency.
- Legal operating restrictions.
- Infrastructure limitations.
- Facility-specific process compatibility.

The same constraint must not be applied twice when it is already reflected in another authoritative capacity value.

For example, if maintenance has already reduced Available Throughput, a separate maintenance penalty must not be applied again to the same calculation.

### 6.3 Capacity Units

Capacity must be measured in units appropriate to the process.

Examples include:

- Tonnes per day.
- Cubic metres per day.
- Units per day.
- Passenger-kilometres per day for an appropriately defined service process.
- Megawatt-hours per day for energy generation.
- Work-hours per day for a process whose capacity is defined in labour terms.

The unit must be explicit and compatible with the process and output definitions. Canonical units and permitted conversions are defined by the Good and Service Catalogue and applicable shared unit conventions.

Quantities measured in incompatible units must not be compared or combined without a valid conversion.

Capacity must distinguish flow rates from stock quantities. Tonnes per day describe throughput; tonnes describe a physical quantity.

### 6.4 Capacity Allocation

Capacity allocation determines how available productive capability is distributed among eligible production orders.

Allocation must respect:

1. Capacity available during the relevant simulation interval.
2. Process and facility compatibility.
3. Priority and scheduling rules.
4. Workforce and other operational constraints.
5. Process-specific batch, minimum-run or setup requirements.
6. Time required to complete an execution.
7. Any applicable legal or operational restrictions.

An order cannot receive more capacity than its assigned facility or line can provide during the relevant interval.

Where orders compete for capacity, Production applies a deterministic scheduling policy. Results must not depend on incidental iteration order, machine timing or data-loading order.

The initial implementation must define a scheduling policy with explicit eligibility, priority and stable tie-breaking rules.

### 6.5 Utilisation

Utilisation describes the degree to which available productive capacity is used.

It is a derived operational statistic, not a substitute for capacity or output.

A facility may have low utilisation because it lacks orders, inputs, workforce or demand. A facility may also operate at high utilisation while producing less than nominal capacity because Effective Throughput has been reduced.

Utilisation must be calculated using compatible capacity and time units, with explicit handling of intervals in which available capacity is zero.

---

## 7. Production Inputs and Outputs

### 7.1 Inputs

Production inputs are the resources and Goods required to execute a process.

They may include:

- Raw materials.
- Intermediate Goods.
- Designed Components.
- Armaments used as inputs where explicitly defined.
- Fuel and energy represented as physical Goods.
- Water and other consumable utilities.
- Consumable materials.
- Packaging.
- Replacement parts.
- Other physical Goods required by the process.

Labour, machine time, environmental conditions and network-delivered utilities may also be required, but they are not necessarily Inventory Goods.

The exact inputs, quantities, substitution rules, yields and process-specific requirements are defined in Production Processes and Recipes.

Production must validate mandatory inputs before execution. The Inventory System is authoritative for physical availability and stock reservation.

An input is not available merely because a Company can afford it, a supplier has promised it, a shipment has been dispatched or an Industry Area contains a relevant resource.

It must be physically available and accessible under Inventory and Logistics rules.

### 7.2 Input Reservations

Where Inventory supports reservations, a production order must reference valid reservations for the required inputs before execution begins, except where the applicable process explicitly uses another supported input-allocation method.

A reservation establishes a claim on available stock under Inventory's rules. It does not consume that stock or create additional stock.

Production must not create a second, independent reservation system or modify authoritative quantities directly.

If reservations are missing, invalid, insufficient, expired or no longer accessible, the order must be blocked or adjusted according to the order and process rules.

### 7.3 Consumption

When production executes, required input quantities are consumed through Inventory's authoritative consumption interface.

Production must not independently subtract quantities from an inventory record.

Input consumption and output generation must be associated with the same Production Execution so that the transformation can be audited.

The transaction boundary must prevent an execution from consuming inputs while failing to record its corresponding physical results because of a partial state update.

The implementation may use a database transaction, a transactional event mechanism or an equivalent design. The authoritative outcome must remain consistent if execution is interrupted or retried.

### 7.4 Outputs

Production outputs may include:

- Primary Goods.
- Secondary Goods.
- Co-products.
- By-products.
- Recoverable scrap.
- Waste or unusable material.
- Energy or other process outputs represented as Goods.

Output definitions and material relationships belong to Production Processes and Recipes. Product identity and classification belong to the Good and Service Catalogue.

Production calculates the output quantities for the execution and requests their registration through Inventory.

Physical Goods must be registered at the appropriate location with valid quantity, ownership, condition and provenance references.

A completed output does not automatically imply that the Goods have been sold, delivered, exported or accepted by a customer.

Services and other non-physical outputs must be registered or reported through their appropriate interfaces, not as conventional physical Inventory.

### 7.5 Yield and Losses

Actual output may differ from theoretical output because of process yield, material loss, spoilage, quality rejection, equipment performance or other explicitly modelled conditions.

Production Processes and Recipes defines the baseline transformation, recipe quantities and applicable yields. Production applies the relevant execution conditions and records the result.

Losses must not be silently discarded. Where waste or recoverable by-products are modelled, they must be recorded through the appropriate output interface.

The system must distinguish between:

- Input material consumed.
- Usable primary output.
- Secondary output and co-products.
- Recoverable scrap.
- Waste or other non-usable material.
- Rejected output.
- Other explicitly modelled process losses.

The detailed quantitative treatment belongs to the recipe and process definitions, not a generic assumption that every input becomes one perfect output.

---

## 8. Workforce and Operational Resources

### 8.1 Workforce Requirements

Industry Data establishes Industry workforce requirements and their relationship to Industry Scale.

Production consumes the operational availability of the workforce assigned to a facility or production line.

Where workforce is divided into skills, occupations or qualification levels, a facility may require specific combinations of labour categories.

Production must distinguish between:

- Required workforce.
- Available workforce.
- Assigned workforce.
- Workforce actually available for the production interval.

The number of employees associated with an Industry does not establish that every employee is available at every facility.

Population and Economy retain authority over population, labour availability and employment allocation.

### 8.2 Workforce Shortages

A workforce shortage may reduce Effective Throughput or prevent a process from operating.

The effect depends on process requirements. A process requiring a minimum number of qualified operators may not be executable below that threshold, even when machine capacity remains available.

Production must not assume that labour can be substituted freely between roles or qualification levels.

Skill substitution, automation and productivity modifiers must be explicitly defined by the applicable process, facility or technology rules.

### 8.3 Utilities and Supporting Infrastructure

Production facilities may require electricity, fuel, water, transport access, communications or other supporting services.

Where utilities are represented as physical Goods, their availability and consumption must be reconciled with Inventory and the applicable resource-delivery systems.

Where utilities are delivered through networks, the relevant infrastructure system determines service availability and delivery constraints.

Production must not assume uninterrupted access to utilities merely because a facility is geographically connected to a network.

### 8.4 Maintenance

Facilities and production lines may require periodic maintenance or repair.

Maintenance can reduce capacity or make equipment temporarily unavailable.

Where maintenance consumes spare parts, fuel, tools or other Goods, the relevant quantities must be accounted for through Inventory.

Maintenance scheduling, deterioration, repair mechanics and equipment-specific reliability are extensible operational features. They must integrate with the core capacity model rather than introduce a competing definition of Available Throughput.

---

## 9. Production Orders and Scheduling

### 9.1 Order Creation

A production order may originate from an authorised industrial decision, Company plan, Government direction, military procurement requirement or another supported demand source.

The originating system is responsible for the intent to produce or procure Goods. Production is responsible for validating and executing the resulting order.

An order must identify:

- The responsible Industry.
- Requested outputs and quantities.
- An eligible process and recipe version, or a valid selection rule.
- An eligible facility or a rule for selecting one.
- Scheduling priority.
- Any required deadline or timing constraints.
- Applicable authorisation or restrictions.
- The source of the order or request.

An order referencing an invalid or incompatible entity must be rejected or marked invalid. It must not be silently reassigned to an unrelated facility or process.

### 9.2 Order States

The initial order lifecycle should support the following states:

| State | Meaning |
|---|---|
| `created` | The order has been recorded but not yet validated |
| `validated` | The order is structurally valid and has passed initial eligibility checks |
| `queued` | The order is awaiting capacity or scheduling |
| `awaiting_inputs` | Required inputs or reservations are not yet available |
| `scheduled` | Capacity and an execution interval have been assigned |
| `in_progress` | Production has started but is not yet complete |
| `partially_completed` | Some requested output has been completed, with outstanding quantity remaining |
| `completed` | The requested production has completed and all required output-registration operations have succeeded |
| `blocked` | An operational, legal, logistical or other constraint prevents progress |
| `cancelled` | The outstanding order has been cancelled |
| `failed` | The order or execution has failed under an explicitly defined failure condition |

A state transition must preserve the order's execution history and completed quantities.

An order must not return to an earlier state in a way that duplicates completed output or consumed inputs.

A completed order must not be treated as active work unless a new order or explicitly defined continuation is created.

### 9.3 Scheduling Priority

Production orders may compete for limited productive capacity.

The scheduling policy must define:

1. Eligibility for execution.
2. Priority evaluation.
3. Capacity allocation.
4. Tie-breaking.
5. Handling of incomplete orders.
6. Treatment of blocked and cancelled orders.
7. Reallocation when capacity or inputs change.
8. Whether and how an active order may be interrupted.

Priority may be influenced by the originating order, contractual deadlines, Government direction or other authorised rules.

Production must not independently decide the economic desirability of an order. It schedules eligible orders according to their authorised priorities and available capability.

The initial implementation must use a deterministic priority order and a stable identifier as a final tie-breaker where other criteria are equal.

### 9.4 Partial Completion

An order may be completed over several execution intervals.

The system must preserve the quantity already completed and schedule only the remaining quantity.

Partial completion must not duplicate input consumption or output generation.

If an order is cancelled after partial completion, completed quantities remain part of the authoritative production history. Only the outstanding quantity is cancelled, subject to explicit cancellation or reversal rules.

### 9.5 Recipe Selection and Order Consistency

Where a production order selects a recipe version, the version must be validated before execution.

An executing order must not silently change its recipe version after consuming inputs or producing outputs.

A change to the selected recipe requires an explicit, validated order amendment. If the amendment affects an execution already committed, it must not rewrite that execution's historical inputs or outputs.

### 9.6 Cancellation and Outstanding Reservations

Cancelling an order must resolve its unused reservations through Inventory.

Production must not independently release or recreate reserved quantities.

Already consumed inputs and completed outputs remain part of the historical record. Reversing a completed transformation requires an explicitly defined recovery or reversal process.

---

## 10. Operating Constraints

Production is permitted only when all mandatory operational conditions are satisfied.

### 10.1 Facility Eligibility

The canonical Facility must be in an eligible lifecycle/condition state under Installations and Facilities rules, commissioned where required, and compatible with the requested process. The Production capability must separately be eligible.

A canonical Facility that is planned, uncommissioned, decommissioned or otherwise unavailable under its owning lifecycle rules must not produce.

A restricted facility may operate only within its defined restrictions.

### 10.2 Input Availability

All mandatory input requirements must be satisfied for the quantity to be executed.

Where inputs are insufficient, production may be blocked or limited to a quantity supported by the available inputs, provided the process permits partial execution.

Substitution is permitted only where explicitly supported by the selected recipe or process definition.

### 10.3 Capacity

The requested execution must fit within Effective Throughput available during the selected interval.

An order cannot exceed the capacity of its assigned productive units.

### 10.4 Workforce

Required labour must be available and assigned according to the applicable workforce rules.

If workforce falls below an essential operating threshold, execution must be blocked or reduced as defined by the relevant process.

### 10.5 Technology and Equipment

The facility must possess the technology, equipment and process capabilities required to execute the requested transformation.

Research availability alone does not satisfy this requirement. Required technology or equipment must be available to the relevant facility under applicable adoption, installation and ownership rules.

### 10.6 Utilities

Mandatory utilities must be available at the required quantities or service levels.

Where utilities are constrained, their effects must be applied consistently with the relevant utility and infrastructure systems.

### 10.7 Location and Access

A facility must occupy a valid physical location and satisfy applicable site, access, environmental and infrastructure constraints.

Changes in terrain, access, infrastructure or territorial conditions may affect production where the relevant systems expose those changes.

Production does not independently calculate territorial ownership or movement permissions.

### 10.8 Legal and Governmental Restrictions

Production must respect applicable legal permissions, restrictions, embargoes, licensing rules and Government directives.

The authoritative rules belong to Government and Law, together with Diplomacy where international restrictions apply.

Where authorisation is absent or revoked, Production must block or restrict execution as directed by the relevant interface.

### 10.9 Environmental and External Conditions

Environmental conditions, disasters, accidents, conflict and other external events may affect facilities or production.

These effects must be received from the authoritative systems that simulate them.

Production applies their defined operational consequences but does not maintain duplicate models of weather, combat damage or environmental geography.

---

## 11. Production Execution

### 11.1 Execution Principle

Production execution is the controlled process through which a valid production order uses its required inputs and generates its defined outputs.

Every execution must be reproducible from the authoritative world state, applicable process and recipe versions, order parameters and simulation interval.

Production must not generate output solely because a daily target exists or because an Industry is expected to operate at a particular Scale.

### 11.2 Execution Preconditions

Before an execution begins, the system must verify:

1. The production order is valid and eligible.
2. The assigned facility and production line can execute the process.
3. The facility is operational or otherwise eligible under its explicit restrictions.
4. The process and recipe versions are valid.
5. Required technology and equipment are available.
6. Sufficient Effective Throughput has been allocated.
7. Required inputs are physically available and validly reserved where reservations are required.
8. Workforce and mandatory utilities are sufficient.
9. Applicable permissions and restrictions permit execution.
10. The requested quantity is consistent with the order's outstanding quantity.
11. All units and conversions are valid.
12. The execution identifier has not already been committed.
13. Inventory can support the required atomic stock changes.

If any mandatory condition fails, execution must not proceed as if successful.

The system should record a structured blocking reason so that the relevant system or Player can identify and address the constraint.

### 11.3 Execution Sequence

A Production Execution follows this logical sequence:

1. **Resolve the order.** Identify the order, Industry, facility, production line, process, recipe version and outstanding quantity.
2. **Evaluate constraints.** Determine eligibility, Effective Throughput, workforce, utilities, permissions and other operating restrictions.
3. **Determine executable quantity.** Establish the quantity that can be produced during the execution interval.
4. **Resolve recipe quantities.** Calculate the required inputs, outputs, by-products and losses using the selected immutable recipe version and applicable execution conditions.
5. **Validate inputs.** Validate input availability and reservations through Inventory.
6. **Prepare the stock transaction.** Submit the required input consumption and output registration changes through Inventory's authoritative interface.
7. **Commit the physical transformation.** Commit the stock changes and execution outcome as one atomic logical operation.
8. **Update order and capacity state.** Record consumed capacity, completed quantities and remaining outstanding work.
9. **Record the execution.** Persist the process version, input and output transaction references, interval and outcome.
10. **Emit system events.** Publish the committed production and inventory events for dependent systems.

The implementation may combine these operations into a single transaction or use an equivalent transactional mechanism. The authoritative outcome must remain consistent if an execution is interrupted or fails.

The exact recipe calculation belongs to Production Processes and Recipes. The authoritative physical stock changes belong to Inventory. Production orchestrates the execution and records its result.

### 11.4 Shared Production Execution Transaction, Atomicity and Recovery

Production and Inventory must implement one shared **Production Execution transaction** for committing the physical result of a production execution. Production orchestrates the transaction and owns the Production Execution record. Inventory owns the authoritative stock changes and stock transaction records. Neither system may independently commit a competing version of the same physical outcome.

The shared transaction must use a stable Production Execution identifier and idempotency key. The same key must identify retries of the same logical execution, and a key must not be reused for a different execution payload.

Before commitment, the transaction must validate the selected immutable recipe version, the target output or production basis, input quantities and units, relevant reservations, and the complete set of calculated outputs. Inventory must validate stock availability, reservation ownership and quantity, product eligibility for physical stock, and the requested stock changes against authoritative inventory state.

The transaction contract must cover, as applicable:

1. Validation of all required input reservations or other explicitly supported input-allocation records.
2. Consumption of the authorised input quantities.
3. Registration of every physical output, including primary outputs, co-products, by-products, recoverable materials and tracked waste.
4. Recording of process losses where they affect stock or require a separately defined non-stock record.
5. Recording of the Production Execution outcome and references to the authoritative Inventory transactions.
6. Updating of the order's completed quantity and remaining outstanding quantity consistently with the committed execution.
7. Publication of success events only after the physical outcome is committed.

All stock changes belonging to one committed execution must form one atomic logical operation. The implementation may use a database transaction, a transactional event/commit protocol, or an equivalent mechanism. This specification defines the required outcome, not a particular database or infrastructure design.

If validation or commitment fails, the operation must not leave partial input consumption, partial output registration, or an execution falsely marked as committed. A retry using the same idempotency key must return or resolve to the existing committed result if commitment already succeeded, and must not duplicate stock changes. A retry with the same key but a materially different payload must be rejected as an idempotency conflict.

Long-running or multi-stage production may use multiple explicitly identified Production Executions. Each execution is a separate transaction with its own inputs, outputs, result and idempotency key. A later stage must not be treated as committed merely because an earlier stage completed.

A failed execution must not be represented as a successful production event. The transaction and recovery mechanism must preserve enough state to distinguish not started, prepared or pending, committed, rejected and failed outcomes without replaying already committed stock changes.

### 11.5 Production Result

A successful execution records the physical outcome of the process.

At minimum, it must identify:

- The production order and facility.
- The process and exact recipe version.
- The simulation interval.
- The inputs consumed.
- The outputs produced.
- The capacity used.
- The resulting order state.
- The relevant Inventory transaction references.
- Any applicable operating constraints or losses.

Production results provide the basis for production statistics, Inventory reconciliation, operational reporting and downstream economic accounting.

They must not directly overwrite financial accounts belonging to an Industry or Company.

### 11.6 Services and Non-Inventory Outputs

A process may produce a Service or another result that is not represented by physical Inventory.

Such outputs must use the relevant service, operational or delivery interface.

Production must still record the execution and its applicable capacity usage, but must not invent a physical stock quantity for a non-physical result.

If a process produces both physical Goods and non-physical outputs, each output must be handled by its appropriate authoritative interface within a consistent execution result.

---

## 12. Simulation Timing and Daily Processing

Project Sovereign advances through a deterministic simulation timeline. Production must execute within that timeline and produce consistent results regardless of incidental program execution order.

### 12.1 Daily Production Cycle

The initial implementation uses a daily production interval.

Within that interval, Production must perform its logical responsibilities:

1. Consume the authoritative facility states and externally supplied constraints for the interval.
2. Resolve workforce, utility and infrastructure availability.
3. Revalidate eligible orders and outstanding input requirements.
4. Determine Available and Effective Throughput.
5. Schedule eligible orders using the documented scheduling policy.
6. Resolve executable quantities.
7. Execute production using valid input stock and selected process and recipe versions.
8. Commit physical stock changes through Inventory.
9. Update order states and execution records.
10. Publish committed production results and relevant events.
11. Reconcile production statistics and expose the resulting state to dependent systems.

This is a Production-local responsibility list, not a competing definition of the global simulation scheduler.

The shared Technical Specifications must define the exact global phase order, snapshot rules, event ordering and timing of cross-system updates.

### 12.2 Daily Processing and Other Systems

Production must operate against a clearly defined authoritative world-state snapshot for the relevant interval.

If an upstream system can change inputs or constraints during the same day, the global scheduler must determine whether those changes affect that day's production or the next interval.

Different facilities must not receive inconsistent treatment merely because one was processed earlier than another.

Cross-system timing must be documented where it materially affects outcomes, particularly for:

- Resource extraction and subsequent processing.
- Power generation and electricity consumption.
- Goods delivery and input consumption.
- Construction and commissioning.
- Workforce changes.
- Infrastructure disruption.
- Government restrictions.
- Production output and subsequent transportation.

Production must not independently prescribe a conflicting global update order.

### 12.3 Determinism

Given the same authoritative state, production orders, process and recipe versions, and deterministic inputs, Production must produce the same outcomes.

Determinism requires:

- Stable ordering of production orders.
- Stable ordering of facilities and production lines.
- Explicit scheduling tie-breakers.
- Consistent quantity and rounding rules.
- No dependence on non-deterministic collection iteration.
- No unseeded randomness in authoritative production outcomes.
- Stable handling of execution retries and duplicate events.
- Stable selection of configuration and recipe versions.

Where stochastic events are introduced, their random inputs must be controlled by the simulation's deterministic random system and reproducible from authoritative simulation state.

### 12.4 Time and Quantity Precision

Production calculations must use defined units and precision rules.

Floating-point arithmetic must not introduce order-dependent discrepancies that create or destroy material quantities.

The implementation should use fixed-point quantities, rational representations or another deterministic numerical method appropriate to the required scale.

Rounding must be applied at documented boundaries. Intermediate rounding must not silently accumulate into significant production gains or losses.

The detailed numerical standard may be established in shared simulation-engine conventions. Production, Recipe and Inventory interfaces must use compatible rules.

---

## 13. Resource Extraction and Industrial Production

Resource extraction is treated as a physical production activity, but it differs from processing existing Goods.

Examples include mining ore, harvesting timber, extracting petroleum and collecting agricultural output.

### 13.1 Extraction Sources

An extraction process must reference an appropriate physical resource source, such as a deposit, forest, agricultural area or other resource-bearing location.

The relevant geographic and resource systems determine the source's physical existence, location, characteristics and applicable limits.

Production executes the extraction activity and generates the resulting Goods through the authorised resource and Inventory interfaces.

An Industry Area alone must not create unlimited extractable resources.

### 13.2 Finite and Renewable Resources

The system must support finite and renewable resources.

Finite resources may be depleted by extraction. Renewable resources may replenish according to their authoritative environmental or resource model.

The source model must define whether a resource is finite, renewable or otherwise constrained.

Extraction must not exceed the quantity physically available under the authoritative source rules.

The exact resource depletion, regeneration and environmental-impact algorithms belong to the resource and environmental specifications. Production must consume or update the source through their defined interfaces.

### 13.3 Extraction Capacity

Extraction remains subject to facility capacity, workforce, equipment, utilities, transport access and process requirements.

The existence of a resource deposit does not guarantee that it can be extracted at an arbitrary rate.

A resource may be physically present but inaccessible, uneconomic to extract, legally restricted or beyond the capability of available equipment.

Production determines physical execution under the applicable constraints. Economy determines the financial implications of the activity.

### 13.4 Deposit State and Inventory State

A resource deposit and extracted stock are different forms of state.

The resource model owns the remaining deposit quantity and any applicable regeneration or depletion mechanics.

Inventory owns extracted physical stock once it has been registered as an Inventory holding.

The same quantity must not be simultaneously counted as unextracted deposit material and available Inventory stock.

Extraction must update the resource source and register the resulting physical Goods through an atomic or equivalent reconciled operation. A failed extraction execution must not deplete the source without recording the corresponding result.

### 13.5 Strategic Resources

Strategic Resource identity and classification belong to the Good and Service Catalogue and relevant economy reference data.

Production may execute extraction or manufacturing processes associated with Strategic Resources, but must not independently redefine which products are Strategic Resources.

The existence of a Strategic Resource designation does not itself prove that the resource can be extracted at a particular location or that a facility has the capability to produce it.

---

## 14. Production and Industry Scale

Industry Scale is defined by Industry Data and represents the economic or organisational scale of an Industry according to Economy's rules.

It must not be used as a direct substitute for process-specific nominal throughput, effective throughput or Actual Output.

Two Industries with the same Scale may have different Production Capacities because of differences in:

- Facility count and configuration.
- Equipment.
- Technology.
- Workforce.
- Input availability.
- Utility access.
- Maintenance.
- Operating efficiency.
- Production process.
- Physical location.
- Infrastructure.
- Damage or restrictions.

Industry Scale may inform initial configurations, scenario setup, economic estimates or establishment requirements where an explicit rule defines the relationship.

Production Capacity must ultimately be grounded in the productive assets and process capabilities represented in the world.

Production must not silently multiply output because an Industry has been assigned a higher economic Scale.

### 14.1 Scale Changes

A change in Industry Scale may affect economic intent, workforce requirements or planned production orders through the relevant interfaces.

It does not automatically construct, acquire, upgrade or destroy physical facilities.

Any relationship between Scale and physical capacity must be implemented through an explicit operation, such as facility construction, equipment installation or a capacity upgrade.

### 14.2 Economic Intent and Executable Output

Economy may communicate intended production activity or an economic target to Production.

That intent is not a guarantee of executable output.

Production must determine the quantity that can actually be produced under the physical constraints of the relevant interval.

Differences between intended and actual production must be reported through explicit operational results rather than resolved by silently overriding capacity or inventory rules.

---

## 15. Technology and Production Capability

Production capabilities may depend on technologies, designs, equipment and process definitions.

Research determines which technologies have been discovered or otherwise become available. The relevant implementation rules determine whether technologies have been adopted, installed and made operational at a particular facility.

Production validates these requirements before executing the associated process.

Technology may affect:

- Available production processes.
- Compatible equipment.
- Configured process-specific nominal throughput.
- Production efficiency.
- Required input quantities.
- Material yield.
- Workforce requirements.
- Energy consumption.
- Quality or reliability.
- Maintenance requirements.

These effects must be explicitly represented in the relevant process, facility or technology data.

A technology upgrade must not retroactively create Goods, rewrite historical executions or automatically change every facility in the world unless an explicit rule authorises that transition.

Changes to productive capability must take effect at a defined simulation boundary and preserve the integrity of existing orders and execution records.

### 15.1 Technology Versus Installation

Discovery of a technology is distinct from facility adoption, equipment installation and operational activation.

Production must validate the capability actually available to the facility. It must not assume that every facility immediately receives every newly discovered technology.

### 15.2 Historical Reproducibility

Every completed execution must retain its exact recipe version and relevant capability configuration references.

A later technology or recipe update must not silently rewrite the inputs or outputs of a historical execution.

If a migration changes the interpretation of an existing production configuration, the migration must be explicit and must preserve sufficient history to audit the original outcome.

---

## 16. Production Failures and Disruption

Production may be interrupted or prevented by operational failures.

The system must distinguish between an order that is waiting, an order that is temporarily blocked, and an execution that has actually failed.

### 16.1 Common Failure Categories

The initial model should support structured failure reasons including:

- `invalid_order`
- `invalid_process`
- `invalid_recipe_version`
- `facility_unavailable`
- `process_incompatible`
- `capacity_unavailable`
- `inputs_unavailable`
- `reservation_invalid`
- `workforce_insufficient`
- `utility_unavailable`
- `technology_unavailable`
- `equipment_failure`
- `maintenance_required`
- `access_restricted`
- `legal_restriction`
- `resource_depleted`
- `output_registration_failed`
- `execution_conflict`
- `cancelled_by_authority`

The enumeration may be extended as dependent systems are implemented. Failure reasons must remain structured and distinguishable rather than being represented solely by free-text descriptions.

### 16.2 Temporary Blockage

A temporary blockage prevents execution until a constraint is resolved.

Examples include missing inputs, unavailable workforce, a closed transport route or a temporary power interruption.

The order may return to an eligible state when the relevant condition changes. This transition must not duplicate completed output or consumed inputs.

### 16.3 Equipment Failure and Damage

Equipment failure may reduce Effective Throughput or make a facility ineligible for a process.

The equipment, maintenance and damage systems determine authoritative condition and recovery requirements. Production applies the resulting constraints.

Where damage occurs during an execution interval, the global event-ordering rules must determine whether the current execution completes, is interrupted or fails.

### 16.4 Cancellation

A production order may be cancelled by an authorised actor or system.

Cancellation must preserve completed output and historical execution records.

Reserved inputs must be released or otherwise resolved through Inventory's reservation interface, subject to reservation rules.

Production must not independently return consumed inputs or reverse completed transformations unless a specific reversal or recovery process is defined.

### 16.5 Failed Execution

A failed execution must record its outcome and structured failure reason.

If failure occurs before the stock transaction commits, the system must leave no unintended partial consumption or output.

If failure occurs after the execution has committed, retrying the same execution must return or recover the committed result without repeating the stock changes.

Any recovery operation that changes authoritative state must itself be explicit, auditable and idempotent.

---

## 17. Interfaces and Events

Production must expose explicit interfaces to dependent systems.

Exact implementation protocols are deferred to the software architecture, but the logical contracts below are mandatory.

### 17.1 Inputs from Industry Data

Production requires access to:

- Industry identifiers and Industry Types.
- Industry-to-Company relationships.
- Industry Scale where relevant.
- Industry Areas and associated locations.
- Establishment and operational prerequisites.
- Workforce requirements and relevant Industry configuration.
- Starting-world Industry configuration.

Industry Data remains authoritative for those records.

### 17.2 Inputs from Economy

Production may receive:

- Intended production activity.
- Authorised production requests.
- Order priorities or Government-directed production requirements.
- Relevant economic restrictions or operating instructions.
- Economic reporting requirements.

Economy owns economic intent and financial state. Production independently validates whether the requested activity is physically executable.

### 17.3 Inputs from Inventory

Production requires interfaces to:

- Check available stock.
- Validate stock locations and access.
- Validate and reference reservations.
- Prepare and commit input consumption.
- Register physical outputs.
- Obtain authoritative consumption and registration results.
- Release reservations when appropriate.
- Reconcile execution outcomes with Inventory transaction references.

Production must not maintain duplicate authoritative stock quantities.

### 17.4 Inputs from Production Processes and Recipes

Production requires access to:

- Process identifiers.
- Recipe identifiers and immutable recipe versions.
- Eligible inputs and their required quantities.
- Primary outputs and their quantities.
- Secondary outputs and by-products.
- Process yields and applicable losses.
- Required equipment and capabilities.
- Required technology.
- Process-level operating constraints.
- Canonical unit definitions and permitted conversions.

Production Processes and Recipes owns these definitions.

### 17.5 Inputs from the Good and Service Catalogue

Production requires canonical product definitions and classifications for all Goods and Services referenced by its processes and recipes.

The Catalogue supplies identity, physical form, classification, canonical units and relevant product attributes.

Production must not invent product identities or assume that every process output is a physical Inventory Good.

### 17.6 Inputs from Research

Production requires relevant technology availability and adoption state.

It must be able to determine whether a facility possesses the capabilities required to execute a process under applicable technology rules.

### 17.7 Inputs from Workforce and Infrastructure

Production requires workforce, utilities, infrastructure and other operational availability data exposed by their authoritative systems.

Where those systems are not yet implemented, Production must expose the dependency explicitly and avoid inventing permanent assumptions about unlimited labour, energy or infrastructure.

### 17.8 Inputs from Government and Law

Production requires relevant permissions, prohibitions and operational restrictions.

These must be evaluated using the applicable rules and simulation timing.

### 17.9 Outputs to Inventory

Production submits authoritative requests or transactions for input consumption and output registration.

A successful production execution must be reconcilable with the resulting Inventory state.

### 17.10 Outputs to Economy

Production provides factual operational results to Economy, including:

- Completed production quantities.
- Input consumption.
- Production activity by Industry and facility.
- Recorded operational resource usage.
- Disruption and capacity information.
- Production completion events.
- Rejected output or process losses where modelled.

Economy determines the resulting financial transactions according to its own rules.

Production must not independently invent market prices, revenue, profit, taxation or financial solvency.

### 17.11 Outputs to Logistics

Production exposes output availability and relevant location information so Logistics can arrange storage, transfer and delivery.

Output registration does not itself dispatch a shipment or establish that a destination has received it.

Logistics and Inventory must preserve the distinction between stock at origin, stock in transit and stock received at destination.

### 17.12 Outputs to Statistics and Reporting

Production should expose derived measures such as:

- Configured Nominal Throughput.
- Available Throughput.
- Effective Throughput.
- Allocated Throughput.
- Actual Output.
- Capacity utilisation.
- Facility operational status.
- Production order backlog.
- Input shortages.
- Blocked production time.
- Recorded losses and rejected output.

These statistics must be derived from authoritative production records and must not become alternative sources of truth.

### 17.13 Event Idempotency

Production events must include stable identifiers sufficient to prevent duplicate processing by dependent systems.

Consumers must be able to distinguish a new committed execution from a replay, retry or duplicate notification.

A repeated notification must not cause duplicate Inventory registration, financial accounting or production statistics.

---

## 18. Production Invariants

The following invariants must hold across every valid simulation state.

### 18.1 Physical Output

Production cannot generate physical Goods without a valid production execution, except where another explicitly authorised system is responsible for creating the relevant stock.

### 18.2 Input Consumption

Every completed process execution must account for its required input consumption according to the selected recipe version and execution conditions.

### 18.3 Inventory Authority

Production must not independently modify authoritative stock quantities, ownership, locations or reservations outside Inventory's authorised interfaces.

### 18.4 Capacity

Total capacity allocated to simultaneous executions must not exceed the applicable Effective Throughput of the relevant facility or production line.

### 18.5 Order Quantities

The completed quantity of an order must not exceed its requested quantity, except where an explicit order or process rule authorises overproduction and defines its treatment.

### 18.6 No Duplicate Execution

Reprocessing the same committed execution must not consume inputs or generate outputs a second time.

### 18.7 Facility Eligibility

An ineligible facility cannot execute a process that requires operational availability.

### 18.8 Process and Recipe Validity

Every execution must reference a valid process definition and exact recipe version.

### 18.9 Determinism

The same authoritative initial state and deterministic inputs must produce the same production outcome.

### 18.10 Industry Independence

Changing Industry Scale alone must not silently change a Facility's configured process-specific nominal throughput or generate physical output.

### 18.11 Ownership Integrity

Production must not silently change ownership of an Industry, facility or output Goods. Ownership changes must be authorised and recorded through the appropriate system.

### 18.12 Conservation and Reconciliation

For each execution, input consumption and output generation must reconcile with the transformation rules, including explicitly modelled yields, by-products, waste and losses.

The simulation must not create or destroy physical quantities through undocumented bookkeeping effects.

### 18.13 In-Transit Separation

Goods dispatched from a production location must not remain simultaneously available as unrestricted stock at origin and destination.

Physical movement and delivery must be reconciled through Inventory and Logistics.

### 18.14 Financial Separation

Production results must not directly mutate Company Capital, Nation Treasury, revenue or expenditure.

Any financial consequence must be recorded through Economy.

### 18.15 Historical Reproducibility

Completed execution records must preserve the recipe version and relevant configuration references required to interpret the historical result.

Subsequent configuration changes must not silently rewrite completed production history.

### 18.16 Service Classification

Non-physical Services must not be registered as physical stock unless the Catalogue explicitly defines the relevant output as a physical Good.

---

## 19. Initial Implementation Scope

The initial Production System should prioritise a coherent and testable physical production loop over an unnecessarily elaborate industrial simulation.

### 19.1 Required for the Initial Implementation

The initial implementation must support:

- Stable Industry and facility identifiers.
- Facilities associated with Industry instances.
- Facility lifecycle and operational states.
- Installed and Effective Throughput.
- A basic production-line abstraction or equivalent facility-level execution.
- Production orders and deterministic scheduling.
- Process compatibility validation.
- Exact recipe-version selection.
- Input availability checks.
- Inventory reservations and consumption through the appropriate interface.
- Execution of defined production processes.
- Atomic output registration through Inventory.
- Partial order completion.
- Structured blocking and failure states.
- Daily production processing.
- Deterministic execution and duplicate-execution protection.
- Production history and basic operational statistics.
- Integration with starting-world Industry and facility data.
- Separation of physical Goods and non-physical outputs.
- Reconciliation between resource extraction and resulting Inventory stock.

### 19.2 Extensible Features

The architecture should permit later implementation of:

- Multi-line factories and complex industrial plants.
- Batch production and changeover times.
- Detailed workforce skills and productivity.
- Equipment deterioration and maintenance schedules.
- Energy and utility network constraints.
- Quality variation and rejected output.
- Production waste and recycling.
- Environmental impacts.
- Dynamic production efficiency.
- Advanced scheduling and priority policies.
- Facility construction and commissioning simulation.
- Industrial expansion and capacity upgrades.
- Detailed resource depletion and regeneration.
- Automation and technology-driven process changes.
- Multi-facility production chains and intermediate transfers.
- Emergency production, mobilisation and Government-directed industrial priorities.
- Detailed service-production and service-delivery mechanics.

These features must extend the established interfaces rather than introduce competing ownership of capacity, inventory, Industry identity or physical output.

---

## 20. Validation and Acceptance Criteria

The Production System is considered functionally valid when the following conditions can be demonstrated through deterministic tests.

### 20.1 Basic Production

Given an operational facility, valid production order, compatible process and recipe version, sufficient inputs and available capacity, the system executes production and registers the expected outputs.

### 20.2 Insufficient Inputs

Given an otherwise valid order with insufficient input stock, the system does not consume unavailable quantities or generate unsupported output.

### 20.3 Capacity Constraint

Given several eligible orders competing for limited capacity, the scheduler allocates no more than the available capacity and produces deterministic results.

### 20.4 Facility Suspension

Given a suspended facility, no prohibited production execution occurs.

### 20.5 Workforce Constraint

Given a process requiring a defined minimum workforce, the system blocks or restricts production when the required labour conditions are not met.

### 20.6 Technology Constraint

Given a process requiring unavailable technology or capability, the system rejects or blocks execution without creating output.

### 20.7 Partial Completion

Given an order that cannot be completed in one interval, the system records completed quantities and continues only with the remaining outstanding quantity.

### 20.8 Inventory Reconciliation

Given a successful production execution, input consumption and output registration are reconciled through Inventory without duplicate or missing quantities.

### 20.9 Execution Recovery

Given a failed or interrupted execution, retrying or replaying the same committed execution cannot duplicate consumption or output.

### 20.10 Resource Extraction

Given a finite resource source with a known remaining quantity, extraction cannot exceed the quantity available under the authoritative resource rules.

### 20.11 Industry Scale Independence

Given two otherwise identical facilities, changing only their Industry Scale does not change physical output unless a separately defined rule explicitly modifies their productive configuration.

### 20.12 Deterministic Replay

Given identical starting state, production orders, process and recipe versions, and deterministic external inputs, repeated simulation runs produce identical production records and final authoritative state.

### 20.13 Ownership and Location

Given a completed production order, the resulting Goods are registered at the correct location and retain ownership according to authoritative ownership rules.

### 20.14 Order Cancellation

Given a partially completed order that is cancelled, completed outputs remain recorded, outstanding work is cancelled and unused reservations are handled through Inventory.

### 20.15 Recipe Version Stability

Given a completed execution and a later recipe update, the historical execution retains its original recipe version and quantities.

### 20.16 No Duplicate Stock

Given a committed production execution that is replayed or whose event notification is repeated, Inventory does not register its outputs or consume its inputs twice.

### 20.17 Service Output

Given a process that produces a non-physical Service, the system records the execution through the appropriate interface without creating an invalid physical Inventory holding.

### 20.18 Global Scheduler Compatibility

Given the same authoritative daily snapshot and order set, production outcomes remain consistent regardless of internal processing order.

---

## 21. Dependencies and Related Specifications

The Production System depends on the following documents and interfaces:

| Specification | Relationship |
|---|---|
| `docs/systems/economy/economy-system.md` | Financial and economic authority |
| `docs/systems/economy/industry-data.md` | Industry identity, configuration, Scale, establishment, workforce requirements and starting-world Industry data |
| `docs/systems/infrastructure/installations-and-facilities-system.md` | Canonical Facility identity, shared lifecycle/condition, ownership/operator/control, shared capabilities and generic capacity definitions |
| `docs/systems/production/production-processes-and-recipes.md` | Transformation definitions, recipe versions, input quantities, output quantities, yields and process requirements |
| `docs/systems/inventory/inventory-system.md` | Authoritative physical stock, reservations, consumption, output registration, transfers and delivery |
| `docs/reference/economy/good-and-service-catalogue.md` | Canonical definitions of Goods, Services, classifications and units |
| Research specifications | Technology availability, adoption and capability requirements |
| Workforce and Population specifications | Labour availability, assignment and qualification |
| Terrain and Geography specifications | Physical facility locations, site conditions and spatial constraints |
| Infrastructure and Logistics specifications | Utilities, transport access and physical delivery |
| Government and Law specifications | Legal permissions, restrictions and Government directives |
| Construction specifications | Facility construction, installation, commissioning and upgrades, where implemented |
| Shared Technical Specifications | Global simulation scheduling, deterministic arithmetic, persistence, transaction boundaries and event processing |

These documents are authoritative for their respective domains. Production references their data and interfaces rather than redefining them.

---

## 22. Explicitly Unresolved Design Decisions

The following decisions require resolution during implementation or in their respective specifications. They must not be settled through undocumented assumptions.

1. **Capacity calculation:** The precise algorithm for combining simultaneous capacity constraints, including bottleneck and non-linear effects.
2. **Scheduling policy:** The initial priority model, pre-emption rules and treatment of competing deadlines.
3. **Numerical representation:** The shared fixed-point or equivalent deterministic arithmetic standard and its rounding boundaries.
4. **Transaction implementation:** The precise mechanism for atomic input consumption, output registration, execution retries and recovery.
5. **Process execution granularity:** Whether individual production lines, facilities or larger industrial units are the primary execution boundary for each process class.
6. **Construction integration:** The detailed rules for construction costs, material consumption, workforce allocation, commissioning and capacity upgrades.
7. **Workforce integration:** The exact interfaces for labour availability, skill matching, shift patterns and productivity.
8. **Utility integration:** The precise treatment of electricity, fuel, water and other networked or consumable utilities.
9. **Resource integration:** The detailed resource-source interfaces for extraction, depletion, regeneration and environmental constraints.
10. **Maintenance and deterioration:** The algorithms for equipment reliability, maintenance scheduling, wear, repair and downtime.
11. **Quality and spoilage:** The quantitative treatment of quality, defective Goods, rejected output and storage-related losses.
12. **Multi-output processes:** The exact representation of joint outputs, by-products, waste and process-specific yield variation.
13. **Starting-world calibration:** The initial canonical Facility configurations, Production capability mappings, process-specific nominal throughput and applicable shared operational states appropriate to the 1955 starting world.
14. **Service production:** The treatment of Services whose outputs are not conventional physical Inventory Goods, including their capacity and delivery semantics.
15. **Cross-system transaction coordination:** The exact implementation used to coordinate Production, Inventory and resource-source state changes without partial commitment.
16. **Recipe migration:** The rules for amending an order when a new recipe version becomes available but the order has not yet completed.

Each decision should be resolved in the owning specification or a documented implementation decision before the corresponding feature becomes authoritative.

---

## 23. Final Rule

The Production System converts authorised production activity into physical outcomes.

Industry Data defines the Industries and their configuration. Economy governs financial state and economic decisions. Production determines whether facilities can execute an activity, schedules eligible orders and orchestrates physical execution. Production Processes and Recipes define the transformations. Inventory records authoritative physical stock and stock movements. The Good and Service Catalogue defines the products those systems reference.

No Industry, Company, production order, facility or economic forecast may bypass these rules to create physical output without a valid, auditable and reproducible production execution.
