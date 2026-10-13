
# Production Processes and Recipes

**Canonical Path:** `docs/systems/production/production-processes-and-recipes.md`  
**System:** Production  
**Document Type:** Systems Specification  
**Status:** Foundational Specification  
**Starting World Date:** 1955

---

## 1. Purpose

The Production Processes and Recipes specification defines the transformations through which industries convert inputs into goods, services, intermediate products, co-products, recoverable materials, and waste.

It defines the technical and quantitative rules governing a transformation, including required inputs, expected outputs, yields, losses, substitutions, operating conditions, and prerequisites.

A Production Process defines a type of transformation. A Recipe defines a versioned quantitative configuration of that transformation. The Production System determines whether, when, where, and at what quantity the transformation may execute. The Inventory System records resulting changes to physical stock.

The system must support extraction, agriculture, energy generation, refining, manufacturing, assembly, construction, maintenance, recycling, and service provision without requiring bespoke execution logic for every industry, company, facility, or product.

### 1.1 Governing Principle

**A process defines the transformation. A recipe defines its quantitative rules. Production authorises and coordinates execution. Inventory records authoritative physical stock changes.**

A process or recipe must never independently create, destroy, consume, reserve, or transfer physical inventory. Recipe calculations must be separable from execution and must not mutate the simulated world.

---

## 2. System Boundaries

Production Processes and Recipes owns transformation definitions and their quantitative rules. It does not own the operational, financial, physical, or legal state that determines whether a transformation can execute.

| System | Authoritative responsibility |
|---|---|
| Economy System | Financial records, Company Capital, transactions, prices, operating expenditure, revenues, and financial obligations |
| Industry Data | Industry Types, Industry instances, Industry Scale, establishment configuration, Industry Areas, and industry-level requirements |
| Production System | Production facilities, production lines, capacity, production orders, scheduling, execution coordination, and production status |
| Production Processes and Recipes | Transformation definitions, recipe versions, input and output quantities, yields, process losses, substitutions, and technical process requirements |
| Inventory System | Physical stock quantities, stock lots, reservations, consumption, transfers, receipts, and other authorised inventory transactions |
| Good and Service Catalogue | Canonical product identifiers, classifications, attributes, and measurement conventions |
| Research System | Technologies, technical knowledge, and research-dependent availability |
| Government System | Treasury, legal permissions, restrictions, and government policy |
| Workforce and Population | Population, workforce availability, occupational capabilities, and labour characteristics |
| Logistics and Transportation | Physical movement of goods, transport capacity, routes, and delivery |
| Terrain and Geography | Locations, terrain, geography, environmental conditions, and geographically determined constraints |
| Military System | Military requirements, acceptance criteria, and military-specific product specifications |

These are ownership boundaries, not a requirement that every system be fully implemented before foundational production can operate.

### 2.1 Ownership Rules

1. Recipes reference products through canonical identifiers defined by the Good and Service Catalogue.
2. Recipes may reference Industry Types and Process Capabilities but do not redefine Industry Data.
3. Recipes define technical requirements but do not establish facility capacity. Production determines available capacity.
4. Recipes do not determine financial feasibility, affordability, prices, or expenditure. Economy owns those decisions and records.
5. Recipes do not establish stock availability. Inventory determines available quantities and validates reservations and consumption.
6. Recipes do not grant technologies, legal permissions, military acceptance, or access to geographic locations.
7. Production coordinates execution and invokes the authorised interfaces of the relevant systems.
8. Inventory alone records authoritative physical stock changes.
9. Executed production records must reference the exact recipe version used.
10. No system may independently record the same physical stock change or production outcome a second time.

---

## 3. Core Concepts

### 3.1 Production Process

A Production Process is a defined type of transformation that converts specified inputs into specified outputs under declared technical and operating conditions.

Examples include:

- Extracting iron ore from a deposit.
- Refining crude oil into petroleum products.
- Growing agricultural crops.
- Smelting iron ore into pig iron.
- Converting pig iron into steel.
- Fabricating steel plate.
- Assembling machinery from components.
- Generating electricity from coal.
- Recovering usable material from scrap.
- Providing a transport service using labour, vehicles, and fuel.

A process may support multiple recipes with different inputs, yields, equipment requirements, or technological prerequisites.

### 3.2 Recipe

A Recipe is a quantitative configuration of a Production Process.

It defines the inputs required and outputs expected for a stated execution quantity, batch, completed unit, or production rate.

Recipes implementing the same process may differ in:

- Input products and quantities.
- Output products and quantities.
- Yield and loss rules.
- Required technologies and capabilities.
- Equipment compatibility.
- Permitted substitutions.
- Operating conditions.
- Production mode.
- By-products and recoverable materials.
- Effective dates.

A recipe may represent a historically appropriate method, an alternative industrial technique, or a method enabled by technological research.

### 3.3 Recipe Version

A Recipe Version is an immutable set of simulation-relevant recipe parameters.

The logical `recipe_id` identifies a recipe across its lifetime. The version identifier distinguishes a specific configuration.

A new version is required whenever a change materially affects simulation behaviour, including input or output quantities, yields, losses, operating requirements, substitutions, execution mode, or technical prerequisites.

