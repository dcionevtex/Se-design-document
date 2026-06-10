# Discovery Questions by Module

Use this file during Step 2 (Discovery Gap Analysis). For each module in scope, verify you have answers to the CRITICAL questions. If an answer is missing, flag it with `[OPEN]` or `[ASSUMPTION]` in the SDD.

---

## Business Context (Always Required)

| # | Question | Impact if Missing |
|---|---|---|
| B1 | What is the business model? (B2C / B2B / D2C / Marketplace / Omnichannel) | Determines entire architecture |
| B2 | What channels are in scope? (web, mobile app, in-store, WhatsApp, etc.) | Storefront and integration scope |
| B3 | What countries/regions are in scope? | Tax, currency, language, regulatory |
| B4 | What is the go-live target date? | Phasing and MVP scope |
| B5 | What is the current platform and why are they migrating? | Risk areas and data migration scope |
| B6 | What are the top 3 business pain points VTEX must solve? | Success criteria |
| B7 | What is the GMV target or order volume at go-live and at 12 months? | Performance and scalability requirements |
| B8 | Who is the system integrator / implementation partner? | Delivery ownership |

---

## Catalog

| # | Question | Impact if Missing |
|---|---|---|
| C1 | How many SKUs are in scope at go-live? | Migration planning, indexing performance |
| C2 | Is there a PIM system? If yes, which one? | Catalog ownership and sync architecture |
| C3 | How deep is the category tree? How many categories? | Taxonomy design |
| C4 | How many product specifications exist? Are they global or category-specific? | Specification architecture |
| C5 | Is the catalog shared across multiple channels / trade policies? | Trade policy and catalog binding |
| C6 | Are there product bundles or kits? | Kit configuration in VTEX |
| C7 | Where are product images hosted today? Will they be migrated? | DAM and image CDN strategy |
| C8 | Does the client use EAN/barcode tracking? | SKU RefID strategy |
| C9 | Is there a multi-brand or multi-store requirement? | Franchise App / multi-account decision |

---

## Pricing

| # | Question | Impact if Missing |
|---|---|---|
| P1 | How many price tables are needed? (by channel, segment, region) | Price table architecture |
| P2 | Is pricing managed in VTEX Admin or driven from an external system (ERP, pricing engine)? | External price connector need |
| P3 | Are there time-based or flash sale prices? | Price scheduling strategy |
| P4 | Does price differ by customer segment or organization (B2B)? | Trade policy and price table binding |
| P5 | What are the installment rules? (number of installments, interest, minimum amount) | Payment condition configuration |
| P6 | Is there a price approval workflow before publishing? | Process and tooling gap |

---

## Promotions

| # | Question | Impact if Missing |
|---|---|---|
| PR1 | What promotion types are required? (% off, fixed, free shipping, BOGO, gift, progressive) | Promotion engine configuration |
| PR2 | Can multiple promotions stack in a single cart? | Stacking rules |
| PR3 | Are promotions managed in VTEX Admin or by an external campaign management tool? | Integration vs. native |
| PR4 | Is coupon management native VTEX or external? | Coupon service integration |
| PR5 | Are promotions targeted by customer segment, cluster, or campaign? | Audience architecture |
| PR6 | Is there a loyalty / points program? If yes, which system? | Gift card protocol or external integration |

---

## Order Management (OMS)

| # | Question | Impact if Missing |
|---|---|---|
| O1 | Is VTEX OMS the order of record or is there an external OMS (SAP, Oracle, Manhattan)? | VTEX OMS role definition |
| O2 | Which system generates the invoice / fiscal document? | Invoice injection architecture |
| O3 | What is the ERP system? What order data does it need? | ERP integration pattern |
| O4 | What is the expected order volume per day at peak? | Feed vs. Hooks decision, scalability |
| O5 | Are there order approval workflows (B2B)? | Approval engine requirement |
| O6 | Is there a return / RMA process? What system manages it? | Returns App vs. external |
| O7 | What payment statuses trigger fulfillment? | OMS flow configuration |
| O8 | Are there split orders (multi-seller or multi-warehouse)? | Order routing configuration |
| O9 | What are the SLAs for order processing, shipping, and delivery? | SLA rule configuration |

---

## Checkout & Payments

| # | Question | Impact if Missing |
|---|---|---|
| CH1 | What payment methods are required? List all. | Connector inventory |
| CH2 | Is there an existing payment gateway contract? Which provider? | Native vs. custom connector |
| CH3 | Are installments required? With or without interest? | Payment condition configuration |
| CH4 | Is SmartCheckout (saved profile / 1-click) acceptable? (LGPD/GDPR compliance check) | SmartCheckout enablement |
| CH5 | Is there a checkout UI customization requirement? What exactly? | Checkout IO app scope |
| CH6 | Is there an anti-fraud solution in place? Which one? | Anti-fraud protocol connector |
| CH7 | Are gift cards or store credits used? | Gift card protocol |
| CH8 | Is PIX required? (Brazil) | PIX connector configuration |
| CH9 | Is split payment (marketplace payout at checkout) required? | Payment split architecture |

