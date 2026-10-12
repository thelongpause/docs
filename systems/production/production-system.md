# Production System

## 1. Purpose

The Production System defines how the world's industries convert available resources, intermediate goods, labour, energy, and productive infrastructure into physical goods and completed production outputs.

It establishes the operational rules governing production capacity, facilities, input requirements, output generation, workforce availability, operating constraints, production orders, and production execution.

The Production System operates against the Industry model established in `docs/systems/economy/industry-data.md`. It does not independently define industries, companies, their economic scale, or the conditions under which an industry is established.

Production is a physical simulation. An industry may possess substantial economic importance, financing, or intended output without necessarily possessing the facilities, inputs, workforce, or infrastructure required to produce at that level.

The system must therefore distinguish between an industry's economic existence and its actual productive capability.

### 1.1 Design Principles

The Production System follows these principles:

1. **Physical capability governs output.** Production is constrained by facilities, installed capacity, available inputs, workforce, utilities, technology, and operating conditions.
2. **Industry and production are distinct.** An Industry is an economic entity. A Production Facility is a physical asset or operational installation through which production occurs.
3. **Capacity is not output.** Nominal capacity describes potential productive throughput under defined operating conditions, not guaranteed production.
4. **Production consumes real inputs.** Required materials and other consumable goods must be available and validly allocated before production can consume them.
5. **Outputs become physical stock.** Completed goods are transferred into authoritative Inventory records rather than being represented solely as an economic statistic.
6. **Ownership remains explicit.** Companies, governments, and other permitted entities may own or operate productive assets according to the relevant ownership and control rules.
7. **Location matters.** Facilities operate at physical locations and are subject to local infrastructure, terrain, accessibility, environmental conditions, and other applicable constraints.
8. **Technology modifies capability.** Research and adopted technology may change production methods, efficiency, capacity, input requirements, or the set of processes an industry can operate.
9. **Production is deterministic.** Identical authoritative world states, orders, and simulation inputs must produce identical production results.
10. **Systems have explicit boundaries.** Production determines physical transformation and execution. It does not independently determine prices, financial solvency, inventory ownership, territorial control, or technological advancement.

---

## 2. System Boundaries

The Production System is the authority for the execution of physical production.

It is not the authority for every economic or logistical activity associated with production.

| System | Responsibility | Relationship to Production |
|---|---|---|
| Economy | Economic entities, financial resources, expenditure, revenue, and economic decisions | Establishes economic context and receives production-related financial events through defined interfaces |
| Industry Data | Industry Types, Industry instances, Company relationships, Industry Scale, establishment prerequisites, financing, Industry Areas, workforce requirements, and starting-world data | Defines the industries for which productive operations may be configured |
| Production | Facilities, productive capacity, production orders, operational constraints, process execution, and physical output | Executes physical production |
| Production Processes and Recipes | Specific transformations, input quantities, output quantities, by-products, and process-level requirements | Supplies the transformation definitions executed by Production |
| Inventory | Authoritative physical stock, quantities, locations, condition, ownership, reservations, transfers, delivery, and consumption records | Supplies valid input stock and receives completed output stock |
| Research | Technologies, research progression, and technology availability | Determines which production capabilities or process configurations may be available |
| Terrain and Geography | Physical locations, terrain, elevation, movement constraints, infrastructure geography, and spatial relationships | Provides location and environmental context for facilities and production areas |
| Government and Law | Legal permissions, policy, regulation, taxation, restrictions, and applicable mandates | Determines whether specific production activities are legally permitted |
| Workforce and Population | Population, labour availability, skills, employment, and labour allocation, where implemented | Supplies workforce availability and constraints |
| Logistics and Transportation | Movement of goods, transportation capacity, routes, and delivery | Determines whether inputs and outputs can be transported between locations |
| Military and Organisations | Military requirements, procurement, acceptance, and operational demand | May generate production demand or receive completed goods through established interfaces |

Where a supporting system has not yet been fully specified, Production must use a defined interface or an explicitly documented placeholder. It must not invent an independent authoritative implementation of that system.

### 2.1 Authoritative Ownership Rules

The following distinctions are mandatory:

- Industry Scale does not automatically determine physical production capacity.
- Production capacity does not represent the quantity of goods currently in Inventory.
- An active production order does not constitute proof that its required inputs have been delivered.
- A production facility does not establish ownership of the goods stored within it.
- Production completion does not automatically establish delivery to a customer.
- A company's financial ability to purchase inputs does not guarantee that the inputs physically exist or are accessible.
- A research discovery does not automatically install or activate a new production capability.
- A production facility's location does not automatically confer legal permission, infrastructure access, or territorial control.
- Planned production is not completed production.
- Economic demand is not equivalent to a valid, executable production order.

These rules prevent the simulation from creating goods, capacity, ownership, or permissions merely because another system expresses an intention to obtain them.

---

## 3. Core Concepts

### 3.1 Industry

An Industry is an economic entity defined by Industry Data.

An Industry may be associated with one or more Companies, have a defined Industry Type, operate at a particular Industry Scale, employ a workforce, possess financial relationships, and maintain one or more Industry Areas.

Production references Industry instances using their stable identifiers.

Production must not redefine Industry identity, Industry Type, Company relationships, or Industry Scale.

### 3.2 Production Facility

A Production Facility is a physical installation, asset, or operational unit capable of performing one or more production activities.

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

A facility must have a stable identifier, a physical location, an operational state, and a defined productive capability.

A facility may contain several production lines or process-capable units. Conversely, a production process may require several cooperating facilities.

The system must not assume that one Industry corresponds to exactly one facility.

### 3.3 Production Line

A Production Line is an operational unit within a facility that executes a defined production process or compatible family of processes.

A production line has its own applicable capacity, operating state, process compatibility, scheduling constraints, and utilisation.

A line may be unavailable even when its parent facility remains operational. Examples include maintenance, equipment failure, missing specialist labour, or a lack of a required utility.

