# Good and Service Catalogue

**Canonical Path:** `docs/reference/economy/good-and-service-catalogue.md`  
**System:** Economic Reference Data  
**Document Type:** Canonical Data Specification  
**Status:** Foundational Specification  
**Starting World Date:** 1955

---

## 1. Purpose

The Good and Service Catalogue defines the canonical goods and services recognised by Project Sovereign.

It establishes the shared identifiers, classifications, measurement units, descriptive attributes, and compatibility rules used by the simulation's economic, industrial, production, inventory, logistics, military, and population systems.

Every system that references a good or service must use the canonical catalogue definition rather than creating an independent definition of the same product.

The catalogue must support the full economic lifecycle of the simulated world, from raw material extraction and agricultural production through manufacturing, distribution, consumption, maintenance, military procurement, and service provision.

It must accommodate both the relatively simple material economy of the 1955 starting world and the increasingly sophisticated products, technologies, and services that become available as the simulation progresses.

### 1.1 Core Principle

**The catalogue defines what a good or service is. It does not define how it is produced, who owns it, where it is held, what it costs, or whether it is currently available.**

A good may be produced by multiple industries, traded by multiple companies, stored at multiple locations, and used by multiple systems while retaining one canonical identity.

Likewise, a service may be offered by multiple providers, consumed by different organisations, and priced differently across the world without requiring a separate canonical definition for each provider or transaction.

---

## 2. System Boundaries

The Good and Service Catalogue owns canonical product and service identity, classification, and shared descriptive definitions.

It does not own the operational or economic state associated with those definitions.

| System | Relationship to the Catalogue |
|---|---|
| Economy System | References goods and services in financial transactions, purchasing, sales, expenditure, revenue, and valuation |
| Industry Data | References goods and services when defining industrial requirements, establishment needs, and industry relationships |
| Production System | References goods and services when coordinating physical production |
| Production Processes and Recipes | References catalogue entries as recipe inputs and outputs |
| Inventory System | References physical goods when recording stock, reservations, consumption, and transfers |
| Logistics and Transportation | References goods when defining cargo, handling requirements, and transport operations |
| Research System | References goods and technical requirements where research unlocks new products or production methods |
| Military System | References equipment, ammunition, supplies, and other goods required by military organisations |
| Government System | References goods and services when defining procurement, regulation, restrictions, or public provision |
| Workforce and Population | References consumer goods and services relevant to household consumption and population needs |
| Terrain and Geography | May reference catalogue-defined infrastructure materials and equipment when interacting with construction or maintenance systems |

The catalogue is shared reference data. Its definitions must remain independent of the operational state maintained by these systems.

### 2.1 Ownership Rules

1. Every catalogue entry must have a stable, unique identifier.
2. Goods and services must be defined once and referenced consistently.
3. Product identity must not depend on the current owner, location, price, supplier, or inventory quantity.
4. The catalogue must not independently create physical stock.
5. The catalogue must not independently create financial value, revenue, expenditure, or market demand.
6. The catalogue must not independently establish production capabilities or recipe availability.
7. The catalogue must not independently grant technological, legal, or military permissions.
8. Changes to a catalogue definition must not silently rewrite historical transactions or executed production records.
9. Product variants and specifications must be represented explicitly when their differences affect simulation behaviour.
10. Systems must not infer unsupported physical, economic, or technical properties merely from a product's name or category.

---

## 3. Fundamental Concepts

### 3.1 Good

A Good is a canonical definition of a physical product or material that can exist as physical stock in the simulated world.

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

A Good definition identifies the product independently of its current quantity, owner, location, condition, or financial value.

### 3.2 Service

A Service is a canonical definition of a non-physical economic activity or deliverable.

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

Services may be purchased, sold, provided, consumed, or contracted. They do not automatically constitute physical inventory.

A service may consume physical goods, require labour, use equipment, or depend on infrastructure, but those requirements must be defined by the relevant production, service, or economic system.

### 3.3 Catalogue Entry

A Catalogue Entry is the canonical record for a Good or Service.

Each entry must have a stable identifier, a name, a classification, and the information required for other systems to reference it correctly.

The entry may include physical properties, measurement conventions, compatibility information, lifecycle status, and other shared attributes relevant to its category.

### 3.4 Product Family

A Product Family groups related catalogue entries.

For example, a family may group industrial fasteners, railway locomotives, petroleum products, or agricultural machinery.

A family supports classification, discovery, reporting, and shared defaults. It does not automatically make its members interchangeable.

### 3.5 Product Variant

A Product Variant represents a distinct version of a product whose differences matter to the simulation.

Examples may include:

- A truck with different load capacities.
- A locomotive with a different gauge compatibility.
- An aircraft with a different engine configuration.
- A machine tool with different precision capabilities.
- A garment in a distinct size or material specification, where those differences matter.
- A weapon system with a different ammunition compatibility.

A variant must be represented separately when its differences affect production, performance, handling, compatibility, quality, or another simulated outcome.

Not every descriptive difference requires a separate catalogue entry.

### 3.6 Product Specification

A Product Specification defines the required properties or performance characteristics of a good or service.

Specifications may be used to determine whether a product is suitable for a particular purpose.

Examples include material grade, purity, dimensions, electrical characteristics, load capacity, pressure rating, or military acceptance criteria.

