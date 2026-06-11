# Module Design Rules

## Table of Contents
1. [Catalog & Product Information](#catalog)
2. [Pricing & Promotions](#pricing)
3. [Order Management (OMS)](#oms)
4. [Checkout & Payments](#checkout)
5. [Marketplace & Seller Management](#marketplace)
6. [Search & Navigation](#search)
7. [Storefront](#storefront)
8. [Master Data & Customer Data](#masterdata)
9. [Logistics & Fulfillment](#logistics)
10. [B2B Configuration](#b2b)

---

## 1. Catalog & Product Information {#catalog}

### Required Content
Every Catalog section must include:

**a) Catalog Hierarchy Decision**
Document the full hierarchy:
- Account model: single-account vs. multi-account (Franchise App / Edition)
- Category tree depth (max recommended: 4 levels)
- Product/SKU structure: one-size vs. multi-SKU products
- Specification architecture: global specifications vs. category-level

**b) Catalog Ownership**
- Who owns the product master: VTEX Admin, external PIM (Akeneo, Stibo, Salsify), ERP
- If PIM is master: document the sync protocol (VTEX Catalog API, batch job, real-time event)
- Read vs. write access model

**c) SKU/Product ID Strategy**
- RefID strategy: use external system IDs or VTEX-generated
- EAN / barcode handling
- Variant structure (color, size, etc.)

**d) Image Management**
- Image hosting: VTEX CDN (native) vs. external DAM
- Naming conventions and image size requirements

**e) Trade Policy Catalog Binding**
- Which catalog (or catalog subset) is visible per trade policy
- Cross-account product sharing if applicable

### Accuracy Rules
- Catalog API has rate limits — document batch migration strategy for large catalogs
- VTEX does not support delta-sync natively — middleware is required for incremental PIM sync
- Multi-catalog setups (Franchise App) require an explicit architecture decision

---

## 2. Pricing & Promotions {#pricing}

### Required Content

**a) Price Table Architecture**
- Number of price tables required
- Binding: which price table maps to which trade policy / sales channel
- List price vs. computed price (promotions engine override)
- Price rule complexity: fixed, percentage, date-based

**b) External Pricing (if applicable)**
- External price connector via VTEX IO app
- Latency requirements and fallback behavior
- Which system is price master: VTEX vs. ERP vs. pricing engine

**c) Promotions Architecture**
- Promotion types in use (document each type)
- Stacking behavior: single most beneficial vs. all applicable
- Promotion ownership: VTEX Admin vs. external campaign tool
- Coupon strategy: VTEX native coupons vs. external coupon service

**d) Campaign Audiences**
- Cluster-based segmentation (VTEX Master Data)
- VTEX Sessions-based personalization
- CDP-driven audience injection

### Accuracy Rules
- Promotions engine and price tables are separate systems — do not conflate
- Progressive discounts require correct promotion type selection
- Promotion simulation: use VTEX Pricing Hub for multi-seller scenarios
- External promotion engines require a VTEX IO connector — cannot replace natively

---

## 3. Order Management (OMS) {#oms}

### Required Content

**a) Order Flow Documentation**
Document each state transition:
```
Placement → Payment Authorization → Payment Capture → Invoicing → 
Ready for Handling → Handling → Invoiced → Shipping → Delivered → 
[Return → Refund]
```
For each state: who triggers it, which system, via which API/event.

**b) VTEX OMS vs. External OMS**
- If VTEX OMS is primary: document configuration, SLA rules, routing logic
- If external OMS (SAP, Oracle, Manhattan): document VTEX OMS role (receive and route) vs. external OMS role (orchestrate)
- Hybrid patterns must have explicit data ownership per state

**c) Invoice Injection**
- CRITICAL: VTEX does NOT generate invoices
- Document: which system generates the invoice (ERP, fiscal middleware, billing system)
- Document: how the invoice is injected back into VTEX (Order API `POST /api/oms/pvt/orders/{orderId}/invoice`)
- Document: timing requirement (real-time vs. batch)

