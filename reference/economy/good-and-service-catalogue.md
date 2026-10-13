# Good and Service Catalogue

**Canonical Path:** `docs/reference/economy/good-and-service-catalogue.md`  
**System:** Economic Reference Data  
**Document Type:** Canonical Data Specification  
**Status:** Foundational Specification  
**Starting World Date:** 1 January 1955

---

## 1. Purpose

The Good and Service Catalogue defines the canonical products and services recognised by Project Sovereign.

It establishes the stable identifiers, classifications, measurement units, descriptive attributes, and compatibility information used by the simulation's Economy, Industry Data, Production, Production Processes and Recipes, Inventory, Logistics and Transportation, Research, Government, Military, and Workforce and Population systems.

Every system that references a good or service must use its canonical Catalogue definition rather than create an independent definition of the same product.

The Catalogue must support the full economic lifecycle of the simulated world, from natural resource extraction and agricultural production through manufacturing, distribution, consumption, maintenance, military procurement, and service provision.

It must accommodate the material economy of the 1955 starting world and the increasingly sophisticated goods, equipment, technologies, and services that become available as the simulation progresses.

### 1.1 Core Principle

**The Catalogue defines what a good or service is. It does not define how it is produced, who owns it, where it is held, what it costs, or whether it is currently available.**

A product may be produced by multiple industries, traded by multiple companies, stored at multiple locations, and used by multiple systems while retaining one canonical identity.

A service may be offered by multiple providers, consumed by different organisations, and priced differently across markets without requiring a separate canonical definition for each provider or transaction.

### 1.2 Design Objectives

The Catalogue must:

1. Provide one authoritative identity for each defined good or service.
2. Support the economic and technical classifications required by dependent systems.
3. Define canonical measurement units and reference shared unit-conversion rules.
4. Distinguish product identity from variants, specifications, physical instances, and stock condition.
5. Support explicit historical validity without implying universal availability.
6. Permit incremental expansion without requiring changes to the fundamental data model for every new product.
7. Preserve the interpretation of historical simulation records when reference definitions change.
8. Prevent duplicate product definitions and incompatible interpretations across systems.

---

## 2. System Boundaries

The Catalogue owns canonical product identity, classification, measurement conventions, and shared descriptive definitions.

It does not own the operational or economic state associated with those definitions.

| System | Relationship to the Catalogue |
|---|---|
| Economy System | References products in financial transactions, purchasing, sales, expenditure, revenue, valuation, and market models |
| Industry Data | References products in industrial requirements, establishment requirements, and industry relationships |
| Production System | References products when coordinating physical production and production-service execution |
| Production Processes and Recipes | References canonical products as transformation inputs and outputs |
| Inventory System | References physical products when recording stock, reservations, consumption, losses, and transfers |
| Logistics and Transportation | References products when determining cargo, handling, storage, and transport requirements |
| Research System | References products and technical requirements associated with research, technology, and design |
| Military System | References equipment, ammunition, components, and supplies required by military organisations |
| Government System | References products in procurement, regulation, restrictions, and public provision |
| Workforce and Population | References consumer goods and services associated with household consumption and population needs |
| Terrain and Geography | May reference construction materials, infrastructure equipment, and other products used in geographical development or maintenance |

The Catalogue is shared reference data. Its definitions must remain independent of the operational state maintained by these systems.

### 2.1 Ownership Rules

1. Every Catalogue entry must have a stable, unique identifier.
2. Goods and services must be defined once and referenced consistently.
3. Product identity must not depend on current owner, location, price, supplier, or inventory quantity.
4. The Catalogue must not independently create physical stock.
5. The Catalogue must not independently create financial value, revenue, expenditure, or market demand.
6. The Catalogue must not independently establish production capabilities or recipe availability.
7. The Catalogue must not independently grant technological, legal, or military permissions.
8. Catalogue changes must not silently rewrite historical transactions or executed production records.
9. Product variants and specifications must be represented explicitly when their differences affect simulation behaviour.
10. Systems must not infer unsupported physical, economic, or technical properties merely from a product's name or classification.
11. Operational systems may maintain their own state and records, but must preserve references to the authoritative Catalogue entry.
12. A Catalogue classification must not substitute for the specific eligibility, compatibility, or permission rules owned by another system.

---

## 3. Fundamental Concepts

### 3.1 Product

A Product is a canonical economic item recognised by the simulation. It may be a physical good, a designed component, an armament, or a non-physical service.

A product has one canonical identity and a defined set of applicable attributes.

Its identity is independent of its current owner, location, price, quantity, condition, or availability.

### 3.2 Good

A Good is a physical product or material that can exist as physical stock in the simulated world.

Examples include:

- Iron ore.
- Coal.
- Crude oil.
- Timber.
- Grain.
- Steel.
- Cement.
- Electrical components.
- Machine tools.
- Vehicles.
- Aircraft.
- Ammunition.
- Medical supplies.
- Consumer electronics.
- Clothing.

A Good may be raw material, an intermediate product, a finished product, a consumable, or a durable asset.

### 3.3 Service

A Service is a non-physical economic activity or deliverable.

Examples include:

- Freight transportation.
- Warehousing.
- Medical consultation.
- Equipment maintenance.
- Commercial insurance.
- Telecommunications.
- Professional services.
- Training.
- Construction services.
- Technical consulting.

Services may be purchased, sold, contracted, provided, and consumed. They do not automatically constitute physical inventory.

A service may consume physical goods, require labour, use equipment, or depend on infrastructure. Those requirements must be defined through the applicable service, production, workforce, inventory, or economic interfaces.

### 3.4 Designed Component

A Designed Component is a physical product whose identity or required characteristics are defined by a technical design, specification, or engineering configuration.

Examples include:

- A specified engine assembly.
- A designed electrical circuit.
- A defined aircraft component.
- A machine assembly.
- A specialised electronic component.
- A component manufactured to a defined engineering specification.

Designed Components are a distinct primary economic class because their identity and use may depend on explicit technical designs and compatibility requirements.

A Designed Component remains a physical product and may be represented as physical inventory.

The Catalogue identifies the component and its relevant shared attributes. Research owns applicable technology and design progression; Production and Recipes govern manufacture; Inventory governs physical stock; and the consuming system determines whether the component meets its requirements.

### 3.5 Armament

An Armament is a physical product whose primary economic class identifies it as a weapon, ammunition, or other explicitly designated military armament.

Examples may include:

- Small arms.
- Artillery weapons.
- Ammunition.
- Missiles.
- Torpedoes.
- Military weapon systems.

The classification does not independently establish military acceptance, availability, allocation, readiness, or operational effectiveness.

Military owns those decisions and requirements.

An Armament is a physical product and may be represented as physical inventory. Its identity, specifications, compatibility, and any instance-tracking requirements must be explicitly defined.

### 3.6 Catalogue Entry

A Catalogue Entry is the authoritative reference record for a product.

Each entry has a stable identifier, canonical name, classification, and required measurement convention. It may also define applicable physical properties, quality dimensions, compatibility information, lifecycle status, and other shared attributes.

### 3.7 Product Family

A Product Family groups related Catalogue entries for classification, discovery, reporting, or shared descriptive defaults.

For example, a family may group industrial fasteners, railway locomotives, petroleum products, or agricultural machinery.

Family membership does not automatically make its members interchangeable.

