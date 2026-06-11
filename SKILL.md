---
name: vtex-solution-design-document
description: >
  Use this skill to create, draft, structure, or improve a VTEX Solution Design Document (SDD) — the primary SE/SA deliverable guiding VTEX implementations. Covers catalog, pricing, promotions, OMS, marketplace, checkout, search, integrations, and technical architecture.

  Trigger on: "create a solution design document", "write an SDD", "draft the solution design for [client]", "build the implementation blueprint", "document the VTEX architecture", "I need the SDD for catalog/pricing/OMS/checkout/search", "document the integration architecture", "create the technical spec for VTEX", or when the user provides discovery notes, RFP, or requirements and asks to produce a solution design. Also trigger when reviewing or improving an existing SDD.

  Produces a structured .docx with discovery gap analysis, section-by-section authoring rules, assumption flagging, ADR guidance, and capability accuracy rules.
---

# VTEX Solution Design Document Skill

## Purpose

The Solution Design Document (SDD) is the **primary SE-to-SA handoff artifact** in a VTEX implementation. It bridges commercial discovery and technical delivery. A well-written SDD:

- Defines what VTEX will do natively vs. what requires custom development or third-party integration
- Documents architecture decisions with explicit rationale
- Surfaces assumptions, risks, and open questions before the project starts
- Enables an SA who was not in the discovery process to own the implementation

**Quality bar:** An SA should be able to start an implementation from this document alone, without re-doing discovery.

---

## Skill Workflow

Follow these steps in order:

### Step 1: Gather Context

Before writing, collect:
1. **Client name, industry, and business model** (B2C, B2B, D2C, Marketplace, Omnichannel)
2. **VTEX modules in scope** — use the module checklist in `references/module-checklist.md`
3. **Discovery inputs available** — notes, RFP, call transcript, architecture diagram, existing proposal
4. **Known constraints** — go-live date, ERP/OMS/PIM in use, existing tech stack, team size, regions

If inputs are incomplete, perform a **Discovery Gap Analysis** (see Step 2) before drafting.

### Step 2: Discovery Gap Analysis

Run this check before writing. For each module in scope, verify you have answers to the critical questions in `references/discovery-questions.md`. 

Flag anything missing as:
- `[ASSUMPTION: <state what you're assuming>]`
- `[OPEN: <question that must be answered before implementation>`]
- `[RISK: <risk if this is not resolved>]`

These flags go inline in the document. Do not hide them. The SA must resolve them during project kick-off.

### Step 3: Draft the Document

Use the canonical structure below. Every section is required unless explicitly marked OPTIONAL.

For the `.docx` output: follow the docx skill (`/mnt/skills/public/docx/SKILL.md`) and VTEX brand guidelines (`/mnt/skills/user/vtex-brand-guidelines/SKILL.md`).

### Step 4: Architecture Decision Validation

For every architecture decision, apply the **VTEX Decision Framework**:
- Is this native VTEX? → Document it as NATIVE
- Does it require a VTEX IO app? → Document the app, owner, and deployment model
- Does it require a third-party integration? → Document the protocol, owner, and data flow
- Does it require custom development outside VTEX? → Flag and justify

Never mark something as NATIVE if it requires custom code or third-party tooling. Use PARTIAL NATIVE + EXTENSION instead.

### Step 5: Output

Produce a `.docx` file to `/mnt/user-data/outputs/[ClientName]-SDD-v[version].docx`.

For formatting guidance specific to VTEX documents, read `references/formatting-guide.md`.

---

## Canonical SDD Structure

Every SDD follows this structure. Read `references/section-authoring-guide.md` for full content rules per section.

```
1. Document Control
2. Executive Summary
3. Engagement Context
4. Scope Definition
5. Architecture Overview
6. Module Design — one sub-section per module in scope:
   6.1 Catalog & Product Information
   6.2 Pricing & Promotions
   6.3 Order Management (OMS)
   6.4 Checkout & Payments
   6.5 Marketplace & Seller Management
   6.6 Search & Navigation
   6.7 Storefront (FastStore / IO Storefront)
   6.8 Master Data & Customer Data
   6.9 Logistics & Fulfillment
   6.10 B2B Configuration (if applicable)
7. Integration Architecture
8. Data Migration Strategy
9. Identity & Access Management
10. Non-Functional Requirements
11. Architecture Decision Register (ADR)
12. Assumptions Register
13. Risks & Open Issues
14. Implementation Phases & Milestones
15. Appendix
```

---

## Section Authoring Rules (Quick Reference)

Read `references/section-authoring-guide.md` for full guidance. Key rules:

**Section 1 — Document Control**
- Version, date, author (SE), reviewer (SA), status (Draft / Under Review / Approved)
- Change log table