Where the implementation does not require line-level simulation, a facility may expose a single implicit production line. This simplification must not change the underlying distinction between facility-level capacity and process-level throughput.

### 3.4 Production Process

A Production Process defines a transformation from one or more inputs into one or more outputs.

The exact material transformation is defined by `docs/systems/production/production-processes-and-recipes.md`.

Production executes a valid process definition but does not independently author or redefine its recipes.

### 3.5 Production Order

A Production Order is an authorised instruction to produce a specified output through an eligible process at a designated facility or production line.

It defines the requested production activity and its operational parameters.

A production order is not itself a financial transaction, inventory transfer, or completed production event.

Orders must reference valid industries, facilities, processes, and output definitions.

### 3.6 Productive Capacity

Productive Capacity represents the maximum throughput a facility, production line, or other explicitly defined productive unit can achieve over a specified period under stated operating conditions.

Capacity must be associated with a measurable unit, a time basis, and the productive activity to which it applies.

For example, a facility might have a nominal capacity of 100 tonnes of a specified output per day. That figure does not imply that 100 tonnes will actually be produced on any given day.

### 3.7 Industry Area

An Industry Area is the spatial extent or operational area associated with an Industry as defined by Industry Data.

Industry Areas and Production Facilities are distinct concepts.

An Industry Area may represent an agricultural region, a mining concession, an industrial district, a group of extraction sites, or another geographically defined economic area.

A facility may be situated inside an Industry Area, serve several Industry Areas, or operate outside an Industry Area where the Industry Data rules permit it.

An Industry Area does not automatically constitute a production facility or confer productive capacity.

---

## 4. Production System Data Model

All persistent production entities must have stable identifiers. Relationships must use identifiers rather than display names.

The following is the logical data model. Exact storage formats, programming-language types, and serialization structures are implementation decisions unless otherwise specified.

### 4.1 Production Facility Record

A Production Facility must support the following information:

| Field | Purpose |
|---|---|
| `facility_id` | Unique, stable facility identifier |
| `industry_id` | Industry responsible for or associated with the facility |
| `facility_type_id` | Reference to the applicable facility type definition |
| `name` | Human-readable facility name |
| `location_id` | Reference to the facility's physical location |
| `industry_area_id` | Optional associated Industry Area |
| `ownership_id` | Reference to the authoritative owner |
| `operator_id` | Optional entity responsible for operations |
| `operational_state` | Current operating condition |
| `installed_capacity` | Nominal installed productive capability |
| `capacity_unit` | Unit used to measure the capacity |
| `capacity_period` | Time interval to which the capacity applies |
| `process_capabilities` | Processes the facility is equipped and permitted to execute |
| `infrastructure_requirements` | Required utilities, access, and supporting infrastructure |
| `technology_requirements` | Required technologies or equipment capabilities |
| `maintenance_state` | Maintenance status and applicable restrictions |
| `commissioning_state` | Whether the facility is planned, under construction, commissioned, or decommissioned |
| `construction_progress` | Applicable construction progress, when construction simulation is supported |
| `workforce_requirements` | Required labour by category, skill, or role |
| `workforce_allocation` | Workforce assigned to the facility, derived from the authoritative labour system where implemented |
| `created_at` | Simulation time at which the facility was created |
| `last_updated_at` | Simulation time of the most recent authoritative update |

Optional fields may be omitted where the associated system is not yet implemented. Their absence must not silently imply that a requirement is satisfied.

The facility record must not become an alternative source of truth for inventory quantities, company finances, population totals, or territorial ownership.

### 4.2 Production Line Record

A Production Line should support:

- `production_line_id`
- `facility_id`
- `operational_state`
- `supported_processes`
- `installed_capacity`
- `capacity_unit`
- `capacity_period`
- `current_process_id`, if assigned
- `current_order_id`, if assigned
- `maintenance_state`
- `effective_capacity`
- `last_updated_at`

The line's effective capacity is derived from its installed capacity and applicable operating constraints.

### 4.3 Production Order Record

A Production Order must support:

| Field | Purpose |
|---|---|
| `production_order_id` | Unique, stable order identifier |
| `industry_id` | Industry requesting or responsible for production |
| `facility_id` | Facility assigned to the order |
| `production_line_id` | Optional production line assignment |
| `process_id` | Process to execute |
| `output_specification` | Required output goods and quantities |
| `requested_quantity` | Quantity requested, expressed in the output's defined unit |
| `priority` | Order priority within the applicable scheduling policy |
| `order_state` | Current execution state |
| `created_at` | Simulation time at which the order was created |
| `earliest_start_time` | Earliest permitted execution time |
| `deadline` | Optional requested completion time |
| `input_reservation_ids` | References to valid Inventory reservations, where applicable |
| `allocated_capacity` | Capacity assigned to the order |
| `completed_quantity` | Quantity successfully produced |
| `rejected_quantity` | Quantity rejected or recorded as unusable, where quality simulation is supported |
| `cancelled_quantity` | Quantity no longer required because the order was cancelled |
| `failure_reason` | Structured reason for a blocked, failed, or cancelled order |
| `last_updated_at` | Simulation time of the most recent order update |

Requested quantities, completed quantities, and rejected quantities must use compatible units. Where a process produces multiple outputs, the order must preserve the relationship between the process execution and every output generated.

### 4.4 Production Execution Record

A Production Execution represents a discrete attempt to execute a process.

It should record:

- A stable execution identifier.
- The originating production order.
- The executing facility and production line.
- The process definition and version used.
- The simulation interval during which execution occurred.
- The effective production rate and capacity allocated.
- The inputs consumed.
- The outputs generated.
- Any by-products or waste generated.
- Any losses, rejected outputs, or recorded spoilage.
- The execution result.
- Any operational constraints that affected the result.

Execution records provide an auditable account of physical production. They must allow the simulation to distinguish an order being scheduled from an order actually being executed.

---

## 5. Production Facility Lifecycle

A facility must pass through a controlled lifecycle.

