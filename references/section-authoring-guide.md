# Section Authoring Guide

Detailed content rules for each section of the SDD. Read when authoring or reviewing a specific section.

---

## Section 1 — Document Control

**Purpose:** Enable version tracking and document lifecycle management.

**Required elements:**
- Title block: client name, document title, version, date, author, reviewer, status
- Classification: always "Confidential" for client-facing SDDs
- Change log table: version, date, author, summary of changes

**Writing rules:**
- This section is not written in prose — it is a structured metadata block
- Version numbering: 0.x = draft, 1.0 = first SA-reviewed version, 2.0+ = post-client-review

---

## Section 2 — Executive Summary

**Purpose:** Enable a VP, CTO, or project sponsor to understand what VTEX will do for this client in 3 minutes.

**Required elements:**
- 1 paragraph: client context (who they are, what they sell, what model)
- 1 paragraph: what VTEX is being implemented to do (scope in business language)
- 1 paragraph: key architecture decisions (3–5 bullets max)
- 1 line: go-live target and implementation partner

**Writing rules:**
- No technical jargon (no "VTEX IO", "Master Data v2", "Order Feed v3" — use business language)
- No bullet walls — this is executive prose
- Max 1 page
- Do NOT copy-paste from the engagement scope document — write fresh for this audience

**Anti-pattern to avoid:**
> "VTEX will implement the Trade Policy-scoped catalog with external pricing connector via IO app, leveraging the Anti-Fraud Provider Protocol and Order Feed v3 for ERP integration."

**Correct pattern:**
> "VTEX will power the online storefront, product catalog, and order management for [Client], enabling their merchandising team to manage products and pricing directly within the VTEX platform. Orders will flow automatically to their SAP ERP for invoicing and fulfillment."

---

## Section 3 — Engagement Context

**Purpose:** Give the SA context on who the client is, why they chose VTEX, and what their current challenges are.

**Required elements:**
- Client profile: industry, geography, channels, GMV size (if known), business model
- Current platform: what they're running today and why they're migrating
- Key pain points: 3–5 specific business problems VTEX is solving
- Why VTEX: brief commercial rationale (not a sales pitch)
- Stakeholder map: who are the key client contacts (business owner, IT lead, project manager)

**Writing rules:**
- This section is narrative — 3–5 paragraphs
- Reference specific client context; do not use generic statements
- If pain points were not confirmed in discovery, flag them as `[ASSUMPTION]`

---

## Section 4 — Scope Definition

**Purpose:** Define exactly what is and is not in this project. Protects both client and VTEX from scope creep.

**Required elements:**
- IN SCOPE: modules, channels, countries, integrations — with phase assignment
- OUT OF SCOPE: explicitly listed items (this is as important as IN SCOPE)
- DEFERRED: what is agreed for a later phase
- Scope table: use the module checklist format from `module-checklist.md`

**Writing rules:**
- Be specific. "Marketplace" is not enough — specify "VTEX Seller Portal for up to 50 sellers, Phase 1"
- Out of scope items must be explicit, not implied. If the client mentioned something in discovery that is not being delivered, list it as OUT OF SCOPE with a brief note.
- Every DEFERRED item needs an agreed definition of "done" for that phase

---

## Section 5 — Architecture Overview

**Purpose:** Give any SA or developer a mental model of the full solution before diving into modules.

**Required elements:**
- Architecture diagram (embed as Figure 1)
- Architecture narrative: 3–5 paragraphs describing the diagram
- Key design principles: bullet list of the 5–8 principles governing this architecture

**Writing rules:**
- The narrative explains WHY the architecture looks this way, not just WHAT it contains
- Design principles should be specific to this client, not generic VTEX marketing
- Example principles:
  - "VTEX OMS is the order of record; all order status changes originate in VTEX"
  - "SAP S/4HANA is the invoice and financial master; VTEX never generates fiscal documents"
  - "All integrations are event-driven via Order Feed v3; no polling from external systems"
  - "Pricing is master-of-record in SAP; VTEX fetches real-time prices via external price connector"

---

## Sections 6.x — Module Design

**Purpose:** Give the SA and developers everything they need to configure, build, and test each module.

**Required structure per module:**

### 6.x.1 Capability Summary
One table per module showing what VTEX does natively vs. what requires extension:

| Capability | Status | Notes |
|---|---|---|
| [Capability name] | NATIVE / PARTIAL NATIVE / CUSTOM / INTEGRATION | Brief note |

