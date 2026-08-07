# ISO/IEC 17025:2017 — Testing & Calibration Laboratory Mapping

**Last reviewed: 2026-08-06 — update this alongside any new module.**

## Scope and how to read this document

This document maps **ISO/IEC 17025:2017**, *General requirements for the
competence, testing and calibration laboratories*, to the application module,
model, or service that supports it.

It is separate from [iso-standards-mapping.md](iso-standards-mapping.md) — which
covers ISO 9001, 14001 and 45001 — for one reason: those three are management
system standards a whole organization certifies to, while 17025 is an
**accreditation** standard for a *laboratory*, and roughly half of it is
technical requirements about measurement that have no counterpart in a
management system at all.

### The one structural fact that determines everything below

**ISO 17025 splits cleanly in two, and this app sits on opposite sides of the
split.**

- **Clause 8 — Management system requirements.** Clause 8.1.2 "Option A" lists
  documentation, document control, records, risks and opportunities,
  improvement, corrective action, internal audit and management review. This is
  ISO 9001 in miniature, and it is **what this application already is.** Nearly
  every Clause 8 row below is Covered.
- **Clauses 6 and 7 — Resource and process requirements.** Equipment,
  metrological traceability, methods, sampling, handling of items, technical
  records, measurement uncertainty, validity of results, and reporting. This is
  **a laboratory**, and this application does not have one. Nearly every Clause
  7 row below is Not covered.

Stated plainly: **a laboratory could run its Clause 8 management system on this
app today and would need a different system for Clauses 6 and 7.** That is a
legitimate and common split — most accredited labs run a LIMS alongside a QMS —
but it should be stated before anyone plans otherwise.

### Who this actually applies to here

Two realistic cases, and they need different amounts of work:

1. **An in-house calibration laboratory** at a manufacturing site seeking
   accreditation for its own instruments. This is the more likely case for a
   site already running this app, and it is the cheaper one: the scope is
   narrow, the customer is internal, and `Asset`/`CalibrationRecord` (§11.7) is
   already the right shape. Gaps **L1** and **L2** are most of the work.
2. **A commercial testing or calibration laboratory** offering services to
   external customers. This needs the whole of Clause 7 — contract review,
   methods, sampling, item handling, uncertainty, reporting, proficiency
   testing — and is a full LIMS build. See the closing note.

### Relationship to the other mapping documents

- The **calibration gaps here are the same gaps** the
  [ICH Q7 / HACCP / HARPC mapping](ich-q7-haccp-harpc-mapping.md) records under
  P2 (specifications, sampling, test results) and P6. `CalibrationRecord` is
  criticized in both documents for the same missing columns, and the fix is one
  piece of work, not two.
- **Clause 7.11** (control of data and information management) is entirely the
  [21 CFR Part 11 mapping](cfr21-part11-mapping.md). A LIMS supporting an
  accredited lab is a computerised system, and Annex 11 / PIC/S PI 041 apply to
  it the same way.
- **Clause 8** overlaps almost completely with the ISO 9001 column of
  [iso-standards-mapping.md](iso-standards-mapping.md).

---

## Clause 4 — General requirements

| Clause | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 4.1 | **Impartiality** — laboratory activities undertaken impartially; the structure and management safeguard impartiality; risks to impartiality **identified on an ongoing basis**, including those arising from relationships, and eliminated or minimized | `RiskOpportunity` `impartiality` domain (§11.2); **`InterestDeclaration`** + `Impartiality::ReviewDeclaration`; `Approvals::Approve` `SelfApprovalError` (§5.2) | **Covered** | `RiskOpportunity#domain` now carries `impartiality`, so an impartiality risk is scored, treated, owned and reviewed by the same machinery as every other risk rather than a parallel one. The ongoing-identification input it was missing is **`InterestDeclaration`**: a periodic per-person declaration with a renewal clock, where "nothing to declare" is a real recorded answer rather than an absent one, reviewed by somebody other than the declarant — `ReviewDeclaration` refuses a self-review outright, the same segregation `SelfApprovalError` and `BbsActions::Verify` already enforce. A declaration that reveals something worth managing is promoted to an `impartiality` risk explicitly, never automatically. |
| 4.2 | **Confidentiality** — legally enforceable commitments to manage all information obtained; the laboratory informs the customer in advance of information it intends to place in the public domain; personnel bound to confidentiality | `Document#confidentiality` (`public`/`internal`); `Incident#confidentiality` (`normal`/`restricted`/`highly_restricted`); OHC module ability shape (§11.19); `AccessControlRule` | **Partial** | Confidentiality is real in places and thin in others. The OHC module is the strongest — its ability shape is the app's one deliberate exception to every other pattern, with **no department-scoped read for anyone**, so a department head cannot see their reports' medical records. `Incident` has a genuine three-tier confidentiality with a neutral label for restricted records. But `Document#confidentiality` has only **two values, `public` and `internal`** — there is no restricted document tier — and there is no confidentiality undertaking record per employee, and no customer-confidentiality commitment. Note also that `AccessControlRule` carries **no `has_paper_trail`**, so changes to access rules are unversioned (Part 11 gap G4 flags this as a first-priority fix). |

---

## Clause 5 — Structural requirements

