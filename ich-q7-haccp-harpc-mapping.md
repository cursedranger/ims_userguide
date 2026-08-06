# ICH Q7, HACCP & HARPC — Product Safety Standards Mapping

**Last reviewed: 2026-08-06 — update this alongside any new module.**

## Scope and how to read this document

This document maps three product-safety frameworks to the application module,
model, or service that supports them:

- **ICH Q7** — *Good Manufacturing Practice Guide for Active Pharmaceutical
  Ingredients* (ICH, adopted by FDA as guidance under 21 CFR 210/211, by the EU
  as GMP Part II, and by WHO/PIC/S). Sections 1–19.
- **HACCP** — *Hazard Analysis and Critical Control Point System*, Codex
  Alimentarius **CXC 1-1969** (General Principles of Food Hygiene, Rev. 2022):
  Chapter One Good Hygiene Practices, Chapter Two the 7 principles / 12
  application steps.
- **HARPC** — *Hazard Analysis and Risk-Based Preventive Controls for Human
  Food*, **21 CFR Part 117** (FDA, FSMA). Subpart C is HARPC proper; Subparts
  B (CGMP), F (records), and G (supply-chain program) are included because in
  practice a PCHF inspection covers all four.

They are in one file because — despite covering two different industries — the
gap analysis converges on the same conclusion, and splitting it would have meant
writing that conclusion three times.

### Three kinds of mapping document, and this is the third kind

- [iso-standards-mapping.md](iso-standards-mapping.md) maps a **management
  system**. Almost every clause has a module behind it, because that is what
  this application was built to be.
- [cfr21-part11-mapping.md](cfr21-part11-mapping.md) maps a **computer system**.
  Every row there is a statement about this codebase's own controls.
- This document maps **the product**. ICH Q7, HACCP, and HARPC are not about
  how you run a management system or how you keep an electronic record. They
  are about a physical thing — a batch of API, a lot of food — being safe,
  identified, tested, released, and recallable.

That distinction is the whole finding, so it is stated up front rather than
buried in a gap list.

### Current overall position, stated plainly

**This application does not implement ICH Q7, HACCP, or HARPC, and no amount of
configuration will make it do so.** It is not a partial implementation waiting
to be finished — the central object all three standards regulate does not exist
in the schema. There is no `Product`, no `Material`, no `Batch`/`Lot`, no
`Specification`, no `TestResult`, no `Recall`. `NonconformingOutput` — the model
that comes closest to touching product — holds `item_description` as free text
and `batch_or_lot_number` as an unvalidated string precisely *because* no
product master exists to point at (architecture.md §11.16 says so explicitly).

What the app does have is the entire **supporting quality system** all three
standards require *around* that missing core, and it has it to an unusually high
standard: document control with immutable approved revisions and a Master
Register, a real CAPA engine with structured RCA, a generic sequential approval
engine, change control, internal audit programme with frequency expansion,
competence with frozen assessment scores, calibration and maintenance, supplier
evaluation, complaint handling, and per-site isolation.

So the honest summary is: **this app can be the QMS layer that sits above a
GMP/food-safety execution system, but it is not that execution system.** The
prioritized list at the end of this document describes what building it would
mean, and it is a large multi-slice programme of work, not a set of fixes.

### Which of the three applies to a given site

- **ICH Q7** applies to a site manufacturing APIs or API intermediates. A
  formulation (finished dosage) site is under 21 CFR 211 / EU GMP Part I /
  Schedule M instead — much of the mapping below still reads the same, but the
  section numbering does not.
- **HACCP** applies to essentially any food business; it is the baseline
  framework nearly every national food law and every GFSI scheme (BRCGS, FSSC
  22000, IFS, SQF) builds on.
- **HARPC** applies to a facility required to register with FDA under section
  415 of the FD&C Act — including a **non-US facility exporting food to the
  US**. A site that has a Codex HACCP plan does *not* automatically satisfy
  HARPC: HARPC adds preventive controls beyond CCPs (sanitation controls,
  allergen controls, supply-chain controls), a mandatory written recall plan, a
  PCQI, and 3-yearly reanalysis.

### Relationship to 21 CFR Part 11

Every record these three standards require, if kept electronically at an
FDA-regulated site, is also a Part 11 record — batch records and CCP monitoring
logs are the archetypal predicate-rule records. So the Part 11 gaps (no
two-component signing, no reason-for-change, no audit-trail viewer, no complete
record export) apply to **everything this document proposes building**, not just
to what exists today. Build the modules below without closing
[cfr21-part11-mapping.md](cfr21-part11-mapping.md) G1–G4 first and you will have
built a compliant-looking system that fails inspection on the signatures.

---

## Part 1 — ICH Q7 (APIs)