### 3.8 Product Variant

A Product Variant represents a distinct version of a product whose differences matter to the simulation.

Examples include:

- A truck with a different load capacity.
- A locomotive with different gauge compatibility.
- An aircraft with a different engine configuration.
- A machine tool with different precision capabilities.
- A weapon with different ammunition compatibility.

A variant must be represented separately when its differences affect production, performance, handling, compatibility, quality, or another simulated outcome.

Not every descriptive difference requires a separate Catalogue entry.

### 3.9 Product Specification

A Product Specification defines required properties or performance characteristics.

Specifications may be used to determine whether a product is suitable for a particular purpose. Examples include material grade, purity, dimensions, electrical characteristics, load capacity, pressure rating, or military acceptance criteria.

A specification may apply to multiple Catalogue entries. Conversely, one Catalogue entry may support multiple explicitly defined specifications where its underlying identity remains valid.

### 3.10 Product Instance

A Product Instance is a specific physical object or individually tracked asset, where the simulation requires object-level tracking.

Examples include an individual aircraft, vehicle, machine, or ship.

A Product Instance references its canonical Catalogue definition but has its own identity and operational state.

Not every Good requires instance-level tracking. Bulk materials, consumables, and other interchangeable goods may be represented through aggregate quantities or stock lots.

The relevant Inventory, equipment, or asset system determines the appropriate level of tracking.

---

## 4. Classification Framework

The Catalogue must distinguish between a product's physical form, primary economic class, economic category, technical classification, and additional designations.

These classifications describe different aspects of a product and must not be conflated.

### 4.1 Physical Form

Every Catalogue entry must define its physical form.

The initial values are:

| Value | Meaning |
|---|---|
| `physical` | The product exists as a physical good, material, component, or armament |
| `non_physical` | The product represents a service or other non-physical economic deliverable |

Physical form determines whether the product can be represented as ordinary physical inventory.

A physical product may provide a service when operated, but the physical product and the service remain distinct.

For example, a truck has physical form `physical`. Freight transportation has physical form `non_physical`.

### 4.2 Primary Economic Class

Every Catalogue entry must have exactly one primary economic class.

| Primary class | Physical form | Meaning |
|---|---|---|
| `Good` | `physical` | A physical product or material not primarily classified as a Designed Component or Armament |
| `Service` | `non_physical` | A non-physical economic activity or deliverable |
| `Designed Component` | `physical` | A physical product defined by an explicit technical design or engineering specification |
| `Armament` | `physical` | A physical weapon, ammunition item, or other designated military armament |

The physical-form and primary-class fields must remain consistent.

A `Service` must have physical form `non_physical`.

A `Good`, `Designed Component`, or `Armament` must have physical form `physical`.

These primary classes are mutually exclusive. A product must not simultaneously be classified as both a `Good` and an `Armament`, for example.

Where a product is a physical component of a larger system, its primary class must be assigned according to its defined identity and intended role. The presence of technical attributes alone does not automatically require classification as a Designed Component.

### 4.3 Economic Categories for Goods

Goods classified with primary economic class `Good` must belong to exactly one of the following initial economic categories:

| Category | Meaning | Examples |
|---|---|---|
| `Staple` | Goods associated with basic or routine household consumption and essential everyday requirements | Grocery staples, basic clothing, personal hygiene products |
| `Luxury` | Goods primarily associated with discretionary consumption, premium positioning, or non-essential expenditure | Luxury clothing, jewellery, premium electronics |
| `Industrial` | Goods primarily used in industrial activity, production, construction, commercial operations, or infrastructure | Industrial machinery, construction materials, industrial chemicals |

These three categories are mutually exclusive within the primary class `Good`.

They do not apply to Services, Designed Components, or Armaments as primary economic categories.

A product may have additional technical or descriptive classifications without changing its economic category.

The economic category must not be inferred solely from price. A high-priced industrial machine remains an Industrial Good, and a low-priced luxury item remains a Luxury Good if its defined category is unchanged.

### 4.4 Additional Product Classifications

The Catalogue may assign additional classifications that do not replace the primary economic class.

Examples include:

- Raw material.
- Agricultural product.
- Food and beverage.
- Fuel.
- Chemical.
- Basic material.
- Intermediate product.
- Component.
- Industrial equipment.
- Infrastructure equipment.
- Vehicle.
- Consumer product.
- Medical product.
- Military equipment.
- Packaging.
- Recovered material.
- Waste material.

These classifications support discovery, technical filtering, reporting, and integration with other systems.

They may be represented through a controlled classification hierarchy, secondary classifications, or typed attributes. The implementation must not treat them as competing primary economic classes.

### 4.5 Strategic Resource Designation

`Strategic Resource` is an additional designation, not a primary economic class.

A product designated as a Strategic Resource retains its primary economic class, physical form, economic category where applicable, and canonical identity.

For example, Steel may be a Good in the Industrial category and also carry the Strategic Resource designation.

Strategic Resource designation does not independently establish scarcity, ownership, extraction rights, production capacity, stock levels, market price, or military control.

Those matters belong to the relevant economic, industry, inventory, government, and military systems.

The initial strategic-resource definitions must remain consistent with the Economy System:

| Strategic resource | Canonical product concept | Initial associated industry |
|---|---|---|
| Petroleum (Oil) | Petroleum or crude oil, as explicitly defined in the Catalogue | Oil |
| Steel | Steel | Steelmaking |
| Aluminium | Aluminium | Industrial Manufacturing |
| Coal | Coal | Mining |
| Tungsten | Tungsten | Mining |
| Uranium | Uranium | Mining |
| Rare-earth Metals | Rare-earth metals | Mining |
| Rich Soils | Rich Soils | Agriculture |

These are the initial Strategic Resource designations. The table does not establish that the listed products are natural deposits in every case.

Steel and Aluminium are produced materials rather than natural deposits. Rich Soils represents an explicit agricultural resource abstraction in the Economy System. The associated industry identifies the initial economic relationship and does not by itself define a recipe or guarantee production.

Additional Strategic Resources may be introduced through explicit Catalogue and Economy updates.

### 4.6 Classification Independence

Classification must not substitute for technical properties or operational permissions.

Two goods in the same category may have different uses, physical properties, compatibility requirements, and production methods.

Two products with similar names may be incompatible.

A product classified as a military good is not automatically an accepted Armament. A product designated as a Strategic Resource is not automatically scarce or restricted.

The relevant system must evaluate the rules it owns.

---

## 5. Canonical Identifiers

Every Catalogue entry must have a stable identifier that other systems can store and reference.

Names are for human readability. Identifiers are for simulation integrity.

### 5.1 Identifier Requirements

A canonical identifier must:

- Be unique within the Catalogue.
- Remain stable throughout the entry's lifetime.
- Be independent of display name and localisation.
- Be independent of price, supplier, ownership, and location.
- Avoid encoding values that may change, such as market price or production method.
- Remain usable in saved games, historical records, production recipes, and transaction logs.
- Never be silently reassigned to a different product.
- Preserve referential integrity when the product is retired.

### 5.2 Identifier Format

The initial recommended readable-code convention is:

- `GOOD_<DOMAIN>_<NAME>`
- `SERVICE_<DOMAIN>_<NAME>`
- `COMPONENT_<DOMAIN>_<NAME>`
- `ARMAMENT_<DOMAIN>_<NAME>`