| Clause | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 5.1–5.2 | A legal entity, or a defined part of one, legally responsible for its activities; identified management with overall responsibility | Organization Profile (§4.1); `Site` (§3A) | **Covered** | One organization per deployment running a corporate office and multiple sites, so a laboratory can be scoped as a site or a department within one. |
| 5.3 | **Define and document the range of laboratory activities** for which it claims conformity, and not claim conformity for activities provided externally on an ongoing basis | `Document` (§10) | **Partial** | The scope of accreditation holds as a controlled document. There is no structured **scope of accreditation** — the list of measurands, ranges, methods and uncertainties an accreditation body actually schedules — and that list is the laboratory's primary identity document. See gap **L4**. |
| 5.4 | Carry out activities so as to meet the standard's requirements, customers', regulatory authorities' and accrediting organizations' requirements | Cross-cutting | **Partial** | |
| 5.5 | Define the organization and management structure, responsibilities, authorities and interrelationships; **document procedures to the extent necessary to ensure consistent application**; specify responsibility, authority and interrelationships of personnel who manage, perform or verify work | `Department`/`DepartmentMembership`/`Role`/`UserRole` (§4.1, §4.2); `Document` (§10) | **Covered** | Departments with heads, 21 roles with real per-module abilities, multi-role assignment, and procedures under full document control with sequential approval and controlled distribution. |
| 5.6 | Have personnel with the authority and resources to implement, maintain and improve the management system, identify deviations, initiate preventive action, report on performance, and ensure the effectiveness of activities | `Role::KEYS`; `Finding`/`CapaCase` (§7) | **Partial** | The roles and the CAPA authority exist. **No laboratory-specific roles exist** — no technical manager, no quality manager for the lab, no authorized signatory. `Role::KEYS` has 21 entries and none is laboratory-facing. See gap **L4**. |
| 5.7 | Ensure communication about the effectiveness of the management system and the importance of meeting requirements | `Notification`/`Notifications::Create` (§5.3); `ManagementReviewMeeting` (§9); `SafetyMeeting` (§11.11) | **Covered** | Every assignment, approval, decision and due date notifies in-app and by email through one pipeline. |

---

## Clause 6 — Resource requirements