### Sections 1–5: quality system, personnel, facilities, equipment

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 1.1–1.3 | Objective, regulatory applicability, scope; where GMP begins in the API process | — | **N/A — procedural** | Defining the API starting material and the point at which GMP applies is a regulatory determination recorded in the site's Quality Manual. It could live as a controlled `Document` (§10), but there is no field anywhere that asserts it. |
| 2.1 | Quality management principles; a documented quality policy and quality system | `Document`/`DocumentVersion` (§10); Organization Profile (§4.1) | **Covered** | The quality manual and policy are ordinary controlled documents with sequential approval, effective revisions, and controlled distribution. |
| 2.2 | **Responsibilities of the Quality Unit(s)** — independent of production; releases/rejects materials, intermediates and APIs; reviews batch records; approves specifications, procedures, deviations, changes, suppliers | `Role::KEYS`; `Abilities::*`; `Approvals::Approve` `SelfApprovalError` | **Not covered** | There is **no Quality Unit role**. `Role::KEYS` has 21 roles and none of them is a quality unit / QA release authority; `capa_manager` is used as the "closest existing quality-process-owner fit" by three modules (§11.16, §11.17, §11.18) precisely because no such role exists. The independence Q7 §2.2 demands is structurally supported — the approval engine refuses self-approval and the permit matrix refuses to auto-approve when unconfigured — but there is nothing to *release a batch*, because there is no batch. |
| 2.3 | Responsibility for production activities | `Department`/`DepartmentMembership` department heads (§4.1) | **Partial** | Departmental accountability exists and drives most ability rules. Production-specific responsibilities (in-process control, yield reconciliation) have no home. |
| 2.4 | **Internal audits (self inspection)** — scheduled, documented, findings and corrective actions followed up | `AuditProgram`/`AuditProgramEntry` (§6.0); `Audit`/`AuditChecklist` (§6); `Finding`/CAPA (§7) | **Covered** | The strongest single match in this document. A programme with a period, an owner, an approved schedule, per-entry **frequency** that expands into one dated audit per interval, standard/clause tagging, coverage %, and a close guard that refuses while any planned audit is neither opened nor cancelled-with-a-reason. Findings raised from checklist items run the full RCA → CAPA → effectiveness-verification loop. |
| 2.5 | **Product Quality Review (APQR)** — annual, per product: batch trends, OOS, deviations, changes, stability, returns/complaints/recalls, with a documented conclusion and corrective action | — | **Not covered** | Nothing aggregates by product because there is no product. `ManagementReviewMeeting` (§9) is an organization-level review on a 16-point agenda, not a per-product annual review. This is a genuine, distinct module. See gap **P10**. |
| 2.6–2.18 | Deviations investigated and documented; quality-critical deviations investigated with conclusions | `Incident` (`incident_kind: quality_incident`) (§11.4); `Finding` (§7); `CapaCase` (§7) | **Partial** | The investigation machinery is real and good — structured fishbone/5-Whys RCA with draft-then-approve, CAPA plans, effectiveness checks. What is missing is the **deviation record itself as a distinct object**: a planned/unplanned deviation raised against a specific batch, step, and specification, with a quality-unit impact assessment on that batch's disposition. Today a deviation is either a `quality_incident` or a `Finding`, neither of which can name the batch it affected. See gap **P9**. |
| 3.1 | Personnel qualifications — adequate education, training, experience; training records kept | `CompetencyRequirement`/`TrainingSession`/`TrainingAttendance`/`AssessmentQuestion`/`AssessmentAttempt` (§11.5) | **Covered** | Evidenced rather than asserted: an attendance register with self check-in and trainer validation, an optional MCQ whose score is frozen at submission, and a certificate that requires validated presence plus a pass. Note the limitation recorded in §11.15 Slice 7 — `CompetencyRequirement` matches `TrainingSession` by free-text string, with no FK, so "does this person hold competency X" is not machine-answerable. |
| 3.2 | **Personnel hygiene** — clean clothing, gowning, illness reporting and exclusion from production, restriction of persons with infectious disease or open lesions | `EmployeeMedicalProfile`/`OhcExamination`/`OhcVisit` (§11.19); `Visitor` (§11.21) | **Partial** | Pre-employment and periodic medical examinations, fitness certificates, and a clinic visit log all exist, and the module's confidentiality boundary (no department-scoped read for anyone) is correct. But there is **no fitness-to-work-in-a-production-area determination** — nothing links a medical finding to an exclusion from a specific area, no gowning qualification, no daily illness declaration. `ContractorMedicalClearance` computes a gate-pass status, which is the right *shape* for this and the obvious thing to generalize. See gap **P12**. |
| 3.3 | Consultants — records of name, address, qualifications, and service provided | `ExternalParty` (`party_type: other`) (§4.1) | **Partial** | An external party can be registered; there is no qualification evidence, no scope-of-service field, and no link from a consultant to the work they signed off. |
| 4.1–4.2, 4.5 | Buildings and facilities: design, construction, defined areas, utilities (HVAC, air handling, gases) | `Location` (§4.1); `Asset` (§11.7) | **Not covered** | `Location` is a flat master (name/code) with no cleanliness classification, no area type, and no containment attribute. HVAC as an `Asset` gets calibration/maintenance frequencies but no qualification state, no pressure-differential or particle-count monitoring. |
| 4.3 | **Water** — quality appropriate to use, monitored, with action limits and corrective action when exceeded | — | **Not covered** | No utility monitoring model of any kind. See gap **P13**. |
| 4.4 | Containment — dedicated areas or campaign production with cleaning validation for highly sensitizing materials (penicillins, cephalosporins, cytotoxics) | — | **Not covered** | Depends on P1 (product identity) and P6 (cleaning validation) both existing first. |
| 4.6 | Sewage, refuse, and waste disposal | `EnvironmentalAspect` (§11.9) | **Partial** | Waste is registrable as an environmental aspect with control measures and periodic review. That is the EMS view, not the GMP contamination-control view. |
| 4.7 | **Sanitation and maintenance** — written sanitation schedules, cleaning agents, pest control, executed records | `MaintenanceRecord` (§11.7); `FirstAidKitInspection` (§11.19) as a *pattern* only | **Not covered** | `MaintenanceRecord` covers equipment maintenance, not facility sanitation, and there is no pest-control record anywhere. `FirstAidKitInspection` (periodic inspection, due date, overdue report, daily reminder job) is exactly the right shape to copy. See gap **P5**. |
| 5.1 | Process equipment design and construction; product-contact surfaces non-reactive/additive/absorptive | `Asset` (§11.7) | **Partial** | An asset has a code, name, department, location, owner, calibration and maintenance frequencies, and a status. No material of construction, no product-contact flag, no criticality. |
| 5.2 | **Equipment maintenance and cleaning** — written procedures, cleaning between batches, "clean" status identification, equipment cleaning and use log | `MaintenanceRecord` (§11.7) | **Partial** | Preventive maintenance with due/completed dates, provider, result and evidence is real. **Cleaning is entirely absent**: no cleaning record, no clean/dirty status on `Asset`, no equipment use log, no changeover record. See gap **P5**. |
| 5.3 | **Calibration** — against traceable standards, at intervals, records retained, action when out of calibration including impact on batches produced since the last good calibration | `Asset`/`CalibrationRecord` (§11.7) | **Partial** | Due date, completed date, provider, `result` as a free string, next due date, certificate attachment, an overdue report and a daily reminder job — genuinely useful. Missing for Q7: **as-found / as-left readings**, acceptance tolerance, the reference standard used and its traceability, and — the one that actually gets cited — no impact assessment when an instrument is found out of calibration, because there are no batches to assess. |
| 5.4 | **Computerized systems** — validated, access-controlled, change-controlled, with audit trails and backup | See [cfr21-part11-mapping.md](cfr21-part11-mapping.md) | **Partial** | Covered in full detail by the Part 11 mapping. Short version: access control and sequencing are strong, validation and signing are not. |

### Sections 6–11: documentation, materials, production, packaging, distribution, laboratory

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 6.1 | **Documentation system and specifications** — all documents prepared, reviewed, approved, distributed and controlled; retention periods; specifications for materials, intermediates and APIs | `Document`/`DocumentVersion`/`DocumentDistribution`/`DocumentAcknowledgement`/`DocumentAssessment` (§10); Master Document Register (§10.3/§10.4) | **Partial — strong on control, absent on specifications** | Document control is production-grade: sequential approvals, immutable approved revisions, `file_checksum`, effective/current version, controlled distribution with acknowledgement, Read & Understood MCQ assessments pinned to one revision, a Master Register that gates release, and Control/Master Copy PDFs with watermarks. **But a specification is not a document** — Q7 §6.1 wants numerical acceptance criteria a test result can be compared against programmatically. There is no `Specification` model. See gap **P2**. Also: **no retention period exists anywhere in the schema** (Part 11 gap G2). |
| 6.2 | **Equipment cleaning and use record** — chronological, per equipment | — | **Not covered** | See §5.2 and gap **P5**. |
| 6.3 | **Records of raw materials, intermediates, labelling and packaging materials** — supplier, receipt, lot/batch, COA, test results, use | — | **Not covered** | No material master, no goods-receipt record, no COA. See gaps **P1**, **P7**, **P8**. |
| 6.4 | **Master Production Instructions (Master Batch Record)** — prepared, dated and signed by one person, independently checked/dated/signed by the quality unit; product name, code, complete list of materials with quantities, production location, equipment, detailed instructions, in-process controls with limits, yields with limits, packaging/labelling instructions | `Document`/`DocumentVersion` (§10) as a file only | **Not covered** | An MBR can be *uploaded* as a controlled document today — which is genuinely useful and is how many small sites operate — but it is an opaque `.docx`. Nothing in the schema knows the bill of materials, the process steps, the in-process limits, or the expected yield range, so nothing can be enforced or reconciled against them. See gap **P1**. |
| 6.5 | **Batch production records** — one per batch, prepared from the current MBR, with dates/times, equipment identity, material lots and quantities, in-process results, signatures of the performer and the checker for critical steps, sampling, deviations, yields | — | **Not covered — this is the single largest absence** | There is no batch record and nothing resembling one. Every other gap in this document is downstream of it. |
| 6.6 | **Laboratory control records** — sample description and source, test method, raw data, calculations, results against acceptance criteria, analyst and reviewer signatures | `OhcExaminationTest` (§11.19) — *unrelated*, clinical only | **Not covered** | The only structured test result in the app is a clinical one on an employee medical examination, and even there the value is free text. There is no QC lab module. Architecture.md §11.19 records that laboratory/diagnostics integration "was explicitly descoped by the site" — that decision was about the OHC clinic, and must not be read as covering QC. See gap **P2**. |
| 6.7 | **Batch production record review** by the quality unit before release; deviations investigated and closed first | — | **Not covered** | Depends on §6.5 existing. |
| 7.1 | Materials management: written procedures for receipt, identification, quarantine, storage, handling, sampling, testing, approval and rejection | — | **Not covered** | See gap **P7**. |
| 7.2 | **Receipt and quarantine** — containers examined for damage/labelling, held under quarantine until sampled/examined/tested and released | — | **Not covered** | |
| 7.3 | **Sampling and testing of incoming production materials** — at least one identity test per lot; supplier COA may substitute for full testing only where supplier reliability is established at appropriate intervals | `Supplier`/`SupplierEvaluation` (§11.6) | **Not covered** | The supplier record exists; the material-level testing and COA it would attach to does not. |
| 7.4 | **Storage** — conditions, segregation, FIFO/FEFO, periodic re-examination | `PharmacyStockBatch` (§11.19) as a *pattern* only | **Not covered** | The pharmacy module tracks batch/expiry for clinic medicines and is the only batch-with-expiry logic in the app. It is a first-aid inventory, not a warehouse. |
| 7.5 | **Re-evaluation** — materials re-evaluated to determine continued suitability | — | **Not covered** | |
| 8.1 | Production operations — materials weighed/measured under appropriate controls, critical weighing witnessed or subject to equivalent control, actual yields compared with expected at designated steps | — | **Not covered** | |
| 8.2 | Time limits between process steps where specified | — | **Not covered** | |
| 8.3 | **In-process sampling and controls** — procedures, sampling plans, results recorded, out-of-specification investigated | — | **Not covered** | This is the pharmaceutical equivalent of a HACCP CCP monitoring record. See gap **P3**. |
| 8.4 | Blending batches; blending out-of-specification batches to bring them into specification is not acceptable | — | **Not covered** | |
| 8.5 | Contamination control — carryover, dedicated equipment, campaign production | — | **Not covered** | |
| 9.1–9.2 | Packaging and containers; packaging materials examined and released | — | **Not covered** | |
| 9.3 | **Label issuance and control** — access restricted, reconciliation of issued/used/returned labels, obsolete labels destroyed with record, printing devices verified | — | **Not covered** | Label control is a discrete, commonly-cited GMP subsystem with no counterpart here. See gap **P11**. |
| 9.4 | Packaging and labelling operations — line clearance, verification of correct labels, records | — | **Not covered** | |
| 10.1 | **Warehousing** — quarantine/release status of stored materials, storage conditions | — | **Not covered** | |
| 10.2 | **Distribution** — batch traceability, records permitting recall, transport conditions | — | **Not covered** | Traceability is the enabling requirement for §15 recalls. See gap **P4**. |
| 11.1 | **Laboratory controls** — specifications, sampling plans, test procedures, OOS investigations, out-of-specification results documented and investigated | — | **Not covered** | See gap **P2**. |
| 11.2 | Testing of intermediates and APIs against specification | — | **Not covered** | |
| 11.3 | Validation of analytical procedures | — | **Not covered** | |
| 11.4 | **Certificates of Analysis** — authentic COA per batch, with specification and results, signed by an authorized person | — | **Not covered** | |
| 11.5 | **Stability monitoring** — ongoing programme, protocol, stability-indicating methods, containers representative of market | — | **Not covered** | |
| 11.6 | Expiry and retest dating | — | **Not covered** | |
| 11.7 | Reserve/retention samples — retained for defined periods, in the marketed container | — | **Not covered** | |

