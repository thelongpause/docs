# Government Procurement and Mobilisation Interface

**Project:** Project Sovereign  
**Canonical Path:** `technical/architecture/government-procurement-and-mobilisation-interface.md`  
**Document ID:** GOV-IF-001  
**Document Type:** Supporting Cross-System Interface Contract  
**Status:** Canonical interface contract; full Government System specification remains outside this document and is not yet present in the repository  
**Audit Date:** 26 October 2026

---

## 1. Purpose and Scope

This document defines the minimum Government-facing authority contract required for Military Logistics, procurement, mobilisation and strategic reserves. It does not attempt to author the full Government or Political System. It establishes which decisions Government must make, which financial state Economy owns, which physical stock state Inventory owns, and how Military and Logistics request government action.

The contract exists because military supply is intended to connect to companies and the wider economy. A government purchase must have a valid authority, budget/policy basis and commercial transaction; physical goods must still be produced or sourced, transported and received through the authoritative Production, Inventory and Movement/Transportation systems.

**Core principle: Government authorises and sets policy; Economy records financial/commercial state; Production executes production; Inventory records physical stock; Logistics coordinates military demand and delivery.** No one system may silently perform another system's transition.

## 2. Ownership

### 2.1 Government Owns

- Public policy and the authority to commit government expenditure.
- Treasury/budget state and budget allocation rules, subject to the future full Government specification.
- Procurement authority and delegation policy.
- Approval, rejection, deferral or conditional approval of government procurement requests.
- Mobilisation authority and applicable mobilisation policy.
- Policy for priority access to scarce goods, emergency procurement and strategic reserve release.
- Government-level restrictions and authorisations arising from legislation, regulation, sanctions, treaties or other public authority.
- Government policy decisions to prioritise military requirements against civilian, industrial or other public demand where the economy model supports the consequences.

### 2.2 Owned Elsewhere

- Military requirement, operational priority, acceptance criteria and resupply demand: Military Organisation and Logistics.
- Supplier identity, Industry Type and establishment configuration: Industry Data.
- Product identity, specification, compatibility and units: Good and Service Catalogue.
- Commercial supplier eligibility, order/contract, transaction price, payment obligation and settlement: Economy.
- Production recipes, facility capacity, production orders and execution: Production.
- Physical stock quantity, ownership/custody, reservations, transfers, receipts, consumption and losses: Inventory.
- Route feasibility, carrier movement, loading/handling and physical delivery process: Movement and Transportation, Infrastructure/Facilities and Logistics under their respective boundaries.
- Research progress and technology availability: Research.
- Actual military use and combat expenditure: Military, Equipment and Combat, with Inventory stock transactions.

Government authority does not itself create a company, product, production capability, inventory stock, transport capacity or military readiness.

## 3. Procurement Request Contract

A government procurement request must reference:
- Stable request ID and originating Military/Logistics demand ID.
- Requesting institution and authorised buyer.
- Canonical product ID and unit of measure for each line.
- Quantity, specification, permitted substitution and acceptance criteria.
- Required delivery location and delivery window.
- Purpose, priority, urgency and operational consequence of delay.
- Budget/Treasury reference and proposed spending limit where applicable.
- Legal/policy basis and required approval level.
- Whether the request is routine, emergency, mobilisation-related or strategic-reserve replenishment.
- Related production, supplier, contract or shipment references as they become available.

The request is an application for authority, not a purchase order, production order, stock reservation or receipt.

## 4. Authority and Decision Outcomes

Government must return an explicit decision:
- Approved.
- Approved with constraints.
- Deferred pending budget, policy, information or authority.
- Rejected.
- Cancelled or expired.
- Unresolved because a required interface or authority cannot confirm the result.

The result must identify the authorised quantity/value/scope, permitted buyer, valid time window, policy reference and any conditions. A request cannot be treated as approved merely because a budget exists or a procurement task was created.

Emergency authority may allow faster approval or delegated limits, but it must be explicit and auditable. Emergency status does not create product availability or guarantee delivery.

## 5. Commercial and Physical Separation

After approval:
1. Economy validates supplier, contract, price, payment and commercial transaction requirements.
2. Industry Data helps identify candidate suppliers but Industry Type alone does not prove product capability.
3. Production determines whether new goods can be manufactured under the relevant recipe, input, facility, labour, utility and capacity constraints.
4. Inventory confirms physical stock, reservations, receipts and movements.
5. Movement and Transportation executes feasible physical movement; Logistics coordinates shipment and delivery.
6. Economy settles financial obligations according to the commercial contract.
7. Military/Logistics validates delivery against the original demand and acceptance criteria.

These stages may be interleaved or simplified only where the transaction type permits it. They must remain separately auditable. A purchase order does not create stock; production completion does not prove delivery; a delivery does not automatically settle payment; payment does not prove that the recipient received the goods.