| Clause | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 6.1 | Available personnel, facilities, equipment, systems and support services | Cross-cutting | **Partial** | |
| 6.2.1–6.2.2 | Personnel who can influence results act impartially, are competent, and work in accordance with the management system; **competence requirements documented for each function**, including education, qualification, training, technical knowledge, skills and experience | `Competency`/`CompetencyRequirement` (§11.5, §11.5.3) | **Covered** | `CompetencyRequirement` holds role/department, required level and a renewal period, and now `belongs_to :competency` — a real master rather than the free-text string it was, which is what makes "does this person meet competency X" answerable by the database at all. `TrainingSessionCompetency` closes the other half: a session declares what it confers, so competence requirements and the training that satisfies them are joined rather than matched by hand. |
| 6.2.3–6.2.5 | Personnel have the competence to perform activities for which they are responsible; procedures and records for determining competence requirements, selection, training, supervision, **authorization**, and **monitoring of competence** | `UserCompetency`; `Competencies::Grant`/`Withdraw`/`GrantFromTraining` (§11.5.3); `TrainingSession`/`TrainingAttendance`/`AssessmentAttempt` (§11.5) | **Partial — authorization now real, monitoring still absent** | The evidence half was already excellent: an attendance register with trainer validation, an MCQ whose **score is frozen at submission**, and a certificate issued only on validated presence plus a pass. **Authorization now exists**: `UserCompetency` records that a named person holds a named competency, from a date, until a date, granted by a named person, on either a `TrainingAttendance` as evidence or a stated `basis` — never neither. Withdrawal is a state with a mandatory reason, not a delete, so "when did this person stop being authorized, and who decided" is answerable. What remains missing is **periodic competence monitoring** — witnessed testing, blind samples, results review — as a dated record against the authorization with its own clock. See gap **L3**. |
| 6.2.6 | Authorize personnel to perform specific activities — develop/modify/verify/validate methods, analyse results, report/review/authorize results | `UserRole` competency gate (§11.5.3); `Role`/`UserRole`; `Abilities::*` | **Partial** | **The gate now exists.** A `CompetencyRequirement` set to `blocking` refuses a role assignment while the person does not hold the competency, enforced as a validation on `UserRole` rather than a service so RailsAdmin — where role assignment actually happens — cannot bypass it. This closed the gap previously recorded here and in three other mapping documents. It is still **per role, not per method**: 17025 wants authorization scoped to a specific activity ("may report results for method X"), and this app's authorization unit is the role. That distinction is what is left of gap **L3**. |
| 6.3.1–6.3.2 | **Facilities and environmental conditions** suitable and not adversely affecting validity; requirements documented | `MonitoredCondition` (§11.7.2); `Location` (§4.1) | **Partial** | The *requirements* are now documented where they matter: a `MonitoredCondition` states the parameter, the limit band and the action band for a named location, which is §6.3.2's "requirements shall be documented" for anything actually monitored. `Location` itself is still a flat master with no cleanliness classification or area type, so facility suitability as a whole remains undescribed. |
| 6.3.3 | **Monitor, control and record environmental conditions** in accordance with relevant specifications, methods or procedures, or where they influence validity | **`MonitoredCondition` / `ConditionReading`** (§11.7.2) | **Covered** | A monitored point with its own parameter, unit, limit band, narrower action band, frequency and due-date clock; readings carrying the value, the taker, the instrument used, and an `assessment` **computed and stored** so a reading keeps the judgement made against the limits in force when it was taken. At least one limit is mandatory — a condition that cannot fail is not being monitored. A reading past an action level warns; a reading outside a limit **raises a Finding without asking**, deliberately unlike the opt-in raises elsewhere in the app, because a number outside a limit the laboratory itself set leaves no discretion to exercise. |
| 6.3.4–6.3.5 | Measures to control facilities — access control, prevention of contamination/interference, effective separation of incompatible activities | `AccessControlRule`; `Visitor` (§11.21) | **Partial** | Physical access control is not the app's job, but the visitor module does hold gate approval and check-in properly. `AccessControlRule` exists but is unversioned. |
| **6.4.1–6.4.4** | **Equipment** — access to equipment required for correct performance; procedures for handling, transport, storage, use and planned maintenance; **verified it conforms to specified requirements before being placed or returned into service** | `Asset`/`MaintenanceRecord` (§11.7); `PssrReview` (§11.13) as a pattern | **Partial** | A planned maintenance programme with a frequency, due dates, completed dates, provider, result, evidence, an overdue report and a daily reminder job. Missing: any **verification before return to service** — `PssrReview`'s refuse-on-"No" authorization gate is the pattern for it — and any equipment attributes beyond `code`, `name`, `department`, `location`, `owner`, two frequency columns and a status. No manufacturer, model, serial number, date received, date placed in service, or location history. See gap **L1**. |
| **6.4.5–6.4.8** | Equipment achieves the **measurement accuracy and/or measurement uncertainty** required; **calibrated where it affects the validity of results**; a calibration programme reviewed and adjusted; equipment **labelled or coded to indicate its calibration status** and the date of next calibration | `Asset`/`CalibrationRecord` (§11.7) | **Partial** | The calibration programme is real: `calibration_frequency_months` per asset, `CalibrationRecord` with a due date, completed date, provider, result, next due date and a certificate attachment, plus a calibration-due report and reminders. What is missing is everything that makes it a *17025* programme: **no measurement uncertainty**, no accuracy requirement, no as-found and as-left readings, no acceptance tolerance, and **no calibration status derived onto the asset** — an accreditation assessor's first act is to look for the status label, and `Asset#status` is a lifecycle field (`active`), not a calibration verdict. See gap **L1**. |
| 6.4.9 | Equipment found to be **defective or outside specified requirements** taken out of service, segregated or labelled; the laboratory **examines the effect of the defect on previous results** and initiates the nonconforming work procedure | `Asset#calibration_state`; `Calibrations::Record` / `EvaluateImpact`; `Finding`/CAPA (§7, §11.7.1) | **Covered** | An out-of-tolerance verdict puts the asset into `out_of_tolerance` (so `calibration_usable?` is false), raises a Finding whose description names the window of suspect results — everything measured since the last acceptable calibration — and `Calibrations::EvaluateImpact` records the conclusion with its actor and timestamp. The asset stays out of tolerance until that evaluation exists **and** a later calibration passes: an unevaluated failure is an open question, and an evaluated failure with no subsequent pass means nothing has shown the instrument is fit again. A passing intermediate check does not clear it. |
| 6.4.10 | **Intermediate checks** to maintain confidence in performance, where required | `CalibrationRecord#kind` (§11.7.1) | **Covered** | `kind` is `calibration` or `intermediate_check`, sharing one table because both answer the same questions and a second model would duplicate every column plus its own scheduling. An intermediate check **never moves the calibration due date**, which is what makes it intermediate, and `Asset#latest_calibration` reads full calibrations only so a check can never satisfy a due date. |
| 6.4.11 | Where calibration data include **reference values or correction factors**, these are updated and implemented to meet specified requirements | — | **Not covered** | |
| 6.4.12 | Measures to prevent unintended adjustments that would invalidate results | — | **N/A — physical** | Sealing and tamper-evidence. |
| 6.4.13 | **Records retained for equipment that can influence laboratory activities** — identity, manufacturer, serial number, verification of conformity, current location, calibration dates and results, adjustments/acceptance criteria/next due date, maintenance plan and maintenance carried out, damage/malfunction/modification/repair | `Asset` / `CalibrationRecord` / `MaintenanceRecord` (§11.7, §11.7.1) | **Covered** | `Asset` gained `manufacturer`, `model`, `serial_number`, `received_on`, `placed_in_service_on`, `accuracy_requirement` and `affects_results`; `CalibrationRecord` gained as-found/as-left readings, the acceptance band and unit, the verdict, the reference standard, the performer, the certificate number, uncertainty and coverage factor. Location and maintenance were already there. The one item still thin is a structured damage/malfunction/repair log — `MaintenanceRecord` holds it as prose. |
| **6.5.1–6.5.3** | **Metrological traceability** — results traceable to the SI through an unbroken chain of calibrations, each contributing to the measurement uncertainty, via a **competent laboratory** (an NMI or an accredited calibration laboratory) or **certified reference materials**; where traceability to the SI is not possible, traceability to an appropriate reference | `CalibrationRecord#provider` (a free string) | **Not covered — the defining technical gap** | Metrological traceability is what 17025 accreditation is *for*, and the app holds it as a free-text provider name. There is no reference standard register, no certificate number, no accreditation body or scope reference for the calibrating laboratory, no traceability chain, no certified reference material record with a certificate, an expiry and a lot, and no uncertainty at any point in the chain. See gap **L2**. |
| 6.6.1–6.6.3 | **Externally provided products and services** — only suitable ones used; procedures for defining, reviewing and approving requirements; **criteria for evaluation, selection, monitoring of performance and re-evaluation** of external providers, with records; requirements communicated to the provider | `Supplier`/`SupplierEvaluation` (§11.6); `ExternalParty` (§4.1) | **Partial** | A supplier record with an `approval_status` (`pending`/`approved`/`suspended`/`rejected`) and a `risk_rating`, plus periodic evaluations scored on quality, delivery and service with a `next_review_date` and an opt-in Finding raise on a poor result. Two limitations for 17025: `approval_status` is a **plain editable field, not an approval workflow** — `Supplier` carries no `Approvable`, so there is no evidence of who approved on what basis — and the evaluation criteria are fixed at quality/delivery/service with no technical dimension, so approving a calibration laboratory for a specific measurand and range is not expressible. Same gap as ICH Q7 §16 and 21 CFR 117.415. |