### Sections 12–19: validation, change control, rejection/reuse, complaints, contractors

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 12.1 | **Validation policy** — overall policy, intentions and approach documented and approved | `Document` (§10) | **Partial — procedural** | Holdable as a controlled document. Nothing structured. |
| 12.2 | Validation documentation — protocol specifying critical steps and acceptance criteria; report cross-referencing the protocol with results, deviations and conclusions | — | **Not covered** | |
| 12.3 | **Qualification** — DQ / IQ / OQ / PQ of critical equipment and systems, completed before process validation | `PssrReview` (§11.13) as a *near miss*; `Asset` (§11.7) | **Not covered** | `PssrReview` is structurally very close — a gated pre-startup walkdown over 12 standard checklist domains including "as-built construction vs. design", "mechanical integrity", "MOC closure" and "training", with an `AuthorizeStartup` guard that refuses while any item is still answered "No", per-item Findings, and paper-trailed response history. It is an OSHA PSM process-safety review, not an equipment qualification, and `Asset` carries no qualification state — but if equipment qualification is ever built, `PssrReview` is the model to copy, not `Audit`. See gap **P6**. |
| 12.4–12.5 | Approaches to process validation (prospective/concurrent/retrospective); validation programme covering critical parameters and number of batches | — | **Not covered** | |
| 12.6 | **Periodic review of validated systems** — evaluate whether they remain in a validated state | `MocRequest` (§11.8); `Document#next_review_date` (§10) | **Partial** | The review-date mechanism exists generically and is the right pattern; there is nothing validated for it to point at. |
| 12.7 | **Cleaning validation** — procedures, residue limits with scientific rationale, validated analytical methods, sampling | — | **Not covered** | See gaps **P5** and **P6**. |
| 12.8 | Validation of analytical methods | — | **Not covered** | |
| **13** | **Change control** — written procedure; classification by impact; evaluated by the quality unit; scientific judgement; additional testing/validation where warranted; affected documents revised; a classification of the change's impact on quality | **`MocRequest`/`MocImpactAssessment`/`MocRiskAssessment`/`MocChecklist`/`MocAction`/`MocVerification`** (§11.8) | **Covered — the second-strongest match in this document** | A change request carries `moc_type`, `nature`, `duration` (with a mandatory `temporary_expiry_date` and an expiry reminder job for temporary changes), a `risk_tier`, structured impact and risk assessments, checklists, affected records and documents, multi-approver sequential approval, implementation actions, and a status flow (`draft/assessment/approval/approved/readiness/implementing/verification/closed`) whose closure **requires verification**. What Q7 §13 additionally wants and this does not have: a **quality unit** as the mandatory reviewing authority (§2.2 — the role does not exist), and the batch/product impact determination, which again needs P1. |
| 14.1 | **Rejection** — rejected materials identified and controlled under quarantine | `NonconformingOutput` (§11.16) | **Partial** | The disposition engine is real: `use_as_is` requires a justification and goes through the generic `Approvable` sign-off; `rework`/`regrade`/`scrap`/`return_to_supplier`/`repair` apply immediately; `Verify` closes with an opt-in Finding raise. What is missing is the material itself — a rejected lot cannot be *quarantined* because there is no inventory status to set. |
| 14.2 | **Reprocessing** — introducing an intermediate/API back into the process and repeating a step; justified, investigated where it becomes routine | `NonconformingOutput#disposition` (§11.16) | **Not covered** | Q7 draws a hard, inspected line between **reprocessing** (§14.2, repeating a step in the established process) and **reworking** (§14.3, a *different* process step, requiring investigation and additional testing including an impurity profile comparison). The app has a single `rework` disposition covering neither properly. |
| 14.3 | **Reworking** — investigated, evaluated, additional testing, impurity profile comparison, batch record entry | `NonconformingOutput#disposition` (§11.16) | **Not covered** | As above. |
| 14.4 | Recovery of materials and solvents — approved procedures, records, suitability demonstrated | — | **Not covered** | |
| 14.5 | Returns — identified, quarantined, recorded with name, batch, reason, quantity, disposition | `NonconformingOutput` `return_to_supplier` (§11.16) | **Partial** | Outbound returns to a supplier are a disposition; **inbound customer returns** — the case §14.5 is actually about — have no record. |
| **15** | **Complaints and recalls** — complaint records with name, address, batch, response and investigation; **written recall procedure**; senior management and quality unit involved; regulatory authorities informed | `Incident` (`incident_kind: customer_complaint`) (§11.4); `Customer`/`CustomerFeedback` (§11.17); `IncidentNotification` (§11.4) | **Partial — complaints yes, recalls no** | A complaint is a real investigated record: an `Incident` with severity, confidentiality, triage, structured investigation, RCA, CAPA and external-notification tracking, cross-referenced from `CustomerFeedback` so a formally investigated complaint is not entered twice. But the complaint **cannot name the batch it concerns**, which is the first thing a complaint investigation asks. And there is **no recall capability at all** — no recall record, no distribution data to trace, no mock-recall exercise, no effectiveness check. See gap **P4**. |
| 16 | **Contract manufacturers and laboratories** — evaluated before contracting, written agreement defining GMP responsibilities, audited, no subcontracting without approval, changes notified | `Supplier`/`SupplierEvaluation` (§11.6); `ExternalParty` (§4.1) | **Partial** | A supplier carries `approval_status` (`pending/approved/suspended/rejected`), a `risk_rating`, free-text `categories`, and periodic evaluations scored on quality/delivery/service with a `next_review_date` and an opt-in Finding raise on a poor result. It cannot express: **what** the supplier is approved *for* (no material master to scope approval to), a quality agreement with its own review date, a supplier audit as a distinct evaluation type, or a contractor's own sub-tier. See gap **P8**. |
| 17 | Agents, brokers, traders, distributors, repackers, relabellers — traceability of the original manufacturer, transfer of quality information, COA handling, stability where repackaged | — | **Not covered** | Entirely dependent on P1/P4. |
| 18 | Specific guidance for APIs manufactured by cell culture / fermentation | — | **N/A unless applicable** | Would be built as extensions to the batch record (P1) and environmental monitoring (P13), not as a separate module. |
| 19 | APIs for use in clinical trials | — | **N/A unless applicable** | |