These are readable canonical codes, not a requirement that the implementation use these exact string prefixes as its internal database key.

Examples:

- `GOOD_MINING_IRON_ORE`
- `GOOD_ENERGY_COAL`
- `GOOD_ENERGY_CRUDE_OIL`
- `GOOD_MATERIAL_STEEL`
- `GOOD_MATERIAL_CEMENT`
- `GOOD_AGRICULTURE_WHEAT`
- `COMPONENT_ENGINE_ASSEMBLY`
- `COMPONENT_ELECTRICAL_CIRCUIT`
- `ARMAMENT_SMALL_ARMS_AMMUNITION`
- `ARMAMENT_ARTILLERY_WEAPON`
- `SERVICE_TRANSPORT_FREIGHT`
- `SERVICE_HEALTHCARE_MEDICAL_CONSULTATION`
- `SERVICE_MAINTENANCE_INDUSTRIAL_EQUIPMENT`

These examples illustrate the naming convention. They do not constitute a complete or final product catalogue.

If opaque internal identifiers or UUIDs are used, readable canonical codes may be retained as stable external identifiers.

### 5.3 Identifier Immutability

Once an identifier is referenced by a committed transaction, executed production order, saved game, inventory record, or other persistent simulation record, it must not be reassigned.

If a definition is retired, its identifier remains reserved.

If a product is fundamentally redefined, the system must determine whether the change warrants a new entry rather than silently changing the meaning of the existing identifier.

### 5.4 Names and Aliases

Each entry should support:

- A canonical name.
- An optional short name.
- An optional description.
- Optional aliases and alternative names.
- Optional historical names.
- Optional localisation strings.

Aliases must resolve to the canonical entry. They must not create duplicate product identities.

Alternative spellings or common names for the same material should normally resolve to one entry where the underlying product definition is equivalent.

Where a name has materially different meanings across countries, periods, or technical standards, the Catalogue must represent the distinction explicitly.

---

## 6. Core Catalogue Data Model

The following fields define the conceptual data contract. The implementation may use separate schemas for products, variants, specifications, families, and technical attributes.

### 6.1 Required Common Fields

Every Catalogue entry must include:

| Field | Description |
|---|---|
| `catalogue_id` | Stable, unique identifier |
| `canonical_name` | Primary human-readable name |
| `description` | Definition of the product |
| `physical_form` | `physical` or `non_physical` |
| `primary_economic_class` | `Good`, `Service`, `Designed Component`, or `Armament` |
| `category` | Required economic category for primary class `Good`; otherwise a defined class-appropriate category |
| `status` | Catalogue lifecycle state |
| `unit_of_measure` | Canonical unit or defined unit convention |
| `introduced_date` | Earliest simulation date on which the definition is historically valid, where applicable |
| `retired_date` | Optional date after which the product is retired for new use |
| `parent_entry_id` | Optional reference to a parent product or family |
| `aliases` | Optional alternative names |
| `classifications` | Optional additional controlled classifications |
| `designations` | Optional additional designations, including Strategic Resource |
| `metadata` | Additional supported descriptive information |

Every entry must satisfy the classification rules in Section 4.

Fields that do not apply may be omitted or explicitly marked as not applicable. A missing field must not be interpreted as permission to invent a value.

### 6.2 Good-Specific Fields

A Good may additionally define:

| Field | Description |
|---|---|
| `material_form` | Solid, liquid, gas, particulate, discrete object, or another supported physical form |
| `material_composition` | Composition where relevant and modelled |
| `mass_per_unit` | Mass per canonical unit, where applicable |
| `volume_per_unit` | Volume per canonical unit, where applicable |
| `density` | Density where supported and required |
| `quality_dimensions` | Supported quality attributes |
| `storage_requirements` | Relevant storage constraints |
| `handling_requirements` | Relevant handling constraints |
| `shelf_life` | Shelf-life definition where applicable |
| `hazard_classification` | Hazard classification where modelled |
| `durability_class` | Optional durable-product classification |
| `instance_tracking_required` | Whether individual instance tracking is required by the applicable model |
| `default_condition` | Default condition where explicitly defined |

These fields are conditional. A bulk commodity may require mass and quality attributes but not individual instance tracking.

### 6.3 Service-Specific Fields

A Service may additionally define:

| Field | Description |
|---|---|
| `service_unit` | Canonical unit in which the service is measured |
| `delivery_mode` | How the service is provided, where relevant |
| `duration_basis` | Time basis where quantity depends on duration |
| `capacity_basis` | Relevant capacity unit where applicable |
| `quality_dimensions` | Service-quality attributes where modelled |
| `location_constraints` | Geographic or location requirements |
| `provider_capabilities` | Capability requirements for service provision |
| `consumption_semantics` | How service delivery and consumption are represented |

A Service must not be assigned a physical stock quantity merely because it is measured in units.

Freight transportation, for example, may be measured in tonne-kilometres or another explicitly defined unit. The service is not physical inventory, and the cargo remains separately accounted for as physical stock.

### 6.4 Designed Component-Specific Fields

A Designed Component may additionally define:

| Field | Description |
|---|---|
| `design_id` | Reference to the applicable authoritative design, where available |
| `specification_ids` | References to applicable technical specifications |
| `interface_requirements` | Defined interfaces and compatibility requirements |
| `performance_attributes` | Typed performance attributes relevant to simulation |
| `manufacturing_constraints` | Shared product constraints, where applicable |
| `revision_id` | Reference to a defined design or product revision |

The Catalogue must not become the authoritative owner of the Research System's technology progression or design-development state.

It may reference authoritative designs and expose the shared product attributes required by dependent systems.

### 6.5 Armament-Specific Fields

An Armament may additionally define:

| Field | Description |
|---|---|
| `military_specification_ids` | References to relevant military specifications |
| `compatibility_requirements` | Explicitly defined compatible platforms, systems, or ammunition |
| `technical_attributes` | Typed physical and performance attributes |
| `consumable_or_durable` | Whether the product is normally consumed or retained as a durable asset |
| `instance_tracking_required` | Whether individual asset tracking is required |

The Catalogue does not determine whether an Armament is approved, available, assigned, or operationally effective. Those decisions belong to Military and the other applicable systems.

### 6.6 Operational State Exclusion

Operationally changing values must remain outside the Catalogue.

Examples include:

- Current market price.
- Current quantity.
- Current owner.
- Current location.
- Current condition of an individual stock lot.
- Current facility capacity.
- Current production output.
- Current market availability.
- Current technology eligibility.
- Current military allocation.

These values belong to the relevant authoritative runtime system.

---

## 7. Units of Measurement

Every product that participates in quantitative simulation must have a defined measurement convention.

Quantities must not be interpreted from product names, category membership, or informal assumptions.

### 7.1 Measurement Dimensions

The Catalogue and shared measurement definitions must support the units required by the simulation.