A specification may be shared by multiple catalogue entries. Conversely, one catalogue entry may support multiple explicitly defined specifications if the underlying product identity remains valid.

### 3.7 Product Instance

A Product Instance is a specific physical object or individually tracked asset, where the simulation requires object-level tracking.

Examples include an individual aircraft, vehicle, machine, or ship.

A product instance references its canonical catalogue definition but has its own identity and state.

Not every good requires instance-level tracking. Bulk materials, consumables, and other interchangeable goods may be represented through aggregate quantities or stock lots.

The relevant inventory, equipment, or asset system determines the appropriate level of tracking.

---

## 4. Classification Framework

The catalogue must support a consistent classification structure without requiring every product to belong to only one conceptual category.

A good may be classified by its material, industrial role, economic use, or technical characteristics. These classifications must not be confused with the product's canonical identity.

### 4.1 Primary Entry Type

Every catalogue entry must be classified as one of the following primary types:

| Type | Meaning |
|---|---|
| `good` | A physical product or material |
| `service` | A non-physical economic activity or deliverable |

A physical asset remains a good even if it provides a service during operation.

For example, a truck is a good. Freight transportation is a service. The truck's physical existence and the transportation service it provides must not be conflated.

### 4.2 Good Categories

The following categories provide an initial classification framework.

| Category | Examples |
|---|---|
| `raw_material` | Iron ore, crude oil, timber, bauxite |
| `agricultural_product` | Wheat, cotton, sugar beet, livestock feed |
| `food_and_beverage` | Flour, preserved food, packaged food |
| `chemical` | Industrial chemicals, fertilisers, solvents |
| `fuel` | Coal, petrol, diesel, fuel oil |
| `energy_carrier` | Electricity, steam, stored electrical energy where modelled |
| `basic_material` | Steel, aluminium, cement, glass |
| `intermediate_good` | Steel plate, machine components, processed textiles |
| `component` | Bearings, motors, electrical components, fasteners |
| `industrial_equipment` | Machine tools, furnaces, industrial pumps |
| `infrastructure_equipment` | Railway signalling equipment, transformers, water pumps |
| `vehicle` | Trucks, buses, railway locomotives, aircraft |
| `consumer_good` | Clothing, furniture, household appliances |
| `medical_good` | Medical equipment, pharmaceuticals, clinical supplies |
| `military_equipment` | Weapons, military vehicles, aircraft, communications equipment |
| `ammunition_and_explosives` | Ammunition and other explicitly defined military or industrial products |
| `packaging` | Containers, bottles, cartons, industrial packaging |
| `recovered_material` | Recovered metal, reclaimed timber, recyclable materials |
| `waste_material` | Defined physical waste streams and residues |
| `other_good` | Physical goods not yet represented by a more specific category |

These categories are illustrative and may be refined as the catalogue expands.

The classification must not be treated as a substitute for specific technical attributes. Two goods in the same category may have entirely different uses, properties, and production requirements.

### 4.3 Service Categories

The initial service classification may include:

| Category | Examples |
|---|---|
| `transportation` | Freight, passenger transport, shipping |
| `warehousing` | Storage, handling, distribution services |
| `professional_services` | Legal, accounting, engineering, consulting |
| `healthcare` | Medical consultation, treatment, diagnostics |
| `maintenance_and_repair` | Equipment maintenance, repair services |
| `construction` | Construction and installation services |
| `telecommunications` | Telephone, data transmission, communications services |
| `utilities` | Water supply, electricity supply, gas supply where represented as a service |
| `financial_services` | Banking, insurance, credit services |
| `education_and_training` | Training, professional instruction |
| `hospitality_and_personal_services` | Accommodation, personal services |
| `government_services` | Public services represented as economic service outputs |
| `other_service` | Services not yet represented by a more specific category |

A category may be revised without changing the entry's stable identity, provided the change does not alter its underlying meaning or operational behaviour.

---

## 5. Canonical Identifiers

Every catalogue entry must have a stable identifier that other systems can store and reference.

Names are for human readability. Identifiers are for simulation integrity.

### 5.1 Identifier Requirements

A canonical identifier must:

- Be unique within the catalogue.
- Remain stable throughout the entry's lifetime.
- Be independent of display name and localisation.
- Be independent of price, supplier, ownership, and location.
- Avoid encoding values that may change, such as market price or production method.
- Remain usable in saved games, historical records, production recipes, and transaction logs.
- Never be silently reassigned to a different good or service.

### 5.2 Identifier Format

The recommended logical format is:

`GOOD_<DOMAIN>_<NAME>`

for physical goods, and:

`SERVICE_<DOMAIN>_<NAME>`

for services.

Examples:

- `GOOD_MINING_IRON_ORE`
- `GOOD_ENERGY_COAL`
- `GOOD_ENERGY_CRUDE_OIL`
- `GOOD_MATERIAL_STEEL`
- `GOOD_MATERIAL_CEMENT`
- `GOOD_AGRICULTURE_WHEAT`
- `GOOD_COMPONENT_BEARING`
- `GOOD_VEHICLE_FREIGHT_TRUCK`
- `SERVICE_TRANSPORT_FREIGHT`
- `SERVICE_HEALTHCARE_MEDICAL_CONSULTATION`
- `SERVICE_MAINTENANCE_INDUSTRIAL_EQUIPMENT`

