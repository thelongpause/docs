# Logistics Supply Source Register

**Project:** Project Sovereign  
**Canonical Path:** systems/military/logistics-supply-source-register.md  
**Document ID:** MIL-LOG-SRC-001  
**Document Type:** Supporting research-source register  
**Status:** Initial source discovery complete; extraction and value approval remain in progress  
**Historical Baseline:** 1 January 1955  
**Technology Horizon:** At least 2026, extensible beyond

## 1. Purpose

This register tracks candidate primary and secondary sources for the physical supply-conversion and logistics-capacity data required by `MIL-LOG-DATA-001`. It distinguishes a source that is useful for locating evidence from a source whose contents have actually been extracted, checked and approved for simulation use.

**No numeric value becomes canonical merely because a source has been discovered or cited here.** Values must be transcribed from the source, checked against the relevant page/table and units, assigned a national/service/equipment/time scope, and reviewed before use.

## 2. Research rules

1. Treat 1 January 1955 as the initial simulation date, not as permission to use a single nation's military model for every nation.
2. Maintain source provenance at the individual data-record level. Record issuing authority, document title/identifier, publication date, page/table/paragraph, country/service, applicable formation/equipment, units, interpretation, confidence and approval state.
3. Prefer contemporary official doctrine, establishment tables, technical manuals, vehicle manuals, ration scales and ammunition tables for the exact country/service and period.
4. Use later manuals for methodological context or technical values only when their applicability to the earlier period and equipment is independently justified.
5. Distinguish authorised scale, planning factor, rated maximum, nominal capacity, physical capacity and operationally usable capacity. They are not interchangeable.
6. Do not transfer values between nations, services, vehicle variants, ammunition natures, fuels, climates or operational contexts without an explicit compatibility/applicability rule.
7. Where a primary document is inaccessible or only indexed by a library/archive, record it as a discovery lead. Do not infer its tables from its title.
8. Record conflicts and uncertainty rather than silently averaging or selecting values.
9. Modern values must not overwrite historical values; use versioned applicability periods and equipment identities.
10. Scenario estimates remain clearly labelled estimates and require explicit approval. No unsupported numeric defaults are permitted.

## 3. Initial source discovery register

### SRC-001 — NATO archive, ammunition supply in war

- **Title:** *AMMUNITION SUPPLY IN WAR*
- **Identifier:** LOSTAN 0952
- **Issuing/archive context:** London Branch Standing Group Secretariat, NATO Archives Online
- **Date:** 21 October 1954
- **Public record:** https://archives.nato.int/ammunition-supply-in-war
- **Potential use:** Contemporary evidence for the existence and framing of allied wartime ammunition-supply standardisation shortly before the simulation start.
- **Limitations:** The catalogue record describes a one-page item. Its contents have not yet been extracted. Do not treat this record as a source for numerical daily ammunition scales unless the document itself explicitly contains them.
- **Status:** Discovered; content extraction required.

### SRC-002 — British operational rations research, 1954

- **Title:** *Army Operational Rations*
- **Author/context:** H. G. Smith, War Office
- **Publication:** Proceedings of the Nutrition Society; published online by Cambridge University Press
- **Public record:** https://doi.org/10.1079/PNS19540014
- **Potential use:** Period-adjacent discussion of British normal and operational ration provision and substitutions under difficult supply conditions.
- **Limitations:** The paper is secondary/contextual evidence, not a substitute for the applicable official ration scale or a complete food/water product bill. The full paper and any quantitative tables must be examined before extracting data.
- **Status:** Discovered; full-text extraction and primary-source corroboration required.

### SRC-003 — US Army Quartermaster reference data, 1950

- **Title:** *FM 10-13, Quartermaster Reference Data*
- **Issuing authority:** US Department of the Army / War Department publication series
- **Date:** 1950
- **Public index:** https://generalstab.org/library/field-manuals/us-army-field-manuals-1939-1999/?hsb_dir=10_quartermaster
- **Potential use:** Candidate for period-specific US Quartermaster tables, unit conversions, supply/transport planning data or material characteristics, subject to inspection of the manual.
- **Limitations:** US-specific and published before the 1955 start. The index entry alone does not establish which data are present or their applicability to any particular formation/equipment.
- **Status:** Discovered; manual extraction required.

### SRC-004 — US Army Quartermaster reference data, 1957