| Measurement dimension | Example units | Typical applications |
|---|---|---|
| Mass | Kilogram, tonne | Ores, coal, steel, grain |
| Volume | Litre, cubic metre | Liquids and gases where defined |
| Count | Individual unit, item | Vehicles, components, packaged goods |
| Length | Metre, kilometre | Timber, cable, pipe |
| Area | Square metre, hectare | Land-related products or services where applicable |
| Time | Second, hour, day | Service delivery and operating duration |
| Energy | Joule, kilowatt-hour | Energy quantities |
| Power | Watt, kilowatt, megawatt | Generation capacity and rated output |
| Distance | Metre, kilometre | Transport and distance-based services |
| Mass-distance | Tonne-kilometre | Freight service measurement where adopted |
| Passenger-distance | Passenger-kilometre | Passenger transportation services |
| Service unit | Explicitly defined unit | Consultations, inspections, treatments, and other deliverables |

The shared measurement contract must define canonical units, conversion rules, dimensional compatibility, precision, and rounding.

### 7.2 Canonical Units

Each quantitative product must specify a canonical unit or unit convention.

Examples:

- Bulk iron ore may be measured in tonnes.
- Crude oil may be measured in barrels or cubic metres, subject to an explicit canonical convention and conversion rules.
- Steel components may be measured by count or mass depending on the product definition.
- Electricity delivered may be measured in kilowatt-hours, while generation capacity is measured in kilowatts or megawatts.
- A medical consultation may be measured per consultation or by duration, depending on the service definition.

The same unit must not be used with incompatible meanings.

### 7.3 Conversion Rules

Conversions must use explicit, valid conversion definitions.

A conversion may be a fixed mathematical relationship or may require product-specific properties.

For example, conversion between mass and volume may require a defined density. If density varies by grade, temperature, composition, or condition, the conversion must use the applicable value.

Where a valid conversion cannot be established, the simulation must reject the conversion rather than guess.

### 7.4 Compound Units

Some products and services require compound units, including:

- Mass per hour.
- Units per day.
- Energy per unit of output.
- Tonne-kilometres.
- Passenger-kilometres.
- Cost per unit, where used by the economic system.

Compound units must have explicit dimensional meaning.

The Catalogue must not conflate a rate with a total quantity. A power rating is not itself an amount of energy generated.

### 7.5 Quantity Precision

The Catalogue defines the measurement dimension and canonical unit. The shared simulation architecture defines numerical precision and rounding.

Discrete goods may require integer quantities. Bulk goods and continuous flows may permit fractional quantities.

Permission to represent a fractional quantity does not automatically imply that a product can be physically split into arbitrary fractions. Such restrictions must be defined by the product or relevant operational system.

### 7.6 Consumer Consumption Intervals

For consumer Goods, Economy may define a Good Unit as an economic convention for expressing the standard consumption requirement of one individual over a specified Consumption Interval. This convention is not a physical measurement unit and does not replace or redefine the Catalogue's canonical unit for a physical product.

The Catalogue is authoritative for the canonical physical unit of each physical product and for the defined delivery unit of a non-physical Service. Other systems may define demand, consumption intervals, rates or economic valuation, but must reference the Catalogue's product identity and must not redefine its unit of measure.

The Catalogue must keep these concepts separate:

- **Physical unit:** The canonical unit in which actual physical quantity is measured, such as kilograms, litres or individual items.
- **Consumption interval:** The period over which a standard consumption requirement is assessed.
- **Consumption requirement:** The quantity or rate expected by the applicable Population or Economy rules.
- **Economic valuation:** The monetary value assigned by the Economy System.

For example, a household may have a weekly requirement for a defined quantity of a physical product. Inventory continues to record the product using its canonical physical unit, while Population and Economy determine the consumption requirement and its economic interpretation. An economic price must explicitly identify the quantity unit or demand convention to which it applies.

A consumption interval or Good Unit convention must not change the physical definition of a product, the unit in which its stock is measured, or the unit used for production, inventory, transport or trade.

### 7.7 Unit Authority

The Catalogue owns the canonical unit convention associated with each product and references the shared measurement contract for conversions.

Recipe inputs and outputs must use compatible units.

Inventory must use compatible units when aggregating, reserving, consuming, and transferring stock.

Economy must use explicit quantity and price denominators when valuing transactions.

No system may silently convert incompatible units or substitute an undocumented conversion factor.

---

## 8. Product Identity, Families, Variants, and Specifications

The Catalogue must represent product differences at the level necessary for simulation without creating unnecessary duplication.

### 8.1 When to Create a Separate Entry

A separate Catalogue entry should be created when a difference materially affects how a product is produced, stored, traded, transported, consumed, or used.

Examples include:

- Steel grades with different industrial uses or processing requirements.
- Distinct fuels with materially different properties.
- Components that are not mechanically interchangeable.
- Ammunition types requiring different weapons or having different operational effects.
- Vehicles with materially different capacities or compatibility.
- Goods with different handling or storage requirements.

A separate entry is not automatically required for a different brand, supplier, owner, location, or price.

### 8.2 When to Use a Variant

A variant is appropriate where products share a common base identity but have distinct configurations that affect simulation behaviour.

A variant may inherit shared descriptive properties from its parent while explicitly defining the attributes that differ.

The implementation must establish which attributes are inherited, which are overridden, and which are mandatory for every variant.

A variant must have a stable identifier if it is referenced independently by recipes, inventory records, equipment records, or other persistent simulation state.

### 8.3 When to Use a Specification

A specification is appropriate when a requirement describes acceptable properties rather than a unique product identity.

For example, a process may accept steel conforming to a defined grade or dimensional standard.

The Catalogue supplies the relevant attribute definitions. The recipe defines the required input conditions. Inventory provides the actual attributes of available stock. Production determines whether the input satisfies the recipe's requirements.

A specification must not silently make otherwise incompatible goods interchangeable.

### 8.4 Interchangeability

The Catalogue may support explicit compatibility relationships between products.

A compatibility relationship should identify:

- The relevant product or specification.
- The compatible alternative.
- The context in which compatibility applies.
- Applicable restrictions or conversion rules.
- Relevant technical prerequisites.

Compatibility is contextual. Two products may be interchangeable for one process but not another.

The Catalogue may define stable compatibility information. Production, Military, Logistics, or another consuming system determines whether the compatibility is sufficient for a particular operation.

### 8.5 Brand and Manufacturer Identity

A branded product may require its own Catalogue entry when brand-specific characteristics affect the simulation.

Manufacturer identity must not automatically define a unique Good.

Two manufacturers may produce the same standardised material, while one manufacturer may produce several distinct variants.

Where brand identity matters, it should be represented through an explicit manufacturer, brand, model, or variant relationship rather than embedded inconsistently in the product name.

---

## 9. Quality, Condition, and Technical Properties

The Catalogue must define which attributes are meaningful for a product. It must not require every product to use the same attribute schema.

### 9.1 Quality Dimensions

Depending on category, quality dimensions may include:

- Grade.
- Purity.
- Composition.
- Moisture content.
- Dimensions.
- Tolerance.
- Strength.
- Durability.
- Energy content.
- Performance rating.
- Contamination.
- Certification.
- Service quality.

Only attributes with a defined meaning, unit where applicable, and validation rule may influence simulation outcomes.

### 9.2 Intrinsic Properties and Stock Condition

The Catalogue defines canonical properties and the dimensions along which condition may vary.

Inventory records the condition of actual stock.

For example, the Catalogue may define the nominal specifications of a machine component. Inventory records whether a particular stock lot is damaged, inspected, contaminated, or otherwise restricted.

The Catalogue must not store a single current condition value as though it applies to every unit in existence.