Descriptive metadata changes that do not affect simulation behaviour need not create a new version.

A version that has been used by an executed production order must not be silently modified. Historical production records must remain reproducible.

An explicit, auditable migration may be permitted for unexecuted orders, subject to Production's order-state and migration rules. Such migration must not rewrite completed historical outcomes.

### 3.4 Process Capability

A Process Capability is a technical ability that may be required to perform a process or recipe.

Examples include iron smelting, petroleum refining, precision machining, chemical synthesis, electrical generation, food preservation, and ship assembly.

A capability is not itself a technology, facility, or Industry Type. The relevant systems determine whether a facility or entity possesses the required capability.

### 3.5 Input

An Input is a resource or service required by a recipe.

Inputs may include raw materials, intermediate goods, components, chemicals, fuel, water, electricity, packaging, reusable equipment, or catalogue-defined services.

Where an input represents a catalogue product or service, it must reference its canonical identifier.

### 3.6 Output

An Output is a result defined by a recipe.

Outputs may include:

- Primary products.
- Secondary products and co-products.
- Intermediate goods.
- Recoverable materials.
- Waste.
- Emissions or other environmental effects.
- Services.
- Persistent assets produced through construction or assembly.

Physical outputs represented as goods may create physical stock only through authorised production execution and Inventory transactions. Services and non-inventory effects must be recorded through their appropriate system interfaces.

---

## 4. Process Classification

Processes should be classifiable by transformation type without requiring a separate hard-coded subsystem for each industrial sector.

A process may have one primary category and additional classifications where required.

### 4.1 Transformation Categories

| Category | Description | Example |
|---|---|---|
| Extraction | Removes resources from a natural deposit or source | Iron ore mining |
| Agriculture | Converts land, biological inputs, and labour into agricultural output | Wheat cultivation |
| Animal Production | Produces animal-derived goods | Livestock farming |
| Refining | Separates or transforms feedstock into usable products | Petroleum refining |
| Chemical Processing | Produces substances through chemical transformation | Fertiliser production |
| Material Processing | Converts raw materials into intermediate materials | Steelmaking |
| Fabrication | Shapes, cuts, joins, or otherwise processes materials | Steel plate fabrication |
| Assembly | Combines components into a finished product | Machinery assembly |
| Construction | Produces a persistent structure or infrastructure asset | Bridge construction |
| Energy Generation | Converts fuel or other energy sources into usable energy | Coal-fired electricity |
| Maintenance and Repair | Maintains or restores an asset's functional condition | Locomotive overhaul |
| Service Provision | Provides a non-physical service through labour, equipment, or other resources | Commercial transport |
| Recycling and Recovery | Recovers usable materials from waste or used goods | Scrap metal recovery |

These categories classify processes. They do not independently determine execution logic, facility capacity, or scheduling.

### 4.2 Production Modes

A recipe must specify its production mode where that mode affects execution semantics.

- **Batch:** A defined quantity is processed as a discrete run.
- **Continuous:** Inputs and outputs are represented as rates over time.
- **Discrete Assembly:** Components are combined into individual units.
- **Extraction:** Resources are removed from a source, potentially subject to finite reserves or regeneration.
- **Service:** A defined quantity of service is delivered over a measurable period or activity.
- **Construction:** Inputs are transformed into a persistent asset through potentially extended execution.

The initial implementation may use a shared recipe and production-order framework for these modes. Specialised execution behaviour may be introduced where necessary.

The production mode does not independently establish capacity, schedule orders, or authorise execution.

---

## 5. Production Process Data Model

Each Production Process must have a stable identifier and a definition of its transformation and technical eligibility requirements.

### 5.1 Required Process Fields

| Field | Description |
|---|---|
| `process_id` | Stable, unique process identifier |
| `name` | Human-readable process name |
| `description` | Explanation of the transformation |
| `process_category` | Primary transformation classification |
| `additional_categories` | Optional secondary classifications |
| `supported_modes` | Supported production modes |
| `required_capabilities` | Capabilities required by the process |
| `applicable_industry_types` | Permitted Industry Types, where restricted |
| `input_constraints` | Process-level restrictions on acceptable inputs |
| `output_constraints` | Process-level restrictions on permissible outputs |
| `technology_requirements` | Technical prerequisites |
| `environmental_constraints` | Environmental or geographic conditions relevant to execution |
| `status` | Availability state of the process definition |
| `metadata` | Optional descriptive and classification information |

The fields define a conceptual data contract. The final storage representation may differ according to the implementation architecture.

### 5.2 Process Eligibility

A process may be restricted by Industry Type, capabilities, technology, location, or operating conditions.

These restrictions define technical eligibility. They do not automatically authorise a facility to execute a recipe.

Production must evaluate process and recipe requirements against the facility, capacity, operational status, and applicable simulation state.

Where no Industry Type restriction exists, a process may be considered by any otherwise eligible industry.

---

## 6. Recipe Data Model

Each recipe must identify its process, version, execution mode, quantitative basis, inputs, outputs, and applicable prerequisites.

### 6.1 Required Recipe Fields