---

## Marketplace & Seller Management

| # | Question | Impact if Missing |
|---|---|---|
| M1 | How many sellers at go-live? How many at 12 months? | Architecture scale |
| M2 | Do sellers have their own OMS / ERP / WMS? | Seller Portal vs. External Seller Protocol |
| M3 | Who owns seller onboarding and KYC? | External tooling requirement |
| M4 | How are commission rates defined? By seller? By category? By product? | Commission data model |
| M5 | Which system calculates and reports commissions? | External commission system |
| M6 | Which system handles seller payouts? | Payment orchestration outside VTEX |
| M7 | Does the marketplace own the catalog or do sellers? | Catalog ownership model |
| M8 | Is catalog matching manual or automatic? | Matcher configuration |
| M9 | Are there seller performance SLAs? Which system monitors them? | External analytics tool |
| M10 | Is there a seller-facing portal requirement (beyond VTEX native)? | Custom seller portal scope |

---

## Search & Navigation

| # | Question | Impact if Missing |
|---|---|---|
| S1 | Is VTEX Intelligent Search the chosen search engine or is there an external solution? | IS vs. external connector |
| S2 | Are there specific relevance rules or business-driven ranking requirements? | Custom relevance configuration |
| S3 | How are synonyms and search terms managed? Who owns the process? | IS admin process |
| S4 | Are there merchandising rules (promoted products, banners)? | IS merchandising configuration |
| S5 | What facets/filters are required? Are they dynamic or fixed? | Facet architecture |
| S6 | Is semantic / AI search required? | IS AI feature enablement |
| S7 | Is there a multilingual search requirement? | IS language configuration |

---

## Storefront

| # | Question | Impact if Missing |
|---|---|---|
| ST1 | Is there a design system or brand guideline? | Storefront design scope |
| ST2 | What is the Core Web Vitals target? (LCP, INP, CLS) | FastStore vs. IO Storefront decision |
| ST3 | Is there a CMS requirement? What is the content ownership model? | CMS architecture |
| ST4 | Is there a mobile app? Native or PWA? | Native app integration or PWA config |
| ST5 | Which analytics platform is used? (GA4, Adobe Analytics, etc.) | GTM / data layer architecture |
| ST6 | Are there third-party widgets or scripts required on the storefront? | Script injection strategy |
| ST7 | What is the expected peak traffic? | CDN and caching strategy |
| ST8 | Are there A/B testing requirements? | Experimentation platform integration |

---

## Logistics & Fulfillment

| # | Question | Impact if Missing |
|---|---|---|
| L1 | How many fulfillment centers / warehouses are in scope? | Dock and inventory configuration |
| L2 | Is there a WMS? Which one? | WMS integration pattern |
| L3 | What carriers are in use? Is there a shipping aggregator? | Carrier configuration |
| L4 | Is Click & Collect (BOPIS) required? | Pickup point configuration |
| L5 | Is same-day or next-day delivery required? | SLA and carrier integration complexity |
| L6 | What is the inventory sync frequency requirement? (real-time / batch) | Integration pattern selection |
| L7 | Is cross-docking or drop-shipping used? | Fulfillment routing configuration |
| L8 | Is there a returns logistics process? Who manages it? | Reverse logistics integration |

---

## Identity & Access

| # | Question | Impact if Missing |
|---|---|---|
| IAM1 | Is there an existing SSO / IdP solution? (Okta, Azure AD, Keycloak, etc.) | VTEX OAuth delegation architecture |
| IAM2 | Is social login required? (Google, Facebook, Apple) | OAuth provider configuration |
| IAM3 | For B2B: is there an org/user hierarchy for access control? | B2B Suite organizations |
| IAM4 | Are there specific VTEX Admin role requirements (RBAC)? | VTEX License Manager configuration |

---

## Non-Functional Requirements

| # | Question | Impact if Missing |
|---|---|---|
| NFR1 | What are the SLA requirements for uptime? | VTEX SLA alignment |
| NFR2 | What is the peak order per minute (OPM) target? | Load testing and scalability scope |
| NFR3 | Are there data residency requirements? | VTEX region selection |
| NFR4 | What are the data retention requirements? | Master Data archiving strategy |
| NFR5 | Are there regulatory compliance requirements? (PCI, LGPD, GDPR, PSD2, GST) | Compliance architecture |
| NFR6 | What is the DR / RTO / RPO requirement? | VTEX SLA + integration resilience |
