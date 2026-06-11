# Architecture Decision Register (ADR) Guide

## Purpose

Every significant architecture decision in a VTEX implementation must be documented in the ADR. The ADR is not a log of what was configured — it is a record of **decisions that had real alternatives** and where the rationale matters for future changes.

A decision belongs in the ADR if:
- There were two or more viable options considered
- The wrong choice would be costly or time-consuming to reverse
- The rationale depends on client-specific context that won't be obvious in 12 months
- A new SA or developer would need to understand why this approach was chosen

---

## ADR Table Format

Include in Section 11 of the SDD as a table with the following columns:

| Column | Description |
|---|---|
| ID | Sequential: ADR-001, ADR-002, etc. |
| Domain | Module area (Catalog, OMS, Checkout, Marketplace, etc.) |
| Decision | One sentence: "We will use X instead of Y" |
| Rationale | 2–5 sentences: why this option, given client context |
| Alternatives Considered | List of other options evaluated |
| Trade-offs | What we give up with this decision |
| Owner | SE who made the decision (validated with SA) |
| Status | Proposed / Accepted / Superseded |
| Date | When decided |

---

## ADR Examples by Domain

### Catalog

**ADR-001 — Catalog Ownership: VTEX as Catalog Master**
- Decision: VTEX Catalog will be the product master of record. The ERP will receive catalog data from VTEX, not the reverse.
- Rationale: Client has no PIM system. The merchandising team will manage products directly in VTEX Admin. ERP integration is finance-only (orders and invoicing).
- Alternatives: External PIM (rejected — no PIM license, not in project scope), ERP as master (rejected — ERP has no product enrichment capability).
- Trade-offs: Catalog governance depends on VTEX Admin discipline. No centralized PIM means managing attributes manually.

**ADR-002 — Multi-Account Architecture: Franchise App**
- Decision: Use the VTEX Franchise App (Edition pattern) with one master account and N franchise accounts per brand.
- Rationale: Client operates 3 retail brands with shared inventory but separate storefronts and order management. Franchise App allows catalog inheritance and independent order flows.
- Alternatives: Single account with multiple trade policies (rejected — insufficient storefront isolation), fully independent accounts (rejected — no shared catalog or inventory).
- Trade-offs: Franchise App adds operational complexity; master account configuration changes propagate to all franchises.

---

### Pricing

**ADR-003 — External Pricing Connector**
- Decision: Implement a VTEX IO external price connector to fetch real-time prices from the SAP ERP pricing engine.
- Rationale: Client's pricing is managed in SAP with complex contract pricing, customer-specific discounts, and promotional eligibility rules that cannot be replicated in VTEX price tables.
- Alternatives: Sync prices from SAP to VTEX price tables (rejected — SAP pricing changes > 10,000 times/day, sync latency unacceptable), manage pricing in VTEX (rejected — business requires SAP as the system of record for financial compliance).
- Trade-offs: Real-time price fetch adds ~200ms latency to checkout. Requires VTEX IO app maintenance. SAP downtime affects pricing availability.

---

### OMS

**ADR-004 — Invoice Injection via ERP**
- Decision: VTEX OMS will not generate invoices. SAP S/4HANA will generate fiscal documents and inject them back into VTEX via the `POST /api/oms/pvt/orders/{orderId}/invoice` API.
- Rationale: Fiscal compliance requires SAP-generated invoices with digital signatures. VTEX does not have invoice generation capability.
- Alternatives: Custom VTEX IO fiscal middleware (rejected — adds complexity and maintenance burden when SAP already owns this), third-party fiscal middleware (considered — deferred if SAP integration is direct).
- Trade-offs: Order status will remain "payment approved" until SAP injects the invoice. Define SLA for injection (target: < 5 minutes after payment capture).