---

## Part 2 — Codex HACCP (CXC 1-1969)

### Chapter One — Good Hygiene Practices (the prerequisite programme)

HACCP is not standalone: Codex is explicit that GHPs must be in place before a
HACCP plan means anything, and most food-safety findings are GHP findings.

| Section | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 1 | Introduction and control of food hazards; management commitment; food safety culture | `ManagementReviewMeeting` (§9); `Document` (§10) | **Partial** | Management review with a 16-point agenda and trackable decisions/action items exists, but its agenda catalogue has no food-safety category and would need one. |
| 2 | **Primary production** — environmental hygiene, hygienic production, handling/storage/transport, cleaning and personnel | `Supplier`/`SupplierEvaluation` (§11.6) | **Partial** | Only reachable through supplier evaluation, which has no agricultural or primary-production criteria. |
| 3 | **Establishment: design of facilities and equipment** — layout, zoning, product-contact surfaces, temperature control equipment | `Location` (§4.1); `Asset` (§11.7) | **Not covered** | `Location` has no hygiene zone concept; `Asset` has no product-contact or material-of-construction attribute. |
| 4 | **Training and competence** — awareness, training programmes, refresher training, records | `CompetencyRequirement`/`TrainingSession`/`TrainingAttendance`/`AssessmentAttempt` (§11.5); `DocumentAssessment` (§10.5) | **Covered** | Directly applicable as built, including the frozen-score MCQ that makes a past training result defensible and the certificate that requires validated presence plus a pass. |
| 5 | **Establishment maintenance, cleaning and disinfection, and pest control** — cleaning programmes with what/when/how/who, monitoring of effectiveness, pest control systems, waste management | `MaintenanceRecord` (§11.7) | **Not covered** | Equipment maintenance only. No cleaning schedule, no sanitation record, no pest-control programme, no verification swabbing. This is the highest-frequency finding area in food audits and there is nothing here. See gap **P5**. |
| 6 | **Personal hygiene** — health status and illness reporting, personal cleanliness, behaviour, visitors | `EmployeeMedicalProfile`/`OhcExamination` (§11.19); `Visitor` (§11.21) | **Partial** | Same position as ICH Q7 §3.2. `Visitor` is unusually well-built for this — approval levels, configurable field definitions, a pass design — and adding a hygiene declaration to a visitor pass is a small, high-value change. See gap **P12**. |
| 7 | **Control of operation** — product/process description, key aspects of control (time/temperature, microbiological cross-contamination, allergen cross-contact, physical and chemical contamination), incoming materials, water, documentation, recall procedures | — | **Not covered** | This section is essentially the whole of gaps **P1**, **P3**, **P4**, **P7** and **P13**. |
| 8 | **Product information and consumer awareness** — lot identification, product labelling including allergen declaration | — | **Not covered** | See gaps **P1**, **P11**. |
| 9 | **Transportation** — protection from contamination, temperature control during transport | — | **Not covered** | |

### Chapter Two — the 12 application steps / 7 principles

| Step / Principle | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| Step 1 | **Assemble the HACCP team and identify scope** — multidisciplinary team, defined scope, product and process covered | `HazopParticipant` (§11.14) as a *pattern*; `AuditParticipant` (§6.1) | **Not covered** | `HazopStudy` already models a facilitated team study with a facilitator, scribe, team members and observers, a single-facilitator validation, invitation/attendance tracking, and a `scope` field for objective and boundaries. A HACCP team is the same shape. Reuse candidate, not existing coverage. |
| Step 2 | **Describe the product** — composition, structure, processing, packaging, storage and distribution conditions, shelf life, instructions for use | — | **Not covered** | Needs a product master. See gap **P1**. |
| Step 3 | **Identify intended use and users** — including vulnerable consumer groups | — | **Not covered** | |
| Step 4 | **Construct a flow diagram** — covering all steps in the operation for the specified product | — | **Not covered** | No process-step model. `HazopNode` (position, node reference, description, design intent) is structurally a process step and is the closest thing in the app. |
| Step 5 | **On-site confirmation of the flow diagram** — verified against actual operation, at all stages and hours | — | **Not covered** | |
| Step 6 / **Principle 1** | **List all potential hazards, conduct a hazard analysis, consider control measures** — biological, chemical (incl. allergens), physical; evaluate significance by likelihood and severity | `HazopStudy`/`HazopNode`/`HazopDeviation` (§11.14); `RiskMatrixLevel`; `RiskOpportunity` (§11.2) | **Not covered — but the engine exists** | `HazopDeviation` already carries causes, consequences, existing safeguards, a `likelihood` × `severity` ranking resolved through the admin-configurable `RiskMatrixLevel.for_axis` with a computed `score`, and a recommendation with an owner, due date and action status. That is a hazard analysis engine in everything but vocabulary — the 9 IEC 61882 guide words and 11 process parameters are wrong for food, but the structure (node → deviation → cause/consequence/safeguard → scored → recommendation) maps almost exactly onto (process step → hazard → source/effect/control measure → scored → CCP decision). See gap **P3**. |
| Step 7 / **Principle 2** | **Determine the Critical Control Points** — using a decision tree or equivalent, with the rationale recorded | — | **Not covered** | No CCP concept. |
| Step 8 / **Principle 3** | **Establish validated critical limits for each CCP** — measurable, with scientific validation | — | **Not covered** | Nothing in the app holds a numerical limit against which an operational reading is judged. The nearest analogue is `WorkPermitGasTest`, which holds gas-test limits and **blocks permit issue when they are exceeded** — a real, enforced limit check (§11.22). That is the pattern a CCP limit check should copy. |
| Step 9 / **Principle 4** | **Establish a monitoring system for each CCP** — what, how, when, by whom; records signed | `WorkPermitGasTest` (§11.22); `BbsChecklistTemplateVersion` (§11.15) as *patterns* | **Not covered** | Monitoring is a high-frequency, high-volume, timestamped operational record with an operator signature — the exact record type this app has never built. `BbsChecklistTemplateVersion`'s immutable-once-published versioning is the right mechanism for the monitoring form itself, so a historical record can never appear to have been taken against wording that did not exist yet. See gap **P3**. |
| Step 10 / **Principle 5** | **Establish corrective actions** — for each CCP, dealing with the deviation *and* the affected product, with records | `Finding`/`CapaPlan`/`CapaAction` (§7); `NonconformingOutput` (§11.16) | **Partial** | The corrective-action half is well covered — RCA, plan, actions, effectiveness verification. The **affected-product** half is not: a HACCP corrective action must state the disposition of everything produced since the last acceptable monitoring result, which requires batch identity and time-bounded production records. |
| Step 11 / **Principle 6** | **Validation of the HACCP plan, and verification procedures** — validate control measures before implementation, verify by review, calibration, testing, audit | `Audit`/`AuditProgram` (§6/§6.0); `Asset`/`CalibrationRecord` (§11.7) | **Partial** | Internal audit and calibration are real and directly usable as verification activities. Validation of control measures, product/environmental testing, and records review as a scheduled activity are not. |
| Step 12 / **Principle 7** | **Establish documentation and record keeping** — hazard analysis, CCP determination, critical limit determination, and the monitoring/deviation/verification records | `Document`/`DocumentVersion` (§10) | **Partial** | The HACCP *plan* is holdable as a controlled document today, with proper approval and revision control. The *records* — the daily monitoring logs the plan generates — are not, and they are what an inspector spends their time on. |