---

## Clause 7 — Process requirements

This is where the application stops. Almost every row is a laboratory function
with no counterpart in a management system, and the honest answer for most of
them is a single sentence.

| Clause | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 7.1 | **Review of requests, tenders and contracts** — requirements defined, documented and understood; the laboratory has the capability and resources; appropriate methods selected; **differences resolved before work begins**; deviations informed to the customer; records of reviews retained | — | **Not covered** | This is the same gap `iso-standards-mapping.md` records as ISO 9001 §8.2 "Not yet covered" — there is no order or contract review module. Note that `Proposal`/`Pricing::Catalog` is the app's **own commercial quoting for module licensing**, not a customer contract review record, and should not be mistaken for one. See gap **L5**. |
| 7.2.1 | **Selection and verification of methods** — appropriate methods used, kept up to date and available; the laboratory verifies it can properly perform a method before introducing it, with records | — | **Not covered** | A method can be uploaded as a controlled `Document` — with sequential approval, immutable approved revisions and controlled distribution, which genuinely satisfies "available and up to date". Nothing else. |
| 7.2.2 | **Validation of methods** — non-standard, laboratory-developed and modified methods validated; the validation as extensive as necessary; records including the procedure, requirements, determination of performance characteristics, results, and a statement on validity | — | **Not covered** | See gap **L5**. |
| 7.3 | **Sampling** — a sampling plan and method available at the site; the plan based on appropriate statistical methods; records of sampling data including the method, date, time, sampler, and environmental conditions | — | **Not covered** | Shared with gap P2 in the [pharma/food mapping](ich-q7-haccp-harpc-mapping.md). |
| 7.4 | **Handling of test or calibration items** — procedures for transportation, receipt, handling, protection, storage, retention and disposal; **an unambiguous identification system retained for the life of the item**; deviations from specified conditions recorded and the customer consulted | — | **Not covered** | The chain-of-custody requirement. Shared with gap P1 (batch identity) in the pharma/food mapping. |
| 7.5 | **Technical records** — records containing results, sufficient to **facilitate identification of factors affecting the result and its uncertainty and to enable repetition under conditions as close as possible to the original**; original observations, data and calculations recorded at the time; the personnel responsible identifiable; **amendments traceable to previous versions, with the original and amended data retained, and the personnel responsible for the amendment** | PaperTrail; `ApprovalStep` (§5.2) | **Not covered** | PaperTrail is a change history for application data, not a technical record — there are no results for it to version. Note that Clause 7.5.2's amendment requirements are almost word-for-word 21 CFR Part 11 §11.10(e), and **fail here for the same reasons**: no reason-for-change column on `versions`, no audit trail viewer in the UI, no name snapshot on `whodunnit`. See Part 11 gaps G1 and G4. |
| 7.6 | **Evaluation of measurement uncertainty** — identify the contributions and evaluate the uncertainty of measurement | — | **Not covered** | |
| 7.7 | **Ensuring the validity of results** — monitoring through use of reference materials, alternative instrumentation, functional checks, control charts, replicate testing, retesting of retained items, correlation of results, review of reported results, intra-laboratory comparisons, blind sample testing; **participation in proficiency testing or interlaboratory comparisons**; data analysed, trended, and action taken where outside predefined criteria | `AuditProgram` (§6.0) as a *scheduling* pattern only | **Not covered** | Proficiency testing participation is the requirement an accreditation body checks hardest, and it is a small, well-defined record: the scheme, the round, the measurand, the assigned value, the reported value, the z-score, the verdict, and — where unsatisfactory — a Finding. The scheduling and reminder machinery to drive it already exists. See gap **L5**. |
| 7.8.1–7.8.2 | **Reporting of results** — accurately, clearly, unambiguously and objectively; reports include all information agreed with the customer and necessary for interpretation, and all information required by the method | Prawn report renderers (§15.3) as a *pattern* | **Not covered** | Nothing to report. The rendering capability is genuinely there — five hand-written Prawn renderers already exist (`Audits::ReportPdf`, `HazopStudies::ReportPdf`, `Ohc::MedicalExaminationRegisterPdf`, `Documents::CoverSheet`, `MasterDocumentRegisters::IndexPdf`) — and `OhcExaminations::CertificatePdf` is structurally a certificate with a reference number, which is the closest existing analogue to a calibration certificate. |
| 7.8.3–7.8.5 | Specific requirements for **test reports**, **calibration certificates** (including the measurement uncertainty, metrological traceability, and conditions under which calibrations were made) and **sampling reports** | — | **Not covered** | |
| 7.8.6 | **Reporting statements of conformity** — the decision rule documented and applied, taking account of the level of risk associated with the decision rule; the report identifies the decision rule and the results the statement applies to | — | **Not covered** | The decision rule (guard bands, shared risk) is a defined 2017-revision requirement and has no analogue anywhere in the app. |
| 7.8.7 | Opinions and interpretations — only by authorized personnel, with the basis documented | — | **Not covered** | |
| 7.8.8 | **Amendments to reports** — amended reports uniquely identified, containing a reference to the original they replace | `DocumentVersion` revision chain (§10.2) as a *pattern* | **Not covered** | The pattern is exactly right and already built: `DocumentVersion` carries `revision`, `superseded_version_id`, `change_summary` and an immutability guard once approved. An amended calibration certificate is the same object with different content. |
| 7.9 | **Complaints** — a documented process for receiving, evaluating and making decisions on complaints, available to any interested party on request; the complaint acknowledged and the outcome formally communicated; **decisions made by, or reviewed and approved by, individuals not involved in the original activities** | `Incident` (`customer_complaint`) (§11.4); `CustomerFeedback` (§11.17) | **Covered** | A complaint is a real investigated record with triage, a structured investigation, RCA, CAPA and external-notification tracking, and `CustomerFeedback` (`source: complaint`) cross-references it so a formally investigated complaint is not entered twice. The "not involved in the original activities" requirement is served by `Approvals::Approve`'s `SelfApprovalError`. The one gap is that the complaint **cannot name the report or item it concerns**. |
| 7.10 | **Nonconforming work** — responsibilities and authorities defined; actions based on the risk levels; **evaluation of the significance, including impact on previous results**; a decision on acceptability; **notification of the customer and recall of the work where necessary**; the responsibility for authorizing the resumption of work defined | `NonconformingOutput` (§11.16); `Finding`/`CapaCase` (§7) | **Partial** | `NonconformingOutput` is a good match for the shape: a severity, a containment action, a disposition where `use_as_is` requires a justification and goes through the generic approval engine, a verification step, and an opt-in Finding raise on closure. What it cannot express is **impact on previous results** (no results) and **recall of the work** (no reports issued, and no recall capability anywhere — the same gap P4 in the pharma/food mapping). |
| 7.11 | **Control of data and information management** — access to data required; systems validated for functionality before introduction; **protected from unauthorized access, safeguarded against tampering and loss**; maintained to ensure integrity; calculations and data transfers checked | See [cfr21-part11-mapping.md](cfr21-part11-mapping.md) | **Partial** | Access control is the app's strongest area — Devise with lockable/timeoutable/trackable, CanCanCan on every controller, view, query and export, per-site isolation enforced by a registry and regression-tested. **Validation is not** — nothing in the repository constitutes a validation package (Part 11 gap G7), and 17025's "validated for functionality before introduction" is the same requirement. |