| Field | Description |
|---|---|
| `recipe_id` | Stable identifier for the logical recipe |
| `recipe_version` | Immutable version identifier |
| `process_id` | Identifier of the implemented Production Process |
| `name` | Human-readable recipe name |
| `description` | Explanation of the recipe configuration |
| `production_mode` | Batch, continuous, discrete assembly, extraction, service, or construction |
| `basis_quantity` | Reference execution quantity or rate |
| `basis_unit` | Canonical unit associated with the basis |
| `inputs` | Defined input requirements |
| `outputs` | Defined output results |
| `required_capabilities` | Additional technical capabilities required |
| `required_equipment` | Equipment or equipment-class requirements |
| `technology_requirements` | Recipe-specific technical prerequisites |
| `operating_constraints` | Permitted operating conditions |
| `valid_from` | Earliest simulation date on which the recipe is valid |
| `valid_until` | Optional final date of validity |
| `status` | Draft, validated, active, retired, or another defined lifecycle state |
| `provenance` | Source, assumptions, or design rationale |

A recipe must pass validation and satisfy availability rules before Production may execute it.

### 6.2 Recipe Identity and Versioning

A new recipe version is required whenever a change materially alters the transformation or its simulation-relevant parameters.

This includes changes to:

- Input or output quantities.
- Yield and loss calculations.
- Technology or capability requirements.
- Equipment compatibility.
- Production mode.
- Substitution rules.
- Operating constraints.
- Quantity scaling rules.

Executed production orders must retain the exact recipe version used.

Retiring or superseding a recipe does not invalidate historical production.

---

## 7. Recipe Inputs

Inputs define the resources required for a recipe execution.

Each input must identify the resource, quantity, unit, functional role, and consumption semantics.

### 7.1 Input Data Fields

| Field | Description |
|---|---|
| `input_id` | Identifier unique within the recipe version |
| `catalogue_entry_id` | Canonical Good and Service Catalogue identifier |
| `quantity` | Required quantity relative to the recipe basis |
| `unit` | Canonical unit of measurement |
| `input_role` | Functional role of the input |
| `consumption_mode` | How the input is consumed, incorporated, retained, or otherwise handled |
| `substitution_group` | Optional identifier for explicitly permitted alternatives |
| `quality_constraints` | Required grade, purity, specification, or condition |
| `minimum_quantity` | Optional minimum requirement |
| `maximum_quantity` | Optional maximum permitted quantity |
| `notes` | Optional explanatory information |

The schema is conceptual and may be normalised during implementation.

### 7.2 Input Roles

| Role | Description |
|---|---|
| Feedstock | Primary material transformed by the process |
| Component | A part incorporated into an assembled product |
| Consumable | A resource used during execution |
| Fuel | A resource used to supply energy |
| Utility | A required resource or service, such as electricity or water |
| Catalyst | A substance that facilitates a reaction and is not intended to be consumed in proportion to output |
| Auxiliary Material | Supporting material required by the process |
| Packaging | Material used to package or protect output |
| Service Input | A catalogue-defined service required for execution |

An input's role does not independently determine its consumption, financial treatment, or stock transaction.

### 7.3 Consumption Modes

The recipe must distinguish between resources that are consumed and resources that remain available after execution.

Conceptual consumption modes include:

- `consumed`: the specified quantity is consumed during execution.
- `incorporated`: the specified quantity becomes part of an output, subject to the transformation rules.
- `reusable`: the resource remains available after execution, subject to explicitly modelled wear or degradation.
- `returned`: some or all of the input is returned as a defined output.
- `service_consumed`: a service quantity is used without assuming that a physical stock item is consumed.

These modes define recipe semantics. Inventory remains authoritative for physical stock, reservations, consumption, returns, and other stock changes.

A reusable catalyst must not be fully consumed for every batch merely because it appears as an input. Any degradation, replacement, or return must be explicitly represented and executed through the appropriate system interfaces.

### 7.4 Input Quality

A recipe may require a particular material grade, purity, condition, specification, or measurable quality.

Where the catalogue supports these attributes, the recipe must reference them rather than create an independent product definition.

Production validates whether selected inputs meet the recipe's requirements. Inventory supplies the authoritative stock and lot attributes used for that validation.

Where multiple grades or specifications are permitted, the recipe must define the accepted alternatives and any associated quantity or yield adjustments. Alternatives without an explicit equivalence or conversion rule must not be assumed interchangeable.

---

## 8. Recipe Outputs

Outputs define the results expected from recipe execution.

A recipe must identify its primary output and any represented secondary products, co-products, recoverable materials, waste, or non-inventory effects.

### 8.1 Output Data Fields

| Field | Description |
|---|---|
| `output_id` | Identifier unique within the recipe version |
| `catalogue_entry_id` | Canonical catalogue identifier, where applicable |
| `quantity` | Expected quantity relative to the recipe basis |
| `unit` | Canonical unit of measurement |
| `output_role` | Primary product, co-product, by-product, recovered material, or other supported result |
| `output_condition` | Expected grade, quality, or condition |
| `yield_rule` | Rule used to calculate the result |
| `destination_type` | Intended handling category |
| `recoverability` | Whether the result is intended to be recovered or reused |
| `notes` | Optional explanatory information |

