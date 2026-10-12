# Production Processes and Recipes

**Canonical Path:** `docs/systems/production/production-processes-and-recipes.md`  
**System:** Production  
**Document Type:** Systems Specification  
**Status:** Foundational Specification  
**Starting World Date:** 1955

---

## 1. Purpose

The Production Processes and Recipes system defines the transformations through which industries convert inputs into goods, services, intermediate products, co-products, and waste.

It specifies the relationship between a production process, its required inputs, its outputs, its operating requirements, and the quantitative rules governing execution.

A process defines a type of transformation. A recipe defines a particular quantitative configuration of that transformation. Production executes the selected recipe through an eligible facility, subject to available capacity, inputs, workforce, technology, infrastructure, and other operating constraints.

The system must support a world in which production can range from relatively simple extraction and manufacturing to complex industrial supply chains involving multiple stages, intermediate products, specialised equipment, and technologically advanced processes.

Production Processes and Recipes must be sufficiently general to represent the world's industrial economy without requiring bespoke code for each industry, company, facility, or product.

### 1.1 Core Principle

**A production process defines how resources are transformed. A recipe defines the quantities and conditions of that transformation. Production determines whether and when the transformation can execute. Inventory records the resulting physical stock.**

No process or recipe may independently create, destroy, consume, reserve, or transfer inventory outside the authorised Production and Inventory execution interfaces.

---

## 2. System Boundaries

Production Processes and Recipes owns the definitions and quantitative rules governing production transformations.

It does not own the broader systems that determine whether production is economically desirable, operationally possible, or physically deliverable.

| System | Responsibility |
|---|---|
| Economy System | Company finances, capital allocation, operating expenditure, prices, revenues, and financial decisions |
| Industry Data | Industry Types, industry instances, Industry Scale, establishment prerequisites, Industry Areas, and workforce requirements |
| Production System | Facilities, production lines, capacity, production orders, scheduling, execution coordination, and operational status |
| Production Processes and Recipes | Transformation definitions, recipe configurations, input and output quantities, yields, process requirements, by-products, and losses |
| Inventory System | Authoritative stock quantities, reservations, transfers, consumption, delivery, and inventory adjustments |
| Good and Service Catalogue | Canonical definitions and identifiers for goods and services referenced by recipes |
| Research System | Technologies, technical knowledge, and research-dependent process availability |
| Government System | Legal permissions, restrictions, and applicable policy constraints |
| Workforce and Population | Workforce availability, occupational capabilities, and labour-related inputs to production eligibility |
| Logistics and Transportation | Movement of inputs and outputs between locations and the accessibility of supply routes |
| Terrain and Geography | Physical location, terrain, environmental conditions, and geographically determined constraints |
| Military System | Military requirements, acceptance criteria, and applicable specifications for military products |

The interfaces described in this document are contractual boundaries. They do not imply that every related system must be fully implemented before the foundational production model can operate.

### 2.1 Ownership Rules

1. Recipes reference goods and services by canonical catalogue identifier. They do not independently define products.
2. Recipes reference Industry Types and process capabilities where required. They do not redefine Industry Data.
3. Recipes do not establish facility capacity. Production determines whether the required capacity exists and is available.
4. Recipes do not establish financial feasibility. Economy determines whether expenditure is authorised and affordable.
5. Recipes do not establish stock availability. Inventory determines whether the required quantities can be reserved and consumed.
6. Recipes do not grant technological, legal, or military permissions.
7. Recipes must not duplicate quantities or outcomes already recorded by an authoritative system.
8. Production execution must use a specific, validated recipe version so that completed production remains reproducible.

---

## 3. Core Concepts

### 3.1 Production Process

A Production Process is a defined transformation that converts specified inputs into specified outputs under a set of technical and operational conditions.

Examples include:

- Extracting iron ore from a mineral deposit.
- Refining crude oil into petroleum products.
- Converting timber into sawn lumber.
- Smelting iron ore into pig iron.
- Converting pig iron into steel.
- Manufacturing steel plate from steel.
- Assembling machinery from manufactured components.
- Generating electricity from coal.
- Providing a service through the application of labour, equipment, and other resources.

A process represents a category of transformation that may have multiple recipe configurations.

### 3.2 Recipe

A Recipe is a versioned, quantitative definition of a Production Process.

It specifies the inputs required and the outputs expected for a defined execution quantity or production rate.

Two recipes may implement the same process while differing in:

- Input materials.
- Input quantities.
- Output quantities.
- Yield and loss rates.
- Required technology.
- Required equipment capabilities.
- Permitted substitutions.
- Operating conditions.
- By-products and waste.
- Production mode.
- Effective dates.

A recipe may represent a historically appropriate production method, an alternative industrial technique, or an improved method made possible by research.

### 3.3 Recipe Version

A Recipe Version is an immutable configuration of a recipe at a particular point in the simulation's rules and history.

Once a recipe version has been used in an executed production order, its defining parameters must not be silently changed.

Corrections or improvements must create a new version. Existing production orders retain the version with which they were validated, unless an explicit, auditable migration process is performed.

This allows historical production, industrial statistics, save games, and economic outcomes to remain reproducible.

### 3.4 Process Capability

A Process Capability is a technical ability required to perform a process or recipe.

Examples include:

- Iron smelting.
- Petroleum refining.
- Precision machining.
- Chemical synthesis.
- Electrical generation.
- Food preservation.
- Ship assembly.

Capabilities may be required by a process, a recipe, a facility, or a combination of these.

A capability is not itself a technology, facility, or industry. It is a requirement that those systems can reference when determining eligibility.

### 3.5 Input

An Input is a resource required by a recipe to perform its transformation.

Inputs may include:

- Raw materials.
- Intermediate goods.
- Components.
- Consumable chemicals.
- Fuel.
- Water.
- Electricity.
- Packaging materials.
- Other catalogue-defined goods or services.

Inputs must reference canonical catalogue identifiers wherever the resource is represented in the Good and Service Catalogue.

### 3.6 Output

An Output is a result of executing a recipe.

Outputs may include:

- Primary products.
- Secondary products.
- Co-products.
- Recoverable materials.
- Waste products.
- Process losses.
- Emissions or other environmental effects, where supported by the relevant simulation systems.

Only outputs represented as physical goods may directly create physical inventory. Services and non-inventory effects must be recorded through their appropriate system interfaces.

---

## 4. Process Classification

Processes should be classifiable by their general transformation type without requiring a separate simulation subsystem for every industrial sector.

A process may have more than one classification where necessary.

### 4.1 Transformation Categories

| Category | Description | Example |
|---|---|---|
| Extraction | Removes resources from a natural deposit or source | Iron ore mining |
| Agriculture | Converts land, biological inputs, and labour into agricultural output | Wheat cultivation |
| Animal Production | Produces animal-derived goods | Livestock farming |
| Refining | Separates or transforms a feedstock into usable products | Petroleum refining |
| Chemical Processing | Produces substances through chemical transformations | Fertiliser production |
| Material Processing | Converts raw materials into intermediate materials | Steelmaking |
| Fabrication | Shapes, cuts, joins, or otherwise processes materials | Steel plate fabrication |
| Assembly | Combines components into a finished product | Machinery assembly |
| Construction | Produces a completed structure or infrastructure asset | Bridge construction |
| Energy Generation | Converts fuel or other energy sources into usable energy | Coal-fired electricity |
| Maintenance and Repair | Restores or maintains the functional condition of an asset | Locomotive overhaul |
| Service Provision | Produces a non-physical service through labour, equipment, or other resources | Commercial transport service |
| Recycling and Recovery | Recovers usable materials from waste or previously used goods | Scrap metal recovery |

These categories provide a classification framework. They do not require separate hard-coded execution logic for every category.

### 4.2 Production Modes

A recipe must identify its execution mode where that mode affects how quantities and execution are interpreted.

Supported conceptual modes are:

- **Batch:** A defined quantity of inputs is processed as a discrete production run.
- **Continuous:** Inputs are processed and outputs generated at a defined rate over time.
- **Discrete Assembly:** A defined set of components is combined into a number of individual units.
- **Extraction:** A quantity of a resource is removed from a source, potentially subject to finite reserves or regeneration.
- **Service:** A defined service quantity is produced over a specified amount of time or other measurable unit.
- **Construction:** Inputs are transformed into a persistent physical asset through a potentially extended execution process.

The initial implementation may execute these modes through a shared recipe and production-order framework. Specialised scheduling or simulation behaviour may be introduced where required.

The mode describes the transformation's execution semantics. It does not independently determine production scheduling or facility capacity.

---

## 5. Production Process Data Model

Each Production Process must have a stable identifier and a definition that describes its transformation and eligibility requirements.

### 5.1 Required Process Fields

| Field | Description |
|---|---|
| `process_id` | Stable, unique process identifier |
| `name` | Human-readable process name |
| `description` | Explanation of the transformation |
| `process_category` | Primary transformation classification |
| `supported_modes` | Production modes supported by the process |
| `required_capabilities` | Capabilities required for the process, where applicable |
| `applicable_industry_types` | Industry Types permitted to use the process, where applicable |
| `input_constraints` | Process-level constraints on acceptable inputs |
| `output_constraints` | Process-level constraints on permissible outputs |
| `technology_requirements` | Technical prerequisites, where applicable |
| `environmental_constraints` | Environmental or geographical conditions relevant to execution |
| `status` | Whether the process definition is available for use |
| `metadata` | Optional classification and descriptive information |

The fields above describe the conceptual data contract. The final storage representation may differ according to implementation architecture.

### 5.2 Process Eligibility

A process may be restricted to certain Industry Types, capabilities, technologies, locations, or operating conditions.

These restrictions define whether the process is eligible for consideration by Production.

They do not automatically authorise a particular facility to execute a recipe. Production must validate the facility, capacity, operational status, and all other applicable constraints.

Where no Industry Type restriction exists, the process may be available to any otherwise eligible industry.

---

## 6. Recipe Data Model

Each recipe must identify the process it implements, the version of the recipe, and the quantitative rules governing the transformation.

### 6.1 Required Recipe Fields

| Field | Description |
|---|---|
| `recipe_id` | Stable identifier for the logical recipe |
| `recipe_version` | Immutable version identifier |
| `process_id` | Identifier of the Production Process implemented |
| `name` | Human-readable recipe name |
| `description` | Explanation of the recipe configuration |
| `production_mode` | Batch, continuous, discrete assembly, extraction, service, or construction |
| `basis_quantity` | Reference execution quantity or rate |
| `basis_unit` | Unit associated with the reference quantity |
| `inputs` | Defined recipe input requirements |
| `outputs` | Defined recipe output results |
| `required_capabilities` | Additional capabilities required by the recipe |
| `required_equipment` | Equipment or equipment-class requirements, where applicable |
| `technology_requirements` | Recipe-specific technological prerequisites |
| `operating_constraints` | Permitted operating conditions |
| `valid_from` | Earliest simulation date on which the recipe is valid |
| `valid_until` | Optional final date of validity |
| `status` | Draft, validated, active, retired, or otherwise defined lifecycle state |
| `provenance` | Source, assumptions, or design rationale for the recipe |

A recipe must not become available to production execution merely because its record exists. It must pass validation and satisfy the applicable availability rules.