**ADR-005 — Order Events: Order Feed v3 over Order Hooks**
- Decision: Use Order Feed v3 (polling) for ERP and WMS integration instead of Order Hooks (push).
- Rationale: ERP and WMS are batch-oriented systems with defined polling cycles. Order Feed provides at-least-once delivery with consumer-managed commit, which is required for guaranteed processing without message loss.
- Alternatives: Order Hooks (rejected — client's ERP cannot expose a stable HTTP endpoint; push model adds complexity for their team).
- Trade-offs: Polling introduces latency (up to 30s depending on poll interval). Requires consumer to manage commit.

---

### Marketplace

**ADR-006 — Seller Architecture: External Seller Protocol**
- Decision: Large sellers (Tier 1, >10K SKUs, own OMS) will integrate via External Seller Protocol. Small sellers (<1K SKUs, no OMS) will use VTEX Seller Portal.
- Rationale: Tier 1 sellers already operate their own ERP and OMS. Forcing them into VTEX Seller Portal would duplicate their workflow. External Seller Protocol allows them to maintain their existing operational systems.
- Alternatives: VTEX Seller Portal for all sellers (rejected — Tier 1 sellers won't accept it; creates dual order management burden), External Seller Protocol for all (rejected — SMB sellers don't have technical capacity to build the integration).
- Trade-offs: Two integration patterns increase implementation complexity. External Seller Protocol requires seller-side development effort and certification.

**ADR-007 — Commission Architecture: External System**
- Decision: Commission rates will be stored in VTEX (per seller / per category), but commission calculation, reporting, and payout will be managed by the external finance platform.
- Rationale: VTEX does not natively calculate or disburse commissions. Commission calculation involves tax treatment and payout logic that requires the client's financial system.
- Alternatives: Commission calculation via VTEX IO custom app (rejected — no native VTEX financial ledger; creates audit and compliance risk), Manual calculation in Excel (rejected — not scalable).
- Trade-offs: Dependency on external finance platform synchronization. VTEX commission data must be kept in sync with finance platform rates.

---

### Checkout

**ADR-008 — Anti-Fraud: Native Protocol vs. Custom**
- Decision: Use VTEX Anti-Fraud Provider Protocol with ClearSale as the provider.
- Rationale: ClearSale has a native VTEX connector. Integration via the protocol is fully managed by VTEX and ClearSale — no custom code required.
- Alternatives: Custom fraud logic in checkout (rejected — not maintainable), External fraud API call via VTEX IO (rejected — protocol approach is simpler and certified).
- Trade-offs: Bound to ClearSale's response time (SLA: < 3s for synchronous evaluation).

---

### Storefront

**ADR-009 — Storefront: FastStore over VTEX IO Store Framework**
- Decision: Implement the storefront using FastStore (Next.js).
- Rationale: Client has Core Web Vitals targets of LCP < 2.5s and INP < 200ms on mobile. FastStore with ISR and VTEX CDN delivers significantly better performance than the VTEX IO Store Framework on these metrics.
- Alternatives: VTEX IO Store Framework (rejected — cannot meet Core Web Vitals targets on client's catalog size and page complexity).
- Trade-offs: FastStore requires React/Next.js development skills. Some native VTEX IO blocks are not available; custom sections required. Longer time-to-market than Store Framework.

---

## ADR Status Lifecycle

- **Proposed**: SE has documented the decision; not yet validated by SA or client
- **Accepted**: Validated by SA and approved (implicitly or explicitly) by client technical team
- **Superseded**: Replaced by a newer ADR (reference the new ADR ID)
- **Deprecated**: Decision is no longer applicable (document why)

---

## Common ADR Mistakes to Avoid

1. **Logging configurations as decisions** — "We configured 3 price tables" is not an ADR. "We chose external pricing over native price tables because..." is an ADR.
2. **Missing alternatives** — Every ADR must document what was considered and rejected.
3. **No trade-offs** — Every architectural choice has costs. Document them honestly.
4. **ADR written after implementation** — ADRs should be written during design, not as post-hoc documentation.
5. **Vague rationale** — "This is best practice" is not a rationale. Tie the decision to client-specific constraints.
