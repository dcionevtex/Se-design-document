# Module Scope Checklist

Use at project start to define what is IN SCOPE, OUT OF SCOPE, or DEFERRED.

Fill this in with the client / SE team before starting the SDD. Include the completed table in Section 4 of the SDD.

---

## Scope Checklist Table

| Module | Sub-Component | Status | Phase | Notes |
|---|---|---|---|---|
| **Catalog** | Category tree design | | | |
| | Product/SKU architecture | | | |
| | Specification groups | | | |
| | PIM integration | | | |
| | Data migration from legacy | | | |
| | Image migration | | | |
| **Pricing** | Price table architecture | | | |
| | Trade policy binding | | | |
| | External pricing connector | | | |
| | Price scheduling | | | |
| **Promotions** | Native VTEX promotions | | | |
| | Coupon management | | | |
| | Loyalty / points program | | | |
| | External promotion engine | | | |
| **OMS** | VTEX OMS as order of record | | | |
| | External OMS integration | | | |
| | Invoice injection | | | |
| | Order Feed / Hooks configuration | | | |
| | Returns / RMA | | | |
| | B2B order approval | | | |
| **Checkout** | SmartCheckout | | | |
| | Checkout UI customization | | | |
| | Payment methods | | | |
| | Payment connector (native/custom) | | | |
| | Anti-fraud integration | | | |
| | Gift cards / store credit | | | |
| **Marketplace** | VTEX Seller Portal | | | |
| | External Seller Protocol | | | |
| | Catalog matching (Matcher) | | | |
| | Order chain / routing | | | |
| | Commission configuration | | | |
| | Seller onboarding workflow | | | |
| | KYC integration | | | |
| **Search** | VTEX Intelligent Search | | | |
| | Synonym management | | | |
| | Relevance rules | | | |
| | AI / semantic search | | | |
| | External search engine | | | |
| **Storefront** | FastStore | | | |
| | VTEX IO Store Framework | | | |
| | VTEX Headless CMS | | | |
| | VTEX Site Editor | | | |
| | External CMS integration | | | |
| | Mobile PWA | | | |
| | Native mobile app | | | |
| **Master Data** | Custom entities | | | |
| | Customer profile | | | |
| | Customer segmentation | | | |
| | CDP integration | | | |
| **Logistics** | Warehouse / dock configuration | | | |
| | Carrier configuration | | | |
| | WMS integration | | | |
| | Click & Collect | | | |
| | Same-day delivery | | | |
| | VTEX Pick & Pack | | | |
| **B2B** | B2B Organizations | | | |
| | B2B Quotes | | | |
| | Quick Order | | | |
| | B2B credit management | | | |
| | Purchase approval workflow | | | |
| **Identity** | SSO / IdP integration | | | |
| | Social login | | | |
| | VTEX RBAC configuration | | | |
| **Analytics** | GTM integration | | | |
| | Data layer events | | | |
| | CDP / data platform | | | |
| **Tax & Compliance** | Tax Provider Protocol | | | |
| | Specific regulation (LGPD/GDPR/GST) | | | |
| **Live Shopping** | VTEX Live Shopping | | | |
| | External live stream integration | | | |

---

## Status Values

| Status | Meaning |
|---|---|
| **IN** | In scope for this project |
| **OUT** | Explicitly out of scope |
| **DEFERRED** | Agreed to be in a later phase |
| **TBD** | Not yet decided — must be resolved |
| **PARTNER** | Owned by SI / implementation partner |

---

## Phase Values

| Phase | Meaning |
|---|---|
| **P1** | MVP / go-live |
| **P2** | Post-launch, within 3 months |
| **P3** | Future roadmap, 3–12 months |