---

## Clause 8 — Management system requirements (Option A)

This is the half the application already is. Read it alongside the ISO 9001
column of [iso-standards-mapping.md](iso-standards-mapping.md).

| Clause | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 8.1 | Options A and B — establish, document, implement and maintain a management system capable of supporting consistent achievement of the standard's requirements | Cross-cutting | **Covered** | A laboratory whose parent organization holds ISO 9001 may take Option B; everything below is Option A. |
| 8.2 | **Management system documentation** — policies and objectives established, documented and acknowledged; addressing competence, impartiality and consistent operation; **evidence of commitment**; documentation communicated to, available to, and implemented by personnel | `Document`/`DocumentVersion`/`DocumentDistribution`/`DocumentAcknowledgement`/`DocumentAssessment` (§10); `QualityObjective` (§8) | **Covered** | Policies and the quality manual under full control, with controlled distribution and **acknowledgement**, and — where a click-through is not enough — a **Read & Understood MCQ assessment pinned to one specific revision**, so a pass is evidence about the text the person actually read. `DocumentAssessment#version_not_repinned` rejects any later change to the pinned revision outright. Objectives carry targets, comparators, frequency, periodic results and a computed achievement. |
| 8.3 | **Control of management system documents** — approved for adequacy prior to issue; **periodically reviewed and updated as necessary**; changes and current revision status identified; relevant versions available at points of use; uniquely identified; **unintended use of obsolete documents prevented**, and suitably identified if retained | `Document`/`DocumentVersion` (§10); Master Document Register (§10.3/§10.4) | **Covered — the strongest clause in this document** | Sequential approvals through the generic engine, `revision` and `display_label` with a unique index per document, `superseded_version_id` maintaining the chain, `EDITABLE_STATUSES` making an approved revision immutable, `file_checksum`, `effective_at`, `review_frequency`/`next_review_date` with reminder jobs, controlled distribution with acknowledgement, watermarked Control and Master Copy PDFs with a cover sheet, and download logging via `DocumentDownloadLog`. The Master Document Register goes further than the clause asks: a register is itself a controlled document whose linked entries snapshot each document's exact revision, it republishes automatically when a linked document publishes, and it can **gate release** — an approved revision rests at `approved` and does not become effective until the register revision listing it is itself approved. |
| 8.4 | **Control of records** — legible records established and retained; procedures for identification, storage, protection, backup, archive, retrieval, **retention time** and disposal; records retained for a period consistent with contractual obligations; access consistent with confidentiality | **`RetentionRegistry` / `RetentionPolicy` / `Archivable`**; `Retention::Archive` / `Dispose` (§17.1); PaperTrail; Active Storage | **Partial — retention now real, backup still undocumented** | Retention time exists: a configurable period per record class with a **mandatory stated basis**, an archive step recording actor and reason, and a disposal step that **refuses until the period has elapsed** and then keeps the row as the evidence that a documented disposal happened. `Archivable` raises on `destroy`, and the cascades that would have removed calibration and reading history became `restrict_with_error`. A registry spec enforces that every listed class has the columns, the concern and a policy. Two things remain: the registry covers the §8.4 laboratory set, not all ~45 controllers (Part 11 gap **G2**), and **backup and restore are still undocumented and untested** — an untested backup is not §8.4 coverage. |
| 8.5 | **Actions to address risks and opportunities** — risks and opportunities associated with laboratory activities considered, to give assurance the management system achieves its intended results, enhance opportunities, prevent or reduce undesired impacts, and achieve improvement; actions **proportional to the potential impact on validity of results** and their effectiveness evaluated | `RiskOpportunity`/`RiskTreatmentAction`/`RiskReview` (§11.2) | **Covered** | A `kind` of risk or opportunity, a scored likelihood × impact through the admin-configurable `RiskMatrixLevel` (not hardcoded labels), existing controls, ordered treatment actions with owners and due dates, residual scoring, and an ordered review history. Add an impartiality/laboratory domain to the existing `domain` enum for 4.1. |
| 8.6 | **Improvement** — opportunities for improvement identified and selected; **feedback, both positive and negative, sought from customers**, analysed and used to improve | `CustomerFeedback` (§11.17); `SafetyRecognition` (§11.12); `ManagementReviewMeeting` decisions (§9) | **Covered** | `CustomerFeedback` mirrors `SupplierEvaluation` field for field: a `source` (`survey`/`direct_feedback`/`repeat_business`/`complaint`/`warranty_claim`/`other`), an optional 1–5 score, a `result`, notes, an optional link to the `Incident` a serious complaint was investigated under, and an opt-in Finding raise on a poor result. A satisfaction trends report exists. **"Both positive and negative" is explicitly satisfied** — the feedback model does not assume a complaint. |
| 8.7 | **Corrective actions** — react to the nonconformity, evaluate the need for action to eliminate the causes so it does not recur, including **reviewing and analysing the nonconformity, determining the causes, and determining whether similar nonconformities exist or could potentially occur**; implement, review effectiveness, update risks and opportunities; records of the nature of the nonconformities, the actions taken and the results | `Finding`/`RootCauseAnalysis`/`RootCauseAnalysisCause`/`RootCauseAnalysisWhyStep`/`CapaCase`/`CapaPlan`/`CapaAction`/`CapaEffectivenessCheck` (§7) | **Covered** | A structured fishbone and 5-Whys root cause analysis on a draft-then-submit-for-approval workflow, a CAPA plan requiring approval, actions with owners and due dates, a segregation-of-duties verifier, a distinct effectiveness check, and closure blocked until an approved RCA and an approved CAPA both exist for a minor or major nonconformity. `CapaScope` covers "whether similar nonconformities exist elsewhere" and `CapaLink` relates the case to the records that triggered it, with a `source_snapshot` so a linked confidential record never leaks its content to someone who can see the link but not the source. One caveat worth naming: **`RootCauseAnalysisCause` and `RootCauseAnalysisWhyStep` carry no `has_paper_trail`** — the reasoning inside a root cause analysis is unversioned, which Part 11 gap G4 flags as a first-priority fix and an assessor would also find. |
| 8.8 | **Internal audits** — at planned intervals, to provide information on whether the management system conforms to the laboratory's own requirements and to this document, and is effectively implemented; **an audit programme planned taking into consideration the importance of the activities, changes affecting the laboratory, and the results of previous audits**; criteria and scope defined for each audit; results reported to relevant management; **appropriate correction and corrective actions undertaken without undue delay**; records retained | `AuditProgram`/`AuditProgramEntry`/`Audit`/`AuditChecklist`/`AuditChecklistItem` (§6.0, §6) | **Covered** | A programme with a period, an owner, an approved schedule, and a per-entry **frequency** that expands on approval into one dated audit per interval across the period — so "at planned intervals" is enforced by the schedule rather than by someone remembering. Criteria and scope are records too: `AuditProgramEntryStandard`/`AuditProgramEntryClause` copy through to `AuditStandard`/`AuditClause`, making per-clause coverage a filter on the audits index. Coverage % is the "is it being implemented?" evidence, and closing a programme is refused while any planned audit is neither opened nor cancelled-with-a-reason. Findings raised from checklist items run the full RCA → CAPA → effectiveness loop. |
| 8.9 | **Management reviews** — at planned intervals; **inputs recorded** including changes in internal and external issues, fulfilment of objectives, suitability of policies and procedures, status of actions from previous reviews, outcome of recent internal audits, corrective actions, assessments by external bodies, changes in the volume and type of work, **customer and personnel feedback**, complaints, effectiveness of improvements, adequacy of resources, results of risk identification, **outcomes of the assurance of the validity of results**, and other relevant factors; **outputs recorded** including decisions and actions | `ManagementReviewMeeting`/`MeetingAgendaItem`/`MeetingActionItem` (§9) | **Partial** | The mechanism is exactly right — a scheduled meeting with participants, an agenda built from a standard catalogue, minutes, decisions, approval, and trackable action items with owners, due dates, comments, attachments, reminders and overdue status. `MeetingAgendaItem::STANDARD_AGENDA_POINTS` was a 16-point consolidated ISO 9001/14001/45001 agenda; the **four inputs §8.9.2 names that none of those three standards do** — assessments by external bodies, changes in the volume and type of work, personnel feedback (as distinct from customer), and outcomes of the assurance of the validity of results (§7.7) — are now in it, carrying `ISO/IEC 17025 §8.9.2` references, so `Meetings::PopulateStandardAgenda` produces a complete §8.9.2 agenda in one click. They are appended rather than interleaved so an organization not running a laboratory still sees the familiar 16 first. What keeps this Partial is §7.7 itself: the agenda now *asks* for the outcomes of result-validity assurance, and there is still no proficiency-testing record to answer it from. See gap **L5**. |