### 5.1 Lifecycle States

| State | Meaning |
|---|---|
| `planned` | The facility has been proposed or recorded but is not yet physically available |
| `under_construction` | The facility is being constructed or installed |
| `commissioning` | Construction is sufficiently complete for testing and operational preparation, but production is not yet generally permitted |
| `operational` | The facility is commissioned and eligible to produce, subject to other constraints |
| `restricted` | The facility may operate only under specified restrictions |
| `suspended` | Production is temporarily prohibited or inactive |
| `damaged` | Physical damage prevents some or all normal operations |
| `decommissioning` | The facility is being withdrawn from service |
| `decommissioned` | The facility is no longer available for production |

A transition must be validated against the facility's current state and the rules governing the transition.

A facility cannot produce merely because a record exists. It must be in an eligible operating state and satisfy all applicable production prerequisites.

### 5.2 Establishment and Commissioning

Industry establishment and financing are governed by Industry Data and Economy.

The Production System consumes the resulting facility and commissioning state.

Where construction is simulated, a facility must not become operational until its required construction work, installation, commissioning, and applicable prerequisites have been satisfied.

Construction may require materials, equipment, labour, capital expenditure, and access to a construction site. These are distinct from the facility's subsequent production requirements.

A construction project may itself require production orders for construction materials or equipment. The system must not assume that those goods exist simply because construction has been authorised.

The exact construction and commissioning mechanics must be integrated with the relevant construction, infrastructure, logistics, and economic systems when those systems are specified.

### 5.3 Suspension and Decommissioning

A facility may be suspended because of:

- Missing or insufficient inputs.
- Insufficient workforce.
- Equipment failure or maintenance.
- Utility interruption.
- Infrastructure failure.
- Environmental conditions.
- Legal or governmental restrictions.
- Damage caused by accidents, disasters, or military action.
- A lack of valid production orders.

These conditions may reduce output without necessarily requiring a permanent lifecycle transition.

A temporary shortage of inputs, for example, should normally block or reduce execution rather than automatically decommission the facility.

Decommissioning must not automatically delete historical execution records, financial records, or inventory. The relevant systems retain authority over their respective records.

---

## 6. Capacity Model

### 6.1 Capacity Definitions

The Production System distinguishes between:

**Installed Capacity:** The nominal productive capability of a facility or line as installed.

**Available Capacity:** The portion of installed capacity currently available after accounting for operational state, maintenance, damage, and other capacity-affecting restrictions.

**Effective Capacity:** The productive capability that can actually be used for a specified process during a specified interval, after accounting for process compatibility, workforce, utilities, and applicable operating constraints.

**Allocated Capacity:** The portion of effective capacity assigned to a production order.

**Actual Output:** The quantity of physical goods successfully produced and accepted into the appropriate output-handling process.

These measures must not be treated as interchangeable.

### 6.2 Effective Capacity

Effective capacity is determined from installed capacity and all applicable constraints.

Conceptually:

`Effective Capacity = Installed Capacity × Capacity Availability × Operational Modifiers`

This expression illustrates the relationship rather than prescribing a final universal numerical algorithm. The precise treatment of simultaneous constraints, non-linear bottlenecks, and process-specific operating limits must be defined by the applicable process and facility rules.

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

For example, if maintenance has already reduced available capacity, a separate maintenance penalty must not be applied again to the same capacity calculation.

### 6.3 Capacity Units

Capacity must be measured in units appropriate to the process.

Examples include:

- Tonnes per day.
- Cubic metres per day.
- Units per day.
- Passenger-kilometres per day, where a service process requires such a measure.
- Megawatt-hours per day for energy generation.
- Work-hours per day for a process whose capacity is defined in labour terms.

The unit must be explicit. Quantities measured in incompatible units must not be compared or combined without a defined conversion.

Capacity must also distinguish between a flow rate and a stock quantity. Tonnes per day describe throughput; tonnes describe a physical quantity.

### 6.4 Capacity Allocation

Capacity allocation determines how available productive capability is distributed among eligible production orders.

Allocation must respect:

1. The capacity available during the relevant simulation interval.
2. Process and facility compatibility.
3. The priority and scheduling rules of the orders.
4. Workforce and other operational constraints.
5. Any process-specific batch, minimum-run, or setup requirements.
6. The time required to complete an execution.

An order cannot receive more capacity than its assigned facility or line can provide during the relevant interval.

Where several orders compete for the same capacity, the system must apply a deterministic scheduling policy. It must not allow the outcome to depend on incidental iteration order, machine timing, or the order in which data happens to be loaded.

The initial implementation should support a documented scheduling policy, with priority, eligibility, and stable tie-breaking rules. More advanced scheduling algorithms may be added later without changing the fundamental capacity contract.

### 6.5 Utilisation

Utilisation describes the degree to which available productive capacity is being used.

It is a derived operational statistic, not a substitute for capacity or output.

A facility may have low utilisation because it lacks orders, inputs, workforce, or demand. A facility may also have high utilisation while producing less than its nominal capacity because its effective capacity has been reduced.

Utilisation must be calculated using compatible capacity and time units, with explicit handling of intervals in which available capacity is zero.

---

## 7. Production Inputs and Outputs

### 7.1 Inputs

Production inputs are the resources and goods required to execute a process.

They may include:

- Raw materials.
- Intermediate goods.
- Components.
- Fuel and energy.
- Water and other utilities.
- Consumable materials.
- Packaging.
- Replacement parts.
- Other physical goods required by the process.

Labour, machine time, and environmental conditions may also be required, but they are not necessarily Inventory goods.

The exact inputs, quantities, substitution rules, and process-specific requirements are defined in Production Processes and Recipes.

Production must validate the availability of all mandatory inputs before consuming them.

An input is not available merely because a company can afford it, a supplier has promised it, a shipment has been dispatched, or an Industry Area contains a relevant resource.

It must be physically available and validly accessible under Inventory and Logistics rules.