### 6.2 Recipe Identity and Versioning

The logical `recipe_id` identifies the recipe across its lifetime. `recipe_version` identifies a specific set of parameters.

A new version is required when a change materially alters the transformation, including:

- Input or output quantities.
- Yield or loss calculations.
- Required technology.
- Required capabilities.
- Equipment compatibility.
- Process mode.
- Substitution rules.
- Operating constraints.

Changes to descriptive metadata alone need not create a new version unless that metadata affects simulation behaviour.

Executed production orders must retain a reference to the exact recipe version used for execution.

---

## 7. Recipe Inputs

Inputs define the resources required to execute a recipe.

Each input must specify the resource, the required quantity relative to the recipe basis, and the manner in which the resource participates in the transformation.

### 7.1 Input Data Fields

| Field | Description |
|---|---|
| `input_id` | Identifier unique within the recipe version |
| `good_or_service_id` | Canonical catalogue identifier |
| `quantity` | Required quantity per recipe basis |
| `unit` | Unit of measurement |
| `input_role` | Functional role of the input |
| `consumption_mode` | Whether the input is consumed, retained, or otherwise handled |
| `substitution_group` | Optional identifier for permitted alternatives |
| `quality_constraints` | Required grade, purity, specification, or condition |
| `minimum_quantity` | Optional minimum required quantity |
| `maximum_quantity` | Optional maximum permitted quantity |
| `notes` | Optional explanatory information |

The schema is conceptual and may be normalised in implementation.

### 7.2 Input Roles

Inputs may fulfil different roles.

| Role | Description |
|---|---|
| Feedstock | Primary material transformed by the process |
| Component | A part incorporated into an assembled product |
| Consumable | A resource consumed during execution but not necessarily incorporated into the primary output |
| Fuel | A resource used to supply energy to the process |
| Utility | A required service or resource such as electricity, water, or compressed air |
| Catalyst | A substance that facilitates a reaction and is not intended to be consumed in proportion to the output |
| Auxiliary Material | A supporting material required by the process |
| Packaging | Material used to package or protect the output |
| Service Input | A catalogue-defined service required for execution |

The input role describes the resource's function. It does not replace the consumption mode or determine how Inventory records the resource.

### 7.3 Consumption Modes

The recipe must distinguish between resources that are consumed and resources that are required without being fully consumed.

Conceptual consumption modes include:

- `consumed`: the specified quantity is consumed during execution.
- `incorporated`: the specified quantity becomes part of an output, subject to the process's transformation rules.
- `reusable`: the resource remains available after execution, subject to wear or degradation where modelled.
- `returned`: some or all of the input is returned as a defined output.
- `service_consumed`: a service quantity is used, with no assumption that a physical stock item is consumed.

These modes describe recipe semantics. Inventory remains authoritative for physical stock and the recording of consumption, returns, and transfers.

A reusable catalyst, for example, must not be consumed in full for every batch merely because it appears as a recipe input. Its availability and any degradation must be handled according to the declared semantics and the relevant execution rules.

### 7.4 Input Quality

A recipe may require a particular material grade, purity, condition, specification, or other measurable quality.

Where the catalogue supports quality attributes, recipe constraints must reference those attributes rather than create an independent definition of the good.

Production must validate whether available input stock meets the required specification before execution.

If multiple grades or specifications are permitted, the recipe must state the permitted alternatives and any associated quantity or yield adjustments. If these adjustments are not explicitly defined, the alternative must not be assumed equivalent.

---

## 8. Recipe Outputs

Outputs define the results of executing a recipe.

A recipe must identify its expected primary outputs and any secondary outputs, recoverable materials, by-products, or waste that the simulation chooses to represent.

### 8.1 Output Data Fields

| Field | Description |
|---|---|
| `output_id` | Identifier unique within the recipe version |
| `good_or_service_id` | Canonical catalogue identifier, where applicable |
| `quantity` | Expected quantity per recipe basis |
| `unit` | Unit of measurement |
| `output_role` | Primary product, co-product, by-product, recovered material, or other supported result |
| `output_condition` | Expected grade, quality, or condition |
| `yield_rule` | Rule used to calculate the output quantity |
| `destination_type` | Intended handling category for the result |
| `recoverability` | Whether the output is intended to be recovered or reused |
| `notes` | Optional explanatory information |

A physical output must reference a canonical good in the Good and Service Catalogue.

A service output must reference a canonical service where such a definition exists.

An environmental effect or non-inventory result must be sent to the relevant system interface rather than represented as an invented physical good.

### 8.2 Output Roles

| Role | Description |
|---|---|
| Primary Product | The principal intended output |
| Co-product | A secondary product deliberately produced alongside the primary product |
| By-product | A secondary result generated as a consequence of the process |
| Recovered Material | A material returned to usable circulation |
| Waste | A residual output that is not treated as a usable product by the applicable process definition |
| Emission | A modelled environmental release |
| Service Output | A non-physical service produced by execution |
| Asset Output | A persistent asset produced by a construction or assembly process |

An output's role does not determine its economic value. Economy determines prices, revenue, and financial treatment.

### 8.3 Multiple Outputs

A recipe may produce multiple outputs from one execution.

For example, a refining process may produce several petroleum products from a single feedstock. A manufacturing process may generate scrap that can be recovered through a separate process.

The recipe must define the quantities or calculation rules for each represented output.

Production must not assume that the primary output is the only result of execution.

Where outputs depend on variable operating conditions, the relevant parameters and calculation rules must be explicit. Undefined variability must not be introduced as an undocumented random adjustment.

---

## 9. Units and Quantities

Recipes must use explicit, compatible units of measurement.