A physical output must reference a canonical Good and Service Catalogue entry whose definition is compatible with a physical product.

A service output must reference a canonical service where such a definition exists.

Environmental effects and other non-inventory results must be handled through the relevant system interfaces, not represented as invented physical goods.

### 8.2 Output Roles

| Role | Description |
|---|---|
| Primary Product | Principal intended output |
| Co-product | Secondary product deliberately produced alongside the primary product |
| By-product | Secondary result generated by the process |
| Recovered Material | Material returned to usable circulation |
| Waste | Residual result not treated as a usable product by the applicable process definition |
| Emission | A modelled environmental release |
| Service Output | A non-physical service produced by execution |
| Asset Output | A persistent asset produced by construction or assembly |

An output's role does not determine its price, revenue, or economic classification. Those matters remain under the authority of the Catalogue and Economy System as applicable.

### 8.3 Multiple Outputs

A recipe may produce multiple outputs from one execution.

For example, petroleum refining may produce several petroleum products from a single feedstock. Manufacturing may generate scrap that can be recovered through a separate process.

The recipe must define the quantity or calculation rule for every represented output.

Production must not assume that the primary output is the only result.

Where outputs depend on operating conditions, input quality, or other variables, those dependencies must be explicit. Undefined variability must not be introduced as an undocumented random adjustment.

---

## 9. Units and Quantities

Recipes must use explicit, compatible measurement units.

The Good and Service Catalogue and the shared measurement contract define canonical units and permitted conversions. Recipes must reference those definitions rather than create competing unit systems.

Examples of relevant dimensions include:

- Mass: kilograms and tonnes.
- Volume: litres and cubic metres.
- Count: individual units, components, or animals.
- Length: metres and kilometres.
- Area: square metres and hectares.
- Time: seconds, hours, shifts, and days.
- Energy: joules and kilowatt-hours.
- Power: watts and kilowatts.
- Service quantity: explicitly defined units of service delivery.

The list is illustrative, not an exhaustive canonical unit registry.

### 9.1 Quantity Basis

Every recipe must define the reference quantity against which inputs and outputs are calculated.

Examples include:

- Inputs required to produce one tonne of steel.
- Outputs generated by processing one tonne of crude oil.
- Components required to assemble one machine.
- Electricity consumed per hour of operation.
- Labour or service quantities required per completed unit.

The basis quantity must have an explicit unit and must not be inferred from a recipe's name or description.

### 9.2 Unit Compatibility

Quantities may only be combined or compared where their units are compatible or a valid conversion exists.

Mass must not be directly compared with volume without a valid conversion rule. A conversion may depend on density or another declared material property.

Energy and power must not be treated as interchangeable. A power requirement expressed as a rate must be integrated over time to calculate total energy consumption.

Count-based outputs must not be silently converted into mass-based quantities without an explicit conversion rule.

### 9.3 Quantity Scaling

For a linearly scalable recipe, input and output quantities scale proportionally with the execution quantity.

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

- \(Q_{\text{basis input}}\) is the input quantity defined at the recipe basis.
- \(Q_{\text{basis output}}\) is the output quantity defined at the recipe basis.
- \(Q_{\text{execution}}\) is the requested execution quantity expressed in a compatible unit.
- \(Q_{\text{basis}}\) is the recipe's reference quantity.

These equations apply only where linear scaling is defined. Fixed setup requirements, minimum batch sizes, indivisible components, nonlinear yields, and conditional relationships must be represented explicitly.

### 9.4 Precision and Rounding

Intermediate calculations must preserve sufficient precision to avoid cumulative drift across repeated production cycles.

Rounding must occur at defined system boundaries rather than arbitrarily during intermediate calculations.

The shared numerical and Inventory contracts must define authoritative storage precision, rounding mode, and the handling of fractional physical quantities.

Until those rules are established, implementations must not assume that every good can be produced, consumed, or transferred in arbitrary fractional quantities.

---

## 10. Yields, Losses, and Recovery

Recipes must define the relationship between processed inputs and resulting outputs.

Yield describes the quantity or proportion of an input that becomes a specified output under defined conditions.

Loss describes a quantity that does not become the intended output.

Recovery describes material that would otherwise be lost or discarded but is returned as a usable output.

### 10.1 Yield Models

A recipe may use one or more of the following models:

- **Fixed Output:** A specified output quantity for a defined execution basis.
- **Proportional Yield:** Output quantity varies proportionally with a specified input.
- **Conversion Yield:** A defined fraction of a feedstock becomes a specified product.
- **Multi-Output Yield:** Multiple outputs are calculated from the same feedstock using explicit allocation or conversion rules.
- **Conditional Yield:** Yield varies according to explicitly defined input quality, technology, equipment, operating conditions, or other parameters.
- **Discrete Yield:** Output is determined in whole units, batches, or other indivisible quantities.

The recipe must identify the applicable model wherever output quantities cannot be derived unambiguously from a fixed basis.

### 10.2 Losses

Losses may include processing losses, rejected material, evaporation, unrecoverable residues, energy conversion losses, and other explicitly modelled inefficiencies.

A loss must not be silently deducted from an output unless the recipe's quantitative rules account for it.