---

## Part 3 — HARPC / 21 CFR Part 117 (Preventive Controls for Human Food)

### Subpart B — Current Good Manufacturing Practice

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 117.10 | **Personnel** — disease control, cleanliness, hygienic practices | `EmployeeMedicalProfile`/`OhcExamination` (§11.19) | **Partial** | As ICH Q7 §3.2 / Codex §6. See gap **P12**. |
| 117.20 | Plant and grounds — maintenance, drainage, pest harborage, adequate space, separation of operations | `Location` (§4.1) | **Not covered** | |
| 117.35 | **Sanitary operations** — buildings and fixtures maintained sanitary; cleaning compounds; pest control; food-contact surface sanitation; storage of cleaning agents | — | **Not covered** | See gap **P5**. |
| 117.37 | Sanitary facilities and controls — water supply, plumbing, sewage, toilets, hand-washing, rubbish | — | **Not covered** | See gaps **P5**, **P13**. |
| 117.40 | Equipment and utensils — design permits cleaning, maintained, instruments adequate and accurate | `Asset`/`CalibrationRecord`/`MaintenanceRecord` (§11.7) | **Partial** | Calibration and maintenance are covered; hygienic design and cleanability are not attributes of `Asset`. |
| 117.80 | Processes and controls — quality control operations, raw material and finished food controls, contamination prevention | — | **Not covered** | |
| 117.93 | Warehousing and distribution — protection against contamination and deterioration | — | **Not covered** | |
| 117.110 | Defect action levels | — | **Not covered** | |

### Subpart C — Hazard Analysis and Risk-Based Preventive Controls

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 117.126 | **Food safety plan** — written, prepared by or overseen by a PCQI; contains the hazard analysis, preventive controls, supply-chain program, recall plan, and the monitoring/corrective action/verification procedures; signed and dated by a responsible individual on initial completion and after each modification | `Document`/`DocumentVersion` (§10) | **Partial** | The plan as a signed, revision-controlled document is well supported — sequential approval, immutable approved revisions, effective dating, and a `file_checksum`. The plan's *contents* as structured, queryable records are not. Note the signing requirement here intersects Part 11 gap G1 (no two-component signing, no signature meaning). |
| 117.130 | **Hazard analysis** — identify known or reasonably foreseeable biological, chemical (including radiological and food allergen) and physical hazards; evaluate severity of illness/injury and probability of occurrence; written, whether or not any hazard requires a preventive control | `HazopStudy`/`HazopDeviation`; `RiskMatrixLevel` (§11.14, §11.2) | **Not covered — engine exists** | Same position as Codex Principle 1. HARPC's severity × probability evaluation is exactly the `RiskMatrixLevel.for_axis` two-axis scoring already used by `RiskOpportunity`, `EnvironmentalAspect` and `HazopDeviation`. |
| 117.135 | **Preventive controls** — process, food allergen, sanitation, supply-chain, recall plan, and other controls, with parameters and values, written and subject to management components | — | **Not covered** | HARPC's four named control categories are broader than HACCP's CCPs. **Allergen controls and sanitation controls in particular have no home in this app at all.** See gaps **P3**, **P5**, **P11**. |
| 117.136 / 117.137 | Circumstances in which a preventive control is not required; provision of written assurances from the customer or another entity | — | **Not covered** | A written-assurance record with an expiry and a responsible customer entity. `Customer` (§11.17) exists as an identity to hang it on. |
| 117.139 | **Recall plan** — written; procedures for direct consignee notification, public notification, effectiveness checks, and appropriate disposition of the recalled food | — | **Not covered** | Mandatory whenever any hazard requires a preventive control. See gap **P4**. |
| 117.140 | **Preventive control management components** — monitoring, corrective actions and corrections, and verification, as appropriate to the control | — | **Not covered** | |
| 117.145 | **Monitoring** — written procedures, performed as appropriate, records with actual values/observations | — | **Not covered** | See gap **P3**. |
| 117.150 | **Corrective actions and corrections** — identify and correct the problem, reduce recurrence, evaluate affected food for safety, prevent adulterated food entering commerce; documented | `Finding`/`CapaPlan`/`CapaAction`/`CapaEffectivenessCheck` (§7); `NonconformingOutput` (§11.16) | **Partial** | The strongest partial in Subpart C. The CAPA machinery — structured RCA, plan approval, actions, effectiveness verification, segregation of duties on verification — genuinely satisfies "reduce the likelihood of recurrence". "Evaluate all affected food" cannot be satisfied without P1 and P4. |
| 117.155 | **Verification** — validation, verification that monitoring is being conducted, verification that appropriate decisions about corrective actions are being made, verification of implementation and effectiveness, reanalysis | `Audit`/`AuditProgram` (§6/§6.0) | **Partial** | Internal audit is a legitimate verification activity and is scheduled, evidenced and followed up properly. Records review as a distinct scheduled activity within 7 working days is not modelled. |
| 117.160 | **Validation** — scientific and technical evidence that a control is capable of controlling the hazard; within 90 calendar days of first production or a written justification for a longer timeframe; performed by or overseen by a PCQI | — | **Not covered** | |
| 117.165 | **Verification of implementation and effectiveness** — calibration or accuracy checks of monitoring and verification instruments, product testing, environmental monitoring, records review | `Asset`/`CalibrationRecord` (§11.7) | **Partial** | Instrument calibration is real, with due dates, an overdue report and reminders. **Product testing and environmental monitoring do not exist**, and environmental monitoring is required whenever a ready-to-eat food is exposed to the environment before packaging. See gaps **P2**, **P13**. |
| 117.170 | **Reanalysis** — of the food safety plan at least once every 3 years, and whenever a significant change, new hazard, unanticipated food safety problem, or ineffective control occurs | `Document#next_review_date`/`review_frequency` (§10); `MocRequest` (§11.8) | **Partial** | The periodic document review mechanism, with reminder jobs, covers the 3-yearly clock adequately if the plan is a controlled document. The **event-driven** triggers — a significant change routed from `MocRequest`, an ineffective control routed from a CAPA effectiveness check — are exactly the kind of cross-module trigger this app already builds elsewhere (`Documents::MasterRegisterUpdateCheckJob` is the precedent) and would be cheap to wire once a plan record exists. |
| 117.180 | **Preventive Controls Qualified Individual** and **qualified auditor** — successful completion of standardized curriculum or job experience sufficient to perform the activities; records of training required | `Role::KEYS`; `CompetencyRequirement`/`TrainingAttendance` (§11.5) | **Not covered** | No PCQI role, and — as recorded in §11.15 Slice 7 — no mechanism anywhere in the app gates a role assignment on holding a competency. This is the same gap Part 11 §11.10(i) raises (gap G9 there) and should be built once for both. |
| 117.190 | **Implementation records** — monitoring, corrective action, verification, supply-chain program records | — | **Not covered** | |