### 7.2 Input Reservations

Where Inventory supports reservations, production orders must reference reservations for their required inputs before execution begins.

A reservation establishes a claim on available stock under Inventory's rules. It does not itself consume that stock.

Production must not create a second, independent reservation system or modify authoritative stock quantities directly.

If required reservations are missing, invalid, insufficient, expired, or no longer accessible, the order must be blocked or adjusted according to the applicable order and process rules.

### 7.3 Consumption

When production executes, the required input quantities are consumed through the Inventory System's authoritative consumption interface.

Production must not independently subtract quantities from an inventory record.

Input consumption and output generation must be associated with the same production execution so that the simulation can audit material transformations.

The transaction boundary must prevent an execution from consuming inputs while failing to record its corresponding physical result because of a partial state update.

The precise transactional implementation is an engineering decision, but the authoritative world state must never retain a partially committed execution.

### 7.4 Outputs

Production outputs may include:

- Primary goods.
- Secondary products.
- By-products.
- Recoverable scrap.
- Waste or unusable material.
- Energy or other process outputs represented as goods.

Output definitions and their material relationships belong to Production Processes and Recipes, with goods themselves defined by the Good and Service Catalogue.

Production must record each output associated with a completed execution.

Valid physical outputs must be passed to Inventory for registration at the appropriate location, with the correct quantity and relevant ownership and condition information.

Output creation must not automatically imply that the goods have been sold, delivered, exported, or accepted by a customer.

### 7.5 Yield and Losses

Actual output may differ from theoretical output because of process yield, material loss, spoilage, quality rejection, equipment performance, or other explicitly modelled conditions.

The process definition establishes the baseline transformation and applicable yields.

Production applies the relevant execution conditions and records the resulting quantities.

Losses must not be silently discarded. Where the simulation models waste or recoverable by-products, these must be recorded as outputs through the appropriate interfaces.

The system must distinguish between:

- Input material consumed.
- Usable primary output.
- Secondary output and by-products.
- Recoverable scrap.
- Waste or other non-usable material.
- Rejected output.

The detailed quantitative treatment belongs to the process specification rather than a generic assumption that every process converts inputs into one perfect output.

---

## 8. Workforce and Operational Resources

### 8.1 Workforce Requirements

Industry Data establishes the industry's workforce requirements and relationships to labour.

Production consumes the operational availability of the workforce assigned to a facility or production line.

Where workforce is divided into skills, occupations, or qualification levels, a facility may require specific combinations of labour categories.

The Production System must distinguish between:

- Required workforce.
- Available workforce.
- Assigned workforce.
- Workforce actually available for the production interval.

The number of employees associated with an Industry does not automatically establish that every employee is available at every facility.

Workforce allocation and employment authority remain with the relevant labour and economic systems.

### 8.2 Workforce Shortages

A workforce shortage may reduce effective capacity or prevent a process from operating.

The effect depends on the process requirements. A process that requires a minimum number of qualified operators may not be executable below that threshold, even when unused machine capacity remains.

Production must not assume that labour can be substituted freely between different roles or qualification levels.

Any skill substitution, automation effect, or productivity modifier must be explicitly defined by the applicable process, facility, or technology rules.

### 8.3 Utilities and Supporting Infrastructure

Production facilities may require electricity, fuel, water, transport access, communications, or other supporting services.

Where utilities are represented as physical goods, their availability and consumption must be reconciled with Inventory and the applicable resource-delivery systems.

Where utilities are delivered through networks, the relevant infrastructure system determines service availability and delivery constraints.

Production must not assume uninterrupted access to utilities simply because a facility is geographically connected to a network.

### 8.4 Maintenance

Facilities and production lines may require periodic maintenance or repair.

Maintenance can reduce capacity or make equipment temporarily unavailable.

Where maintenance consumes spare parts, fuel, tools, or other goods, the relevant quantities must be accounted for through Inventory.

Maintenance scheduling, deterioration, repair mechanics, and equipment-specific reliability are extensible operational features. They must integrate with the core capacity model rather than introduce a competing definition of available capacity.

---

## 9. Production Orders and Scheduling

### 9.1 Order Creation

A production order may originate from an authorised industrial decision, a company plan, government direction, military procurement requirement, or another supported demand source.

The originating system is responsible for the intent to produce or procure goods.

Production is responsible for validating and executing the resulting production order.

An order must identify:

- The responsible Industry.
- The requested outputs and quantities.
- An eligible process.
- An eligible facility or a rule for selecting one.
- The relevant scheduling priority.
- Any required deadline or timing constraints.
- Any applicable authorisation or restrictions.

An order that references an invalid or incompatible entity must be rejected or marked invalid. It must not be silently reassigned to an unrelated facility or process.

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
| `completed` | The requested quantity has been produced, subject to the defined completion and output-registration rules |
| `blocked` | An operational, legal, logistical, or other constraint prevents progress |
| `cancelled` | The outstanding order has been cancelled |
| `failed` | The order or execution has failed under an explicitly defined failure condition |

A state transition must preserve the order's execution history and completed quantities.

An order must not return to an earlier state in a way that duplicates already completed output or consumed inputs.

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

Priority may be influenced by the originating order, contractual deadlines, government direction, or other authorised rules.

The Production System must not independently decide the economic desirability of an order. It schedules eligible orders according to their authorised priorities and available capability.

The initial implementation should use a deterministic priority order and a stable identifier as a final tie-breaker where all other scheduling criteria are equal.

### 9.4 Partial Completion

An order may be completed over several execution intervals.

The system must preserve the quantity already completed and schedule only the remaining quantity.

Partial completion must not duplicate input consumption or output generation.

If an order is cancelled after partial completion, the completed quantity remains part of the authoritative production history. Only the outstanding quantity is cancelled, subject to any explicit cancellation or reversal rules.

---

## 10. Operating Constraints

Production is permitted only when all mandatory operational conditions are satisfied.

### 10.1 Facility Eligibility