Where a loss produces a physical residual that the simulation tracks, that residual must be represented as an output. Non-material losses must be represented through the relevant calculation or system interface.

### 10.3 Recovery

Recoverable materials must be represented explicitly when they affect subsequent production or inventory.

For example, a steelmaking process may generate scrap that can be used by a suitable recycling process.

Recovery must not restore the original input in full unless the recipe explicitly defines that outcome.

Recovered material may differ from the original input in quantity, quality, condition, or eligibility.

### 10.4 Yield Validation

Recipe validation must detect quantitative inconsistencies where the relevant physical relationships are known.

A transformation must not unintentionally generate additional material without a defined source, conversion, or transformation rule.

A universal mass-balance equation must not be applied indiscriminately to every process. Energy generation, chemical reactions, agriculture, biological production, and services may require domain-specific treatment.

Where a mass-balance or other conservation rule applies, the validation method and acceptable tolerance must be explicit.

---

## 11. Substitutions and Alternative Inputs

A recipe may permit alternative materials or grades where those alternatives are technically valid.

### 11.1 Substitution Rules

A substitution rule must specify:

- The input that may be replaced.
- The permitted alternative catalogue entries.
- Applicable quality constraints.
- Permitted substitution quantities or ratios.
- Any associated yield adjustments.
- Any additional operating requirements.
- Any technology or equipment prerequisites.

Economic costs may be evaluated by Economy, but recipe execution must not independently purchase resources or make financial decisions.

Substitution must be explicit. Products must not be treated as interchangeable merely because they have similar names, categories, or intended uses.

### 11.2 Substitution Groups

A substitution group may identify several products that satisfy a shared functional requirement.

For example, a process may permit a defined set of fuels with different consumption rates or operating constraints.

The group does not guarantee equivalence. The recipe must define permitted alternatives and the effects of each alternative on the transformation.

An alternative without a defined conversion rule must not be selected automatically.

### 11.3 Input Selection

The selection of a valid input alternative must be deterministic.

Where multiple alternatives are available, the selection rule must be explicit, such as a declared priority order or a rule supplied by Production.

Economy may influence procurement and allocation decisions through its own interfaces. Recipe calculations must not independently infer purchasing preferences or alter inventory allocation.

---

## 12. Process and Recipe Prerequisites

Processes and recipes may impose technical requirements that must be satisfied before execution.

### 12.1 Technology Requirements

A process or recipe may require a technology, research result, or technical capability.

Research determines whether the relevant technology is available to the applicable entity or facility. Production evaluates whether the required technology is applicable to the proposed execution.

A recipe must not grant its own technological prerequisites merely because it exists in the database.

### 12.2 Equipment Requirements

A recipe may require equipment with specific capabilities.

Examples include:

- Furnaces capable of reaching a specified temperature.
- Machine tools capable of producing a required tolerance.
- Refining equipment capable of processing a particular feedstock.
- Power-generation equipment compatible with a defined fuel.
- Assembly equipment capable of handling a specified product.

Production determines whether the facility and its equipment satisfy these requirements.

The recipe defines the requirement. It does not create equipment, establish a facility, or determine production capacity.

### 12.3 Operating Conditions

A recipe may require particular operating conditions, including temperature, pressure, material purity, moisture content, environmental conditions, minimum or maximum operating rates, or other measurable constraints.

Only conditions supported by explicit data and calculation rules may affect execution.

Conditions documented as future requirements must not be treated as active simulation constraints until their behaviour is implemented and defined.

### 12.4 Legal and Geographic Restrictions

A process or recipe may declare requirements dependent on legal permissions, environmental conditions, or geographic circumstances.

The relevant systems determine whether those requirements are satisfied.

A recipe does not independently grant permits, establish locations, create transport connections, or override government restrictions.

---

## 13. Batch and Continuous Transformations

Recipes must support discrete and rate-based transformations without conflating their quantities or execution semantics.

### 13.1 Batch Production

A batch recipe defines a discrete production run.

It may specify:

- Minimum batch quantity.
- Standard batch quantity.
- Maximum batch quantity.
- Setup requirements.
- Input quantities per batch.
- Output quantities per batch.
- Batch-specific losses.
- Whether partial batches are permitted.

Production must enforce batch constraints when creating and scheduling production orders.

An incomplete batch must not automatically generate the output of a full batch.

Where partial batches are permitted, the scaling and yield rules must be explicit.

### 13.2 Continuous Production

A continuous recipe defines inputs and outputs as rates over time.

Examples include tonnes of material processed per hour, cubic metres of water treated per day, and energy generated over a specified period.

A rate-based recipe must define the time unit and reference operating rate.

Production determines the execution period and available capacity. Resulting quantities are calculated using the actual execution duration and applicable rate and operating conditions.

### 13.3 Discrete Assembly

Assembly recipes may define requirements per completed unit.

For example, one finished machine may require a specified number of engines, gear assemblies, electrical components, and fasteners.

Indivisible component counts must not be rounded into a valid completed unit unless the recipe explicitly supports that behaviour.

Production must distinguish completed units from incomplete work in progress where such intermediate states are modelled.

### 13.4 Long-Running Processes

Some transformations, particularly construction and complex assembly, may take place over an extended period.