### Subparts F and G

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 117.305 | General requirements applying to records — actual values and observations, sufficient detail, created concurrently, accurate/indelible/legible, identify the plant, date and time of the activity, signed or initialed by the person performing it, and where appropriate the identity of the product and lot code | PaperTrail; `ApprovalStep`; Devise/CanCanCan | **Partial** | "Created concurrently" and "signed by the person performing the activity" is exactly Part 11 §11.50 territory and fails there for the same reasons — see gap G1 in [cfr21-part11-mapping.md](cfr21-part11-mapping.md). "Identity of the product and lot code" needs P1. |
| 117.310 | Food safety plan signed and dated by a responsible individual upon initial completion and after each modification | `DocumentVersion#approved_at`; `ApprovalStep` (§5.2/§10.2) | **Partial** | The mechanism is right; the signature lacks a stated meaning and a second identification component (Part 11 G1). |
| 117.315 | **Record retention** — at least 2 years after creation; records supporting the food safety plan retained at least 2 years after their use is discontinued | — | **Not covered** | **There is no retention period concept anywhere in the schema**, and 44 controllers expose `destroy` with `dependent: :destroy` cascades. This is Part 11 gap G2 and it is a HARPC gap for the same reason. |
| 117.320 | Records available for official review and copying within 24 hours | CSV export (§15); report PDFs (§15.3) | **Partial** | Same limitation as Part 11 §11.10(b): no complete-record export bundling a record with its audit trail, signatures and attachments. Part 11 gap G3. |
| 117.330 | Existing records need not be duplicated | — | **N/A** | An organizational decision, not a system one. |
| 117.405–117.410 | **Supply-chain program** — written, for each raw material or other ingredient for which the receiving facility has identified a supply-chain-applied control | `Supplier`/`SupplierEvaluation` (§11.6) | **Not covered** | The supply-chain program is organised **per material**, not per supplier. This app has suppliers but no materials, so the program's primary key does not exist. |
| 117.415 | Responsibilities of the receiving facility — approving suppliers, determining and conducting verification activities, documenting them | `Supplier#approval_status` (§11.6) | **Partial** | `approval_status` (`pending/approved/suspended/rejected`) is a plain editable field, not an approval workflow — there is no `Approvable` on `Supplier`, no approval evidence, and no record of who approved on what basis. Compare `NonconformingOutput`, which routes its one consequential decision through the generic approval engine; `Supplier` does not. |
| 117.420 | **Using approved suppliers** — receive raw materials only from approved suppliers, or on a temporary basis from unapproved suppliers subject to adequate verification before use | — | **Not covered** | Nothing prevents anything, because nothing is received. |
| 117.425–117.430 | **Determining and conducting supplier verification activities** — onsite audit, sampling and testing, review of food safety records, or other; frequency appropriate to the hazard; **an onsite audit is required annually where the hazard is one for which there is a reasonable probability of serious adverse health consequences or death (SAHCODHA)** | `SupplierEvaluation` (§11.6) | **Partial** | A periodic scored evaluation with a `next_review_date` and an opt-in Finding raise is real and useful, but it is one undifferentiated evaluation type. HARPC needs the *kind* of verification activity recorded (audit / sampling / records review), tied to the material and its hazard, with the SAHCODHA annual-audit rule enforceable. See gap **P8**. |
| 117.435 | **Onsite audit** — performed by a qualified auditor; report retained; may be substituted by inspection results from FDA or an officially recognized authority | `Audit` (§6) | **Partial** | The audit engine can genuinely run a supplier audit — it has team roles including lead auditor, external `AuditParticipant`s with company details, checklist templates, evidence, a report workflow and a report PDF. It is not currently pointed at `Supplier` as an auditee, and "qualified auditor" is unenforced for the same reason PCQI is. |
| 117.475 | Records documenting the supply-chain program | `SupplierEvaluation` + attachments (§11.6) | **Partial** | Evidence attaches; the structure it should attach to does not exist. |

---

## What we are missing — prioritized

Ordered by what blocks what, not by effort. **P1 blocks most of the rest**, and
nothing below it should be started before it. The Standard column marks which of
the three frameworks each gap belongs to.

### P1 — Product, material, and batch identity
**ICH Q7 §6.3–6.7, §8; Codex Steps 2/4; 21 CFR 117.305, Subpart G · Blocking, all three**

The application has no object representing a thing that is made, received, or
shipped. Everything else in this document is downstream of that.

- A **material master** (raw material, packaging material, intermediate,
  finished product) with a specification reference, hazard attributes
  (allergen, sensitizing, hazardous), storage conditions, and shelf life.
- A **lot/batch** record with a unique identifier, quantity, status
  (`quarantine` / `released` / `rejected` / `on hold` / `consumed`), manufacture
  and expiry/retest dates, and the material it instantiates.
- **Genealogy** — which input lots went into which output lot. This is the
  single field that makes recall and traceability possible; without it a mock
  recall cannot be run.
- A **process route / step** model. `HazopNode` (position, reference,
  description, design intent) is the structural precedent already in the repo.
- Retrofit `NonconformingOutput#batch_or_lot_number` and `#item_description`
  (free text today, by explicit design decision recorded in §11.16) onto real
  associations, and add a batch reference to `Incident` so a complaint can name
  the lot it concerns.
- Every new model joins `SiteScopable::REGISTRY` (CLAUDE.md is unambiguous here,
  and `spec/abilities/site_isolation_spec.rb` will fail otherwise) and declares
  a retention behaviour (see Part 11 gap G2).

### P2 — Specifications, sampling, and test results
**ICH Q7 §6.1, §6.6, §11; Codex Principle 3; 21 CFR 117.165 · Blocking, all three**

A specification in this app can only be an uploaded file. Nothing can compare a
result to a limit.

- `Specification` with versioned, approved, numerical acceptance criteria per
  material and test — approved through the existing generic approval engine and
  made immutable once effective, exactly as `DocumentVersion` already is.
- `Sample` (source, point, plan, who took it, when) and `TestResult` (method,
  analyst, raw value, unit, pass/fail against the specification revision **in
  force at the time of test**, reviewer).
- An **out-of-specification / out-of-trend investigation** that routes into the
  existing `CapaCase` engine rather than duplicating it.
- Certificates of Analysis, per batch, generated not typed.
- The pattern to copy is `AssessmentQuestion`/`AssessmentAttempt`: grade once at
  submission and store the outcome, so amending the specification later can
  never retroactively move a past result (§11.5.2 makes exactly this argument
  for assessments, and it is the same argument).

### P3 — Hazard analysis, CCPs / preventive controls, and monitoring
**ICH Q7 §8.3; Codex Principles 1–4; 21 CFR 117.130–117.145 · Blocking, food; high, pharma**

The engine exists and is pointed at the wrong domain.

- Generalize the `HazopStudy` structure — team with roles, scope, nodes,
  deviations with causes/consequences/safeguards, `RiskMatrixLevel` severity ×
  likelihood scoring, recommendation with owner and due date — into a
  **hazard analysis** whose vocabulary is configurable, so a food hazard
  analysis and a HAZOP are two configurations of one engine rather than two
  modules. The IEC 61882 guide words become one of several guide-word sets.
- A **CCP / preventive control** record: control category (process, allergen,
  sanitation, supply-chain, other), the step it sits on, its validated critical
  limit with units, and the rationale for the CCP determination.
- A **monitoring record**: high-volume, timestamped, operator-attributed, taken
  against a versioned monitoring form. Copy `BbsChecklistTemplateVersion`'s
  immutable-once-published versioning so a historical reading can never appear
  to have been taken against a form that did not exist yet, and copy
  `WorkPermitGasTest`'s enforced-limit behaviour — a reading outside the limit
  must *block* and raise, not merely record.
- A deviation from a critical limit auto-raises through the existing
  `Findings::RaiseFromSource` path (the same opt-in mechanism
  `ComplianceEvaluation`, `SupplierEvaluation`, `PssrChecklistItem`,
  `BbsObservation` and `NonconformingOutput` already use) — except here it must
  be **mandatory, not opt-in**, and must also place the affected production on
  hold.

### P4 — Recall, traceability, and market action
**ICH Q7 §10.2, §15; Codex §7; 21 CFR 117.139 · Blocking, all three**

There is no recall capability of any kind, and HARPC makes a written recall plan
mandatory whenever any hazard requires a preventive control.

- A **recall plan** as a controlled document with mandatory acknowledgement.
- A **recall/market-action record**: classification, decision authority,
  affected lots resolved from P1 genealogy, direct consignee notification with
  responses, public notification, effectiveness checks against a target
  percentage, disposition of returned product, and regulatory notification —
  reusing `IncidentNotification`'s existing external-notification shape.
- **Mock recall exercises** with a measured traceability time, which is what
  every GFSI scheme and most regulators actually test.
- Forward traceability (customer, quantity, date shipped) and backward
  traceability (supplier lot) — both fall out of P1 if it is built with
  genealogy from the start, and are near-impossible to retrofit if it is not.