### 9.3 Quality Constraints

A recipe, military specification, or other authorised requirement may impose quality constraints on a product.

The Catalogue supplies the relevant attribute definitions and supported values. The requesting system determines the required threshold and whether the product satisfies it.

Where a quality dimension is not defined, the simulation must not invent a numerical value or assume compatibility.

### 9.4 Technical Properties

Goods may require technical properties such as:

- Rated power.
- Load capacity.
- Maximum pressure.
- Operating temperature.
- Dimensions.
- Mechanical compatibility.
- Electrical characteristics.
- Material composition.
- Required consumables.
- Supported interfaces.

These properties should be represented only where they affect simulation behaviour or are needed by dependent systems.

Technical data must remain structured, typed, and relevant to defined system behaviour. The Catalogue must not become an unrestricted repository for every imaginable attribute.

---

## 10. Lifecycle and Historical Availability

Products may be introduced, modified, restricted, or retired as the simulation develops.

The Catalogue must distinguish between the existence of a definition and its availability for use in a particular historical period or jurisdiction.

### 10.1 Lifecycle States

Catalogue entries may use the following lifecycle states:

- `draft`: The definition is incomplete or under development.
- `validated`: The definition has passed structural validation but is not necessarily available for use.
- `active`: The definition is approved for reference, subject to other constraints.
- `deprecated`: The definition should no longer be selected for new content but remains supported for compatibility or historical reasons.
- `retired`: The definition is no longer available for new use under the applicable rules.

The implementation may use a different state model provided these distinctions are preserved.

### 10.2 Historical Introduction

A product may have an introduction date that determines when its definition is historically valid in the simulation.

A Catalogue entry may exist in reference data before its introduction date, but its presence in the Catalogue does not make it available to the starting world.

A product introduced after 1955 must not become available merely because its definition is loaded.

### 10.3 Technological Availability

Historical validity and technological availability are separate.

A product may have existed historically but remain unavailable to a particular country, company, or facility because the necessary technology, equipment, licensing, infrastructure, or supply chain is absent.

The Catalogue defines the product. Research, Production, Government, Economy, and other applicable systems determine availability in a specific context.

### 10.4 Retirement

Retiring an entry must not delete historical references.

Existing inventory records, transaction records, executed recipes, and saved games must remain interpretable.

Where a product is replaced by a successor, the Catalogue may record a successor relationship.

A successor relationship does not automatically convert existing stock or make products interchangeable.

### 10.5 Starting-World Catalogue

The Catalogue must support a comprehensive reference library while allowing the 1955 starting world to select historically appropriate goods and services.

Starting-world configuration determines which products are available through existing technologies, industries, imports, inventories, and other applicable simulation state.

The Catalogue must not assume that every active entry exists in every country or market.

---

## 11. Production Integration

Production uses Catalogue entries to identify the physical goods and services involved in industrial activity.

The Catalogue does not independently define production methods.

### 11.1 Recipe Inputs

Production Processes and Recipes must reference products through canonical Catalogue identifiers.

A recipe input may represent a raw material, intermediate Good, Designed Component, Armament, consumable, fuel, utility, or another explicitly supported input.

The Catalogue supplies product identity and relevant shared properties. The recipe defines the quantity required and the conditions under which the input is consumed or otherwise used.

### 11.2 Recipe Outputs

A recipe output must reference a canonical Catalogue entry where the result is represented by a defined product.

Production determines the actual quantity generated during execution.

Inventory records physical outputs. Non-physical service outputs must be handled through the applicable service or economic execution contract.

The Catalogue must not independently create an output or update inventory.

### 11.3 Industry Compatibility

An industry may require or produce particular products.

Industry Data and Production define the applicable industrial relationships and execution constraints.

The Catalogue provides the referenced product definitions. It must not independently determine which industries are permitted to manufacture a product.

### 11.4 Production Capability

A Catalogue entry may describe technical properties relevant to production, but it does not establish that a facility possesses the necessary capabilities.

Production determines whether the facility, equipment, technology, workforce, inputs, and operating conditions satisfy the requirements.

### 11.5 Industry Scale

Catalogue quantities and units must not be confused with Industry Scale.

A product's canonical unit describes how its quantity is measured. Industry Scale describes the relevant economic magnitude of an industry instance under Industry Data.

The Catalogue must not define an industry's size, capacity, output target, workforce, facilities, or financial value.

### 11.6 Recipe Versioning

An executed production record must retain a reference to the applicable recipe version and enough information to interpret its inputs and outputs historically.

A Catalogue update must not silently change the meaning of an already executed recipe.

Changes that materially alter product identity, measurement meaning, or technical compatibility must follow the versioning and migration rules of the shared simulation architecture.

---

## 12. Inventory Integration

Inventory uses Catalogue definitions to identify and validate physical stock.

### 12.1 Physical Products

Only entries with physical form `physical` may be held as ordinary physical inventory.

This includes products whose primary economic class is `Good`, `Designed Component`, or `Armament`.

A Service may be represented through service-delivery records, contracts, or other relevant system state, but it must not automatically become a physical stock holding.

### 12.2 Stock Attributes

The Catalogue defines the supported properties and quality dimensions that may apply to a product.

Inventory records the actual values or states associated with a stock lot or individual instance.

A stock lot may contain only a subset of the Catalogue's possible attributes where the remaining attributes are irrelevant or fixed by the product definition.

### 12.3 Storage and Handling

The Catalogue may define storage and handling requirements.

Inventory records stock quantity, location, and relevant condition. The applicable location, facility, and Logistics systems determine whether storage and handling requirements are satisfied.

### 12.4 Unit Compatibility

Inventory must use compatible units when aggregating, reserving, consuming, or transferring stock.

The Catalogue's canonical unit and shared conversion rules must be used where conversion is permitted.

Inventory must not assume that products sharing a category can be combined into one quantity.

### 12.5 No Stock Creation

Adding a Catalogue entry does not create stock.

Making a product historically valid does not create stock.

Creating a recipe that outputs a product does not create stock.

Physical stock must enter Inventory through an authorised receipt, production output, extraction result, transfer, or other explicitly supported operation.

Inventory remains the authoritative owner of physical stock quantities and stock-changing transactions.

---

## 13. Economy and Market Integration

The Catalogue identifies what is being traded. Economy determines the financial consequences and market behaviour associated with that activity.

### 13.1 Prices

The Catalogue must not assign a universal current market price to each product.

A product may have different prices depending on market, location, period, quality, contractual terms, scarcity, taxes, and other economic conditions.

Any reference price or indicative valuation stored alongside Catalogue data must be clearly distinguished from the authoritative price used in an economic transaction.

The Economy System remains authoritative for its defined pricing concepts, including Base Price, Global Price, Domestic Market Price, Company Selling Price, and Transaction Price.

### 13.2 Currency

Catalogue entries must not define their own currency.

Monetary values belong to Economy and its financial records. The simulation's authoritative currency is Dollar, with canonical identifier `DOLLAR`, represented by `$`.

The Catalogue must not introduce foreign-exchange rules or independently perform currency conversion.

### 13.3 Demand

The existence of a product does not imply that demand exists for it.

Demand may arise from households, companies, government procurement, military requirements, production inputs, infrastructure maintenance, or other systems.

The Catalogue may provide classifications that help those systems identify relevant products, but it must not independently create demand.