The recipe defines required inputs and expected results. Production defines scheduling, progress, capacity allocation, and execution state.

Inventory determines stock availability and records authorised reservations and consumption.

A partially completed process must not automatically create the full final output. Any intermediate output or work-in-progress representation must be explicitly defined by the relevant systems.

---

## 14. Recipe Execution Contract

Production Processes and Recipes supplies validated transformation definitions and quantitative calculations. Production remains responsible for coordinating execution.

### 14.1 Execution Inputs

Production supplies, directly or through an execution record:

- The selected recipe identifier and version.
- The requested execution quantity or operating duration.
- The eligible facility and production line.
- Applicable operating conditions.
- Validated input quantities and selected alternatives.
- Relevant execution parameters.
- The simulation date or execution period.

The concrete data structure is an implementation decision, but the information needed to calculate a deterministic result must be available.

### 14.2 Execution Calculation

Recipe calculation determines:

1. Input quantities required for the requested execution.
2. Minimum batch, discrete quantity, and scaling constraints.
3. Expected quantities of primary and secondary outputs.
4. Defined process losses and recoverable materials.
5. Conditional yield adjustments.
6. Supported non-inventory effects.
7. Whether the requested execution satisfies the recipe's operating conditions.

The calculation must return a structured result that Production can validate before committing execution.

### 14.3 Execution Results

A calculation result should identify:

- The recipe version used.
- Requested and accepted execution quantities.
- Required input quantities.
- Expected output quantities.
- Expected by-products and recoverable materials.
- Expected losses and other supported effects.
- Validation failures.
- Warnings or unresolved conditions.

Production remains responsible for coordinating execution and reporting the actual result to the appropriate systems.

Expected results are not evidence that production has occurred. Only authorised execution can establish completed production.

### 14.4 No Independent Inventory Mutation

Recipe calculation must be separable from inventory mutation.

Calculating a recipe must not consume input stock, reserve stock, create output stock, transfer goods, or alter other world state.

The recipe calculation may return an execution plan containing required inputs and expected outputs. Production and Inventory then coordinate authorised execution and record the resulting transactions.

This separation allows recipes to be validated, previewed, tested, and compared without changing the simulated world.

### 14.5 Actual Versus Expected Results

The recipe defines expected results under specified conditions. Production records execution progress and actual completion. Inventory records the resulting physical stock transactions.

Where actual output differs from expected output, the deviation must be represented through an explicit execution result or supported adjustment rule.

The system must not silently replace expected quantities with actual quantities or invent unrecorded losses.

---

## 15. Execution Consistency and Failure Handling

Process and recipe definitions must support reliable execution when an order cannot start, cannot complete, or produces less than expected.

### 15.1 Pre-Execution Validation

Before execution begins, Production must validate that:

- The process and recipe version exist.
- The recipe is valid for the simulation date.
- The recipe is validated and available for use.
- The requested quantity conforms to recipe constraints.
- Required inputs and permitted substitutions are defined.
- Outputs have valid catalogue definitions or supported non-inventory handling.
- Required units and conversions are valid.
- Execution parameters satisfy the recipe's declared constraints.

Production must separately validate facility eligibility, capacity, available inputs, workforce, technology, utilities, legal permissions, and other operational requirements.

### 15.2 Failure Categories

Failures attributable to process or recipe definitions should be distinguishable from failures attributable to Production, Inventory, or other systems.

Proposed identifiers include:

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

These identifiers are proposed. The final error-code convention must follow the shared simulation-wide error contract.

### 15.3 Partial Execution

If an order executes only partially, its outputs must correspond to the quantity actually processed, subject to the recipe's scaling and yield rules.

The system must not create the full expected output merely because an order was accepted, scheduled, or started.

For a batch process that does not support partial batches, Production must enforce the batch constraint.

### 15.4 Determinism

Given the same recipe version, execution quantity, operating parameters, relevant inputs, and applicable simulation state, recipe calculations must produce the same result.

Any stochastic behaviour must be explicitly defined, use the simulation's authoritative randomisation mechanism, and remain reproducible under the save-and-reload contract.

Recipe calculations must not depend on hidden randomness, execution order, machine timing, or unrecorded external state.

### 15.5 Atomicity and Idempotency

Recipe calculation must be repeatable and must not mutate physical stock.

Production and Inventory must ensure that execution retries cannot consume the same inputs twice or create duplicate outputs.

Repeated calculation requests with identical inputs and parameters must return the same result.

Repeated execution-commit requests must be handled according to the shared Production Execution transaction contract defined by Production and Inventory. The recipe supplies the immutable quantitative transformation definition; it does not commit inventory or financial changes.

The transaction must reference the exact recipe version used and must preserve the calculated quantities for every output, each with its own canonical Catalogue identifier, unit, quantity and relationship to the Production Execution. Retrying calculation must not mutate stock, and retrying a committed execution must not duplicate input consumption or output registration.

If an execution transaction fails, the shared contract must apply its defined failure and recovery behaviour so physical stock is not left in an inconsistent state.

---

## 16. Process Chains and Multi-Stage Production

Industrial supply chains consist of multiple transformations.

For example:

Iron Ore → Pig Iron → Steel → Steel Plate → Manufactured Components → Finished Machinery

Each transformation should be represented by its own process and recipe where the stages have distinct inputs, outputs, technical requirements, or operational constraints.

### 16.1 Separation of Stages

A recipe should represent a coherent transformation rather than an entire industrial economy.

Multi-stage production must be represented through separate production orders or another explicitly defined execution structure.

The completion of one stage may create an output required by a subsequent stage. Inventory records that physical output, and the subsequent stage must obtain the required input through the normal Inventory interface.

Related recipes must not bypass intermediate stock, supply requirements, production execution, or logistics merely because their processes are connected.

### 16.2 Intermediate Goods

Intermediate goods use the same canonical identifiers as other goods in the Good and Service Catalogue.

A good may be the output of one recipe and the input of another.

Its role as an intermediate good is contextual and does not require a separate product definition for every process in which it participates.

### 16.3 Process Dependencies

A process chain may declare dependencies or compatibility requirements where necessary for planning or validation.

A recipe must not guarantee that its output will be available to a subsequent process. Actual availability depends on completed production, Inventory state, location, and logistics.

### 16.4 Initial Scope

The foundational implementation should support multiple independent production orders connected through inventory.

Integrated multi-stage scheduling, automatic process-chain optimisation, and dynamic coordination across multiple facilities may be added later without changing the fundamental process and recipe contract.

---

## 17. Historical Validity and Technological Change

The industrial economy develops over simulation time. Production methods must therefore accommodate changes in technology, capabilities, and operating conditions.

### 17.1 Effective Dates

Recipes may define an earliest valid date and an optional expiry date.

Availability must be checked against the simulation date and applicable technical prerequisites.

A recipe valid in a later period must not automatically be available in the starting world of 1955.

### 17.2 Technological Improvements

A technological improvement may introduce a new process, create a new recipe version, or make an existing recipe eligible for additional facilities.

The appropriate approach depends on whether the improvement changes the transformation itself, the quantitative configuration, or only the availability of the method.

A technology that changes input consumption, output yield, operating rate, or equipment requirements must not silently alter an already executed recipe version.

### 17.3 Historical Reproducibility

Historical production records must retain the recipe version used at execution.

Retired or superseded recipes must remain interpretable in historical records.

Retiring a recipe must not retroactively invalidate completed production.

### 17.4 Starting-World Data

Recipes available at the 1955 starting date must be explicitly designated through validity and availability rules.

The starting world must not automatically include every recipe defined in the database.

Initial production capabilities must reflect the starting world's technology, industrial configuration, and other applicable constraints.

---

## 18. Data Validation and Integrity

Recipe definitions must be validated before activation.

Validation should detect structural errors, missing references, inconsistent quantities, invalid conversions, and unsupported behaviour.

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
- Whether yield parameters fall within permitted ranges.
- Whether output quantities can be calculated for the supported execution basis.
- Whether discrete quantities and batch constraints are respected.
- Whether loss and recovery rules are internally consistent.
- Whether applicable conservation constraints are satisfied within defined tolerances.

Validation must not reject a recipe solely because a simplistic universal material-balance assumption does not apply to its process category.

### 18.3 Reference Integrity

Recipe references must remain valid across catalogue and system updates.

Removing or retiring a catalogue entry must not silently rewrite historical recipe definitions or completed production records.

Changes to referenced definitions must follow the relevant system's compatibility and versioning rules.

### 18.4 Activation

A recipe may be activated only when the required validation succeeds.

Draft or invalid recipes may be retained for authoring and testing, but Production must not execute them.

An active recipe must still satisfy its effective dates, technology requirements, facility requirements, and other eligibility conditions.

---

## 19. System Interfaces

Production Processes and Recipes must expose a stable set of logical interfaces.

These are conceptual contracts, not mandates for specific programming function names or API formats.

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
- Missing or invalid references.

Validation must not modify inventory, production orders, finances, or other world state.

### 19.3 Execution Calculation

**Purpose:** Calculate required inputs and expected results for a proposed execution.

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
- Supported non-inventory effects.
- Validation result.

The calculation must not independently execute the recipe.

### 19.4 Process Eligibility Information

**Purpose:** Expose technical requirements so Production can evaluate a facility's eligibility.

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

Event names are illustrative and must follow the project's shared event naming and versioning conventions.

---

## 20. Simulation Timing and Daily Processing

The recipe system provides definitions and calculations. It does not independently run a daily production cycle or establish a competing global scheduler.

During the relevant simulation phase, Production may request recipe calculations for eligible production orders.

The conceptual interaction is:

1. Production identifies an eligible order and selected recipe version.
2. Production validates the facility, capacity, operating conditions, and applicable constraints.
3. The recipe system calculates required inputs and expected results.
4. Inventory validates stock availability and performs any authorised reservation through its own interface.
5. Production executes the permitted quantity according to its scheduling and capacity rules.
6. Production and Inventory coordinate the authorised execution transaction, including input consumption and output registration.
7. Production records the actual completed quantity and execution result.
8. Relevant economic, statistical, and other systems receive the appropriate records or events through their authorised interfaces.

