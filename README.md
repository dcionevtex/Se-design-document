# VTEX Solution Design Document Framework

A comprehensive toolkit for VTEX Solution Engineers and Solution Architects to create, structure, and validate Solution Design Documents (SDDs) — the primary SE-to-SA handoff artifact for VTEX implementations.

## Overview

The Solution Design Document is the bridge between commercial discovery and technical delivery in VTEX projects. A well-written SDD:

- Defines what VTEX will do natively vs. what requires custom development or third-party integration
- Documents architecture decisions with explicit rationale
- Surfaces assumptions, risks, and open questions before the project starts
- Enables an SA unfamiliar with the discovery process to own the implementation independently

**Quality bar:** An SA should be able to start an implementation from this document alone, without re-doing discovery.

## Project Structure

### Core Skill
- **SKILL.md** — Complete skill documentation including the 5-step workflow for creating an SDD and the canonical SDD structure (11 sections)

### Reference Guides (in `/references/`)

1. **adr-guide.md** — Architecture Decision Register (ADR) format and examples
   - How to document decisions with real alternatives
   - ADR table format with ID, domain, decision, rationale, trade-offs, and status
   - Real examples from catalog, pricing, OMS, and marketplace domains

2. **discovery-questions.md** — Critical discovery questions by module
   - Business context questions (business model, channels, regions, go-live date, pain points, GMV targets)
   - Module-specific questions for Catalog, Pricing, OMS, Checkout, Marketplace, Search, Storefront, Master Data, Logistics, and B2B

3. **formatting-guide.md** — Document formatting and style rules
   - Applies VTEX brand guidelines to SDD outputs
   - Ensures consistency across all solution design artifacts

4. **module-checklist.md** — Checklist of all VTEX modules in scope
   - Catalog & Product Information
   - Pricing & Promotions
   - Order Management (OMS)
   - Checkout & Payments
   - Marketplace & Seller Management
   - Search & Navigation
   - Storefront (FastStore / IO Storefront)
   - Master Data & Customer Data
   - Logistics & Fulfillment
   - B2B Configuration

5. **module-design-rules.md** — Domain-specific design rules for each VTEX module
   - Rules for native VTEX functionality vs. custom extensions
   - Capability accuracy and architectural constraints per module

6. **section-authoring-guide.md** — Content rules for each of the 11 SDD sections
   - What to include, what to avoid, expected depth per section
   - Inline flagging patterns for assumptions, open items, and risks

## How to Use This Toolkit

### Step 1: Gather Context
Collect the client name, business model, modules in scope, discovery inputs, and known constraints.

### Step 2: Run Discovery Gap Analysis
For each module, verify you have answers to the critical questions in `discovery-questions.md`. Flag missing items with `[ASSUMPTION]`, `[OPEN]`, or `[RISK]` inline in the document.

### Step 3: Draft the Document
Follow the canonical 11-section structure defined in SKILL.md and detailed in `section-authoring-guide.md`.

### Step 4: Validate Architecture Decisions
Apply the VTEX Decision Framework:
- Is this native VTEX?
- Does it require a VTEX IO app?
- Does it require a third-party integration?
- Does it require custom development?

Document each significant decision in the ADR (Section 11) using the format in `adr-guide.md`.

### Step 5: Output and Review
Produce a `.docx` file following the formatting rules in `formatting-guide.md` and VTEX brand guidelines.

## Canonical SDD Structure

Every SDD includes these 11 sections:

1. **Document Control** — Version, status, authors, approval dates
2. **Executive Summary** — Business context and solution overview (1–2 pages)
3. **Engagement Context** — Business model, channels, regions, pain points, go-live timeline
4. **Scope Definition** — What's included, what's out of scope, phasing
5. **Architecture Overview** — High-level VTEX topology and integration landscape
6. **Module Design** — Detailed design for each module in scope (one subsection per module):
   - 6.1 Catalog & Product Information
   - 6.2 Pricing & Promotions
   - 6.3 Order Management (OMS)
   - 6.4 Checkout & Payments
   - 6.5 Marketplace & Seller Management
   - 6.6 Search & Navigation
   - 6.7 Storefront (FastStore / IO Storefront)
   - 6.8 Master Data & Customer Data
   - 6.9 Logistics & Fulfillment
   - 6.10 B2B Configuration (if applicable)
7. **Integration Architecture** — ERP, OMS, PIM, payment, fulfillment, search integrations
8. **Data Migration Strategy** — Catalog, pricing, customer, and order data migration plans
9. **Identity & Access Management** — VTEX IAM, B2B roles, API security
10. **Non-Functional Requirements** — Performance, scalability, availability, security, SLA targets
11. **Architecture Decision Register** — Decisions with alternatives, rationale, trade-offs, and status

## Key Concepts

### Native vs. Extended
Always be explicit about whether functionality is:
- **Native VTEX** — Built-in VTEX capability, no custom code
- **VTEX IO App** — App from VTEX App Store or custom-built IO app
- **Third-Party Integration** — External system (ERP, PIM, payment processor, search engine)
- **Custom Development** — Code outside VTEX ecosystem
- **Partial Native + Extension** — Native VTEX + custom extension or integration

### Discovery Flags
Flag inline in the document:
- `[ASSUMPTION: <your assumption>]` — You've made an assumption that must be validated
- `[OPEN: <question>]` — Critical question that must be answered before implementation
- `[RISK: <risk description>]` — Risk that must be addressed during project kick-off

### Architecture Decisions
Every decision with alternatives goes in the ADR (Section 11). Include:
- What you decided and why (given client context)
- What alternatives you considered and why you rejected them
- What you give up with this decision

## Reference Materials

For comprehensive guidance on:
- **VTEX modules and architecture**, see [VTEX Developers](https://developers.vtex.com)
- **Solution design methodology**, see SKILL.md and the reference guides
- **VTEX brand guidelines**, refer to your organization's brand standards

## Contributing

When improving this toolkit:
1. Update the relevant reference guide with new insights or examples
2. Keep the 5-step workflow in SKILL.md as the canonical process
3. Maintain consistency across all guides
4. Add real examples (anonymized client names) to the ADR and module design rules guides

## License

Internal use only — VTEX Solution Engineering and Solution Architecture teams.

---

**Version:** 1.0  
**Last Updated:** June 2026  
**Maintained By:** VTEX Solution Design Practice