### 6.x.2 Configuration Requirements
- Specific settings, rules, and parameters to configure
- Who owns the configuration (VTEX team, SI team, client team)
- Configuration dependencies (e.g., "price tables must be created before trade policy binding")

### 6.x.3 Extension & Custom Development
- For each CUSTOM or PARTIAL NATIVE item: what VTEX IO app is needed, who builds it, deployment model
- Be explicit: "This requires a new VTEX IO service app. Owner: SI team. Estimated effort: 3 weeks."

### 6.x.4 Integration Points
- APIs used (with specific VTEX API endpoint references)
- Events consumed or published
- Data flow diagram if complex

### 6.x.5 Data Ownership Map
Short table: for each data entity in this module, who is the master?

| Data Entity | Master | VTEX Role | Sync Direction |
|---|---|---|---|

### 6.x.6 Open Questions & Assumptions
- All `[ASSUMPTION]` and `[OPEN]` flags specific to this module
- These are also consolidated in Sections 12 and 13

**Writing rules:**
- Do not describe VTEX capabilities generically — be specific to this client's use case
- If a capability is NOT being used (e.g., VTEX Returns App out of scope), say so explicitly
- If there is a known VTEX limitation relevant to this client, document it with a mitigation

---

## Section 7 — Integration Architecture

**Purpose:** Give the integration developer a complete map of all system-to-system connections.

**Required elements:**
- Integration inventory table (see formatting-guide.md)
- For each integration: protocol, direction, data entities exchanged, sync type, frequency, owner
- Integration principles (event-driven vs. polling, middleware strategy)
- Failure handling strategy: what happens when an integration fails

**Writing rules:**
- Every external system mentioned elsewhere in the SDD must appear in the integration inventory
- Do not leave "TBD" for protocol or ownership — if unknown, flag as `[OPEN]`
- Document rate limit considerations for VTEX APIs (especially Catalog and Order APIs)

---

## Section 8 — Data Migration

**Purpose:** Define how data moves from the legacy platform to VTEX at go-live.

**Required elements:**
- Data types in scope for migration: products, prices, customers, order history
- Source system(s) for each data type
- Migration approach: full load, delta, or real-time
- Data quality requirements and validation approach
- VTEX API limits relevant to migration (Catalog API rate limits, batch import capabilities)
- Migration timeline relative to go-live

**Writing rules:**
- Do not overcommit on migration scope — flag data quality risks explicitly
- Historical order migration is often optional and adds risk; flag if in scope
- Customer password migration is typically not possible — document the re-login strategy

---

## Section 9 — Identity & Access Management

**Purpose:** Document authentication, authorization, and access control for both storefront and admin.

**Required elements:**
- Storefront identity: native VTEX profile vs. external IdP (OAuth 2.0 delegation)
- Admin access: VTEX License Manager roles and RBAC
- B2B identity: org-level access control if applicable
- SSO architecture if an external IdP is in scope

---

## Section 10 — Non-Functional Requirements

**Purpose:** Document performance, scalability, compliance, and operational requirements.

**Required elements:**
- Performance: peak OPM, page load targets (Core Web Vitals), API response time SLAs
- Scalability: expected GMV and SKU growth over 12/24 months
- Availability: VTEX SLA alignment, any additional HA requirements
- Compliance: GDPR, LGPD, PCI DSS, GST, PSD2 — document which apply and how they are met
- Data residency: VTEX region (US, EU, etc.) and client requirements
- Monitoring: VTEX native observability vs. external APM (Datadog, New Relic)

---

## Section 14 — Implementation Phases & Milestones

**Purpose:** Give the project manager and SA a delivery roadmap anchored to the SDD scope.

**Required elements:**
- Phase table: phase name, scope, target date, dependencies, owner
- Key milestones: discovery complete, design approved, development start, UAT, go-live, hypercare end
- Known dependencies that could affect the timeline

**Writing rules:**
- Do not promise specific dates unless confirmed with the client and SI partner
- Flag any dependencies on client-side work (data migration, content, third-party contracts)
- Phase milestones should match the scope table in Section 4

---

## Section 15 — Appendix

Optional. Include any of the following if relevant:
- Glossary of terms
- VTEX API reference links
- Architecture diagram source files (link to draw.io, Miro, etc.)
- Discovery call transcripts or notes (summarized, not verbatim)
- Third-party vendor evaluation notes
- VTEX App Store apps referenced in the document