---

## What we are missing — prioritized

Ordered by what an accreditation assessor would open first. **L1 and L2 together
are the in-house calibration laboratory case**; L3–L5 are what a commercial
laboratory additionally needs.

### L1 — Equipment records to 6.4.13, and calibration status as a fact
**§6.4.1–6.4.13 · Closed 2026-08-06**

**Built** — see architecture.md §11.7.1. `Asset` carries the §6.4.13 identity
set (manufacturer, model, serial number, date received, date placed in service,
required accuracy, and `affects_results`); `CalibrationRecord` carries as-found
and as-left readings, the acceptance band and unit, the verdict, the reference
standard, the performer, the certificate number, uncertainty and coverage
factor. `Asset#calibration_state` is the §6.4.4 status, derived by
`Calibrations::RefreshState` and swept nightly, deliberately distinct from the
lifecycle `status`. Out-of-tolerance raises a Finding and blocks the asset until
a §6.4.9 impact evaluation exists *and* a later calibration passes; §6.4.10
intermediate checks share the table and never move the due date.

The same columns closed ICH Q7 §5.3 and the mechanical-integrity half of
29 CFR 1910.119(j) — one piece of work, three standards.

**What is still open**, and it is small: a **location history** on `Asset`
(current location is held, movements are not), and a structured
damage/malfunction/repair log — `MaintenanceRecord` holds that as prose today.