### 13.4 Substitutes

Two products may be substitutes in a market without being technically interchangeable in production or use.

Market substitution belongs to the relevant economic decision rules.

Technical compatibility belongs to the applicable product specification and consuming system.

The Catalogue must not equate economic substitutability with physical equivalence.

### 13.5 Product Value

The Catalogue may support attributes used by valuation or economic models, but it does not independently determine market value, company revenue, profitability, or asset valuation.

Those outcomes depend on the relevant financial and economic systems.

### 13.6 Financial Transactions and Physical Delivery

A purchase, sale, or financial settlement does not by itself establish that physical delivery has occurred.

Economy owns the financial record and transaction state. Inventory owns the physical stock movement. Logistics owns the applicable movement of goods.

The Catalogue provides the common product identity referenced by these records.

A product identifier must remain consistent across the financial transaction, any delivery instruction, the Inventory movement, and the final receipt.

---

## 14. Logistics and Transportation Integration

Products must expose sufficient information for Logistics to determine handling and transportation requirements where those requirements are simulated.

### 14.1 Cargo Properties

Relevant physical products may define:

- Mass per unit.
- Volume per unit.
- Physical form.
- Packaging requirements.
- Handling restrictions.
- Hazard classification.
- Temperature or environmental constraints.
- Loading or unloading requirements.
- Other supported transport attributes.

Logistics uses these properties alongside transport capacity, route constraints, infrastructure, and operating rules.

### 14.2 Transport Compatibility

The Catalogue may define transport restrictions or handling requirements.

Logistics determines whether a particular vehicle, ship, aircraft, railway, or other transport asset can carry a product under current conditions.

A product's existence in the Catalogue does not imply that every transport mode can carry it.

### 14.3 Service Products

Transportation services may be defined as Services even though the goods being transported remain physical inventory.

The freight service and freight cargo must retain separate identities.

The Service represents the economic activity of transportation. The cargo represents the physical products being moved.

### 14.4 In-Transit Goods

The Catalogue does not track goods in transit.

Inventory and Logistics must ensure that physical stock remains accounted for during transport without being simultaneously available as stock at both the origin and destination.

The product's canonical identity must remain unchanged during the transfer unless an explicitly defined, authorised transformation occurs.

---

## 15. Military and Government Integration

The Catalogue must support the specialised requirements of Government and Military without making every product a military-specific product.

### 15.1 Military Products

Military equipment, ammunition, vehicles, communications equipment, medical supplies, and other military products must reference canonical Catalogue entries.

Where military compatibility or acceptance criteria affect use, the relevant specification must be represented explicitly.

A generic product category is not sufficient to establish that a product is acceptable for military use.

### 15.2 Equipment and Consumables

The Catalogue must support the distinction between durable equipment and consumable products where that distinction affects the simulation.

Examples include:

- A vehicle as a durable asset.
- Fuel as a consumable.
- Ammunition as a consumable.
- Spare parts as components.
- Maintenance as a non-physical Service.

The relevant system determines whether a durable product requires individual instance tracking, whether it can be repaired, and how its condition changes.

### 15.3 Military Specifications

Military may define acceptance requirements, compatibility constraints, and operational classifications.

These requirements must reference Catalogue entries and their relevant properties.

The Catalogue must not independently establish military doctrine, equipment allocation, readiness, or operational effectiveness.

### 15.4 Government Procurement and Regulation

Government may reference products when defining procurement programmes, regulated products, public services, or restrictions.

The Catalogue identifies the referenced product. Government determines the legal or policy consequences.

A product's existence in the Catalogue does not imply that its manufacture, sale, import, export, ownership, or use is legally permitted in every jurisdiction.

---

## 16. Data Validation and Integrity

Catalogue definitions must be validated before activation for use by authoritative simulation systems.

### 16.1 Structural Validation

A Catalogue entry must not be activated if:

- Its canonical identifier is missing or duplicated.
- Its canonical name is missing.
- Its primary economic class is invalid.
- Its physical form is missing or inconsistent with its primary economic class.
- Its required economic category is missing or invalid.
- Its required unit convention is missing.
- Its parent or successor reference is invalid.
- Its historical dates are contradictory.
- Its required attributes do not conform to the defined schema.
- Its mandatory references cannot be resolved.
- Its designation or classification references are invalid.
- Its required measurement definitions are unavailable.

### 16.2 Classification Validation

Validation must confirm that:

- Every entry has exactly one primary economic class.
- A Service has physical form `non_physical`.
- A Good, Designed Component, or Armament has physical form `physical`.
- A product with primary class `Good` belongs to exactly one of `Staple`, `Luxury`, or `Industrial`.
- The three economic categories are not assigned as primary economic categories to Services, Designed Components, or Armaments.
- Strategic Resource is represented as an additional designation rather than a primary economic class.
- Additional classifications do not contradict the primary class or physical form.

### 16.3 Unit Validation

Validation must confirm that:

- Required units are defined.
- Conversion rules are dimensionally compatible.
- Fixed conversions use valid definitions.
- Property-dependent conversions identify the required properties.
- Rates and total quantities are not treated as interchangeable.
- Unsupported conversions are rejected.
- Consumer consumption intervals are not treated as physical measurement units.
- Economic price denominators are not confused with physical units.

### 16.4 Attribute Validation

Product attributes must use defined data types and units.

A mass must not be stored as an untyped string where a numeric quantity is required. A categorical condition must not be represented as arbitrary free text if other systems depend on a defined state.

The implementation must distinguish between mandatory attributes, optional attributes, and attributes that are not applicable.

### 16.5 Reference Integrity

All systems must reference canonical identifiers rather than create independent product definitions.

Where another system needs a local representation for performance or historical purposes, it must preserve the reference to the authoritative Catalogue entry and follow the applicable versioning rules.

### 16.6 Duplicate Detection

The Catalogue should support detection of potential duplicate entries.

Duplicate detection may compare names, aliases, classifications, technical properties, and specifications.

Automated similarity must not merge entries without validation. Two products may have similar names while differing materially in physical properties, identity, or compatibility.

### 16.7 Validation Outcomes

Validation should return structured results identifying:

- Missing fields.
- Invalid references.
- Incompatible units.
- Contradictory attributes.
- Invalid lifecycle dates.
- Potential duplicate entries.
- Unsupported properties.
- Invalid classifications.
- Other relevant integrity failures.

Invalid definitions must not be silently repaired by guessing missing values.

---

## 17. Versioning and Change Management

The Catalogue must preserve stable product identity while allowing definitions to evolve.

### 17.1 Definition Changes

A change to a descriptive field, such as correcting a spelling or improving a description, may not require a new semantic version.

A change that materially alters the product's identity, measurement meaning, technical compatibility, or simulation behaviour requires explicit review.

Where the change would alter the interpretation of historical records, the system must preserve the previous definition or create a new Catalogue entry.

### 17.2 Historical References

Executed production records, inventory transactions, and historical economic records must remain interpretable after Catalogue updates.

A saved game must not silently reinterpret a historical transaction because a Catalogue description or technical definition changed.

Where a product definition requires versioned technical attributes, relevant records must retain enough information to reconstruct the definition applicable at the time.

### 17.3 Recipe and Catalogue Versioning

An executed production record must reference the exact recipe version used for execution.