The facility must be in an eligible operational state, commissioned where required, and compatible with the requested process.

A planned, uncommissioned, decommissioned, or otherwise ineligible facility must not produce.

A restricted facility may operate only within its defined restrictions.

### 10.2 Input Availability

All mandatory input requirements must be satisfied for the quantity to be executed.

Where inputs are insufficient, production may be blocked or limited to a quantity supported by the available inputs, provided the process permits partial execution.

Substitution is permitted only where explicitly supported by the process definition.

### 10.3 Capacity

The requested execution must fit within the effective capacity available during the selected interval.

An order cannot exceed the capacity of its assigned productive units.

### 10.4 Workforce

Required labour must be available and assigned in accordance with the applicable workforce rules.

If the required workforce falls below an essential operating threshold, execution must be blocked or reduced as defined by the relevant process.

### 10.5 Technology and Equipment

The facility must possess the technology, equipment, and process capabilities required to execute the requested transformation.

Research availability alone does not satisfy this requirement. Any required technology or equipment must be available to the relevant facility under the applicable adoption, installation, and ownership rules.

### 10.6 Utilities

Mandatory utilities must be available at the required quantities or service levels.

Where utilities are constrained, their effects must be applied consistently with the utility and infrastructure systems.

### 10.7 Location and Access

A facility must occupy a valid physical location and satisfy applicable site, access, environmental, and infrastructure constraints.

Changes in terrain, access, infrastructure, or territorial conditions may affect production where the relevant systems expose those changes.

Production does not independently calculate territorial ownership or movement permissions.

### 10.8 Legal and Governmental Restrictions

Production must respect applicable legal permissions, restrictions, embargoes, licensing rules, and government directives.

The authoritative rules belong to Government and Law.

Where authorisation is absent or revoked, Production must block or restrict execution as directed by the relevant interface.

### 10.9 Environmental and External Conditions

Environmental conditions, disasters, accidents, conflict, and other external events may affect facilities or production.

These effects must be received from the authoritative systems that simulate them.

Production is responsible for applying their defined operational consequences, not for maintaining duplicate models of weather, combat damage, or environmental geography.

---

## 11. Production Execution

### 11.1 Execution Principle

Production execution is the controlled process by which a valid production order consumes its required inputs and creates its defined outputs.

Every execution must be reproducible from the authoritative world state, applicable process definition, order parameters, and simulation interval.

Production must not generate output solely because a daily production target exists or because an Industry is expected to operate at a particular scale.

### 11.2 Execution Preconditions

Before an execution begins, the system must verify:

1. The production order is valid and eligible.
2. The assigned facility and production line can execute the process.
3. The facility is operational.
4. The process definition is valid and available.
5. Required technology and equipment are available.
6. Sufficient effective capacity has been allocated.
7. Required inputs are physically available and validly reserved where reservations are supported.
8. Workforce and mandatory utilities are sufficient.
9. Applicable permissions and restrictions permit execution.
10. The requested execution quantity is consistent with the order's outstanding quantity.

If any mandatory condition fails, the execution must not proceed as if successful.

The system should record a structured blocking reason so that the relevant system or player can identify and address the constraint.

### 11.3 Execution Sequence

A production execution follows this logical sequence:

1. **Resolve the order.** Identify the order, responsible Industry, facility, production line, process, and outstanding quantity.
2. **Evaluate constraints.** Determine eligibility, effective capacity, workforce, utilities, permissions, and other applicable restrictions.
3. **Determine executable quantity.** Establish the quantity that can be produced during the execution interval.
4. **Validate inputs.** Determine the required inputs for that quantity and validate their availability and reservations.
5. **Commit input consumption.** Consume the required physical inputs through the authoritative Inventory interface.
6. **Apply the process transformation.** Calculate the resulting primary outputs, secondary outputs, by-products, and losses according to the process definition and applicable conditions.
7. **Register outputs.** Pass the resulting physical goods to Inventory for authoritative registration.
8. **Update execution and order state.** Record quantities, capacity usage, completion status, and any remaining outstanding work.
9. **Emit system events.** Publish the relevant production and inventory events for dependent systems.

The implementation may combine these operations into a single transaction or use an equivalent transactional mechanism. The authoritative outcome must remain consistent if an execution is interrupted or fails.

### 11.4 Atomicity and Recovery

Input consumption and output registration must not result in duplicated or lost physical quantities because of a partial commit.

An execution must have a stable identifier that can be used to detect duplicate processing.

If the execution transaction fails before commitment, the world must not retain an unintended partial consumption or output.

If an execution has already committed, replaying the same execution identifier must not consume its inputs or create its outputs again.

The implementation must use transactional operations, idempotency controls, or an equivalent mechanism to enforce this invariant.

A failed execution must not be represented as a successful production event.

### 11.5 Production Result

A successful execution records the physical outcome of the process.

At minimum, it must identify:

- The production order and facility.
- The process and process-definition version.
- The simulation interval.
- The inputs consumed.
- The outputs produced.
- The capacity used.
- The resulting order state.
- Any applicable operational constraints or losses.

Production results are the basis for production statistics, inventory updates, operational reporting, and downstream economic accounting.

They must not directly overwrite the financial accounts of the responsible Industry or Company.

---

## 12. Simulation Timing and Daily Processing

Project Sovereign advances through a deterministic simulation timeline. Production must execute within that timeline and produce consistent results regardless of incidental program execution order.

### 12.1 Daily Production Cycle

The initial implementation uses a daily production cycle.

Within each daily cycle, the system must perform the following logical operations:

1. Resolve changes to facility states and externally supplied operational constraints.
2. Resolve workforce and utility availability for the production interval.
3. Revalidate eligible production orders and outstanding input requirements.
4. Determine available productive capacity.
5. Schedule eligible orders according to the documented scheduling policy.
6. Execute scheduled production using valid input stock and process definitions.
7. Register outputs through Inventory.
8. Update order states and execution records.
9. Publish completed production results and relevant events.
10. Reconcile production statistics and expose the resulting state to dependent systems.