## 6. Budget, Allocation and Scarcity

Government policy may allocate spending authority among services, branches, programmes, mobilisation, strategic reserves and other priorities. Military identifies demand and operational consequences; Government resolves public authority and budget policy; Economy records resulting commercial commitments and payments.

A government may approve a procurement request that cannot be fulfilled immediately because suppliers lack stock, production is constrained, goods are unavailable, routes are blocked or capacity is insufficient. The procurement outcome must expose those limits rather than manufacture a successful delivery.

Where the economy model supports civilian competition, government demand may affect company sales, market prices, production queues, input availability and civilian access to goods. Such effects are resolved by Economy, Production, Inventory and Population systems, not by a separate Government or Logistics stock ledger.

## 7. Mobilisation and Strategic Reserves

Mobilisation authority may change permitted force-generation activity, procurement priorities, industrial policy, reserve release or delegated purchasing limits. Those policy changes do not instantly create units, equipment, personnel, goods or capacity. Military, Production, Inventory, Logistics, Movement and other systems resolve actual implementation.

A reserve target is a policy value, not physical stock. Inventory records actual reserve holdings. Government authorises release under the configured policy; Logistics coordinates distribution; Inventory commits any reservation/transfer/issue. The same stock cannot be counted as freely available to routine demand and protected for an unreleased contingency.

## 8. Company Eligibility and Industry Types

Industry Type is a candidate-discovery hint. It is not a guarantee that a company produces or stocks a specific product.

For example:
- A company with Industry Type `consumer_goods` may supply food or personal goods only if its actual product/stock/production records support those canonical Catalogue IDs.
- A company with Industry Type `oil` may supply compatible refined fuel only if the requested product is actually stocked or can be produced through a feasible refining/production chain. Crude petroleum is not interchangeable with refined fuel.
- A company with Industry Type `transportation` may provide a transport service, but that classification alone does not establish available carriers, route feasibility, capacity, price or delivery time.

Supplier selection and product compatibility are resolved by Economy, Industry Data, Catalogue, Production, Inventory and Movement/Transportation. Government may constrain or authorise procurement but must not override product facts.

## 9. Auditability and Idempotency

Every government decision must have a stable decision ID, originating request ID, authority/policy reference, effective time, outcome, approved scope, source actor/institution and version/provenance.

Retries must not duplicate budget authorisation or create multiple approvals for one request unless the policy explicitly permits a new version. A revised approval must be recorded as a new decision/version with a link to the superseded state.

The contract must integrate with the shared event envelope and common data-model conventions. Until the full Government schema is defined, interfaces must return explicit unknown/unresolved states rather than silently assume approval.

## 10. Minimum Acceptance Tests

1. A military procurement request without valid government authority is rejected or deferred.
2. An approved request does not itself create an Economy purchase, Production order or Inventory stock.
3. A valid government approval can be followed by a commercial transaction through Economy.
4. An Industry Type does not prove that a company can supply the requested product.
5. A request for refined fuel cannot be fulfilled by crude petroleum without a valid production chain.
6. A procurement may be authorised but unfulfilled due to supplier stock, production capacity or delivery constraints.
7. A purchase contract does not create physical stock before valid production/receipt transitions.
8. An emergency approval does not bypass Inventory quantity, product compatibility, transport capacity or legal restrictions unless an explicit policy changes the relevant authority constraint.
9. A reserve release requires government authority and changes stock availability only through Inventory's authorised transaction.
10. Mobilisation policy does not instantly create units, personnel, equipment or supplies.
11. Duplicate request submission or event replay does not duplicate budget authority or commercial commitment.
12. Player and AI government decisions follow the same policy and transaction rules.
13. Save/load preserves approval status and does not replay an approval as a new decision.
14. Civilian/industrial competition is reflected only through the owning economic and production systems where those mechanisms are implemented.
15. The company-to-unit sustainment vertical slice traces Government approval, Economy transaction, Production/Inventory availability, Movement/Logistics delivery and final unit receipt as separate states.

## 11. Open Decisions

- Full Government System canonical path and document ownership.
- Treasury/budget schema and spending-control rules.
- Procurement delegation levels by government, service, command, base and unit.
- Emergency procurement powers and expiry/review requirements.
- Mobilisation policy and its interaction with Industry, Production, Population and Military.
- Strategic reserve policy and protected-stock categories.
- Government procurement law, sanctions, export controls and treaty constraints.
- Exact event schemas, policy versioning and integration with Economy financial records.
- Treatment of procurement failure, supplier breach, contract cancellation and refunds.
- How civilian demand and market effects are represented in the initial economy model.

This interface contract is sufficient to define the Government decisions needed by the Military sustainment vertical slice. It is not a substitute for the full Government System specification, which should be authored as part of the Government top-level system after the Military integration dependencies are closed.