Where interpretation depends on a particular Catalogue definition or technical specification version, the record must preserve the relevant reference or snapshot required by the shared historical-record contract.

Updating a Catalogue entry must not silently rewrite previously executed production quantities, input requirements, output identities, or historical stock records.

### 17.4 Retirement and Replacement

Retired entries must remain available for historical reference.

Where a product is replaced, the Catalogue may define relationships such as:

- `replaced_by`
- `successor_of`
- `variant_of`
- `member_of_family`

These relationships must have explicit meanings.

A replacement relationship must not automatically convert stock, update production recipes, or declare technical equivalence.

### 17.5 Save Compatibility

Catalogue changes must follow the project's save-compatibility and data-migration conventions.

A saved game referencing an older Catalogue definition must either continue to resolve that definition or undergo an explicit, validated migration.

Unknown identifiers must not be silently substituted with a vaguely similar product.

---

## 18. Determinism and Runtime Behaviour

The Catalogue is primarily reference data. Its runtime role is to supply stable definitions and validated attributes to other systems.

### 18.1 Deterministic Lookup

A lookup by canonical identifier must resolve to one authoritative entry under the applicable Catalogue version.

The result must not depend on display name, localisation, database ordering, or incidental execution timing.

### 18.2 No Implicit Simulation Effects

Creating or loading a Catalogue entry must not automatically:

- Create physical inventory.
- Establish a production facility.
- Create an industrial company.
- Generate demand.
- Set a market price.
- Unlock a technology.
- Authorise a product for military use.
- Grant a legal permission.
- Create a transport route.
- Make the product available in every country.
- Establish a Strategic Resource deposit.
- Create a company or industry that produces the product.

Those effects must be produced by the systems that own them.

### 18.3 Reference Data and Runtime State

The Catalogue must separate stable product definitions from state that changes throughout the simulation.

A vehicle's canonical definition may specify its type and nominal characteristics. An individual vehicle's location, owner, condition, fuel level, and operational status belong to the relevant runtime systems.

A material's canonical definition may specify its measurement unit and quality dimensions. A stock lot's actual quantity, condition, ownership, and location belong to the relevant Inventory and ownership records.

### 18.4 Performance and Caching

Systems may cache Catalogue definitions for performance, provided cached data remains consistent with the authoritative definition and versioning rules.

A cache must not become an independent source of truth.

Catalogue updates and save migrations must invalidate or refresh affected cached definitions according to the shared architecture.

### 18.5 Deterministic Conversion and Compatibility

Unit conversion and compatibility checks must produce deterministic results under identical inputs and the same applicable definitions.

Results must not depend on unspecified rounding, database ordering, or missing attributes that are silently guessed.

---

## 19. System Interfaces

The Catalogue should expose a small set of stable logical interfaces.

These are conceptual contracts rather than mandatory programming-language function names.

### 19.1 Get Catalogue Entry

**Purpose:** Retrieve a canonical product definition.

**Inputs:**

- Catalogue identifier.
- Optional required definition version.

**Outputs:**

- Canonical definition.
- Physical form and primary economic class.
- Applicable economic category and additional classifications.
- Unit convention.
- Relevant shared attributes.
- Lifecycle and historical validity information.
- Definition version metadata, where supported.

### 19.2 Search Catalogue

**Purpose:** Discover entries by category, family, name, alias, or other supported descriptive criteria.

**Inputs:**

- Optional search term.
- Optional primary economic class.
- Optional physical form.
- Optional category.
- Optional family.
- Optional additional classification or designation.
- Optional historical or lifecycle filters.

**Outputs:**

- Matching Catalogue identifiers.
- Relevant descriptive metadata.

Search results must not replace canonical identifier references in persistent simulation records.

### 19.3 Validate Catalogue Reference

**Purpose:** Confirm that an identifier resolves to an appropriate Catalogue entry.

**Inputs:**

- Catalogue identifier.
- Expected primary economic class, where required.
- Expected physical form, where required.
- Optional applicability context.

**Outputs:**

- Reference validity.
- Resolved product classification.
- Relevant lifecycle or applicability information.
- Structured failure information.

A valid Catalogue reference does not automatically mean the product is available or suitable for a particular operation.

### 19.4 Validate Product Compatibility

**Purpose:** Evaluate whether a product satisfies a defined compatibility or specification requirement.

**Inputs:**

- Product or variant identifier.
- Required specification or compatibility condition.
- Relevant context, where applicable.

**Outputs:**

- Compatibility result.
- Relevant attribute comparisons.
- Missing information or structured failure reasons.

The consuming system remains responsible for deciding whether the result satisfies its operational requirements.

### 19.5 Resolve Unit Conversion

**Purpose:** Retrieve or calculate a valid conversion between compatible units.

**Inputs:**

- Catalogue identifier where product-dependent conversion is required.
- Source unit.
- Destination unit.
- Required conversion properties, where applicable.

**Outputs:**

- Conversion definition or result.
- Applicable conditions.
- Structured failure information if the conversion is undefined.

### 19.6 Catalogue Events

Where runtime Catalogue updates are supported, the system may publish events such as:

- `catalogue_entry_created`
- `catalogue_entry_validated`
- `catalogue_entry_activated`
- `catalogue_entry_updated`
- `catalogue_entry_retired`
- `catalogue_entry_replaced`

These are illustrative names and should follow the shared event-naming and versioning conventions.

Catalogue events must not independently trigger physical production, inventory creation, purchases, or other effects that belong to separate systems.

---

## 20. Initial Catalogue Scope

The initial Catalogue must be broad enough to support the core economic and industrial systems of the 1955 starting world without attempting to model every product that could exist throughout the simulation.

### 20.1 Required Initial Coverage

The foundational Catalogue should include entries necessary to represent:

- Natural resources and extracted materials.
- Agricultural goods and food inputs.
- Fuels and energy-related goods.
- Basic industrial materials.
- Intermediate materials and components.
- Designed Components and engineering assemblies.
- Industrial equipment and tools.
- Transport equipment.
- Construction materials.
- Consumer goods required by the household consumption model.
- Medical and public-service goods where relevant.
- Military equipment, Armaments, and supplies.
- Core commercial and industrial services.
- Freight, storage, maintenance, and other services required by the operating economy.
- Waste and recovered materials where these are tracked.
- The initial Strategic Resources defined by the Economy System.

### 20.2 Initial Economic Good Categories

The Economy System's initial economic categories must remain consistent with this Catalogue.

**Staple Goods**

- Grocery Staples.
- Basic Clothing.
- Household Cleaning Products.
- Personal Hygiene Products.
- Basic Household Supplies.
- Basic Furniture and Household Essentials.

**Luxury Goods**

- Luxury Clothing and Accessories.
- Jewellery and Watches.
- Cosmetics and Perfumes.
- Furniture and Home Furnishings.
- Premium Electronics.
- Luxury Motor Vehicles.
- Premium Recreational Products.

**Industrial Goods**

- Industrial Machinery.
- Machine Tools.
- Industrial Equipment.
- Construction Materials.
- Electrical Equipment.
- Industrial Chemicals.
- Commercial Vehicles.
- Agricultural Machinery.
- Transport Equipment.
- General-Purpose Industrial Components.

These are initial product concepts and categories. Each concept must be decomposed into canonical entries where distinct products, units, specifications, or simulation behaviours require separate identities.