This is a logical ordering contract. The implementation may use multiple internal phases or shared scheduling infrastructure, provided the resulting authoritative state is equivalent and deterministic.

### 12.2 Daily Processing and Other Systems

Production must operate against a clearly defined snapshot of the authoritative world state for the relevant interval.

If the simulation permits an upstream system to change inputs or constraints during the same day, the phase ordering must determine whether those changes affect that day's production or the next interval.

The initial implementation must not allow different facilities to receive inconsistent treatment merely because one was processed earlier than another.

Cross-system timing must be documented where it materially affects outcomes, particularly for:

- Resource extraction and subsequent processing.
- Power generation and electricity consumption.
- Goods delivery and input consumption.
- Construction and commissioning.
- Workforce changes.
- Infrastructure disruption.
- Government restrictions.
- Production output and downstream transportation.

### 12.3 Determinism

Given the same authoritative state, production orders, process definitions, and deterministic inputs, the Production System must produce the same outputs and state transitions.

Determinism requires:

- Stable ordering of production orders.
- Stable ordering of facilities and production lines.
- Explicit scheduling tie-breakers.
- Consistent quantity and rounding rules.
- No dependence on non-deterministic collection iteration.
- No use of unseeded randomness in authoritative production outcomes.
- Stable handling of execution retries and duplicate events.

Where stochastic events are later introduced, their random inputs must be controlled by the simulation's deterministic random system and recorded or reproducible from the authoritative simulation state.

### 12.4 Time and Quantity Precision

Production calculations must use defined units and precision rules.

Floating-point arithmetic must not introduce order-dependent discrepancies that create or destroy material quantities.

The implementation should use fixed-point quantities, rational representations, or another deterministic numerical method appropriate to the required scale.

Rounding must be applied at documented boundaries. Intermediate rounding must not silently accumulate into significant production gains or losses.

The detailed numerical standard may be established in the shared simulation-engine conventions, but all production and inventory interfaces must use compatible rules.

---

## 13. Resource Extraction and Industrial Production

Resource extraction is treated as a physical production activity, but it differs from the processing of existing goods.

Examples include mining ore, harvesting timber, extracting petroleum, and collecting agricultural output.

### 13.1 Extraction Sources

An extraction process must reference an appropriate physical resource source, such as a deposit, forest, agricultural area, or other resource-bearing location.

The relevant geographical and resource systems determine the source's physical existence, location, characteristics, and any applicable limits.

Production executes the extraction activity and generates the resulting goods.

An Industry Area alone must not create unlimited extractable resources.

### 13.2 Finite and Renewable Resources

The system must support both finite and renewable resources.

Finite resources may be depleted by extraction. Renewable resources may replenish according to their authoritative environmental or resource model.

The source model must define whether the resource is finite, renewable, or otherwise constrained.

Extraction must not exceed the amount physically available under the authoritative source rules.

The exact resource-depletion, regeneration, and environmental-impact algorithms belong to the resource and environmental specifications. Production must consume or update the source through their defined interfaces.

### 13.3 Extraction Capacity

Extraction remains subject to facility capacity, workforce, equipment, utilities, transport access, and process requirements.

The existence of a resource deposit does not guarantee that it can be extracted at any arbitrary rate.

A resource may be physically present but inaccessible, uneconomic to extract, legally restricted, or beyond the capability of the available equipment.

Production determines physical execution under the applicable constraints. Economy determines the financial implications of the activity.

---

## 14. Production and Industry Scale

Industry Scale is defined by Industry Data and describes the economic or organisational scale of an Industry according to that system's rules.

It must not be used as a direct substitute for installed capacity, effective capacity, or actual output.

Two industries with the same Industry Scale may have different production capacities because of differences in:

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

Industry Scale may be used by other systems to inform initial configurations, scenario setup, economic estimates, or establishment requirements. Any such relationship must be explicitly defined.

Production capacity must ultimately be grounded in the actual productive assets and process capabilities represented in the world.

The Production System must not silently multiply output because an Industry has been assigned a higher economic scale.

---

## 15. Technology and Production Capability

Production capabilities may depend on technologies, designs, equipment, and process definitions.

Research determines which technologies have been discovered or otherwise become available. The relevant implementation rules determine whether those technologies have been adopted, installed, and made operational at a particular facility.

Production validates these requirements before executing the associated process.

Technology may affect:

- Available production processes.
- Compatible equipment.
- Installed capacity.
- Production efficiency.
- Required input quantities.
- Material yield.
- Workforce requirements.
- Energy consumption.
- Quality or reliability.
- Maintenance requirements.

These effects must be explicitly represented in the relevant process, facility, or technology data.

A technology upgrade must not retroactively create goods, rewrite historical executions, or automatically change every facility in the world unless an explicit rule authorises that transition.

Changes to productive capability must take effect at a defined simulation boundary and preserve the integrity of existing orders and execution records.

---

## 16. Production Failures and Disruption

Production may be interrupted or prevented by operational failures.

The system must distinguish between an order that is waiting, an order that is temporarily blocked, and an execution that has actually failed.

### 16.1 Common Failure Categories

The initial model should support structured failure reasons including:

- `invalid_order`
- `invalid_process`
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

The exact enumeration may be extended as dependent systems are implemented. Failure reasons must remain structured and distinguishable rather than being represented solely by free-text descriptions.

### 16.2 Temporary Blockage

A temporary blockage prevents execution until a constraint is resolved.

Examples include missing inputs, unavailable workforce, a closed transport route, or a temporary power interruption.

The order may return to an eligible state when the relevant condition changes. Such a transition must not duplicate previously completed output or consumed inputs.

### 16.3 Equipment Failure and Damage

Equipment failure may reduce effective capacity or make a facility ineligible for a process.

The equipment, maintenance, and damage systems determine the authoritative condition and any recovery requirements.

Production applies the resulting constraints.