The unit system must support the quantities needed to model extraction, agriculture, manufacturing, energy, services, and other productive activities.

Examples include:

- Mass: kilograms and tonnes.
- Volume: litres and cubic metres.
- Count: individual units, components, or animals.
- Length: metres and kilometres.
- Area: square metres and hectares.
- Time: seconds, hours, shifts, and days.
- Energy: joules and kilowatt-hours.
- Power: watts and kilowatts.
- Service quantity: defined units of service delivery.

The Good and Service Catalogue and shared measurement conventions must determine the canonical unit definitions and permitted conversions.

### 9.1 Quantity Basis

Every recipe must state the reference quantity against which its inputs and outputs are calculated.

Examples:

- Inputs required to produce 1 tonne of steel.
- Outputs generated by processing 1 tonne of crude oil.
- Components required to assemble 1 machine.
- Electricity consumed per hour of operation.
- Labour or service quantities required per completed unit.

The basis quantity must use a declared unit and must not be inferred from the recipe's name or description.

### 9.2 Unit Compatibility

Quantities may only be combined or compared where their units are compatible or a valid conversion is defined.

Mass must not be directly compared with volume without a valid conversion rule. Such conversions may depend on material density or other properties.

Energy and power must not be treated as interchangeable. A power requirement expressed as a rate must be integrated over time when calculating total energy consumption.

Count-based outputs must not be silently converted into mass-based outputs without an explicit conversion rule.

### 9.3 Quantity Scaling

For a recipe with a fixed reference basis, quantities scale according to the requested execution quantity unless the recipe explicitly defines a nonlinear or conditional relationship.

For a linearly scalable input:

\[
Q_{\text{required}} =
Q_{\text{basis input}}
\times
\frac{Q_{\text{execution}}}{Q_{\text{basis}}}
\]

For a linearly scalable output:

\[
Q_{\text{output}} =
Q_{\text{basis output}}
\times
\frac{Q_{\text{execution}}}{Q_{\text{basis}}}
\]

Where:

- \(Q_{\text{basis input}}\) is the recipe's input quantity at its reference basis.
- \(Q_{\text{basis output}}\) is the recipe's output quantity at its reference basis.
- \(Q_{\text{execution}}\) is the requested execution quantity expressed in the same compatible unit as the basis.
- \(Q_{\text{basis}}\) is the recipe's reference quantity.

These equations apply only where the recipe defines linear scaling. Fixed setup requirements, minimum batch sizes, discrete components, nonlinear yields, and other constraints must be represented explicitly.

### 9.4 Precision and Rounding

Intermediate calculations must preserve sufficient precision to avoid cumulative drift across repeated production cycles.

Rounding must occur at defined system boundaries, not arbitrarily during each intermediate calculation.

The authoritative storage precision, rounding mode, and handling of fractional physical quantities must be defined by the shared numerical and Inventory contracts.

Until those rules are established, recipe implementations must not assume that every good can be produced or consumed in arbitrary fractional quantities.

---

## 10. Yields, Losses, and Recovery

Recipes must define the relationship between the quantity processed and the quantity of each resulting output.

Yield describes the proportion or quantity of an input that becomes a specified output under defined conditions.

Loss describes material or other process quantities that do not become the intended output.

Recovery describes material that would otherwise be lost or discarded but is instead returned as a usable output.

### 10.1 Yield Models

A recipe may use one or more of the following conceptual yield models:

- **Fixed Output:** A specified quantity of output for a defined execution basis.
- **Proportional Yield:** Output quantity varies in proportion to a specified input.
- **Conversion Yield:** A defined fraction of a feedstock is converted into a specified product.
- **Multi-Output Yield:** One or more outputs are calculated from the same feedstock according to a defined allocation or conversion rule.
- **Conditional Yield:** Yield varies according to explicitly defined input quality, technology, equipment, operating conditions, or other parameters.
- **Discrete Yield:** Output is determined in whole units, batches, or other indivisible quantities.

A recipe must identify the applicable yield model whenever output quantity cannot be derived unambiguously from a fixed basis.

### 10.2 Losses

Losses may include:

- Processing losses.
- Material rejected for failing quality requirements.
- Evaporation or other material loss.
- Unrecoverable residues.
- Energy conversion losses.
- Other explicitly modelled process inefficiencies.

A loss must not be silently deducted from an output unless the recipe's quantitative definition accounts for it.

Where losses produce a physical residual that the simulation tracks, the residual must be represented as an output. Where losses are non-material, they must be represented through the relevant quantitative rule or system interface.

### 10.3 Recovery

Recoverable materials must be represented explicitly when they affect subsequent production or inventory.

For example, a steelmaking process may generate scrap that can be returned to a suitable recycling process.

Recovery must not automatically restore the original input in full unless the recipe explicitly defines that outcome.

Recovered materials may have different quantities, qualities, or eligibility requirements from the original input.

### 10.4 Yield Validation

Recipe validation must detect obvious quantitative inconsistencies where the relevant physical relationships are known.

For example, a process that transforms a single material into several outputs should not unintentionally generate additional material without a defined source, conversion, or transformation rule.

However, a universal mass-balance equation must not be applied indiscriminately to every process. Energy generation, chemical reactions, agricultural growth, biological production, and services require appropriate domain-specific treatment.

Where a mass-balance or other conservation rule is applicable, the validation method and acceptable tolerance must be explicit.

---

## 11. Substitutions and Alternative Inputs

Some processes may accept multiple materials or alternative grades of the same material.

A recipe may define permitted substitutions where those substitutions are technically valid.

### 11.1 Substitution Rules

A substitution rule must specify:

- The input that may be replaced.
- The permitted alternative goods.
- Any applicable quality constraints.
- The permitted substitution quantity or ratio.
- Any associated yield, cost, or operating adjustments.
- Any technology or equipment prerequisites.

Substitution must be explicit. Production must not treat goods as interchangeable merely because they share a similar name, category, or intended use.

### 11.2 Substitution Groups

A substitution group may be used where several goods satisfy a shared functional requirement.

For example, a process may permit a defined set of fuels subject to different consumption rates or operating constraints.

The group itself does not guarantee equivalence. The recipe must define the permitted alternatives and how each alternative affects the transformation.

If an alternative has no defined conversion rule, it must not be substituted automatically.

### 11.3 Input Selection

The selection of a valid input alternative must be performed deterministically.

Where multiple alternatives are available, the selection rule must be explicit, such as a defined priority order or a rule supplied by Production.

Economy may influence procurement decisions, but recipe execution must not independently infer economic preferences or purchase resources.

---

## 12. Process and Recipe Prerequisites

Processes and recipes may impose technical requirements that must be satisfied before execution.

### 12.1 Technology Requirements

A process or recipe may require a particular technology, research result, or technical capability.

Research determines whether the relevant technology is available to the applicable entity or facility.

Production validates whether the required technology is available and applicable.

A recipe must not grant its own technological prerequisites merely because it is present in the database.

### 12.2 Equipment Requirements

A recipe may require equipment with specific capabilities.

Examples include:

- Furnaces capable of reaching a specified temperature.
- Machine tools capable of producing a required tolerance.
- Refining equipment capable of processing a particular feedstock.
- Power-generation equipment compatible with a defined fuel.
- Assembly equipment capable of handling a specified product.

Production determines whether the facility and its equipment satisfy those requirements.

The recipe defines the requirement; it does not create the equipment or establish facility capacity.

### 12.3 Operating Conditions

A recipe may require particular operating conditions, including:

- Temperature.
- Pressure.
- Material purity.
- Moisture content.
- Environmental conditions.
- Minimum or maximum operating rates.
- Other measurable technical constraints.

Only conditions supported by explicit data and calculation rules may affect execution.

Unimplemented conditions may be documented as future requirements but must not be treated as simulated constraints until their behaviour is defined.

### 12.4 Legal and Geographic Restrictions

Recipes may declare requirements that depend on legal permissions, environmental conditions, or geographic circumstances.

The relevant systems determine whether those requirements are satisfied.

A recipe does not independently grant a permit, establish a location, create a transport connection, or override a government restriction.

---

## 13. Batch and Continuous Transformations

Recipes must support discrete and rate-based transformations without conflating them.

### 13.1 Batch Production

A batch recipe defines a discrete production run.

The recipe may specify:

- Minimum batch quantity.
- Standard batch quantity.
- Maximum batch quantity.
- Setup requirements.
- Input quantities per batch.
- Output quantities per batch.
- Batch-specific losses.
- Whether partial batches are permitted.

If a recipe requires a minimum batch size, Production must enforce that requirement when creating and scheduling production orders.

The recipe must not assume that an incomplete batch produces a full batch's output.

Where partial batches are permitted, the scaling rule must be explicit.

### 13.2 Continuous Production

A continuous recipe defines inputs and outputs as rates over time.

Examples include:

- Tonnes of material processed per hour.
- Cubic metres of water treated per day.
- Kilowatt-hours generated over a period.
- Tonnes of petroleum products produced per day.

A rate-based recipe must specify the time unit and reference operating rate.

Production determines the period for which the recipe executes and the capacity available during that period.

The resulting quantities must be calculated using the actual execution duration and the applicable rate and operating conditions.

### 13.3 Discrete Assembly

Assembly recipes may define requirements per completed unit.

For example, one finished machine may require a specified number of engines, gear assemblies, electrical components, and fasteners.

If the required component count is indivisible, partial input quantities must not be rounded into a valid completed unit unless the recipe explicitly supports that behaviour.

Production must distinguish between completed units and incomplete work in progress where such intermediate states are modelled.

### 13.4 Long-Running Processes

Some transformations, particularly construction and complex assembly, may take place over an extended period.

The recipe defines the required inputs and expected results. Production defines scheduling, progress, capacity allocation, and execution state.

Inventory determines when inputs are reserved and consumed under the agreed execution contract.

A partially completed process must not automatically create the full final output.

Any intermediate output or work-in-progress representation must be explicitly defined by the relevant systems.

---

## 14. Recipe Execution Contract

Production Processes and Recipes provides the validated transformation definition that Production uses when executing an order.

The execution contract must make the intended quantities and outcomes unambiguous before execution begins.

### 14.1 Execution Inputs

Production supplies the following information, directly or through an execution record:

- The selected recipe identifier and version.
- The requested execution quantity or operating duration.
- The eligible facility and production line.
- The applicable operating conditions.
- The validated input quantities and selected alternatives.
- The relevant execution parameters.
- The simulation date or execution period.

The precise data structure is an implementation decision, but the information needed to calculate a deterministic result must be available.

### 14.2 Execution Calculation

The recipe calculation must determine:

1. The input quantities required for the requested execution.
2. Any minimum batch, discrete quantity, or scaling constraints.
3. The expected quantities of primary and secondary outputs.
4. Any defined process losses and recoverable materials.
5. Any applicable conditional yield adjustments.
6. Any relevant non-inventory effects supported by the simulation.
7. Whether the requested execution falls within the recipe's valid operating conditions.

The calculation must return a structured result that Production can validate before physical execution is committed.

### 14.3 Execution Results

The result should identify:

- The recipe version used.
- The requested and accepted execution quantities.
- The required input quantities.
- The expected output quantities.
- The expected by-products and recoverable materials.
- The expected losses and other supported effects.
- Any validation failures.
- Any warnings or unresolved conditions.

Production remains responsible for coordinating execution and reporting the final result to the appropriate systems.

### 14.4 No Independent Inventory Mutation

Recipe calculation must be separable from inventory mutation.

Calculating a recipe must not consume input stock or create output stock.

The recipe calculation may produce an execution plan containing required inputs and expected outputs. Production and Inventory then coordinate the authorised execution and record the resulting transactions.

This separation permits recipes to be validated, previewed, tested, and compared without changing the simulated world.

---

## 15. Execution Consistency and Failure Handling

The process and recipe definitions must support reliable execution even when an order cannot complete.

### 15.1 Pre-Execution Validation

Before an execution begins, Production must validate that:

- The process and recipe version exist.
- The recipe is valid for the simulation date.
- The recipe is available for use.
- The requested quantity conforms to the recipe's constraints.
- The relevant inputs and substitutions are defined.
- All required outputs have valid definitions or supported non-inventory handling.
- All required units and conversions are valid.
- The execution parameters satisfy the recipe's defined constraints.

Production must separately validate facility eligibility, capacity, available inputs, workforce, technology, utilities, legal permissions, and other operating requirements.

### 15.2 Failure Categories

Failures attributable to process or recipe definitions should be distinguishable from failures attributable to execution or other systems.

Examples include:

- `PROCESS_NOT_FOUND`
- `RECIPE_NOT_FOUND`
- `RECIPE_VERSION_NOT_FOUND`
- `RECIPE_NOT_VALIDATED`
- `RECIPE_NOT_ACTIVE`
- `RECIPE_NOT_YET_VALID`
- `RECIPE_EXPIRED`
- `INVALID_EXECUTION_QUANTITY`
- `INVALID_UNIT`
- `INCOMPATIBLE_UNITS`
- `UNDEFINED_CONVERSION`
- `INVALID_INPUT_DEFINITION`
- `INVALID_OUTPUT_DEFINITION`
- `UNDEFINED_SUBSTITUTION`
- `UNSATISFIED_RECIPE_CONSTRAINT`
- `UNSUPPORTED_YIELD_MODEL`
- `INVALID_RECIPE_CONFIGURATION`

These are proposed identifiers. The final error-code convention should be consistent across the simulation.

### 15.3 Partial Execution

If an order executes only partially, the outputs must correspond to the quantity actually processed, subject to the recipe's defined scaling and yield rules.

The system must not create the full expected output merely because an order was accepted or scheduled.

For a batch process that does not support partial batches, Production must enforce the batch constraint rather than treating a partial execution as a completed batch.

### 15.4 Determinism

Given the same recipe version, execution quantity, operating parameters, relevant inputs, and applicable simulation state, recipe calculations must produce the same result.

Any random or stochastic behaviour must be explicitly defined, use the simulation's authoritative randomisation mechanism, and remain reproducible under the save-and-reload contract.

Recipe calculations must not depend on hidden randomness, execution order, machine timing, or unrecorded external state.

### 15.5 Atomicity and Idempotency

Recipe calculation itself must be repeatable and must not mutate physical stock.

The Production and Inventory execution contract must ensure that retries cannot accidentally consume the same inputs or create the same outputs twice.

A repeated request to calculate an execution result must return the same result for identical inputs and parameters.

A repeated request to commit an execution must be handled according to the authoritative transaction and idempotency rules established by Production and Inventory.

---

## 16. Process Chains and Multi-Stage Production

Industrial supply chains frequently consist of multiple distinct processes.

For example:

Iron Ore → Pig Iron → Steel → Steel Plate → Manufactured Components → Finished Machinery

Each transformation should be represented by its own process and recipe where the stages have distinct inputs, outputs, technical requirements, or operational constraints.

### 16.1 Separation of Stages

A recipe should represent a coherent transformation rather than an entire industrial economy.

Multi-stage production must be represented through separate production orders or other explicitly defined execution structures.

The completion of one stage may create an output required by a subsequent stage. Inventory records that physical output, and the subsequent stage must obtain the necessary input through the normal Inventory interface.

The recipe system must not bypass intermediate stock, supply requirements, or production execution merely because two processes are related.

### 16.2 Intermediate Goods

Intermediate goods must use the same canonical identifiers as other goods in the Good and Service Catalogue.

A good may be the output of one recipe and the input of another.

Its role as an intermediate good is contextual. It does not require a separate product definition for every process in which it participates.

### 16.3 Process Dependencies

A process chain may declare dependencies or compatibility requirements where these are needed for planning or validation.

However, a recipe must not guarantee that its output will be available to a subsequent process. The availability of that output depends on actual production execution, inventory, location, and logistics.

### 16.4 Initial Scope

The foundational implementation should support multiple independent production orders connected through inventory.

More advanced features, including integrated multi-stage scheduling, automatic process-chain optimisation, and dynamic coordination across multiple facilities, may be added later without changing the basic process and recipe contract.

---

## 17. Historical Validity and Technological Change

Project Sovereign's industrial economy develops over simulation time. Production methods must therefore be capable of changing as technologies, capabilities, and operating conditions evolve.

### 17.1 Effective Dates

Recipes may define the earliest date on which they are valid and an optional expiry date.

The recipe's availability must be checked against the simulation date and applicable technology requirements.

A recipe that is valid in a later period must not automatically be available in the starting world of 1955.

### 17.2 Technological Improvements

A technological improvement may introduce a new process, create a new recipe version, or make an existing recipe eligible for additional facilities.

The choice depends on whether the improvement changes the transformation itself, the quantitative configuration, or only the availability of the method.