The broad names in this list must not be treated as sufficient technical definitions for every product within them.

### 20.3 Initial Strategic Resources

The initial Strategic Resource designations are:

1. Petroleum (Oil).
2. Steel.
3. Aluminium.
4. Coal.
5. Tungsten.
6. Uranium.
7. Rare-earth Metals.
8. Rich Soils.

Each designation must reference an appropriate canonical Catalogue entry.

The designation is additional metadata. It does not replace the primary economic class or create a separate physical product.

### 20.4 Starting-World Availability

The initial Catalogue must distinguish between entries defined in the reference library and entries available to the world on 1 January 1955.

Starting-world configuration determines:

- Which products are historically valid.
- Which products are produced domestically.
- Which products may be imported.
- Which products exist in initial inventory.
- Which products require technologies or capabilities not yet available.
- Which products are unavailable or restricted in particular jurisdictions.

The Catalogue supplies the definitions needed to express these conditions. It does not independently determine their initial distribution.

### 20.5 Incremental Expansion

New entries may be added as the simulation requires additional industrial sectors, consumer markets, military systems, technologies, or services.

Adding a new entry must not require changes to the core Inventory, Production, or Economy data models if the entry conforms to the established Catalogue contract.

Where a new product requires new simulation behaviour, the relevant system must define and implement that behaviour explicitly.

---

## 21. Acceptance Criteria

The Good and Service Catalogue is ready for foundational integration when the following conditions are satisfied.

### 21.1 Identity and Classification

- [ ] Every Catalogue entry has a stable, unique identifier.
- [ ] Every entry has exactly one primary economic class.
- [ ] Physical form is explicitly defined and consistent with the primary class.
- [ ] Goods, Services, Designed Components, and Armaments are distinguished explicitly.
- [ ] Goods belong to exactly one of `Staple`, `Luxury`, or `Industrial`.
- [ ] Strategic Resource is an additional designation rather than a primary class.
- [ ] Entries have canonical names and defined categories.
- [ ] Product families, variants, and specifications can be represented where needed.
- [ ] Aliases resolve to canonical entries without creating duplicates.
- [ ] Product identity is independent of owner, location, price, and stock quantity.

### 21.2 Units and Attributes

- [ ] Every quantitatively modelled product has a defined unit convention.
- [ ] Units and conversions follow the shared measurement contract.
- [ ] Product-dependent conversions require explicit properties.
- [ ] Technical attributes use defined types and units.
- [ ] Quality dimensions can be represented where relevant.
- [ ] Consumer consumption intervals are distinct from physical units.
- [ ] Economic valuation is distinct from physical quantity.
- [ ] Physical goods can be distinguished from non-physical services in Inventory and Production interfaces.

### 21.3 Integration

- [ ] Production recipes can reference canonical products.
- [ ] Inventory can reference physical products without redefining their identity.
- [ ] Economy can reference products in financial transactions.
- [ ] Logistics can access relevant cargo and handling properties.
- [ ] Military systems can reference products and technical specifications.
- [ ] Historical availability can be evaluated independently of Catalogue existence.
- [ ] Strategic Resource designations remain separate from product identity and physical stock.

### 21.4 Integrity and History

- [ ] Invalid Catalogue definitions are rejected.
- [ ] Duplicate identifiers are prohibited.
- [ ] Invalid references are detected.
- [ ] Retired entries remain available for historical reference.
- [ ] Material changes preserve historical interpretation.
- [ ] Catalogue updates do not independently create stock or financial transactions.
- [ ] Save-and-load operations preserve valid Catalogue references or perform explicit migrations.
- [ ] Executed production records remain interpretable against the applicable recipe and Catalogue definitions.

### 21.5 Determinism

- [ ] Lookup by canonical identifier resolves unambiguously.
- [ ] Unit conversion produces deterministic results under identical definitions and inputs.
- [ ] Compatibility checks use explicit rules.
- [ ] Missing attributes do not produce invented technical properties.
- [ ] The same product definition can be referenced consistently across systems.
- [ ] Physical form, primary economic class, and economic category validation is deterministic.

---

## 22. Unresolved Design Decisions

The following decisions should be resolved through shared contracts and implementation testing rather than assumed by this specification.

1. **Canonical identifier implementation:** Determine whether readable codes, opaque identifiers, or a combination will be used.
2. **Measurement registry:** Finalise canonical units, compound units, conversion definitions, precision, and rounding.
3. **Catalogue schema:** Determine the concrete representation for common fields, product-specific fields, and technical attributes.
4. **Classification hierarchy:** Finalise additional classifications and product-family structures without weakening the primary economic classification rules.
5. **Variants and specifications:** Establish precise relationships between base products, variants, technical specifications, and product instances.
6. **Quality attributes:** Define shared conventions for grades, purity, condition, dimensions, and other relevant properties.
7. **Historical dates:** Determine how product introduction, retirement, and historical availability are represented and validated.
8. **Versioning:** Establish how semantic changes, technical revisions, and saved-game migrations are handled.
9. **Service modelling:** Define the shared representation of service quantities, service delivery, capacity, and consumption.
10. **Product-dependent conversions:** Determine how density, composition, temperature, and other properties influence unit conversions.
11. **Technical compatibility:** Establish how product specifications and compatibility relationships are represented without duplicating system-specific rules.
12. **Durable asset tracking:** Determine which products require individual instance identifiers and which can remain aggregate inventory.
13. **Starting Catalogue:** Define the initial goods and services required for the 1955 starting world.
14. **Reference data packaging:** Determine how Catalogue definitions are stored, loaded, validated, cached, and versioned within the simulation.
15. **Historical and regional standards:** Determine how differing technical standards, measurements, and product specifications across countries and periods will be represented.
16. **Economic category assignment:** Define the data-authoring rules used to assign Good entries to `Staple`, `Luxury`, or `Industrial`, including products whose use may vary by context.
17. **Designed Component boundary:** Establish the precise authoring criteria for deciding when a physical product requires the primary class `Designed Component` rather than `Good`.
18. **Armament boundary:** Establish the precise criteria for primary-class Armament, including the treatment of dual-use products and military equipment that is not itself a weapon or ammunition.
19. **Strategic Resource designation:** Define how additional Strategic Resources are introduced and how their designations are linked to Economy and Industry Data without duplicating product definitions.

Until these decisions are resolved, implementations must preserve the identity, classification, measurement, and ownership boundaries defined in this document.

---

## 23. Governing Principle

The Good and Service Catalogue is the common reference layer for the simulated economy.

It defines the products and services that systems can reference, the canonical units through which their quantities are expressed, and the shared attributes needed to interpret their identity and compatibility.

It does not create the economic world by itself. Industries, technologies, production methods, inventories, markets, infrastructure, and institutions determine how Catalogue entries become part of the operating simulation.

Industry Data establishes and describes industries. Production coordinates and executes production. Production Processes and Recipes define transformations and their quantitative requirements. Inventory records physical stock. Economy governs financial decisions and transactions. Logistics moves goods. Government and Military apply their own authoritative rules.

The Catalogue provides these systems with a consistent definition of what they are handling.

**Every product must have one canonical identity, one primary economic class, an explicitly defined physical form, and a measurement convention appropriate to its use. Every system must use that identity without independently redefining the product.**