**d) Order Events Architecture**
- Order Feed v3 vs. Order Hooks: document the choice and why
  - Order Feed: polling, at-least-once delivery, multiple consumers
  - Order Hooks: push, HTTP callback, single endpoint per event type
- Event consumers: ERP, WMS, CRM, BI, notification services

**e) Returns & Refunds**
- VTEX Returns App (native): document configuration
- External RMA system: document integration pattern
- Refund flow: payment gateway refund trigger ownership

### Accuracy Rules
- VTEX OMS status machine is configurable but has defined transitions — validate custom transitions
- Order Feed v3 requires consumer commitment queue — document commit strategy
- Cancel order flow has timing constraints — document SLA for cancellation window

---

## 4. Checkout & Payments {#checkout}

### Required Content

**a) SmartCheckout Configuration**
- Profile reuse: enabled (native) or disabled (for compliance / UX reasons)
- Guest vs. identified checkout strategy
- Cart abandonment strategy (VTEX native vs. external)

**b) Checkout UI Customization**
- Native checkout (no customization): fastest path
- VTEX IO Checkout UI Customization app: document customization points
- Custom fields, custom steps, custom validations: document each

**c) Payment Architecture**
- Payment methods in scope (list each: credit, debit, PIX, boleto, installments, BNPL, etc.)
- Payment connectors: native VTEX connector vs. custom via Payment Provider Protocol
- For each connector: document provider name, protocol compliance, installment rules
- Payment splitting: document if more than 2 payment methods required simultaneously

**d) Anti-Fraud**
- VTEX Anti-Fraud Provider Protocol: native integration point
- Document the anti-fraud provider (Clearsale, Konduto, ClearSale, Signifyd, etc.)
- Synchronous vs. asynchronous fraud evaluation: document the flow

**e) Gift Cards & Loyalty**
- Native VTEX Gift Card Provider Protocol: document if using
- External loyalty / points system: document integration pattern

### Accuracy Rules
- Payment Provider Protocol compliance requires certification — factor in timeline
- SmartCheckout profile reuse requires consent management consideration (LGPD/GDPR)
- Checkout is hosted by VTEX — deep customization has limits; document constraints

---

## 5. Marketplace & Seller Management {#marketplace}

### Required Content

**a) Seller Architecture Decision**
This is the most critical decision in a marketplace SDD. Document explicitly:

| Factor | VTEX Seller Portal | External Seller Protocol |
|---|---|---|
| Seller portal UI | VTEX native | Custom-built |
| Catalog management | Via VTEX admin | Via seller's own system |
| Order management | Via VTEX admin | Via seller's own system |
| Integration effort | Low (seller) | High (seller) |
| Marketplace control | High | High |
| Recommended for | SMB sellers, managed onboarding | Large sellers with own systems |

**b) Catalog Flow**
- Seller SKU submission → Marketplace catalog approval workflow
- Matcher configuration: automatic vs. manual matching
- Catalog inheritance: seller catalog vs. marketplace catalog enrichment

**c) Order Routing & Chain**
- Automatic order routing (by trade policy / seller availability)
- Manual chain assignment
- Multi-seller cart split: document behavior

**d) Commission Architecture**
- CRITICAL: VTEX does NOT calculate commissions natively
- Commission rates stored in VTEX (per seller / per category)
- Actual commission calculation and reporting: external system
- Document: which system owns commission calculation, reporting, and payout

**e) Seller Onboarding**
- KYC: external (document provider)
- Seller Portal access: VTEX native role management
- Onboarding workflow: manual vs. automated (via Master Data + VTEX IO)

### Accuracy Rules
- Broadcaster: seller stock/price notification to marketplace — configure correctly for scale
- External Seller Protocol requires the seller to build and host the integration endpoint
- Commission rates in VTEX are informational — not used for calculation

---

## 6. Search & Navigation {#search}

### Required Content

