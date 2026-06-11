# SDD Formatting Guide

## Document Metadata

```
Title:     [Client Name] — VTEX Solution Design Document
Version:   1.0 (Draft) → 1.1 (Under Review) → 2.0 (Approved)
Date:      YYYY-MM-DD
Author:    [SE Name], Solution Engineer, VTEX
Reviewer:  [SA Name], Solution Architect, VTEX
Status:    Draft / Under Review / Approved
Classification: Confidential
```

---

## Typography (VTEX Brand)

| Element | Style |
|---|---|
| Document Title (Cover) | VTEX Trust, 28pt, Serious Black `#142032` |
| Section Heading (H1) | VTEX Trust, 18pt, Serious Black `#142032` |
| Sub-section (H2) | VTEX Trust, 14pt, Serious Black `#142032` |
| Component heading (H3) | VTEX Trust, 12pt, bold, Serious Gray `#5B6E84` |
| Body text | VTEX Trust (fallback: Arial), 11pt, Serious Black |
| Table header | 10pt, bold, white text on Serious Black `#142032` background |
| Table body | 10pt, alternating white / Soft Blue `#F5F9FF` rows |
| Caption | 9pt, italic, Cool Gray `#A1A8B7` |
| Flags [ASSUMPTION], [OPEN], [RISK] | Bold, colored background (see below) |

---

## Flag Formatting

Use consistent inline flags throughout the document:

| Flag | Background Color | Text |
|---|---|---|
| `[ASSUMPTION: ...]` | Yellow `#FFF9C4` | Black |
| `[OPEN: ...]` | Orange `#FFE0B2` | Black |
| `[RISK: ...]` | Red `#FFCDD2` | Black |
| `[ROADMAP: ...]` | Blue `#BBDEFB` | Black |

In the `.docx` output, use a shaded paragraph or table cell with the appropriate color to visually distinguish these flags.

---

## Table Conventions

All tables must follow this pattern:

1. **Header row**: Serious Black background `#142032`, white text, 10pt bold
2. **Data rows**: Alternating white and Soft Blue `#F5F9FF`
3. **Column widths**: Proportional, never let text wrap in header row
4. **Borders**: Light gray `#E7E9EE`, single 1pt
5. **Cell padding**: 80 top/bottom, 120 left/right (DXA units in docx)

### Standard Table Types in SDD

**Scope Table (Section 4):**
| Module | In Scope | Phase | Notes |
|---|---|---|---|

**Integration Inventory Table (Section 7):**
| System | Direction | Protocol | Owner | Sync Type | Frequency |
|---|---|---|---|---|---|

**ADR Table (Section 11):**
| ID | Domain | Decision | Rationale | Alternatives | Trade-offs | Owner | Status |
|---|---|---|---|---|---|---|---|

**Assumptions Register (Section 12):**
| ID | Module | Assumption | Impact if Wrong | Validation Owner |
|---|---|---|---|---|

**Risk Register (Section 13):**
| ID | Module | Description | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|---|---|

---

## Section Dividers

Use a thin Rebel Pink `#F71963` horizontal rule (2pt) to separate major sections (H1 level). Do not use full-width colored section divider pages in a document context (that pattern is for presentations).

---

## Cover Page Layout

```
[Top: VTEX logo, top-left]

[Center-left, vertical center:]
Client Name
VTEX Solution Design Document
[Rebel Pink accent line, 3pt, full width]
Version X.X | YYYY-MM-DD
Confidential

[Bottom-left:]
Prepared by: [SE Name]
Reviewed by: [SA Name]
```

Background: Serious Black `#142032`
All text: white
Logo: white version

---

## Page Layout

- Paper: A4 (international) or US Letter (North American clients)
- Margins: 2.5cm / 1 inch on all sides
- Header: Document title (abbreviated) + version — right-aligned
- Footer: Client name + "Confidential" (left) | Page number (right)
- Font fallback if VTEX Trust unavailable: Arial

---

## Diagram Embedding

- All architecture diagrams must be embedded as images (PNG, minimum 150dpi)
- Caption required below every diagram: "Figure X: [Description]"
- If diagram is too wide, rotate page to landscape for that section only
- Reference diagrams from the text: "See Figure 3 — Integration Architecture"
- Preferred diagram sources: draw.io, Lucidchart, Miro exported as PNG

---

## Capability Label Formatting

Use consistent inline labels throughout:

| Label | Meaning | Visual Style |
|---|---|---|
| **NATIVE** | Out-of-the-box VTEX | Green bold |
| **PARTIAL NATIVE** | Native + config or VTEX IO app | Yellow bold |
| **CUSTOM** | New VTEX IO app required | Orange bold |
| **INTEGRATION** | Third-party system required | Blue bold |
| **NOT SUPPORTED** | Cannot be done on VTEX | Red bold |
| **ROADMAP** | On VTEX roadmap, no delivery date | Gray bold |

---

## Version Control

| Version | Date | Author | Changes |
|---|---|---|---|
| 0.1 | YYYY-MM-DD | [SE] | Initial draft |
| 0.2 | YYYY-MM-DD | [SE] | Added OMS and integration sections |
| 1.0 | YYYY-MM-DD | [SA] | SA review complete, sent for client review |
| 1.1 | YYYY-MM-DD | [SE] | Client feedback incorporated |
| 2.0 | YYYY-MM-DD | [SA] | Final approved version |

---

## File Naming Convention

```
[ClientName]-SDD-v[X.X]-[YYYYMMDD].docx

Examples:
TataCLiQ-SDD-v1.0-20250610.docx
Bronisze-SDD-v0.2-20250503.docx
AlRais-SDD-v2.0-20250401.docx
```

---

## Length Guidelines

| Section | Recommended Length |
|---|---|
| Executive Summary | 1 page max |
| Engagement Context | 1–2 pages |
| Scope Definition | 1 page + scope table |
| Architecture Overview | 2–3 pages + diagram |
| Each Module Section | 2–4 pages |
| Integration Architecture | 2–4 pages + integration table |
| Data Migration | 1–2 pages |
| ADR Table | 1 row per decision, no length limit |
| Assumptions Register | 1 row per assumption |
| Risk Register | 1 row per risk |
| Total SDD | 30–60 pages typical; complex enterprise: up to 100 |

**Do not pad.** A 35-page SDD that answers every critical question is better than a 70-page SDD full of generic content.