- **Title:** *FM 10-13, Quartermaster Reference Data*
- **Issuing authority:** US Department of the Army
- **Date:** 1957
- **Public index:** https://generalstab.org/library/field-manuals/us-army-field-manuals-1939-1999/?hsb_dir=10_quartermaster
- **Potential use:** Candidate source for US Quartermaster reference tables close to the initial simulation date.
- **Limitations:** Published after the start date. A value is usable for 1955 only if it describes equipment/practice already applicable then, and that applicability is verified. The index entry is not itself evidence for a numerical value.
- **Status:** Discovered; manual extraction and historical applicability review required.

### SRC-005 — US Army Quartermaster organisation and operations in divisions, 1951

- **Title:** *FM 10-7, Quartermaster Organization and Operations in Divisions*
- **Issuing authority:** US Department of the Army
- **Date:** 1951
- **Public index:** https://generalstab.org/library/field-manuals/us-army-field-manuals-1939-1999/?hsb_dir=10_quartermaster
- **Potential use:** Candidate for organisational structure and supply responsibilities, including how division-level supply functions were organised.
- **Limitations:** It should not be treated as a source for a generic cross-national depot or transport capacity. Extract any quantitative establishment or throughput only from an identified table and with its stated conditions.
- **Status:** Discovered; manual extraction required.

### SRC-006 — US Army Quartermaster service company, 1956

- **Title:** *FM 10-6, Quartermaster Service Company*
- **Issuing authority:** US Department of the Army
- **Date:** 1956
- **Public index:** https://generalstab.org/library/field-manuals/us-army-field-manuals-1939-1999/?hsb_dir=10_quartermaster
- **Potential use:** Candidate for the staffing, equipment and organisation of a service-company supply element.
- **Limitations:** Post-start and US-specific. Any capacity must be derived from explicit source content and checked for 1955 applicability; do not infer capacity from unit title or headcount alone.
- **Status:** Discovered; manual extraction and historical applicability review required.

### SRC-007 — US Army Quartermaster organisation and service in Army and Corps, 1952

- **Title:** *FM 10-17, Quartermaster Organization and Service in Army and Corps*
- **Issuing authority:** US Department of the Army
- **Date:** 1952
- **Public index:** https://generalstab.org/library/field-manuals/us-army-field-manuals-1939-1999/?hsb_dir=10_quartermaster
- **Potential use:** Candidate for operational-level supply organisation, distribution echelons and responsibilities.
- **Limitations:** US-specific. Organisational descriptions do not automatically provide numeric throughput, stock capacity or universal supply rates.
- **Status:** Discovered; manual extraction required.

### SRC-008 — Library of Congress guide to US Army Field Manuals, Quartermaster series

- **Title:** *Series 10: Quartermaster — United States Army Field Manuals: A Resource Guide and Inventory*
- **Publisher:** Library of Congress
- **Public record:** https://guides.loc.gov/us-army-field-manuals/series-1-20-series-10
- **Potential use:** Archival discovery and bibliographic cross-checking for Quartermaster manuals and dates.
- **Limitations:** Finding aid, not the technical source of a supply value. It helps locate the relevant manual but does not validate its contents.
- **Status:** Discovered; use as a finding aid.

### SRC-009 — NATO logistics history and national responsibility

- **Title:** *NATO's role in logistics*
- **Publisher:** NATO
- **Updated:** 22 January 2025
- **Public record:** https://www.nato.int/en/what-we-do/deterrence-and-defence/natos-role-in-logistics
- **Potential use:** Modern institutional history for the principle that Cold War logistics remained primarily a national responsibility, and for the distinct concepts of supply, movement, maintenance and services.
- **Limitations:** Modern overview, not a source for 1955 numeric capacity or consumption values. Use only for architectural context and cross-check against contemporary historical sources.
- **Status:** Reviewed for contextual framing; no numeric values extracted.

### SRC-010 — US Army historical supply table, 1941

- **Title:** *Staff Officers' Field Manual* (June 1941 edition)
- **Public record:** https://www.generalstaff.org/BBOW/LOV-DUP/FM101-10_JUN-1941.pdf
- **Potential use:** Historical comparative example showing how prescribed loads, rations, fuel, water and ammunition could be expressed in physical quantities and related to vehicle/trailer cargo capacity.
- **Limitations:** This is a 1941 US source and is not a valid 1955 baseline by itself. Any figures must be treated as historical comparison only unless an independent source establishes continued applicability. The document's own unit, formation, purpose and assumptions must be preserved.
- **Status:** Discovered; not approved for initial simulation values.

### SRC-011 — British Army ration scale, Canal Zone, 1955