These examples illustrate the naming convention. They do not constitute a complete or final catalogue.

The final implementation may use opaque internal identifiers, UUIDs, or another stable key format. If so, the readable identifiers above may be maintained as canonical codes.

### 5.3 Identifier Immutability

Once referenced by a committed transaction, executed production order, saved game, or other persistent simulation record, an identifier must not be reassigned.

If a definition is retired, its identifier remains reserved.

If a product is fundamentally redefined, the system must determine whether the change warrants a new entry rather than silently changing the meaning of the existing identifier.

### 5.4 Naming and Aliases

Each entry should support:

- A canonical name.
- An optional short name.
- An optional description.
- Optional aliases or alternative names.
- Optional historical names.
- Optional localisation strings.

Aliases must resolve to the canonical entry. They must not create duplicate product identities.

For example, alternative spellings or common names for the same material should normally resolve to the same catalogue entry where the underlying product definition is equivalent.

---

## 6. Core Catalogue Data Model

The following fields define the conceptual data contract. The implementation may use separate schemas for goods, services, specifications, and product variants.

### 6.1 Required Common Fields

Every catalogue entry must include:

| Field | Description |
|---|---|
| `catalogue_id` | Stable, unique identifier |
| `entry_type` | `good` or `service` |
| `canonical_name` | Primary human-readable name |
| `description` | Definition of the good or service |
| `category` | Primary classification |
| `status` | Current catalogue lifecycle state |
| `introduced_date` | Earliest simulation date on which the entry is valid, where applicable |
| `retired_date` | Optional date after which the entry is no longer available for new use |
| `unit_of_measure` | Canonical unit or defined unit convention |
| `parent_entry_id` | Optional reference to a parent family or base product |
| `aliases` | Optional alternative names |
| `tags` | Optional classification metadata |
| `metadata` | Additional supported descriptive information |

Fields that do not apply to a particular entry type may be omitted or explicitly marked as not applicable.

A missing field must not be interpreted as permission to invent a value.

### 6.2 Good-Specific Fields

A physical good may additionally define:

| Field | Description |
|---|---|
| `physical_form` | Solid, liquid, gas, particulate, discrete object, or other supported form |
| `material_composition` | Composition information where relevant and modelled |
| `mass_per_unit` | Mass per canonical unit, where applicable |
| `volume_per_unit` | Volume per canonical unit, where applicable |
| `density` | Density where supported and required |
| `quality_dimensions` | Supported quality attributes |
| `storage_requirements` | Relevant storage constraints |
| `handling_requirements` | Relevant handling constraints |
| `shelf_life` | Shelf-life definition where applicable |
| `hazard_classification` | Hazard classification where modelled |
| `durability_class` | Optional classification for durable goods |
| `instance_tracking_required` | Whether the good requires individual instance tracking under the applicable model |
| `default_condition` | Initial or default condition where explicitly defined |

These fields are conditional. For example, a bulk commodity may require mass and quality attributes but not individual instance tracking.

### 6.3 Service-Specific Fields

A service may additionally define:

| Field | Description |
|---|---|
| `service_unit` | Canonical unit in which the service is measured |
| `delivery_mode` | How the service is provided, where relevant |
| `duration_basis` | Time basis where service quantity depends on duration |
| `capacity_basis` | Relevant capacity unit where applicable |
| `quality_dimensions` | Service-quality attributes where modelled |
| `location_constraints` | Geographic or location requirements |
| `provider_capabilities` | Capability requirements for service provision |
| `consumption_semantics` | How service delivery and consumption are represented |

A service must not be assigned a physical stock quantity merely because it is measured in units.

For example, freight transportation may be measured in tonne-kilometres or another explicitly defined unit, but the service itself is not a physical inventory item.

### 6.4 Optional Attributes

The catalogue may include additional fields when they are shared, stable properties of the product definition.

Operationally changing values must remain outside the catalogue.

For example:

- Current market price belongs to the relevant economic pricing model.
- Current quantity belongs to Inventory.
- Current facility capacity belongs to Production.
- Current availability belongs to the relevant production, inventory, or market system.
- Current ownership belongs to the authoritative ownership and inventory records.
- Current technological eligibility belongs to the applicable research and production rules.

---

## 7. Units of Measurement

Every good or service that participates in quantitative simulation must have a defined measurement convention.

Quantities must not be interpreted from names, category membership, or informal assumptions.

### 7.1 Measurement Categories

The catalogue and shared measurement definitions must support the units required by the simulation.

| Measurement dimension | Example units | Typical applications |
|---|---|---|
| Mass | Kilogram, tonne | Ores, coal, steel, grain |
| Volume | Litre, cubic metre | Liquids, gases where defined |
| Count | Individual unit, item | Vehicles, components, packaged goods |
| Length | Metre, kilometre | Timber, cable, pipe |
| Area | Square metre, hectare | Land-related products or services where applicable |
| Time | Second, hour, day | Service delivery, operating duration |
| Energy | Joule, kilowatt-hour | Energy quantities |
| Power | Watt, kilowatt, megawatt | Power generation and rated output |
| Distance | Metre, kilometre | Transport and distance-based services |
| Area-time | Defined compound units | Land-based or capacity services where applicable |
| Mass-distance | Tonne-kilometre | Freight service measurement where adopted |
| Service unit | Explicitly defined unit | Consultations, inspections, treatments, or other deliverables |