### P5 — Sanitation, cleaning, and pest control
**ICH Q7 §4.7, §5.2, §6.2, §12.7; Codex §5; 21 CFR 117.35, 117.135 · High, all three**

The highest-frequency finding area in food audits, and there is nothing here.

- A **sanitation schedule** (area or equipment, method, agent, frequency,
  responsible role) and an executed **cleaning record** with verification.
- Clean/dirty status on `Asset`, plus an **equipment cleaning and use log** —
  ICH Q7 §6.2 asks for it by name.
- **Pest control**: contractor, device map, inspection findings, trend, and
  corrective action.
- **Cleaning validation**: residue limits with rationale, sampling locations,
  results against limits (which needs P2).
- Copy `FirstAidKitInspection` wholesale for the periodic-inspection half — it
  already has the due-date, overdue-report and daily-reminder-job shape, and
  reproducing it badly would be worse than reusing it.

### P6 — Equipment qualification and process validation
**ICH Q7 §12; 21 CFR 117.160 · High, pharma; medium, food**

- A **qualification** record per critical asset: DQ/IQ/OQ/PQ protocol and
  report, acceptance criteria, executed evidence, approval, and a qualification
  state on `Asset` that other modules can read.
- **Process validation**: protocol, defined critical process parameters, the
  batches executed against it, the report and its conclusion, and a periodic
  revalidation review.
- Copy `PssrReview` (§11.13), not `Audit`. It already has the right shape: a
  categorized checklist, an authorization gate that refuses while any item is
  answered "No", per-item Findings that must resolve before the item can move,
  and paper-trailed response history with an actor. That is a qualification
  protocol in all but name.
- Note the overlap with Part 11 gap G7 — the *computer system* validation
  package and the *equipment/process* validation records are different things
  and should not be conflated, but both are missing and both are mostly
  documentation.

### P7 — Materials management: receipt, quarantine, release, storage
**ICH Q7 §7, §10.1; Codex §7; 21 CFR 117.80, 117.93 · High, all three**

- Goods receipt against a purchase reference, with container examination.
- **Quarantine by default**, with release only by the quality unit (P14) after
  sampling and testing (P2) — enforced in a transactional service with an
  explicit guard, matching the `Approvals::Approve` / `WorkPermits::Issue`
  precedent, never in a callback.
- Storage conditions, segregation, FIFO/FEFO, re-evaluation/retest dates with a
  reminder job, and expiry blocking.
- Extend `NonconformingOutput`'s disposition vocabulary to distinguish
  **reprocessing** from **reworking** (ICH Q7 §14.2 vs §14.3 — different
  regulatory obligations, and a single `rework` value satisfies neither), and
  add an inbound customer-returns record (§14.5).

### P8 — Supplier approval per material and supply-chain verification
**ICH Q7 §7.3, §16, §17; 21 CFR 117.405–117.475 · High, all three**

`Supplier` is org-wide, unversioned in its approval decision, and scoped to
nothing.

- Approval **per supplier × material**, not per supplier, with a status, an
  effective date, and the basis for approval.
- Route the approval decision through the existing `Approvable` engine so there
  is real evidence of who approved and why — today `approval_status` is a plain
  editable string field.
- Verification activity as a typed record (onsite audit / sampling and testing /
  records review / other) with a frequency driven by the material's hazard, and
  the **SAHCODHA annual onsite audit rule** enforceable rather than advisory.
- Point the existing `Audit` engine at `Supplier` as an auditee — it already
  supports external participants with company details, checklist templates,
  evidence and a report PDF, so this is wiring, not a new module.
- **Quality/food-safety agreements** with their own review dates, and
  notification requirements for supplier-side changes.

### P9 — Deviation as a first-class record, and quality-unit disposition
**ICH Q7 §2.16–2.18, §6.7; Codex Principle 5; 21 CFR 117.150 · High, all three**

A deviation today is either a `quality_incident` `Incident` or a `Finding`.
Neither can name the batch it affected, and neither carries a batch-impact
determination.

- `Deviation`: planned/unplanned, the batch/step/specification it departs from,
  immediate action, quality-unit impact assessment on the affected batch, and
  disposition — feeding the existing `CapaCase` engine for the systemic half
  rather than duplicating RCA.
- A **hold/release** action on a batch, exercisable independently of the
  deviation's own closure.
- The existing `CapaLink` allowlist pattern (`SOURCE_TYPES`, checked before any
  `constantize`) is the right way to relate deviations, batches, NCRs and
  complaints without inventing a new linking mechanism.

### P10 — Product Quality Review, HACCP plan review, and reanalysis
**ICH Q7 §2.5; Codex Principle 6; 21 CFR 117.170 · Medium, all three**

- An **annual Product Quality Review** per product aggregating batch results,
  OOS, deviations, changes, stability, returns, complaints and recalls, with a
  documented conclusion and resulting actions. Every input is a query over
  P1–P9; the module itself is thin once they exist.
- **Reanalysis triggers**: the 3-yearly clock is already served by
  `Document#review_frequency`/`next_review_date` and its reminder job, but the
  event-driven triggers (a significant `MocRequest`, a failed
  `CapaEffectivenessCheck`, a new hazard) need wiring. `Documents::MasterRegisterUpdateCheckJob`
  — enqueued unconditionally after every publish and a safe no-op when
  irrelevant — is the precedent to copy.

### P11 — Labelling, packaging, and allergen control
**ICH Q7 §9; Codex §8; 21 CFR 117.135 · Medium, pharma; high, food**

- Label master with approved artwork under document control, issuance and
  **reconciliation** (issued / used / returned / destroyed), and line clearance.
- **Allergen management**: allergen attributes on the material master,
  cross-contact risk assessment, production sequencing rules, changeover
  cleaning verification, and label declaration verification. Allergens are one
  of HARPC's four named preventive control categories and have no representation
  in this app whatsoever.

### P12 — Personnel hygiene, fitness for production areas, and visitor control
**ICH Q7 §3.2; Codex §6; 21 CFR 117.10 · Medium, all three**

Mostly wiring of things that already exist, which makes it the best value on
this list.

- A **fitness-to-work-in-area** determination on the OHC side, linking a medical
  outcome to permission to enter a defined area — generalize
  `ContractorMedicalClearance`'s computed gate-pass status, which already does
  exactly this for contract labour and already has an expiry reminder job.
- An **illness/injury declaration** with an exclusion decision and a return-to-
  work clearance, respecting the OHC module's confidentiality boundary (no
  department-scoped read for anyone — architecture.md §11.19 Slice 1 is explicit
  that a department head must never see their reports' medical records, and a
  hygiene exclusion must not become a backdoor to that).
- A **hygiene declaration and gowning requirement on the visitor pass** —
  `Visitor` already has configurable `VisitorFieldDefinition`s, approval levels
  and a pass design, so this is configuration plus a gate, not a new module.

### P13 — Utilities, water, and environmental monitoring
**ICH Q7 §4.2–4.3; Codex §7; 21 CFR 117.37, 117.165 · Medium, all three**

- Water and utility (compressed air, steam, gases) monitoring with sampling
  points, action and alert limits, results, and trend.
- **Environmental monitoring** — zones, sampling points, organisms, action
  limits, trend, and an investigation on exceedance. Required under 117.165
  whenever a ready-to-eat food is exposed to the environment before packaging,
  and increasingly expected in sterile and biological API manufacture.
- Both are P2's `Sample`/`TestResult` engine pointed at a location instead of a
  batch, so build P2 first and this is largely configuration.

### P14 — The Quality Unit, PCQI, and HACCP team leader roles
**ICH Q7 §2.2; Codex Step 1; 21 CFR 117.180 · Medium, cheap, all three**

`Role::KEYS` has 21 roles and none of them is a quality release authority — the
reason three separate modules (§11.16, §11.17, §11.18) all record `capa_manager`
as "the closest existing quality-process-owner fit".

- Add `quality_unit` (release/reject authority, independent of production),
  `pcqi`, and a HACCP team leader designation.