- **Title:** *British Army, Canal Zone (Ration Scale)*
- **Publisher:** UK Parliamentary Hansard, House of Commons written answer
- **Date:** 1 April 1955
- **Public record:** https://api.parliament.uk/historic-hansard/written-answers/1955/apr/01/british-army-canal-zone-ration-scale-1
- **Potential use:** Contemporary evidence that the overseas-garrison ration comprised a basic scale plus a supplement for strenuous work, and that exceptional local supplements could be added or withdrawn. This supports context-dependent ration profiles rather than a universal food quantity.
- **Limitations:** The page excerpt does not provide a complete item-by-item physical scale or potable-water requirement. It is not sufficient by itself to populate physical food conversion records.
- **Status:** Discovered; full parliamentary context and underlying official ration tables required.

### SRC-012 — British Army instructional film on depot materials handling, 1956

- **Title:** *An Introduction to Materials Handling*
- **Publisher/collection:** Imperial War Museums; Army Kinema Corporation production
- **Production date:** 1956
- **Public record:** https://film.iwmcollections.org.uk/record/4499/media_id/13234
- **Potential use:** Evidence of period British Army depot handling methods and equipment, including manual/mechanical handling, forklifts, pallet trucks, tractors, cranes, conveyors and loading/unloading operations.
- **Limitations:** A film description does not supply measured tonnes-per-hour throughput. It can inform equipment and process identification but cannot establish a numeric handling rate without measured or doctrinal data.
- **Status:** Discovered; film inspection and separate quantitative source required.

### SRC-013 — National Army Museum Bedford QL truck collection record

- **Title:** *Bedford 3 ton QL truck, 1942 (c)*
- **Publisher:** National Army Museum, London
- **Public record:** https://collection.nam.ac.uk/detail.php?acc=1989-07-69-1
- **Potential use:** Museum record for identifying a British military transport vehicle family and distinguishing body/variant-specific roles. The record notes variant-specific fuel tank arrangements for the QLT troop-carrier variant.
- **Limitations:** The record is not a complete technical manual and does not establish payload limits for every QL variant, fuel consumption, or a generic Supply Column capacity. The vehicle's nominal class label must not substitute for verified variant specifications.
- **Status:** Discovered; variant identification and primary technical manual required.

## 4. Extracted evidence records

The following findings have been transcribed from accessible source text. They remain **research evidence**, not approved universal conversion factors. Their source scope and limitations must travel with any derived record.

### EXT-001 — British Army operational ration profile, 1954 publication

- **Source:** H. G. Smith, “Army Operational Rations,” *Proceedings of the Nutrition Society*, vol. 13, symposium proceedings 1954, pp. 46–48 in the available PDF pagination.
- **Source URL:** https://doi.org/10.1079/PNS19540014
- **PDF:** https://www.cambridge.org/core/services/aop-cambridge-core/content/view/37D08975AC25F213A644172EAF9985F2/S0029665154000158a.pdf/army_operational_rations.pdf
- **Author context:** The paper identifies Smith as a War Office catering officer and states that publication was permitted by the Director of Supplies and Transport, War Office, and the Government Chemist.
- **Extracted facts:**
  - The paper identifies operational ration types including ten-person composite, five-person composite, 24-hour General Service and 24-hour snow rations, plus a separate emergency ration.
  - The ten-person composite ration is described as food for ten people for one day, or five people for two days.
  - The five-person composite ration is described as one day's food for five people and as designed for tank crews.
  - The paper reports an average energy value of 4,100 Calories per person per day for the ten-person and five-person composite ration profiles.
  - The 24-hour GS ration is described as one person's food for one day in circumstances where each person must carry their own food; the reported calorific value is 3,000 Calories.
  - The paper describes a distinct cold-weather 24-hour snow ration with a reported energy value of 4,250 Calories, demonstrating that ration profiles vary by operating context.
  - The ten-person composite pack is described as a case containing 30 cans or smaller containers, with biscuits provided separately; the paper gives a case weight of 42 lb. The packaging/weight wording and whether this is gross shipping weight versus another measure must be checked against the original page image before use as a payload record.
- **Interpretation:** This is useful evidence that ration demand must distinguish person-days, ration type, operational environment, pack composition and packaging. It is not a complete ration bill of materials, potable-water allowance, or general-purpose supply-column load factor.
- **Historical applicability:** Published in 1954, but its exact validity as the authorised ration scale on 1 January 1955 must be confirmed against the relevant official ration instructions. The paper's reported calorific values are not physical food mass and must not be converted into kilograms of food without a separate item-level composition source.
- **Confidence:** Source text located and extracted; exact physical pack-weight interpretation requires image-level verification.
- **Status:** Extracted, not corroborated or approved.