### L2 — Metrological traceability and measurement uncertainty
**§6.5, §7.6, §6.3.3 · Partially closed 2026-08-06 — was Blocking**

**Built**: the **reference standard register** (`ReferenceStandard`) with the
artefact's own certificate, the issuing laboratory, its accreditation body and
number, the certificate validity, the contributed uncertainty and coverage
factor, and a `traceability_route` covering both §6.5.2 routes plus CRM,
intrinsic and an honest `not_traceable`. `#si_traceable?` requires a live
certificate and an active status, and `Calibrations::Record` **refuses** a
calibration measured against a lapsed or retired standard — the chain cannot be
claimed through a broken link. **Environmental condition monitoring** (§6.3.3)
is built as `MonitoredCondition`/`ConditionReading`, sharing its shape with
gap S2 in the Factories Act / OSHA mapping and P13 in the pharma/food mapping.

**What remains:**

- A **certified reference material** register proper — CRM is a
  `traceability_route` today, but there is no lot, expiry or assigned-value
  record distinct from a reference standard.
- An **uncertainty budget** per method or measurand — the contributions, their
  distributions, the combined and expanded uncertainty, and the coverage
  factor. `ReferenceStandard` and `CalibrationRecord` each carry *an*
  uncertainty, but nothing composes them. This is still the one genuinely new
  kind of calculation on this list, and it depends on a `Method` record
  (gap **L5**).

### L3 — Per-method authorization and competence monitoring
**§6.2.3–6.2.6 · Medium — reduced, was High**

**Partially closed (2026-08-06).** The competency master, held competencies
(`UserCompetency`), and the competency gate on role assignment are built —
see architecture.md §11.5.3. `Competencies::Grant`/`Withdraw` record and remove
an authorization with an actor, a date range, evidence-or-basis, and a
mandatory withdrawal reason; a `blocking` `CompetencyRequirement` refuses a
role assignment while the competency is unheld. That closed the shared gap this
document previously listed alongside ICH Q7 §3.1, 21 CFR 117.180,
Factories Act §41C(b) and Part 11 §11.10(i).

Two things remain, both specific to a laboratory:

- **Per-method authorization.** The app's authorization unit is the *role*;
  17025 §6.2.6 wants it scoped to a named activity — "may verify method X",
  "may authorize results for method Y". Once a `Method` record exists (gap
  **L5**), the natural shape is a requirement attached to the method rather
  than only to a role, reusing `UserCompetency` unchanged.
- **Competence monitoring** — witnessed testing, blind samples, results review
  — as a dated record against the authorization with its own clock, which is
  what keeps an authorization current rather than merely granted.

### L4 — Laboratory identity: scope and roles
**§5.3, §5.6 · Partially closed 2026-08-06 — was Medium**

**Built:** the four **§8.9.2 agenda points** are in
`MeetingAgendaItem::STANDARD_AGENDA_POINTS` with their `ISO/IEC 17025`
references, and the **impartiality domain plus `InterestDeclaration`** closed
§4.1.