- **Gate the role assignment on holding the required competency.** The
  competency machinery exists; nothing anywhere in this app uses it to gate
  anything (§11.15 Slice 7 records this explicitly). This is the same gap as
  Part 11 §11.10(i)/G9 and should be built once to serve both.
- Enforce quality-unit review as a mandatory approval stage on batch release,
  specification approval, deviation closure, change control (ICH Q7 §13 requires
  it by name), and supplier approval — using the existing sequential approval
  engine, whose `SelfApprovalError` and step-position guard already give the
  independence these standards demand.

### P15 — Records rule: retention, concurrency, and complete export
**ICH Q7 §6.1; 21 CFR 117.305–117.320 · Blocking, but tracked under Part 11**

Not restated in detail here because it is the same work as
[cfr21-part11-mapping.md](cfr21-part11-mapping.md) gaps **G1** (two-component
signing and signature meaning), **G2** (retention and soft delete), **G3**
(complete record export), and **G4** (audit trail review and reason for change).
It is listed so nobody plans P1–P14 as if the record controls were already in
place. **Every record P1–P14 creates is a predicate-rule record**, and 117.305's
"signed or initialed by the person performing the activity, created
concurrently" is a signature requirement, not a timestamp requirement.

---

## What already exists and is directly reusable

Stated explicitly, because the gap list above is long enough to read as "nothing
is here", and that would be wrong. These are built, tested, and would be
genuinely expensive to rebuild:

| Need | What already serves it |
|---|---|
| Document control for the quality manual, SOPs, HACCP plan, food safety plan | `Document`/`DocumentVersion` (§10) — sequential approvals, immutable approved revisions, `file_checksum`, effective dating, Master Register with release gating, controlled distribution, acknowledgement, Read & Understood MCQ assessments pinned to one revision, Control/Master Copy PDFs |
| Corrective and preventive action | `Finding`/`RootCauseAnalysis` (fishbone + 5-Whys, draft-then-approve)/`CapaCase`/`CapaPlan`/`CapaAction`/`CapaEffectivenessCheck` (§7) |
| Change control | `MocRequest` with impact/risk assessments, checklists, multi-approver sequential approval, temporary changes with expiry reminders, and verification-gated closure (§11.8) |
| Internal audit / self-inspection / supplier audit | `AuditProgram` with frequency expansion and coverage %, `Audit` with team roles, checklists, evidence and report PDFs (§6.0, §6) |
| Training and competence | `CompetencyRequirement`/`TrainingSession`/`TrainingAttendance` with validated presence, frozen MCQ scores, and auto-issued certificates (§11.5) |
| Calibration and maintenance | `Asset`/`CalibrationRecord`/`MaintenanceRecord` with overdue reports and reminder jobs (§11.7) |
| Complaint handling | `Incident` (`customer_complaint`) with triage, structured investigation, RCA, CAPA and external notification; `CustomerFeedback` (§11.4, §11.17) |
| Nonconformance disposition with sign-off | `NonconformingOutput` with approval-gated `use_as_is` concession (§11.16) |
| Two-axis risk scoring with configurable axes | `RiskMatrixLevel`, used by `RiskOpportunity`, `EnvironmentalAspect` and `HazopDeviation` |
| Team-based facilitated hazard study | `HazopStudy`/`HazopNode`/`HazopDeviation` (§11.14) |
| Gated pre-startup verification with a refuse-on-"No" guard | `PssrReview` (§11.13) |
| Immutable versioned operational checklist | `BbsChecklistTemplateVersion` (§11.15 Slice 2) |
| Enforced numerical limit that blocks an operation | `WorkPermitGasTest` + `WorkPermits::Issue` (§11.22) |
| Periodic inspection with overdue report and reminder job | `FirstAidKitInspection` (§11.19 Slice 5) |
| Regulatory obligation register with periodic evaluation | `ComplianceObligation`/`ComplianceEvaluation` (§11.3) |
| Sequential approval with segregation of duties | `Approvals::Submit`/`Approve` with `SelfApprovalError` and step-position guard (§5.2) |
| Reference numbering, site isolation, notifications, comments, attachments | `ReferenceNumberService`, `SiteScopable::REGISTRY`, `Notifications::Create`, `Commentable`, `Attachable` |

---

## A note for Indian pharmaceutical and food manufacturers

An Indian site does not become subject to ICH Q7 or HARPC by being in India — it
becomes subject when its product reaches a market that imposes them. The
domestic equivalents apply regardless, and should be read alongside:

- **Schedule M, Drugs and Cosmetics Rules 1945** (revised 2024, phased
  enforcement) is the domestic GMP requirement, and its Part I now carries
  explicit expectations on product quality review, quality risk management,
  computerised systems and data integrity that closely track ICH Q7 and EU GMP.
  Every gap P1–P9 above is also a Schedule M gap. **Schedule M applies to
  formulations under the revised structure; APIs are Part II** — a site making
  both is under both.
- **CDSCO** and state FDA inspections increasingly mirror FDA 483 observations
  on data integrity and batch record review, which puts the Part 11 gaps
  (G1–G4) squarely in domestic scope too.
- **FSS Act 2006 and the Food Safety and Standards (Licensing and Registration
  of Food Businesses) Regulations 2011, Schedule 4** is the domestic GHP/HACCP
  requirement — Part II onwards imposes a Codex-derived hygiene and HACCP
  regime, and FSSAI's own audit scheme checks against it. Gaps P3, P5, P11 and
  P12 are the ones Schedule 4 audits find.
- **FSSAI (Food Recall) Regulations 2017** make a documented recall plan and a
  recall committee a licence condition for many food businesses, independent of
  any export requirement. That makes **P4 a domestic compliance gap, not only a
  HARPC one.**
- **FoSTaC** (Food Safety Training and Certification) requires a certified food
  safety supervisor at a defined ratio per shift — which is the same
  role-gated-on-competency mechanism gap **P14** describes, and which the
  existing `CompetencyRequirement`/`TrainingAttendance` machinery could evidence
  once role assignment is actually gated.
- **BIS / Legal Metrology** obligations on declarations and net-quantity
  verification intersect gap **P11** (labelling) and are already registrable as
  `ComplianceObligation` rows today.
- **GFSI schemes** (BRCGS, FSSC 22000, IFS, SQF) all require HACCP plus a
  substantial prerequisite programme, and are what an export customer will audit
  against rather than Codex directly. FSSC 22000 in particular layers ISO 22000
  (which this app's ISO machinery partly serves) over ISO/TS 22002-1
  prerequisites (which it does not).

As in the other mapping documents: none of this is legal or regulatory advice.
It is a description of where in the app a regulatory assessment's outputs get
configured, and — mostly, here — where the app currently cannot hold them.

---

## What an ICH Q7, HACCP, or HARPC assessment will not find here

Stated so nobody goes looking: there is **no** ERP, no inventory or warehouse
management, no production planning or execution, no shop-floor data collection,
no LIMS, no instrument or SCADA interface, no electronic batch record, no
serialization or track-and-trace, and no labelling system. Nothing in
`ModuleFlag::CATALOG`'s 31 modules addresses any of them.

This is a management-system application. The realistic architecture for a
regulated site is that a GMP/food-safety execution system owns the product,
batch and test data, and **this app owns the quality system around it** —
documents, CAPA, change control, audits, training, calibration, complaints,
supplier evaluation — with defined integration points at deviation, CAPA, change
control and complaint. Building P1–P15 would mean deciding, deliberately, to
become the execution system too. That is a legitimate choice; it is not a small
one, and it should not be arrived at by accident one module at a time.

---

## Keeping this document current

Update this file whenever a module changes its posture against any of the three
frameworks — a new model that holds product, batch, specification or monitoring
data, a new role, a gap from the list above getting built, or a new reusable
pattern worth pointing the "directly reusable" table at. Treat a stale row here
the same as a stale line in `architecture.md`: fix it in the same change that
touches the code.

When a gap is closed, move it from the prioritized list into the tables above
with its status changed, and record the change. A gap analysis that quietly
deletes its own history is worth less than one that shows it.