The final unit registry must define the canonical units, conversion rules, and precision requirements.

### 7.2 Canonical Units

Each entry must specify a canonical unit or unit convention.

Examples:

- Bulk iron ore may be measured in tonnes.
- Crude oil may be measured in barrels or cubic metres, subject to an explicit canonical convention and conversion rules.
- Steel components may be measured by count or mass depending on the defined product.
- Electricity may be measured in kilowatt-hours for energy delivered, while generation capacity is measured in kilowatts or megawatts.
- A medical consultation may be measured per consultation or by duration, depending on the defined service.

The catalogue must not use multiple incompatible meanings for the same unit.

### 7.3 Conversion Rules

Conversions must use explicit, valid conversion definitions.

A unit conversion may be a fixed mathematical relationship or may require product-specific properties.

For example, converting between mass and volume may require a defined density. If the density varies by grade, temperature, composition, or condition, the conversion must use the applicable value.

Where a valid conversion cannot be established, the simulation must reject the conversion rather than guess.

### 7.4 Compound Units

Some products and services require compound units.

Examples include:

- Mass per hour.
- Units per day.
- Energy per unit of output.
- Tonne-kilometres.
- Passenger-kilometres.
- Cost per unit, where the economic system uses a defined denominator.

Compound units must have explicit dimensional meaning.

The catalogue must not conflate a rate with a total quantity. A power rating, for example, is not itself an amount of energy generated.

### 7.5 Quantity Precision

The catalogue must define the expected measurement dimension and canonical unit, while the shared simulation architecture defines numerical precision and rounding.

Discrete goods may require integer quantities. Bulk goods and continuous flows may permit fractional quantities.

The ability to represent a fractional quantity must not automatically imply that the product can be physically split into arbitrary fractions. Such restrictions must be defined by the good, recipe, or relevant operational system.

---

## 8. Product Identity, Families, Variants, and Specifications

The catalogue must represent product differences at the level necessary for simulation without creating unnecessary duplication.

### 8.1 When to Create a Separate Good

A separate catalogue entry should be created when a difference materially affects how the product is produced, stored, traded, transported, consumed, or used.

Examples include:

- Steel grades that have different industrial uses or processing requirements.
- Distinct fuels with materially different properties.
- Components that are not mechanically interchangeable.
- Ammunition types that require different weapons or have different operational effects.
- Vehicles with materially different capacities or compatibility.
- Goods with different handling or storage requirements.

A separate entry is not automatically required for a different brand, supplier, owner, location, or price.

### 8.2 When to Use a Variant

A variant is appropriate where products share a common base identity but have distinct configurations that affect simulation behaviour.

A variant may inherit shared descriptive properties from its parent entry while explicitly defining the attributes that differ.

The implementation must establish which attributes are inherited, which are overridden, and which are mandatory for every variant.

A variant must have a stable identifier if it is referenced independently by production recipes, inventory records, equipment records, or other persistent simulation state.

### 8.3 When to Use a Specification

A specification is appropriate when a requirement describes acceptable properties rather than a unique product identity.

For example, a process may accept steel conforming to a defined grade or dimensional standard.

Production may use the specification to validate candidate inputs. Inventory provides the actual attributes of the available stock.

A specification must not silently make otherwise incompatible goods interchangeable. Compatibility must be explicitly defined.

### 8.4 Interchangeability

The catalogue may support explicit compatibility relationships between products.

A compatibility relationship should identify:

- The relevant product or specification.
- The compatible alternative.
- The context in which compatibility applies.
- Any restrictions or conversion rules.
- Any technical prerequisites.

Compatibility is contextual. Two products may be interchangeable for one process but not another.

The catalogue should define stable compatibility information where appropriate. Production, Military, Logistics, or another consuming system must determine whether the compatibility is sufficient for a particular operation.

### 8.5 Brand and Manufacturer Identity

A branded product may require its own catalogue entry when brand-specific characteristics affect the simulation.

However, manufacturer identity must not automatically define a unique good.

Two manufacturers may produce the same standardised material, while one manufacturer may produce several distinct product variants.

Where brand identity matters, it should be represented through an explicit manufacturer, brand, model, or variant relationship rather than embedded inconsistently in the product name.

---

## 9. Quality, Condition, and Technical Properties

The catalogue must define which attributes are meaningful for a good or service.

It should not require every product to use the same attribute schema.

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

The catalogue defines the product's canonical properties and the dimensions along which its condition may vary.

Inventory records the condition of actual stock.

For example, the catalogue may define the nominal specifications of a machine component. Inventory records whether a particular lot of that component is damaged, inspected, or otherwise restricted.

The catalogue must not store a single current condition value as though it applies to every unit in existence.

### 9.3 Quality Constraints

A production recipe, military specification, or other authorised requirement may impose quality constraints on a catalogue entry.

The catalogue supplies the relevant attribute definitions and supported values.

The requesting system determines the required threshold and whether the product satisfies it.

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

These properties should be represented only where they affect the simulation or are needed by dependent systems.