Where damage occurs during an execution interval, the simulation must use a defined event-ordering rule to determine whether the current execution completes, is interrupted, or fails.

### 16.4 Cancellation

A production order may be cancelled by an authorised actor or system.

Cancellation must preserve completed output and historical execution records.

Reserved inputs must be released or otherwise resolved through Inventory's reservation interface, subject to the relevant reservation rules.

Production must not independently return consumed inputs or reverse completed transformations unless a specific reversal or recovery process is defined.

---

## 17. Interfaces and Events

The Production System must expose explicit interfaces to dependent systems.

Exact implementation protocols are deferred to the software architecture, but the logical contracts below are mandatory.

### 17.1 Inputs from Industry Data

Production requires access to:

- Industry identifiers and Industry Types.
- Industry-to-Company relationships.
- Industry Scale where relevant.
- Industry Areas and associated locations.
- Establishment and operational prerequisites.
- Workforce requirements and relevant industry configuration.
- Starting-world industry configuration.

Industry Data remains authoritative for these records.

### 17.2 Inputs from Inventory

Production requires interfaces to:

- Check available stock.
- Validate stock locations and access.
- Validate and reference reservations.
- Consume inputs.
- Register outputs.
- Obtain authoritative consumption and registration results.
- Release reservations when appropriate.

Production must not maintain duplicate authoritative quantities.

### 17.3 Inputs from Production Processes and Recipes

Production requires access to:

- Process identifiers.
- Process-definition versions.
- Eligible inputs and their required quantities.
- Primary outputs and their quantities.
- Secondary outputs and by-products.
- Process yields and applicable losses.
- Required equipment and capabilities.
- Required technology.
- Process-level operating constraints.
- Unit definitions and conversion rules.

The process specification owns these definitions.

### 17.4 Inputs from Research

Production requires the relevant technology availability and adoption state.

It must be able to determine whether a facility possesses the capabilities required to execute a process under the applicable technology rules.

### 17.5 Inputs from Workforce and Infrastructure

Production requires the workforce, utilities, infrastructure, and other operational availability data exposed by their authoritative systems.

Where those systems are not yet implemented, Production must expose the dependency explicitly and avoid inventing permanent assumptions about unlimited labour, energy, or infrastructure.

### 17.6 Inputs from Government and Law

Production requires relevant permissions, prohibitions, and operational restrictions.

These must be evaluated using the applicable rules and the correct simulation timing.

### 17.7 Outputs to Inventory

Production emits authoritative requests or transactions for input consumption and output registration.

A successful production execution must be reconcilable with the resulting Inventory state.

### 17.8 Outputs to Economy

Production provides factual operational results to Economy, including:

- Completed production quantities.
- Input consumption.
- Production activity by Industry and facility.
- Recorded operational costs or resource usage where authoritative cost interfaces exist.
- Disruption and capacity information.
- Production completion events.

Economy determines the resulting financial transactions according to its own rules.

Production must not independently invent market prices, revenue, profit, taxation, or financial solvency.

### 17.9 Outputs to Logistics

Production exposes output availability and relevant location information so that Logistics can arrange storage, transfer, and delivery.

The existence of an output does not itself dispatch a shipment or establish that a destination has received it.

### 17.10 Outputs to Statistics and Reporting

Production should expose derived measures such as:

- Installed capacity.
- Effective capacity.
- Allocated capacity.
- Actual output.
- Capacity utilisation.
- Facility operational status.
- Production order backlog.
- Input shortages.
- Blocked production time.
- Recorded losses and rejected output.

These statistics must be derived from authoritative production records and must not become alternative sources of truth.

---

## 18. Production Invariants

The following invariants must hold across every valid simulation state.

### 18.1 Physical Output

Production cannot generate output without a valid production execution, except where another explicitly authorised system is responsible for creating the relevant physical stock.

### 18.2 Input Consumption

Every completed process execution must account for its required input consumption according to its process definition and execution conditions.

### 18.3 Inventory Authority

Production must not independently modify authoritative stock quantities, ownership, locations, or reservations outside the Inventory interface.

### 18.4 Capacity

Total capacity allocated to simultaneous executions must not exceed the applicable effective capacity of the relevant facility or production line.

### 18.5 Order Quantities

The completed quantity of an order must not exceed its requested quantity, except where an explicit process or order rule authorises overproduction and defines its treatment.

### 18.6 No Duplicate Execution

Reprocessing the same committed execution must not consume inputs or generate outputs a second time.

### 18.7 Facility Eligibility

An ineligible facility cannot execute a process that requires operational availability.

### 18.8 Process Validity

Every execution must reference a valid process definition and version.

### 18.9 Determinism

The same authoritative initial state and deterministic inputs must produce the same production outcome.

### 18.10 Industry Independence

Changing Industry Scale alone must not silently change a facility's installed capacity or generate physical output.

### 18.11 Ownership Integrity

Production must not silently change ownership of an Industry, facility, or output goods. Any ownership changes must be authorised and recorded through the appropriate system.

### 18.12 Conservation and Reconciliation

For each execution, input consumption and output generation must reconcile with the transformation rules defined by the process, including any explicitly modelled losses, by-products, and waste.

The simulation must not create or destroy physical quantities through undocumented bookkeeping effects.

---

## 19. Initial Implementation Scope

The initial Production System should prioritise a coherent and testable physical production loop over an unnecessarily elaborate industrial simulation.

### 19.1 Required for the Initial Implementation

The initial implementation must support:

- Stable Industry and facility identifiers.
- Facilities associated with Industry instances.
- Facility lifecycle and operational states.
- Installed capacity and effective capacity.
- A basic production-line abstraction or equivalent facility-level execution.
- Production orders and deterministic scheduling.
- Process compatibility validation.
- Input availability checks.
- Inventory reservations and consumption through the appropriate interface.
- Execution of defined production processes.
- Output registration through Inventory.
- Partial order completion.
- Structured blocking and failure states.
- Daily production processing.
- Deterministic execution and duplicate-execution protection.
- Production history and basic operational statistics.
- Integration with starting-world industry and facility data.

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
- Emergency production, mobilisation, and government-directed industrial priorities.