**Section 2 — Executive Summary**
- Max 1 page. Business context, VTEX scope, key decisions, go-live target
- No technical jargon. Written for a VP or CTO to read in 3 minutes

**Section 3 — Engagement Context**
- Client profile, business model, channels, current platform, pain points
- Why VTEX was selected (commercial summary, not a sales pitch)

**Section 4 — Scope Definition**
- IN SCOPE: modules, channels, countries, integrations
- OUT OF SCOPE: explicitly listed — this protects both sides
- PHASED: what is deferred to a later phase

**Section 5 — Architecture Overview**
- Single architecture diagram (embed or reference)
- 3–5 paragraphs describing the architecture narrative
- Key design principles applied (e.g., event-driven integrations, native-first, OMS as orchestration layer)

**Sections 6.x — Module Design**
- See module authoring rules in `references/module-design-rules.md`
- Each module section must contain:
  - Capability Statement (what VTEX does natively for this module)
  - Configuration Requirements (what must be configured)
  - Custom / Extension Requirements (VTEX IO apps, third-party, or custom)
  - Integration Points (APIs, events, webhooks)
  - Data Ownership Map
  - Open Questions / Assumptions for this module

**Section 7 — Integration Architecture**
- Integration inventory table: System | Direction | Protocol | Owner | Sync/Async | Frequency
- Event-driven vs. polling decisions justified
- Middleware strategy (if any)

**Section 8 — Data Migration**
- Source systems and data types
- Migration approach: full load vs. delta vs. real-time sync
- Data quality dependencies
- VTEX catalog/pricing API limitations relevant to bulk migration

**Section 11 — Architecture Decision Register (ADR)**
- Minimum one ADR per significant decision
- Format: ID | Decision | Rationale | Alternatives Considered | Owner | Status
- Read `references/adr-guide.md` for ADR authoring rules

**Section 12 — Assumptions Register**
- All `[ASSUMPTION]` flags from the document consolidated here
- Format: ID | Module | Assumption | Impact if Wrong | Validation Owner

**Section 13 — Risks & Open Issues**
- All `[OPEN]` and `[RISK]` flags consolidated here
- Format: ID | Module | Description | Probability | Impact | Mitigation | Owner

---

## VTEX Capability Accuracy Rules

These are non-negotiable. Violating them creates delivery debt.

| Rule | Description |
|---|---|
| NATIVE | Only use for out-of-the-box VTEX capabilities, no code required |
| PARTIAL NATIVE | Native capability exists but requires configuration, a VTEX IO app, or a partner app from the App Store |
| CUSTOM | Requires new VTEX IO app development, not available in VTEX App Store |
| INTEGRATION | Requires a third-party system (ERP, PIM, POS, etc.) to own or provide the capability |
| NOT SUPPORTED | VTEX cannot provide this capability natively or via extension |
| ROADMAP | Capability is on VTEX roadmap — do not commit to delivery date |

**Always apply the most conservative accurate label.** Never upgrade a PARTIAL NATIVE to NATIVE to simplify the document.

**Known areas where overclaiming is common — always validate:**
- Invoice generation → VTEX does NOT generate invoices. ERP/OMS owns this.
- Commission calculation → VTEX does not calculate seller commissions natively.
- KYC / seller onboarding validation → Requires external tooling.
- GST / tax compliance (India, Brazil) → Requires Tax Protocol Provider (e.g., ClearTax, Avalara, AvaTax). VTEX passes order data; third party calculates and persists.
- Payout and escrow → Outside VTEX.
- Advanced B2B credit management → Requires B2B Suite + possible custom extension.
- Real-time inventory sync at high frequency → Subject to API rate limits; document the sync strategy.

---

## Module-Specific Quick Guides

For fast reference during authoring. Full detail in `references/module-design-rules.md`.

### Catalog
- Hierarchy: Account > Trade Policy > Category Tree > Product > SKU > Specification
- Single vs. multi-catalog strategy decision (Franchise App, multi-account)
- Catalog ownership: VTEX Admin vs. PIM (Akeneo, Stibo, etc.)
- SKU change propagation: VTEX native vs. middleware trigger
- Specification groups: global vs. category-level

### Pricing
- Price tables: list price, computed price, rule-based price
- Trade Policy binding: which price table per channel/trade policy
- Promotions vs. Pricing: promotions engine is separate from price tables
- External price via VTEX IO app (for ERP-driven pricing): document the latency and fallback
- Price inheritance in franchise/multi-account setups