The catalogue should not become an unrestricted repository for every imaginable attribute. Technical data must remain structured, typed, and relevant to defined system behaviour.

---

## 10. Lifecycle and Historical Availability

Goods and services may be introduced, modified, restricted, or retired as the simulation develops.

The catalogue must distinguish between the existence of a definition and its availability for use in a particular historical period.

### 10.1 Lifecycle States

Catalogue entries may use the following lifecycle states:

- `draft`: the definition is incomplete or under development.
- `validated`: the definition has passed structural validation but is not necessarily available for use.
- `active`: the definition is approved for use subject to its other constraints.
- `retired`: the definition is no longer available for new use under the applicable rules.
- `deprecated`: the definition should no longer be selected for new content, but may remain supported for compatibility or historical reasons.

The final implementation may use a different state model, provided the underlying distinctions are preserved.

### 10.2 Historical Introduction

A good or service may have an introduction date that determines when it becomes historically valid in the simulation.

A catalogue entry may exist in the reference data before its introduction date, but its presence in the catalogue does not make it available to the starting world.

For example, a product introduced after 1955 must not become available merely because its definition is loaded.

### 10.3 Technological Availability

Historical validity and technological availability are separate.

A product may have existed historically but remain unavailable to a particular country, company, or facility because the necessary technology, equipment, licensing, infrastructure, or supply chain is absent.

The catalogue defines the product. The relevant technology, production, government, and economic systems determine its availability in a specific context.

### 10.4 Retirement

Retiring an entry must not delete historical references.

Existing inventory, transaction records, executed recipes, and saved games must remain interpretable.

Where a product is replaced by a successor, the catalogue may record a successor relationship.

A successor relationship does not automatically convert existing stock or make the products interchangeable.

### 10.5 Starting-World Catalogue

The catalogue must support a comprehensive reference library while allowing the 1955 starting world to select only historically appropriate goods and services.

The starting-world configuration must determine which products are available through existing technologies, industries, imports, inventories, and other applicable simulation state.

The catalogue must not assume that every active entry exists in every country or market.

---

## 11. Production Integration

Production uses catalogue entries to identify the physical goods and services involved in industrial activity.

The catalogue does not independently define production methods.

### 11.1 Recipe Inputs

Production Processes and Recipes must reference goods and services through their canonical identifiers.

A recipe input may represent a raw material, intermediate good, component, consumable, fuel, utility, or other explicitly supported input.

The catalogue supplies the product identity and relevant shared properties. The recipe defines the quantity required and the conditions under which it is consumed or otherwise used.

### 11.2 Recipe Outputs

A recipe output must reference a canonical good or service where the result is represented by a catalogue entry.

Production determines the actual quantity generated during execution.

Inventory records physical outputs. Non-physical service outputs must be handled through the applicable service or economic execution contract.

### 11.3 Industry Compatibility

An industry may require or produce particular categories of goods.

Industry Data and Production define the applicable industrial relationships and execution constraints.

The catalogue provides the referenced product definitions. It must not independently determine which industries are permitted to manufacture a product.

### 11.4 Production Capability

A catalogue entry may describe technical properties that are relevant to production, but it does not establish that a facility possesses the necessary capabilities.

Production determines whether the facility, equipment, technology, workforce, and operating conditions satisfy the requirements.

### 11.5 Industry Scale

Catalogue quantities and units must not be confused with Industry Scale.

A good's canonical unit describes how its quantity is measured. Industry Scale describes the relevant industrial scale of an industry instance under Industry Data.

The catalogue must not define an industry's size, capacity, output target, or financial value.

---

## 12. Inventory Integration

Inventory uses catalogue definitions to identify and validate physical stock.

### 12.1 Physical Goods

Only entries classified as physical goods may be held as ordinary physical inventory.

A service may be represented through service-delivery records, contracts, or other relevant system state, but it must not automatically become a physical stock holding.

### 12.2 Stock Attributes

The catalogue defines the supported properties and quality dimensions that may apply to a good.

Inventory records the actual values or states associated with a stock lot.

A stock lot may contain only a subset of the catalogue's possible attributes where the remaining attributes are irrelevant or fixed by the product definition.

### 12.3 Storage and Handling

The catalogue may define storage and handling requirements for a good.

Inventory records the stock's location and relevant condition. The applicable location, facility, or logistics systems determine whether the storage and handling requirements are satisfied.

### 12.4 Unit Compatibility

Inventory must use compatible units when aggregating, reserving, consuming, or transferring stock.

The catalogue's canonical unit and shared conversion rules must be used where conversion is permitted.

Inventory must not assume that goods sharing a category can be combined into a single quantity.

### 12.5 No Stock Creation

Adding a catalogue entry does not create stock.

Making a good historically available does not create stock.

Creating a recipe that outputs a good does not create stock.

Physical stock must enter the inventory record through an authorised receipt, production output, extraction result, transfer, or other explicitly supported operation.

---

## 13. Economy and Market Integration

The catalogue identifies what is being traded. Economy determines the financial consequences and market behaviour associated with that activity.

### 13.1 Prices

The catalogue must not assign a universal current market price to each entry.

A good or service may have different prices depending on the market, location, period, quality, contractual terms, scarcity, taxes, and other economic conditions.