**What remains** — both genuinely about the app recognising that a laboratory
exists:

- A **scope of accreditation** record — measurand, matrix, range, method
  reference, and uncertainty — as structured data rather than an uploaded PDF.
  This is the laboratory's primary identity document and the thing an
  accreditation body schedules against.
- **Laboratory roles** in `Role::KEYS`: technical manager, laboratory quality
  manager, authorized signatory, approved analyst. None of the 21 existing
  roles is laboratory-facing — and now that role assignment can be gated on a
  competency (§11.5.3), an "authorized signatory" role would actually mean
  something.

### L5 — The laboratory itself: methods, samples, results, reports, PT
**§7.1–7.8 · Blocking for a commercial laboratory; deferrable for an in-house one**

Everything in Clause 7 that is not complaints or nonconforming work. This is a
LIMS, and it should be planned as one rather than discovered a model at a time.

- **Contract review** (7.1) — which is also the ISO 9001 §8.2 gap
  `iso-standards-mapping.md` already records as "Not yet covered", so it serves
  two standards.
- **Method** register with selection, verification and validation records
  (7.2), each method versioned and approved through the existing document
  machinery.
- **Sample** and **item** with an unambiguous identity retained for the life of
  the item, receipt condition, storage, retention and disposal (7.3, 7.4) —
  shared with gaps P1 and P2 in the [pharma/food mapping](ich-q7-haccp-harpc-mapping.md).
- **Technical records** (7.5) sufficient to repeat the measurement, with
  amendments traceable to previous versions — which requires Part 11 gap G4
  (reason for change, audit trail viewer) to be closed first, since 7.5.2 and
  §11.10(e) are the same requirement in different words.
- **Reports and certificates** (7.8) with uncertainty, traceability statements,
  the decision rule for any statement of conformity, and amended reports that
  reference the original they replace. The rendering capability exists —
  `OhcExaminations::CertificatePdf` is structurally a certificate with a
  reference number — and `DocumentVersion`'s `superseded_version_id` chain is
  exactly the amendment pattern 7.8.8 describes.
- **Proficiency testing and interlaboratory comparison** (7.7) — scheme, round,
  measurand, assigned value, reported value, z-score, verdict, and a Finding on
  an unsatisfactory result. Small, well-defined, checked hardest by assessors,
  and buildable well before the rest of Clause 7.

**If only one thing from L5 is built first, build proficiency testing.** It is
the cheapest, it is the one an assessor will ask about at the opening meeting,
and it does not depend on any of the others.

---

## A note on NABL and accreditation bodies

An Indian laboratory seeking ISO/IEC 17025 accreditation applies to **NABL**
(National Accreditation Board for Testing and Calibration Laboratories), which
assesses against ISO/IEC 17025:2017 plus its own specific criteria documents
(the NABL 100-series), and those add requirements this document does not cover —
notably proficiency testing participation frequency per discipline, the format
of the scope of accreditation, and specific traceability expectations. The
equivalents elsewhere are A2LA, ANAB and UKAS.

Two practical consequences for anything built from gaps L1–L5:

- **The scope of accreditation is the deliverable.** Everything structured in L4
  should be shaped by the accreditation body's own scope format, not invented.
- **A2LA/NABL assessors read records, not screens.** The gaps in
  [cfr21-part11-mapping.md](cfr21-part11-mapping.md) — no complete record export
  (G3), no audit trail viewer (G4), no retention (G2) — will surface in an
  accreditation assessment exactly as they would in an FDA inspection.

None of this is accreditation advice. It is a description of where in the app an
accreditation assessment's outputs get configured, and where the app currently
cannot hold them.

---

## What an ISO 17025 assessment will not find here

Stated so nobody goes looking: there is **no LIMS**, no sample login, no
worksheet, no instrument interface, no chromatography or spectroscopy data
system, no uncertainty calculator, no control charting, and no report or
certificate generation for measurement results. The only structured test result
in the entire application is `OhcExaminationTest` — a **clinical** result on an
employee medical examination, with a free-text `value` — and architecture.md
§11.19 records that laboratory and diagnostics integration was explicitly
descoped for that module.

The realistic architecture is the one most accredited laboratories already run:
**a LIMS owns Clauses 6.4 (in part), 6.5 and 7; this application owns Clause 8
and most of Clauses 4, 5 and 6.2**, with defined integration points at
nonconforming work, corrective action, internal audit, document control and
management review. Building L5 would mean deciding, deliberately, to become the
LIMS too — which is the same decision the
[pharma/food mapping](ich-q7-haccp-harpc-mapping.md) describes at its own close,
and it should be made once, for both, rather than twice by accident.

---

## Keeping this document current

Update this file whenever a module changes its posture against ISO/IEC 17025 —
new equipment or calibration fields, an authorization mechanism, a scope record,
laboratory roles, agenda points carrying a 17025 reference, or a gap from the
list above getting built. Treat a stale row here the same as a stale line in
`architecture.md`: fix it in the same change that touches the code.

Note in particular that **L1 and L3 are shared with three other mapping
documents.** When either is built, update this file, the
[pharma/food mapping](ich-q7-haccp-harpc-mapping.md), the
[Factories Act / OSHA mapping](factories-act-osha-mapping.md) and the
[Part 11 mapping](cfr21-part11-mapping.md) in the same change — a gap closed in
one place and left standing in three others is worse than no mapping at all.