**a) VTEX Intelligent Search Configuration**
- Indexing strategy: full catalog vs. trade-policy scoped
- Synonym rules: source and management process
- Relevance rules: default vs. custom
- Banners and sponsored content (native IS)
- Facet configuration: category-level vs. global

**b) AI Search Features**
- Semantic search: enabled/disabled
- Autocomplete configuration
- Spell correction
- "Did you mean" behavior

**c) Search Customization**
- IO Storefront: `search-result` block overrides
- FastStore: `useSearchPage`, `useFacets`, custom section overrides
- External search engine replacement (Algolia, Bloomreach): document API extension layer

**d) Merchandising Rules**
- Manual ordering vs. algorithm-driven
- Promoted products configuration
- Seasonal rule management process

---

## 7. Storefront {#storefront}

### Required Content

**a) Framework Decision**
Document the decision between:

| Criterion | FastStore | VTEX IO Store Framework |
|---|---|---|
| Performance requirement | Core Web Vitals critical | Standard |
| Design freedom | Full custom | Block-based |
| CMS | VTEX Headless CMS | VTEX Site Editor |
| Developer profile | React/Next.js | VTEX IO |
| Time to market | Longer (custom) | Faster (blocks) |
| Native VTEX blocks | Limited | Full library |

**b) CMS Architecture**
- Content ownership: VTEX Headless CMS vs. external CMS (Contentful, Sanity, etc.)
- Page structure: which pages are CMS-managed vs. hard-coded
- Localization strategy

**c) Analytics & Tag Management**
- GTM integration strategy
- Data layer events: document all required events
- Custom dimensions / metrics

**d) Performance Requirements**
- Core Web Vitals targets (LCP, INP, CLS)
- CDN strategy: VTEX CDN vs. additional CDN layer

---

## 8. Master Data & Customer Data {#masterdata}

### Required Content

**a) Master Data v2 Architecture**
- Custom data entities required (document schema per entity)
- Indexing strategy for custom entities
- Scroll/search API usage: document for bulk reads

**b) Customer Profile**
- Profile ownership: VTEX native profile vs. external CRM/CDP as master
- PII handling and data residency requirements
- GDPR/LGPD compliance approach

**c) Segmentation**
- Customer clusters: definition and management
- VTEX Sessions: session context used for personalization
- External CDP integration (if applicable)

---

## 9. Logistics & Fulfillment {#logistics}

### Required Content

**a) Inventory Management**
- Inventory ownership: VTEX native vs. WMS
- Multi-warehouse / multi-dock strategy
- Real-time vs. batch inventory sync: document frequency and method

**b) Shipping Strategy**
- Carriers configured in VTEX
- Carrier tables (by weight, zip, region): document management process
- External shipping calculation: VTEX IO app or Shipping Rate Template
- Click & Collect: dock configuration, pickup point management

**c) Fulfillment Orchestration**
- VTEX OMS routing to fulfillment
- WMS integration: order out (VTEX → WMS), fulfillment status in (WMS → VTEX)
- Drop-shipping: seller-fulfilled orders (marketplace scenario)

**d) Pick & Pack (if applicable)**
- VTEX Pick & Pack: document if in scope
- Custom picking app via VTEX IO

---

## 10. B2B Configuration {#b2b}

### Required Content (only when B2B is in scope)

**a) B2B Suite Components**
Document which components are in scope:
- B2B Organizations: org/cost center/user hierarchy
- B2B Quotes: quote workflow, approvals
- B2B Checkout Settings: payment method restrictions per org
- Quick Order: CSV/SKU-based bulk ordering
- Wishlist: shared wishlists per org

**b) Price Table Assignment**
- Per-organization price table assignment
- Dynamic pricing via external system (B2B pricing engine)

**c) Credit Management**
- VTEX native credit (B2B Suite): document limits and approval workflow
- External credit system: document integration pattern

**d) Approval Workflow**
- Purchase approval rules: by amount, by category, by user role
- VTEX native approvals vs. external workflow (ServiceNow, SAP Ariba)
