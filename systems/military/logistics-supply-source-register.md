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

## 4. What this first pass establishes

- There are identifiable period-adjacent sources for ammunition-supply doctrine, ration practice, Quartermaster reference data and supply-unit organisation.
- The source families are nationally and temporally scoped. They cannot be collapsed into a universal set of 1955 logistics values.
- A 1955 baseline requires a country/service/equipment applicability key for every sourced record. This is especially important for formation establishments, ammunition expenditure, vehicle payload, depot configuration and fuel compatibility.
- The current findings are source-discovery results, not completed data extraction. No physical conversion or capacity values have been approved by this register.

## 5. Data extraction queue

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

## 6. Related specifications

- [Logistics and Sustainment](logistics-and-sustainment.md)
- [Logistics Supply Reference Data](logistics-supply-reference-data.md)
- [Military Interface Audit and End-to-End Vertical Slice](../../technical/architecture/military-interface-audit-and-vertical-slice.md)
- [Canonical Specification Register](../../technical/architecture/canonical-specification-register.md)
