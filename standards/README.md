# Standards & Regulatory Mapping

**Last reviewed: 2026-08-07.**

Which standard is covered by what in this application, clause by clause. Nine
frameworks across five documents.

Every row in these documents was checked against the current models and
services — not against what was planned — and each one states plainly where the
app falls short. They are working gap analyses, not sales material.

---

## Start here — the one-screen summary

| Framework | Document | Where the app stands |
|---|---|---|
| **ISO 9001** Quality<br>**ISO 14001** Environment<br>**ISO 45001** OH&S | [iso-standards-mapping.md](iso-standards-mapping.md) | **This is what the app is.** Nearly every clause has a module behind it. Two known gaps: ISO 9001 §8.2 contract review, ISO 45001 §8.1.4 contractor pre-qualification. |
| **Factories Act 1948** (India)<br>**OSHA** 29 CFR 1904/1910 (US) | [factories-act-osha-mapping.md](factories-act-osha-mapping.md) | **Strong.** Permit to Work, HAZOP, PSSR, incidents with PSE tiering, emergency drills and OHC cover most duties — 10 of OSHA PSM's 14 elements included. Weak on statutory registers, working hours, exposure monitoring and PPE. |
| **ISO/IEC 17025** Laboratories | [iso-17025-mapping.md](iso-17025-mapping.md) | **Split in two.** Clause 8 (management system) is largely covered; equipment, traceability and environmental monitoring now are too. Clause 7 (methods, samples, results, reporting) is a LIMS the app does not have. |
| **21 CFR Part 11** E-records & e-signatures (FDA) | [cfr21-part11-mapping.md](cfr21-part11-mapping.md) | **Foundations strong, package incomplete.** Access control, sequencing and audit trail are real; electronic signing landed recently. Validation, retention and complete-record export remain. |
| **ICH Q7** API GMP<br>**HACCP** Codex CXC 1-1969<br>**HARPC** 21 CFR 117 | [ich-q7-haccp-harpc-mapping.md](ich-q7-haccp-harpc-mapping.md) | **Not implemented, and configuration will not change that.** These regulate a *product* — there is no `Product`, `Batch`, `Specification` or `Recall` in the schema. The app can be the QMS layer above a GMP execution system, not that system. |

---

## Choosing the right document

The five documents are not the same *kind* of thing, and reading them as if
they were is the commonest mistake:

- **[ISO 9001 / 14001 / 45001](iso-standards-mapping.md)** maps a **management
  system** — processes the app tracks. A gap is a missing register.
- **[Factories Act / OSHA](factories-act-osha-mapping.md)** maps **statutory
  duties**. Many are physical (fence the machine, provide the canteen); the app
  holds the register and the evidence, never the guard itself.
- **[ISO/IEC 17025](iso-17025-mapping.md)** maps a **laboratory**, and is an
  accreditation standard rather than a certification one.
- **[21 CFR Part 11](cfr21-part11-mapping.md)** maps the **computer system
  itself**. Every row is a statement about this codebase. A gap here is a
  software defect, not a missing register.
- **[ICH Q7 / HACCP / HARPC](ich-q7-haccp-harpc-mapping.md)** maps **the
  product** — a batch of API, a lot of food.

---

## Reading a status

| Status | Means |
|---|---|
| **Covered** | The app holds the record the clause requires, with the workflow and evidence an auditor would expect. |
| **Partial** | Some of it is there; the row says exactly what is missing. |
| **Not covered** | Nothing in the app addresses it. |
| **N/A — physical** | A duty no software discharges — guarding, ventilation, facilities. |
| **Procedural** | A written policy, an identity check, a letter to a regulator. Will never become "Covered" by shipping code. |

**Covered never means "you are compliant".** It means the app can hold the
evidence. Certification and accreditation are granted to an organization, not
to its software.

---

## Cross-cutting gaps

Some gaps appear in several documents at once. They are worth doing once, not
three times:

| Gap | Appears as | Status |
|---|---|---|
| Competency-gated authorization | 17025 §6.2.6 (L3) · ICH Q7 §3.1 · 117.180 (P14) · Factories Act §41C(b) (S4) · Part 11 §11.10(i) (G9) | **Closed** — architecture.md §11.5.3 |
| Equipment records & calibration traceability | 17025 §6.4/§6.5 (L1, L2) · ICH Q7 §5.3 (P6) · OSHA 1910.119(j) (S1) | **Closed for 17025** — architecture.md §11.7.1 |
| Exposure / environmental monitoring | 17025 §6.3.3 (L2) · Factories Act §41F (S2) · 117.165 (P13) | **Closed for 17025** (§11.7.2); workplace exposure limits still open |
| Record retention & disposal | 17025 §8.4 · Part 11 §11.10(c) (G2) · 117.315 | **Mechanism built** (§17.1), applied to the 17025 set only |
| Electronic signature meaning & two-component signing | Part 11 §11.50/§11.200 (G1) · 17025 §7.5.2 · 117.310 | **Built** — see the Part 11 mapping |
| Product / batch / specification identity | ICH Q7 §6.5 (P1) · HACCP Step 2 · 117.305 | **Open** — the foundational absence |

---

## Keeping these current

Each document carries its own "Keeping this document current" section, and they
all say the same thing: **treat a stale row here the same as a stale line in
`architecture.md` — fix it in the change that touches the code.**

When a cross-cutting gap closes, update **every** document that lists it. A gap
closed in one place and left standing in three others is worse than no mapping
at all.

---

*Related: [architecture.md](../../../architecture.md) for the technical
description of each module · [the module walkthroughs](../README.md) for how to
actually use them.*