Any reference price or indicative valuation stored alongside catalogue data must be clearly distinguished from the authoritative price used in an economic transaction.

### 13.2 Currency

Catalogue entries must not define their own currency.

Monetary values belong to the Economy System and its financial records. The simulation's authoritative currency and conversion conventions must be applied consistently.

### 13.3 Demand

The existence of a good does not imply that demand exists for it.

Demand may arise from households, companies, government procurement, military requirements, production inputs, infrastructure maintenance, or other systems.

The catalogue may provide classifications that help those systems identify relevant products, but it must not independently create demand.

### 13.4 Substitutes

Two goods may be substitutes in a market without being technically interchangeable in production or use.

Market substitution belongs to the relevant economic decision rules.

Technical compatibility belongs to the applicable product specification and consuming system.

The catalogue must not equate economic substitutability with physical equivalence.

### 13.5 Product Value

The catalogue may support attributes used by valuation or economic models, but it does not independently determine market value, company revenue, profitability, or asset valuation.

Those outcomes depend on the relevant financial and economic systems.

---

## 14. Logistics and Transportation Integration

Goods must expose sufficient information for Logistics to determine their handling and transportation requirements where those requirements are simulated.

### 14.1 Cargo Properties

Relevant goods may define:

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

The catalogue may define transport restrictions or handling requirements.

Logistics determines whether a particular vehicle, ship, aircraft, railway, or other transport asset can carry the goods under the current conditions.

A product's existence in the catalogue does not imply that every transport mode can carry it.

### 14.3 Service Products

Transportation services may be defined as services even though the goods being transported remain physical inventory.

The freight service and the freight cargo must retain separate identities.

The service represents the economic activity of transportation. The cargo represents the physical goods being moved.

---

## 15. Military and Government Integration

The catalogue must support the specialised requirements of government and military systems without making every good a military-specific product.

### 15.1 Military Goods

Military equipment, ammunition, vehicles, communications equipment, medical supplies, and other military goods must reference canonical catalogue entries.

Where military compatibility or acceptance criteria affect use, the relevant military specification must be represented explicitly.

A generic product category is not sufficient to establish that a good is acceptable for military use.

### 15.2 Equipment and Consumables

The catalogue must distinguish between durable equipment and consumable goods where that distinction affects the simulation.

Examples include:

- A vehicle as a durable asset.
- Fuel as a consumable.
- Ammunition as a consumable.
- Spare parts as components.
- Maintenance services as non-physical services.

The relevant system determines whether a durable good requires individual instance tracking, whether it can be repaired, and how its condition changes.

### 15.3 Military Specifications

Military systems may define acceptance requirements, compatibility constraints, and operational classifications.

These requirements should reference catalogue entries and their relevant properties.

The catalogue must not independently establish military doctrine, equipment allocation, readiness, or operational effectiveness.

### 15.4 Government Procurement and Regulation

Government systems may reference goods and services when defining procurement programmes, regulated products, public services, or restrictions.

The catalogue identifies the referenced product. Government systems determine the legal or policy consequences.

---

## 16. Data Validation and Integrity

Catalogue definitions must be validated before they can be activated for use by authoritative simulation systems.

### 16.1 Structural Validation

A catalogue entry must not be activated if:

- Its canonical identifier is missing or duplicated.
- Its entry type is invalid.
- Its canonical name is missing.
- Its category is undefined or unsupported.
- Its required unit convention is missing.
- Its parent or successor reference is invalid.
- Its historical dates are contradictory.
- Its required attributes do not conform to the defined schema.
- Its physical and non-physical classifications are inconsistent.
- Its mandatory references cannot be resolved.

### 16.2 Unit Validation

Validation must confirm that:

- Required units are defined.
- Conversion rules are dimensionally compatible.
- Fixed conversions use valid definitions.
- Property-dependent conversions identify the required properties.
- Rates and total quantities are not treated as interchangeable.
- Unsupported conversions are rejected.

### 16.3 Attribute Validation

Product attributes must use defined data types and units.

A mass must not be stored as an untyped string where a numeric quantity is required. A categorical condition must not be represented as an arbitrary free-text value if other systems depend on a defined state.

The implementation should distinguish between mandatory attributes, optional attributes, and attributes that are not applicable.

### 16.4 Reference Integrity

All systems must reference canonical identifiers rather than copy catalogue definitions into their own independent product records.

Where another system needs a local representation for performance or historical purposes, it must preserve the reference to the authoritative catalogue entry and follow the relevant versioning rules.

### 16.5 Duplicate Detection

The catalogue should support detection of potential duplicate entries.

Duplicate detection may compare names, aliases, classifications, technical properties, and specifications.

Automated similarity must not merge entries without validation. Two products may have similar names while differing materially in physical properties or compatibility.

### 16.6 Validation Outcomes

Validation should return structured results identifying:

- Missing fields.
- Invalid references.
- Incompatible units.
- Contradictory attributes.
- Invalid lifecycle dates.
- Potential duplicate entries.
- Unsupported properties.
- Other relevant integrity failures.

Invalid definitions must not be silently repaired by guessing missing values.

---

## 17. Versioning and Change Management

The catalogue must preserve stable product identity while allowing definitions to evolve.

### 17.1 Definition Changes

