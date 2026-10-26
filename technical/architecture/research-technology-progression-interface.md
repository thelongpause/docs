# Research and Technology Progression Interface

**Project:** Project Sovereign  
**Canonical Path:** `technical/architecture/research-technology-progression-interface.md`  
**Document ID:** RES-IF-001  
**Document Type:** Supporting Cross-System Interface Contract  
**Status:** Canonical interface contract; full Research System specification remains outside this document and is not yet present in the repository  
**Audit Date:** 26 October 2026

---

## 1. Purpose and Scope

This document defines the minimum Research/Technology Progression interface required by Military equipment design, testing, production, doctrine, communications, sensors, electronic warfare and other technology-dependent systems. It does not author the full Research System. It records the shared ownership boundaries and transitions needed to prevent research knowledge, design validity, production output and fielded capability from being treated as the same state.

**Core principle: Research establishes knowledge and validated unlocks; design establishes a particular candidate/configuration; testing establishes evidence; Production executes manufacture; Inventory records physical goods; Military and other domains determine fielding and operational use.** No one state implies all the others.

## 2. Ownership

### 2.1 Research/Technology Progression Owns

- Technology/knowledge domain identity and version.
- Research topics, prerequisites and progression state.
- Research effort, project/task state and completion evidence under the eventual Research specification.
- Technology availability and validated unlocks for a nation, institution or other authorised actor.
- Knowledge diffusion, transfer, licensing, publication, secrecy and other progression mechanisms where modelled.
- Research completion events and the scope/time from which an unlock is available.
- Historical and country-specific availability rules for technologies.
- Research uncertainty and confidence where the simulation models incomplete or disputed knowledge.

### 2.2 Owned Elsewhere

- Equipment design candidates, component compatibility, engineering integration, prototypes, design versions and design approval: Equipment Design/Research/Production specification.
- Test plans, test articles, trial execution and evidence records: the relevant design/test system, using actual test conditions.
- Production process, recipes, facility capability, capacity, scheduling and execution: Production and Production Processes/Recipes.
- Physical output, stock quantity, lots, reservations and receipt: Inventory.
- Procurement authority, budget and policy: Government; commercial and financial transactions: Economy.
- Unit establishment, equipment allocation, readiness and operational use: Military Organisation, Equipment and Command.
- Actual equipment performance, reliability, damage and serviceability: Equipment and Materiel and the relevant domain systems.
- Historical data, reference catalogue entries and numerical performance definitions: their designated catalogues and data schemas.

Research must not create a physical prototype, grant a company production capacity, create Inventory stock or field an equipment design automatically.

## 3. Technology Knowledge and Availability

A Technology Record should have a stable ID, definition, prerequisites, historical/technical context, dependencies, knowledge state and version. A Technology Unlock should identify the actor/entity for whom it is available, its effective time, authority/evidence and permitted uses.

The system must distinguish:
- Technology exists in the world.
- A nation or institution knows the relevant principles.
- A specific organisation has access to that knowledge.
- A design process may use the technology.
- A candidate design successfully integrates the technology.
- A prototype has been built.
- A prototype/test has validated the relevant performance.
- A production process and facility can manufacture the item.
- Physical units exist in Inventory.
- Military or another domain has accepted and fielded those units.
- A particular unit has serviceable, crewed and operationally available equipment.

These are separate states. Calendar date alone must not grant technology or upgrade existing formations. A scenario may explicitly start with an existing capability or historical exception, but the initialisation must identify that state.

## 4. Equipment Design and Research Interface

The equipment design system may declare research prerequisites for components, subsystems, materials, manufacturing processes, test methods or complete designs. Research returns whether the required knowledge is available to the relevant nation/institution and from what effective time.

Research completion does not automatically approve a design. The design system must still resolve:
- Candidate architecture and component compatibility.
- Engineering integration and design constraints.
- Prototype construction and material availability.
- Test plan, test conditions and evidence.
- Failure, redesign and re-test where required.
- Design version, qualification and approval status.

A design may remain infeasible even when all nominal research prerequisites are met. Conversely, a nation may understand a technology but lack the industrial capability, materials, tooling, capital, workforce, test infrastructure or production capacity needed to use it at scale.

## 5. Production, Procurement and Fielding Interface

After a design is approved, Production determines whether the relevant process, recipe, facility, inputs, labour, utilities, tooling and capacity can execute manufacture. Inventory records the resulting physical output through authorised stock transactions.