### EXT-002 — British garrison ration structure, Canal Zone, 1 April 1955

- **Source:** UK House of Commons, Hansard written answer, “British Army, Canal Zone (Ration Scale),” HC Deb 1 April 1955, vol. 539, c. 64W.
- **URL:** https://api.parliament.uk/historic-hansard/written-answers/1955/apr/01/british-army-canal-zone-ration-scale-1
- **Extracted facts:** The answer states that British troops in the Canal Zone received the ration scale applicable to overseas garrisons generally, comprising a basic ration and a supplement for strenuous work. A special supplement had included meat, bacon, rolled oats, potatoes and dried peas or beans, and was discontinued after the exceptional conditions that prompted it had changed.
- **Interpretation:** Ration requirement profiles may be conditional on duty intensity and operational circumstances, and special supplements should be modelled as explicit policy/profile changes rather than an unexplained permanent multiplier.
- **Limitations:** This parliamentary answer does not provide the complete basic ration scale, item quantities, packaging or potable-water requirement.
- **Status:** Extracted; useful for context-dependent profile structure, not sufficient to populate physical quantities.

### EXT-003 — British 24-hour ration pack report, 1956

- **Source:** UK House of Commons, Hansard, “Business of the House,” 15 March 1956.
- **URL:** https://hansard.parliament.uk/commons/1956-03-15/debates/3dc46532-cadf-463f-a5bf-a578c2a4e009/BusinessOfTheHouse
- **Extracted fact:** A speaker described a new type of 24-hour ration pack as weighing 27 oz and listed several of its components.
- **Interpretation:** This is a post-start-date reference for a later ration-pack design, not a 1955 baseline value. It may help establish a later dated equipment/product variant after its context and exact statement are checked.
- **Limitations:** The parliamentary speech is not, by itself, an official technical specification; the weight and composition must be corroborated with a formal specification before operational use.
- **Status:** Extracted as a later historical lead; not applicable to the initial 1955 baseline without further evidence.

## 5. What this first pass establishes

- There are identifiable period-adjacent sources for ammunition-supply doctrine, ration practice, Quartermaster reference data and supply-unit organisation.
- The source families are nationally and temporally scoped. They cannot be collapsed into a universal set of 1955 logistics values.
- A 1955 baseline requires a country/service/equipment applicability key for every sourced record. This is especially important for formation establishments, ammunition expenditure, vehicle payload, depot configuration and fuel compatibility.
- The current findings are source-discovery results, not completed data extraction. No physical conversion or capacity values have been approved by this register.

## 6. Data extraction queue

Extract and review in this order:

1. **Formation/personnel baseline:** Identify official establishment tables for one explicitly scoped 1955 formation, with authorised personnel, equipment and transport establishment.
2. **Basics:** Identify official ration scales and potable-water requirements with units, environmental/operational conditions, packaging and substitutions.
3. **Fuel:** Identify exact vehicle/equipment variants, fuel types, tank capacities and consumption basis. Separate tank capacity from daily consumption and from depot bulk storage.
4. **Ammunition:** Identify weapon-specific authorised load and consumption/allowance tables, with ammunition nature, unit of issue, packaging and operational conditions. Do not infer daily combat expenditure from basic load.
5. **Supply Column:** Identify actual vehicle types and their payload/volume constraints, then derive column capacity from the verified composition rather than assigning a generic unit-size factor.
6. **Supply Base handling:** Locate establishment/equipment tables and measured or doctrinal throughput for a specific depot configuration. A personnel count or nominal facility label alone is insufficient.
7. **Supply Base storage:** Locate explicit storage capacities and distinguish usable storage, physical capacity, bulk fuel tankage and handling throughput.
8. **Validation:** Cross-check source dates, units, historical applicability, duplicate values and contradictions; submit candidate records for approval before marking them canonical.

### Required status labels

- `discovered`: bibliographic lead found.
- `extracted`: exact data transcribed with page/table and units.
- `corroborated`: independent or primary-source cross-check completed.
- `reviewed`: applicability and interpretation checked.
- `approved`: permitted for canonical simulation use.
- `rejected` / `superseded`: retained for audit but not used.

## 7. Related specifications

- [Logistics and Sustainment](logistics-and-sustainment.md)
- [Logistics Supply Reference Data](logistics-supply-reference-data.md)
- [Military Interface Audit and End-to-End Vertical Slice](../../technical/architecture/military-interface-audit-and-vertical-slice.md)
- [Canonical Specification Register](../../technical/architecture/canonical-specification-register.md)