### Promotions
- Types: percentage, fixed, free shipping, gift, buy together, buy X get Y, progressive discount
- Stacking rules: single vs. multiple promotions per cart
- Promotion ownership: VTEX Admin vs. external promotion engine
- Coupon management: native VTEX vs. external
- Campaign audiences: native cluster/segment vs. VTEX Segments (new) vs. external CDP

### OMS
- Order flow: placement → payment → invoicing → fulfillment → delivery → return
- VTEX OMS as orchestration layer vs. external OMS (SAP, Oracle, etc.)
- Invoice injection: always external — document the invoicing system and API call
- Status change hooks: Order Feed v3 vs. Order Hooks — document which and why
- Return / RMA: native VTEX Returns App vs. external
- SLA configuration: carriers, docks, inventory management strategy

### Checkout
- SmartCheckout: native profile reuse, 1-click purchase
- Custom checkout: VTEX IO Checkout UI Customization app
- Checkout anti-fraud: Anti-Fraud Provider Protocol (native, document the provider)
- Payment: Payment Provider Protocol (document the connector: native vs. custom)
- Gift cards / loyalty: native VTEX Gift Card Provider Protocol vs. external
- Split payment / multi-payment: native for 2 methods; document if more needed

### Marketplace
- Seller architecture: VTEX Seller Portal (managed) vs. External Seller Protocol (direct API)
- Catalog flow: seller SKU → marketplace catalog matching (Matcher)
- Order split: automatic vs. manual chain
- Commission architecture: VTEX does not calculate commissions — document where commissions live
- Broadcaster / Notification: seller stock and price updates to marketplace

### Search
- VTEX Intelligent Search (IS): native, facets, synonyms, relevance rules, banners
- IS + AI features: semantic search, autocomplete, spell correction
- Custom search extensions: VTEX IO Search Result block overrides
- Facets and filters: configuration strategy (category-level vs. global)
- Search customization: `searchState`, `useSearchPage`, `useFacets`

### FastStore / Storefront
- FastStore: Next.js, ISR, API Extensions, Section Overrides
- VTEX IO Storefront: VTEX Store Framework, blocks, VTEX IO React apps
- Decision: FastStore (performance-critical, custom design) vs. IO Storefront (faster go-live, more native blocks)
- CMS: VTEX Headless CMS (native for FastStore) vs. VTEX Site Editor (IO Storefront)
- Analytics: GTM integration, data layer strategy

---

## Common Integration Patterns

Document these explicitly in Section 7:

| Integration | Pattern | Notes |
|---|---|---|
| ERP (SAP, Oracle) | Bidirectional, async, event-driven | Order out, invoice in, stock in |
| PIM (Akeneo, Stibo) | Unidirectional, PIM → VTEX Catalog API | Batch or event-triggered |
| WMS / Logistics | Bidirectional | Order out to WMS, fulfillment status in |
| Payment Gateway | VTEX Payment Provider Protocol | Custom connector if not in App Store |
| Tax Engine | VTEX Tax Provider Protocol | ClearTax, Avalara, AvaTax |
| Anti-Fraud | VTEX Anti-Fraud Protocol | Clearsale, Konduto, etc. |
| CRM / CDP | Outbound from VTEX via Feed/Hooks | Order events to CRM |
| SSO / IdP | VTEX OAuth 2.0 delegation | External IdP as identity provider |
| Search (external) | Replace IS or augment via API Extension | Algolia, Bloomreach, Elasticsearch |

---

## Output Document Requirements

The final `.docx` must:
- Follow VTEX brand guidelines (see `/mnt/skills/user/vtex-brand-guidelines/SKILL.md`)
- Include a cover page with: client name, document title, version, date, classification (Confidential)
- Include a table of contents (auto-generated)
- Use a consistent heading hierarchy (H1 = Section, H2 = Sub-section, H3 = Component)
- Include all tables in proper docx table format (not plain text)
- Embed or clearly reference all architecture diagrams
- Include page numbers and document version in the footer
- Be saved as: `[ClientName]-SDD-v[X.X]-[YYYYMMDD].docx`

For document generation code, follow `/mnt/skills/public/docx/SKILL.md`.

---

## Reference Files

| File | When to Read |
|---|---|
| `references/module-design-rules.md` | Full per-module authoring rules and content requirements |
| `references/discovery-questions.md` | Discovery gap analysis — required questions per module |
| `references/adr-guide.md` | Architecture Decision Register format and examples |
| `references/formatting-guide.md` | Document layout, styles, table formats, VTEX brand application |
| `references/module-checklist.md` | Scope checklist — which modules are in/out/deferred |
| `references/integration-patterns.md` | Detailed integration pattern library with data flow diagrams |
| `references/competitive-positioning.md` | How to frame VTEX decisions vs. alternatives in the SDD |