Research does not create production orders or bypass economic constraints. Government/Economy determine applicable procurement authority and commercial commitments. Military decides requirements and acceptance/fielding priorities under its own authority. Inventory records the physical stock and custody.

Technology may be licensed, transferred, reverse-engineered, captured or diffused only through explicit rules and evidence. Captured equipment does not automatically grant complete design knowledge or production capability. A country may possess a physical item without knowing how to reproduce it.

## 6. Historical Availability and Technology Diffusion

Technology progression must be data-driven and historically bounded. A technology may be:
- Not yet discovered.
- Under research.
- Known but not available to the actor.
- Available through licensed or transferred knowledge.
- Available only to a particular institution/company.
- Known but not industrially reproducible.
- Integrated into a candidate design but not validated.
- Validated but not producible at scale.
- Produced but not yet accepted or fielded.

The exact taxonomy should be formalised in the Research schema. These states are conceptual distinctions and must not be collapsed into one boolean `unlocked` field if doing so would erase required dependencies.

Research effort may produce partial progress, failed hypotheses, improved understanding or a completed unlock according to the eventual Research rules. A research project must not automatically finish solely because a calendar date is reached unless an explicit scenario event grants the result.

## 7. Shared Event and Query Contract

The minimum interface must support:
- Query technology identity, version, prerequisites and current availability.
- Query actor-specific research/knowledge state at a simulation time.
- Submit a research requirement or project request under the relevant authority and resource constraints.
- Return progression status, completion evidence, uncertainty and effective unlock time.
- Notify dependent systems when a prerequisite becomes available or is withdrawn/revised under an explicit rule.
- Record a technology transfer/licence/diffusion event with source, recipient, scope, conditions and effective time.
- Preserve stable technology/project/event IDs and idempotent completion handling.
- Reconcile saved research state with designs, production orders and fielded equipment without retroactively rewriting historical outcomes.

Every response must distinguish known unavailable, not yet researched, not authorised, unsupported and unknown states. Missing technology data must not be treated as an automatic unlock.

## 8. AI and Player Parity

Player and AI nations use the same technology prerequisites, research constraints, diffusion rules, design validation and production requirements. AI may differ in research priorities, institutional efficiency, planning, portfolio selection and willingness to accept risk only through explicit rules.

AI must not gain technology because the Player has researched it, unless the technology-diffusion rules create a valid transfer. AI must not inspect hidden research projects or design details of another nation without an authorised information path.

## 9. Minimum Acceptance Tests

1. A technology does not become available to a nation solely because the simulation date advances.
2. A technology known by one actor is not automatically known by another.
3. Research completion does not automatically approve an equipment design.
4. A design approval does not create a prototype or physical inventory item.
5. Prototype success requires an actual configured test/evidence result.
6. A validated design does not create production capacity or guarantee production output.
7. Production requires valid inputs, process, facility, capacity and execution; Inventory records physical output.
8. Physical capture of equipment does not automatically grant complete reverse-engineering knowledge.
9. Licensed technology transfer respects its scope, restrictions and effective time.
10. Research and technology changes are deterministic and idempotent under save/load and event replay.
11. Player and AI obey the same prerequisites, resource constraints and diffusion rules.
12. Existing fielded equipment retains its historical design/configuration record when a later technology is discovered.
13. A dependent system receives an explicit event when a prerequisite changes, rather than silently assuming the unlock.
14. Unknown technology data does not default to available.
15. A research unlock can coexist with a lack of materials, tooling, skilled labour, test capacity or production capability.

## 10. Open Decisions

- Full Research System canonical path and document ID.
- Research project/effort model, resources, staffing, institutions and progress rules.
- Technology taxonomy, dependency graph, versions and historical availability data.
- Knowledge diffusion, licensing, publication, secrecy, export control and reverse engineering.
- How research uncertainty, failed projects and competing hypotheses are represented.
- Relationship between general knowledge, design-specific know-how, process know-how and tacit industrial skill.
- Exact interfaces to Government, Economy, Population/education, Production, Equipment Design and Military.
- Technology availability for AI nations and the information boundaries for foreign research.
- Shared event schema, persistence, idempotency and migration policy.

This contract is sufficient to identify the Research/Technology boundary needed by Military and Equipment Design. It is not a substitute for the full Research System specification, which should be authored as part of the Research top-level system after the Military integration dependencies are closed.