A change to a descriptive field, such as correcting a spelling or improving a description, may not require a new semantic version.

A change that materially alters the product's identity, measurement meaning, technical compatibility, or simulation behaviour requires explicit review.

Where the change would alter the interpretation of historical records, the system must preserve the previous definition or create a new catalogue entry.

### 17.2 Historical References

Executed production records, inventory transactions, and historical economic records must remain interpretable after catalogue updates.

A saved game must not silently reinterpret a historical transaction because a catalogue description or technical definition changed.

Where a product definition requires versioned technical attributes, the relevant records must retain enough information to reconstruct the definition applicable at the time.

### 17.3 Retirement and Replacement

Retired entries must remain available for historical reference.

Where a product is replaced, the catalogue may define relationships such as:

- `replaced_by`
- `successor_of`
- `variant_of`
- `member_of_family`

These relationships must have explicit meanings.

A replacement relationship must not automatically convert stock, update production recipes, or declare technical equivalence.

### 17.4 Save Compatibility

Catalogue changes must follow the project's save compatibility and data migration conventions.

A saved game referencing an older catalogue definition must either continue to resolve that definition or undergo an explicit, validated migration.

Unknown identifiers must not be silently substituted with a vaguely similar product.

---

## 18. Determinism and Runtime Behaviour

The catalogue is primarily reference data. Its runtime role is to supply stable definitions and validated attributes to other systems.

### 18.1 Deterministic Lookup

A lookup by canonical identifier must resolve to one authoritative entry under the applicable catalogue version.

The result must not depend on display name, localisation, database ordering, or incidental execution timing.

### 18.2 No Implicit Simulation Effects

Creating or loading a catalogue entry must not automatically:

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

Those effects must be produced by the systems that own them.

### 18.3 Reference Data and Runtime State

The catalogue should separate stable product definitions from state that changes throughout the simulation.

For example, a vehicle's canonical definition may specify its type and nominal characteristics. An individual vehicle's location, owner, condition, fuel level, and operational status belong to the relevant runtime systems.

Likewise, a material's canonical definition may specify its measurement unit and quality dimensions, while a stock lot's actual quantity, condition, and location belong to Inventory.

### 18.4 Performance and Caching

Systems may cache catalogue definitions for performance, provided that cached data remains consistent with the authoritative definition and versioning rules.

A cache must not become an independent source of truth.

Catalogue updates and save migrations must invalidate or refresh affected cached definitions according to the shared architecture.

---

## 19. System Interfaces

The catalogue should expose a small set of stable logical interfaces.

These are conceptual contracts rather than mandatory programming-language function names.

### 19.1 Get Catalogue Entry

**Purpose:** Retrieve a canonical good or service definition.

**Inputs:**
- Catalogue identifier.

**Outputs:**
- Canonical definition.
- Entry type and category.
- Unit convention.
- Relevant shared attributes.
- Lifecycle and historical validity information.
- Version or definition metadata, where supported.

### 19.2 Search Catalogue

**Purpose:** Discover entries by category, family, name, alias, or other supported descriptive criteria.

**Inputs:**
- Optional search term.
- Optional category.
- Optional entry type.
- Optional family.
- Optional historical or lifecycle filters.

**Outputs:**
- Matching catalogue identifiers.
- Relevant descriptive metadata.

Search results must not replace canonical identifier references in persistent simulation records.

### 19.3 Validate Catalogue Reference

**Purpose:** Confirm that an identifier resolves to an appropriate catalogue entry.

**Inputs:**
- Catalogue identifier.
- Expected entry type, where required.
- Optional applicability context.

**Outputs:**
- Reference validity.
- Resolved entry type.
- Relevant lifecycle or applicability information.
- Structured failure information.

A valid catalogue reference does not automatically mean the product is available or suitable for a particular operation.

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

The relevant consuming system remains responsible for deciding whether the result satisfies its operational requirements.

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

Where runtime catalogue updates are supported, the system may publish events such as:

- `catalogue_entry_created`
- `catalogue_entry_validated`
- `catalogue_entry_activated`
- `catalogue_entry_updated`
- `catalogue_entry_retired`
- `catalogue_entry_replaced`

These are illustrative names and should follow the shared event naming and versioning conventions.

Catalogue events must not independently trigger physical production, inventory creation, purchases, or other effects that belong to separate systems.

---

## 20. Initial Catalogue Scope

The initial catalogue must be broad enough to support the starting world's core economic and industrial systems without attempting to model every product that could exist throughout the simulation.

### 20.1 Required Initial Coverage

The foundational catalogue should include the categories necessary to represent:

- Natural resources and extracted materials.
- Agricultural goods and food inputs.
- Fuels and energy-related goods.
- Basic industrial materials.
- Intermediate materials and components.
- Industrial equipment and tools.
- Transport equipment.
- Construction materials.
- Consumer goods where household consumption is modelled.
- Medical and public-service goods where relevant.
- Military equipment and supplies.
- Core commercial and industrial services.
- Freight, storage, maintenance, and other services required by the operational economy.
- Waste and recovered materials where these are tracked.

### 20.2 Starting-World Availability

The initial catalogue should distinguish between entries defined in the reference library and entries available to the world at the 1955 start date.

The starting-world configuration must determine:

- Which goods and services are historically valid.
- Which products are produced domestically.
- Which products may be imported.
- Which products are present in initial inventory.
- Which products require technologies or capabilities not yet available.
- Which products are unavailable or restricted in particular jurisdictions.

The catalogue supplies the definitions needed to express these conditions. It does not independently determine their initial distribution.

### 20.3 Incremental Expansion

New entries may be added as the simulation requires additional industrial sectors, consumer markets, military systems, technologies, or services.

Adding a new entry must not require changes to the core inventory, production, or economic data model if the entry conforms to the established catalogue contract.

Where a new product requires new simulation behaviour, the relevant system must define and implement that behaviour explicitly.

---

## 21. Acceptance Criteria

The Good and Service Catalogue is ready for foundational integration when the following conditions are satisfied.

### 21.1 Identity and Classification

- [ ] Every catalogue entry has a stable, unique identifier.
- [ ] Goods and services are distinguished explicitly.
- [ ] Entries have canonical names and defined categories.
- [ ] Product families and variants can be represented where needed.
- [ ] Aliases resolve to canonical entries without creating duplicates.
- [ ] Product identity is independent of owner, location, price, and stock quantity.

### 21.2 Units and Attributes

- [ ] Every quantitatively modelled entry has a defined unit convention.
- [ ] Units and conversions follow the shared measurement contract.
- [ ] Product-dependent conversions require explicit properties.
- [ ] Technical attributes use defined types and units.
- [ ] Quality dimensions can be represented where relevant.
- [ ] Goods can be distinguished from services in inventory and production interfaces.

### 21.3 Integration

- [ ] Production recipes can reference canonical goods and services.
- [ ] Inventory can reference physical goods without redefining their identity.
- [ ] Economy can reference goods and services in financial transactions.
- [ ] Logistics can access relevant cargo and handling properties.
- [ ] Military systems can reference goods and technical specifications.
- [ ] Historical availability can be evaluated independently of catalogue existence.

### 21.4 Integrity and History

- [ ] Invalid catalogue definitions are rejected.
- [ ] Duplicate identifiers are prohibited.
- [ ] Invalid references are detected.
- [ ] Retired entries remain available for historical reference.
- [ ] Material changes preserve historical interpretation.
- [ ] Catalogue updates do not independently create stock or financial transactions.
- [ ] Save-and-load operations preserve valid catalogue references or perform explicit migrations.

### 21.5 Determinism

- [ ] Lookup by canonical identifier resolves unambiguously.
- [ ] Unit conversion produces deterministic results under identical inputs and definitions.
- [ ] Compatibility checks use explicit rules.
- [ ] Missing attributes do not produce invented technical properties.
- [ ] The same product definition can be referenced consistently across systems.

---

## 22. Unresolved Design Decisions

The following decisions should be resolved through shared contracts and implementation testing rather than assumed by this specification.

1. **Canonical identifier implementation:** Determine whether readable codes, opaque identifiers, or a combination will be used.
2. **Measurement registry:** Finalise canonical units, compound units, conversion definitions, precision, and rounding.
3. **Catalogue schema:** Determine the concrete representation for common fields, good-specific fields, service-specific fields, and technical attributes.
4. **Classification hierarchy:** Finalise the category and product-family structure and determine whether entries may have multiple classifications.
5. **Variants and specifications:** Establish the precise relationships between base products, variants, technical specifications, and product instances.
6. **Quality attributes:** Define shared conventions for grades, purity, condition, dimensions, and other relevant product properties.
7. **Historical dates:** Determine how product introduction, retirement, and historical availability are represented and validated.
8. **Versioning:** Establish how semantic changes, technical revisions, and saved-game migrations are handled.
9. **Service modelling:** Define the shared representation of service quantities, service delivery, capacity, and consumption.
10. **Product-dependent conversions:** Determine how density, composition, temperature, and other properties influence unit conversions.
11. **Technical compatibility:** Establish how product specifications and compatibility relationships are represented without duplicating system-specific rules.
12. **Durable asset tracking:** Determine which goods require individual instance identifiers and which can remain aggregate inventory.
13. **Starting catalogue:** Define the initial goods and services required for the 1955 starting world.
14. **Reference data packaging:** Determine how catalogue definitions are stored, loaded, validated, cached, and versioned within the simulation.
15. **Historical and regional standards:** Determine how differing technical standards, measurements, and product specifications across countries and periods will be represented.

Until these decisions are resolved, implementations must preserve the identity, classification, and ownership boundaries defined in this document.

---

## 23. Governing Principle

The Good and Service Catalogue is the common reference layer for the simulated economy.

It defines the goods and services that systems can reference, the canonical units through which their quantities are expressed, and the shared attributes needed to interpret their identity and compatibility.

It does not create the economic world by itself. Industries, technologies, production methods, inventories, markets, infrastructure, and institutions determine how catalogue entries become part of the operating simulation.

Industry Data establishes industries. Production executes transformations. Production Processes and Recipes define those transformations. Inventory records physical stock. Economy governs financial decisions and transactions. Logistics moves goods. Government and Military apply their own authoritative rules. The catalogue gives these systems a common and consistent definition of what they are handling.

**Every good or service must have one canonical identity, and every system must use that identity without independently redefining the product.**