A technology that changes input consumption, output yield, operating rate, or required equipment must not silently alter an already executed recipe version.

### 17.3 Historical Reproducibility

Historical production records must retain the recipe version used at execution.

If a recipe is retired or superseded, historical records must remain interpretable.

Retiring a recipe must not retroactively invalidate completed production.

### 17.4 Starting-World Data

Recipes available at the 1955 starting date must be explicitly designated through their validity and availability rules.

The starting world must not automatically include every recipe defined in the database.

The initial industrial configuration must select only those processes and recipes intended to be available under the starting world's technology, industrial capabilities, and other applicable constraints.

---

## 18. Data Validation and Integrity

Recipe definitions must be validated before they can be activated for production.

Validation should detect structural errors, missing references, inconsistent quantities, and unsupported behaviour.

### 18.1 Structural Validation

A recipe must not be activated if:

- Its process identifier does not resolve.
- Its recipe version is missing or duplicated.
- A required input lacks a valid catalogue reference.
- A required physical output lacks a valid catalogue reference.
- A required quantity or unit is missing.
- A required unit conversion is undefined.
- Its production mode is unsupported.
- Its execution basis is invalid.
- A required capability or technology reference cannot be resolved.
- Its validity dates are contradictory.
- Its substitution or yield rules are incomplete.
- Its output calculation cannot be performed deterministically.

### 18.2 Quantitative Validation

Where applicable, validation must check:

- Whether input and output quantities use compatible units.
- Whether the basis quantity is positive and valid.
- Whether minimum and maximum quantities are consistent.
- Whether yield parameters fall within their permitted ranges.
- Whether output quantities can be calculated for the supported execution basis.
- Whether discrete quantities and batch constraints are respected.
- Whether defined loss and recovery rules are internally consistent.
- Whether relevant conservation constraints are satisfied within explicitly defined tolerances.

Validation must not reject a recipe solely because a simplistic universal material-balance assumption does not apply to its process category.

### 18.3 Reference Integrity

Recipe references must remain valid across catalogue and system updates.

Removing or retiring a catalogue item must not silently rewrite historical recipe definitions or completed production records.

Changes to referenced definitions must follow the relevant system's compatibility and versioning rules.

### 18.4 Activation

A recipe may be activated only when the required validation succeeds.

Draft or invalid recipes may be retained for authoring and testing, but Production must not execute them.

A recipe's active status does not override its effective dates, technological prerequisites, facility requirements, or other eligibility conditions.

---

## 19. System Interfaces

Production Processes and Recipes must expose a small, stable set of logical interfaces.

These are conceptual contracts rather than mandates for specific programming functions or API names.

### 19.1 Recipe Lookup

**Purpose:** Retrieve a specific recipe version.

**Inputs:**
- Recipe identifier.
- Recipe version.

**Outputs:**
- Recipe definition.
- Referenced process definition.
- Version and validity information.
- Validation and availability metadata.

### 19.2 Recipe Validation

**Purpose:** Determine whether a recipe definition is structurally and quantitatively valid.

**Inputs:**
- Recipe definition.
- Relevant catalogue references.
- Applicable measurement and conversion rules.

**Outputs:**
- Validation result.
- Structured errors and warnings.
- Identified missing or invalid references.

Validation must not modify inventory, production orders, finances, or world state.

### 19.3 Execution Calculation

**Purpose:** Calculate the required inputs and expected results for a requested execution.

**Inputs:**
- Recipe identifier and version.
- Execution quantity or duration.
- Applicable operating parameters.
- Selected input alternatives.
- Relevant supported conditions.

**Outputs:**
- Required input quantities.
- Expected output quantities.
- Defined losses and recoveries.
- Other supported execution effects.
- Validation result.

The calculation must not independently execute the recipe.

### 19.4 Process Eligibility Information

**Purpose:** Expose the technical requirements of a process or recipe so that Production can evaluate a facility's eligibility.

**Inputs:**
- Process or recipe identifier and version.

**Outputs:**
- Required capabilities.
- Equipment requirements.
- Technology requirements.
- Operating constraints.
- Other declared prerequisites.

Production remains responsible for evaluating these requirements against the facility and current simulation state.

### 19.5 Authoritative Events

The system may publish events such as:

- `process_created`
- `process_updated`
- `recipe_created`
- `recipe_validated`
- `recipe_activated`
- `recipe_retired`
- `recipe_version_created`

Execution events, such as production started or completed, are owned by Production.

Inventory transaction events are owned by Inventory.

The event names are illustrative and must follow the project's shared event naming and versioning conventions.

---

## 20. Deterministic Daily Processing

The recipe system primarily provides definitions and calculations. It does not independently run a daily production cycle.

During the simulation's production phase, Production may request recipe calculations for eligible production orders.

The conceptual interaction is:

1. Production identifies an eligible order and its selected recipe version.
2. Production validates the facility and operating conditions.
3. The recipe system calculates required inputs and expected results.
4. Inventory validates or reserves the necessary physical stock through the agreed interface.
5. Production executes the permitted quantity according to capacity and scheduling rules.
6. The authoritative execution result is committed through the Production and Inventory interfaces.
7. Production reports the actual completed quantity and outputs.
8. Relevant economic, statistical, and other systems receive the appropriate resulting records or events.

The exact ordering of reservations, consumption, and execution commitment must be defined by the Production and Inventory transaction contract.

The recipe system must not establish an independent daily schedule, bypass Production's capacity allocation, or create stock outside the authorised execution process.

---

## 21. Initial Implementation Scope

The first implementation should establish a functional, extensible transformation model before introducing highly specialised industrial simulation.

### 21.1 Required Initial Capabilities

The initial implementation should support:

- Stable process identifiers.
- Versioned recipe definitions.
- Catalogue-referenced inputs and outputs.
- Explicit quantity bases and units.
- Linear quantity scaling.
- Fixed-output and proportional-yield models.
- Multiple outputs per recipe.
- Explicit by-products and losses.
- Basic batch and rate-based execution.
- Input roles and consumption semantics.
- Basic technology and capability prerequisites.
- Recipe validity dates.
- Structural and quantitative validation.
- Deterministic execution calculations.
- Production integration without direct inventory mutation.
- Historical retention of executed recipe versions.
- Explicit failure codes for invalid definitions and calculations.

### 21.2 Deferred Capabilities

The following features may be introduced as the relevant simulation systems mature:

- Advanced chemical reaction modelling.
- Detailed thermodynamic and energy-balance simulation.
- Complex nonlinear yield curves.
- Dynamic quality degradation.
- Advanced multi-output optimisation.
- Automated recipe discovery and technological invention.
- Detailed process-control simulation.
- Dynamic substitution optimisation.
- Complex multi-stage production scheduling.
- Detailed work-in-progress simulation.
- Sophisticated environmental emissions and waste treatment.
- Advanced agricultural growth and biological production models.

Deferring these features must not require replacing the core process and recipe model.

---

## 22. Acceptance Criteria

The system is ready for initial integration when the following conditions are satisfied.

### 22.1 Definitions

- [ ] A Production Process can be created with a stable identifier and declared requirements.
- [ ] A Recipe can reference a Production Process and define an immutable version.
- [ ] Inputs and physical outputs reference canonical catalogue entries.
- [ ] Each recipe has an explicit execution basis and valid measurement units.
- [ ] Multiple outputs can be represented within a single recipe.
- [ ] Recipe validity dates and lifecycle states are supported.

### 22.2 Calculation

- [ ] A linearly scalable recipe calculates input requirements correctly.
- [ ] The same recipe calculates expected output quantities deterministically.
- [ ] A recipe can represent multiple outputs, including co-products and by-products.
- [ ] Defined losses and recoveries are represented without implicit quantity changes.
- [ ] Invalid units and undefined conversions are rejected.
- [ ] Batch constraints and discrete quantities are enforced where configured.
- [ ] Unsupported yield models fail explicitly rather than producing guessed results.

### 22.3 Integration

- [ ] Production can retrieve and validate a specific recipe version.
- [ ] Production can request an execution calculation without mutating world state.
- [ ] Recipe calculations do not independently consume inventory.
- [ ] Recipe calculations do not independently create inventory.
- [ ] Facility eligibility and production capacity remain under Production's authority.
- [ ] Stock availability remains under Inventory's authority.
- [ ] Technology and legal prerequisites are evaluated through their authoritative systems.
- [ ] Historical production records retain the recipe version used.

### 22.4 Determinism and Integrity

- [ ] Identical execution parameters produce identical calculation results.
- [ ] Recipe updates do not silently alter historical versions.
- [ ] Invalid recipe definitions cannot be executed.
- [ ] Failed calculations return structured errors.
- [ ] Repeated calculation requests do not create duplicate transactions or mutate state.
- [ ] Recipe definitions remain interpretable when later versions are introduced.

---

## 23. Unresolved Design Decisions

The following decisions should be resolved through shared system contracts or implementation testing rather than being assumed by this specification.

1. **Canonical units and conversions:** Define the authoritative measurement registry, conversion rules, and treatment of material-dependent conversions.
2. **Numerical precision:** Establish the shared numerical representation, rounding policy, and handling of fractional quantities.
3. **Recipe storage format:** Determine the concrete schema, normalisation strategy, and serialisation format.
4. **Version identifiers:** Determine whether recipe versions use sequential integers, semantic versions, immutable identifiers, or another scheme.
5. **Conditional yields:** Define the supported initial set of conditions and the representation of nonlinear relationships.
6. **Mass and energy validation:** Establish the scope and tolerances of conservation checks for each applicable process category.
7. **Batch execution:** Define how minimum batches, partial batches, setup losses, and interrupted batches are handled by Production.
8. **Continuous execution:** Define how operating rates interact with simulation time steps, downtime, and partial periods.
9. **Reusable inputs:** Establish the shared execution contract for reusable catalysts, tools, and other non-consumed resources.
10. **Quality representation:** Define how input and output grades, purity, condition, and specifications are represented in the catalogue and validated by Production.
11. **Work-in-progress:** Determine how partially completed transformations and long-running construction processes are represented.
12. **Non-inventory outputs:** Define interfaces for environmental effects, service delivery, and other results that are not physical stock.
13. **Recipe migration:** Establish whether and under what conditions an unexecuted production order may migrate to a newer recipe version.
14. **Starting recipe library:** Define the initial processes and recipes available in 1955, including their historical validity and technical prerequisites.
15. **Production transaction boundaries:** Finalise the interaction between recipe calculations, reservations, input consumption, execution, and output recording with the Production and Inventory specifications.

Until these decisions are resolved, implementations must preserve the ownership boundaries and deterministic behaviour defined in this document.

---

## 24. Governing Principle

Production Processes and Recipes defines the transformations available to the industrial economy. It does not independently determine whether those transformations are affordable, schedulable, legally permitted, or physically executable.

Industry Data establishes the industries. Economy governs financial decisions. Production determines the execution of productive activity. Production Processes and Recipes define the transformations and their quantitative rules. Inventory records authoritative physical stock. The Good and Service Catalogue defines the goods and services referenced throughout the simulation.

**No industrial output may be created merely because a recipe exists. Output must result from authorised production execution, with the resulting physical stock recorded through Inventory.**