These features must extend the established production interfaces rather than introduce competing ownership of capacity, inventory, industry identity, or physical output.

---

## 20. Validation and Acceptance Criteria

The Production System is considered functionally valid when the following conditions can be demonstrated through deterministic tests.

### 20.1 Basic Production

Given an operational facility, a valid production order, a compatible process, sufficient inputs, and available capacity, the system executes production and registers the expected outputs.

### 20.2 Insufficient Inputs

Given an otherwise valid order with insufficient input stock, the system does not consume unavailable quantities or generate the unsupported output.

### 20.3 Capacity Constraint

Given several eligible orders competing for limited capacity, the scheduler allocates no more than the available capacity and produces deterministic results.

### 20.4 Facility Suspension

Given a suspended facility, no prohibited production execution occurs.

### 20.5 Workforce Constraint

Given a process requiring a defined minimum workforce, the system blocks or restricts production when the required labour conditions are not met.

### 20.6 Technology Constraint

Given a process that requires an unavailable technology or capability, the system rejects or blocks execution without creating output.

### 20.7 Partial Completion

Given an order that cannot be completed in one interval, the system records completed quantities and continues only with the remaining outstanding quantity.

### 20.8 Inventory Reconciliation

Given a successful production execution, input consumption and output registration are reconciled through Inventory, without duplicate or missing quantities.

### 20.9 Execution Recovery

Given a failed or interrupted execution, retrying or replaying the same committed execution cannot duplicate consumption or output.

### 20.10 Resource Extraction

Given a finite resource source with a known remaining quantity, extraction cannot exceed the quantity available under the authoritative resource rules.

### 20.11 Industry Scale Independence

Given two otherwise identical facilities, changing only their Industry Scale does not change physical output unless a separately defined rule explicitly modifies their productive configuration.

### 20.12 Deterministic Replay

Given identical starting state, production orders, process definitions, and deterministic external inputs, repeated simulation runs produce identical production records and final authoritative state.

### 20.13 Ownership and Location

Given a completed production order, the resulting goods are registered at the correct location and retain ownership according to the authoritative ownership rules.

### 20.14 Order Cancellation

Given a partially completed order that is cancelled, completed outputs remain recorded, outstanding work is cancelled, and unused reservations are handled through Inventory.

---

## 21. Dependencies and Related Specifications

The Production System depends on the following documents and interfaces:

| Specification | Relationship |
|---|---|
| `docs/systems/economy/economy-system.md` | Financial and economic authority |
| `docs/systems/economy/industry-data.md` | Industry identity, configuration, scale, establishment, workforce requirements, and starting-world industry data |
| `docs/systems/production/production-processes-and-recipes.md` | Process transformations, input quantities, output quantities, yields, and process requirements |
| `docs/systems/inventory/inventory-system.md` | Authoritative physical stock, reservations, consumption, output registration, and transfers |
| `docs/reference/economy/good-and-service-catalogue.md` | Canonical definitions of goods referenced by processes and Inventory |
| Research specifications | Technology availability, adoption, and capability requirements |
| Workforce and population specifications | Labour availability, assignment, and qualification |
| Terrain and geography specifications | Physical facility locations, site conditions, and spatial constraints |
| Infrastructure and logistics specifications | Utilities, transport access, and physical delivery |
| Government and law specifications | Legal permissions, restrictions, and government directives |
| Construction specifications | Facility construction, installation, commissioning, and upgrades, where implemented |

These documents should be treated as the authoritative owners of their respective domains. Production references their data and interfaces rather than redefining them.

---

## 22. Explicitly Unresolved Design Decisions

The following decisions require resolution during implementation or in their respective specifications. They must not be settled through undocumented assumptions.

1. **Capacity calculation:** The precise algorithm for combining simultaneous capacity constraints, including bottleneck and non-linear effects.
2. **Scheduling policy:** The initial priority model, pre-emption rules, and treatment of competing deadlines.
3. **Numerical representation:** The shared fixed-point or equivalent deterministic arithmetic standard and its rounding boundaries.
4. **Transaction implementation:** The precise mechanism for atomic input consumption, output registration, execution retries, and recovery.
5. **Process execution granularity:** Whether individual production lines, facilities, or larger industrial units are the primary execution boundary for each process class.
6. **Construction integration:** The detailed rules for construction costs, material consumption, workforce allocation, commissioning, and capacity upgrades.
7. **Workforce integration:** The exact interfaces for labour availability, skill matching, shift patterns, and productivity.
8. **Utility integration:** The precise treatment of electricity, fuel, water, and other networked or consumable utilities.
9. **Resource integration:** The detailed resource-source interfaces for extraction, depletion, regeneration, and environmental constraints.
10. **Maintenance and deterioration:** The algorithms for equipment reliability, maintenance scheduling, wear, repair, and downtime.
11. **Quality and spoilage:** The quantitative treatment of quality, defective goods, rejected output, and storage-related losses.
12. **Multi-output processes:** The exact representation of joint outputs, by-products, waste, and process-specific yield variation.
13. **Starting-world calibration:** The initial facility configurations, installed capacities, and operational states appropriate to the 1955 starting world.
14. **Service production:** The treatment of services whose outputs are not conventional physical inventory goods, including their capacity and delivery semantics.

Each decision should be resolved in the owning specification or a documented implementation decision before the corresponding feature becomes authoritative.

---

## 23. Final Rule

The Production System converts authorised production activity into physical outcomes.

Industry Data establishes the industries. Economy governs financial decisions. Production determines whether facilities can execute a process and performs the physical transformation. Production Processes and Recipes define the transformation. Inventory records the authoritative physical stock. The Good and Service Catalogue defines the goods those systems reference.

No industry, company, production order, facility, or economic forecast can bypass these rules to create physical output without a valid and auditable production execution.