The precise ordering and transaction boundaries for reservations, consumption, execution commitment, and output registration must be defined by the Production and Inventory contracts.

This conceptual sequence does not establish a global daily phase order. The shared simulation scheduler and Technical Specification own that responsibility.

The recipe system must not bypass Production's capacity allocation or create physical stock outside the authorised execution process.

---

## 21. Initial Implementation Scope

The first implementation should establish a functional, extensible transformation model before introducing highly specialised industrial simulation.

### 21.1 Required Initial Capabilities

The initial implementation should support:

- Stable process identifiers.
- Versioned recipe definitions.
- Canonical catalogue references for inputs and outputs.
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
- Structured failure codes for invalid definitions and calculations.
- Clear separation between expected and actual execution results.

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
- [ ] Every recipe has an explicit execution basis and valid measurement units.
- [ ] Multiple outputs can be represented within a single recipe.
- [ ] Recipe validity dates and lifecycle states are supported.

### 22.2 Calculation

- [ ] A linearly scalable recipe calculates input requirements correctly.
- [ ] Expected output quantities are calculated deterministically.
- [ ] A recipe can represent multiple outputs, including co-products and by-products.
- [ ] Defined losses and recoveries are represented without implicit quantity changes.
- [ ] Invalid units and undefined conversions are rejected.
- [ ] Batch constraints and discrete quantities are enforced where configured.
- [ ] Unsupported yield models fail explicitly rather than producing guessed results.

### 22.3 Integration

- [ ] Production can retrieve and validate a specific recipe version.
- [ ] Production can request an execution calculation without mutating world state.
- [ ] Recipe calculations do not independently reserve or consume inventory.
- [ ] Recipe calculations do not independently create or transfer inventory.
- [ ] Facility eligibility and production capacity remain under Production's authority.
- [ ] Stock availability and stock mutations remain under Inventory's authority.
- [ ] Technology and legal prerequisites are evaluated through their authoritative systems.
- [ ] Historical production records retain the recipe version used.
- [ ] Actual execution outcomes are distinguished from expected recipe results.

### 22.4 Determinism and Integrity

- [ ] Identical execution parameters produce identical calculation results.
- [ ] Recipe updates do not silently alter historical versions.
- [ ] Invalid recipe definitions cannot be executed.
- [ ] Failed calculations return structured errors.
- [ ] Repeated calculation requests do not create transactions or mutate state.
- [ ] Repeated execution commits cannot duplicate input consumption or output registration.
- [ ] Recipe definitions remain interpretable when later versions are introduced.

---

## 23. Unresolved Design Decisions

The following decisions should be resolved through shared system contracts or implementation testing rather than assumed by this specification.

1. **Canonical units and conversions:** Define the authoritative measurement registry, conversion rules, and treatment of material-dependent conversions.
2. **Numerical precision:** Establish the shared numerical representation, rounding policy, and handling of fractional quantities.
3. **Recipe storage format:** Determine the concrete schema, normalisation strategy, and serialisation format.
4. **Version identifiers:** Determine whether recipe versions use sequential integers, immutable identifiers, or another scheme.
5. **Conditional yields:** Define the supported initial set of conditions and representation of nonlinear relationships.
6. **Mass and energy validation:** Establish the scope and tolerances of conservation checks for each applicable process category.
7. **Batch execution:** Define how minimum batches, partial batches, setup losses, and interrupted batches are handled by Production.
8. **Continuous execution:** Define how operating rates interact with simulation time steps, downtime, and partial periods.
9. **Reusable inputs:** Establish the shared execution contract for reusable catalysts, tools, and other non-consumed resources.
10. **Quality representation:** Define how input and output grades, purity, condition, and specifications are represented in the Catalogue and validated by Production.
11. **Work in progress:** Determine how partially completed transformations and long-running construction processes are represented.
12. **Non-inventory outputs:** Define interfaces for environmental effects, service delivery, and other results that are not physical stock.
13. **Recipe migration:** Establish whether and under what conditions an unexecuted production order may migrate to a newer recipe version.
14. **Starting recipe library:** Define the initial processes and recipes available in 1955, including historical validity and technical prerequisites.
15. **Production transaction boundaries:** Finalise the interaction between recipe calculations, reservations, input consumption, execution, output registration, rollback, and recovery with the Production and Inventory specifications.
16. **Actual versus expected output:** Define how deviations from expected output are recorded, including measured losses, quality differences, and incomplete execution.
17. **Event contracts:** Establish shared event names, payload schemas, versioning, and failure handling.

Until these decisions are resolved, implementations must preserve the ownership boundaries, deterministic calculations, and historical reproducibility defined in this document.

---

## 24. Governing Principle

Production Processes and Recipes defines the transformations available to the industrial economy. It does not independently determine whether those transformations are affordable, schedulable, legally permitted, or physically executable.

Industry Data defines industry configuration. Economy governs financial decisions and records. Production determines and coordinates execution. Production Processes and Recipes define transformations and their quantitative rules. Inventory records authoritative physical stock. The Good and Service Catalogue defines the canonical products referenced throughout the simulation.

**No industrial output may be created merely because a recipe exists. Output must result from authorised Production execution, and resulting physical stock must be recorded through the Inventory System.**
