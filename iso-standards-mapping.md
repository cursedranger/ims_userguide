# ISO Standards & Clause Mapping

**Last reviewed: 2026-07-29 — update this alongside any new module.**

## Scope and how to read this document

This document maps ISO 9001:2015 (Quality Management), ISO 14001:2015
(Environmental Management), and ISO 45001:2018 (Occupational Health &
Safety) clauses to the specific application module or model that supports
them. It exists to answer one question repeatedly, as the app grows: *if
we're certifying against this clause, what in the software actually backs
it up?*

This is a mapping of built functionality, not a certification checklist
and not a legal reference. It reflects the codebase as of the date above —
cross-reference [architecture.md](../architecture.md) §11 for the full
technical description of each module, and treat this file as the thing
that goes stale first when a module changes shape. Every table row below
was checked against the actual current models/services, not against what
was originally planned.

### A note for manufacturing companies in India

This app models a single organization's *management system* — audits,
findings, CAPA, objectives, documents, and the operational modules in
§11. It does not itself encode any Indian statute or regulation, and
never will: obligations differ by state, sector, and site, and belong in
data the organization configures, not in application code. What this app
gives an Indian manufacturer is a place to **register and track** those
obligations against the same evaluation/evidence/nonconformity machinery
used for everything else:

- **`ComplianceObligation` / `ComplianceEvaluation`** (§11.3) is where a
  site records its own applicable statutory and regulatory requirements —
  the Factories Act 1948 (state Factory Rules), the Legal Metrology Act
  2009 (for any product sold by weight/measure), BIS product certification
  (mandatory under the relevant Quality Control Order for many
  manufactured goods), the Water (Prevention and Control of Pollution) Act
  and Air (Prevention and Control of Pollution) Act consents issued by the
  State Pollution Control Board, Hazardous & Other Wastes (Management &
  Transboundary Movement) Rules, the Extended Producer Responsibility (EPR)
  rules under the Plastic Waste Management Rules and the Battery Waste
  Management Rules, the Environment (Protection) Act 1986, and the
  Occupational Safety, Health and Working Conditions Code 2020 as it comes
  into force state by state — each becomes one `ComplianceObligation` row
  with its own owner, evaluation frequency, and evidence trail. A
  noncompliant evaluation can raise a Finding through the same
  `Findings::RaiseFromSource` path every other module uses.
- **`Incident`** (§11.4) is where a reportable environmental release,
  workplace injury, or process safety event is logged, investigated, and
  — where an Indian statute requires external notification (e.g. a
  factory-act-reportable injury to the Chief Inspector of Factories, a
  pollution-control-board-reportable release) — that notification is
  tracked as part of the incident record, not hardcoded as a rule.
- **`Asset` / `CalibrationRecord`** (§11.7) is where Legal Metrology
  calibration/verification obligations for weighing and measuring
  instruments are tracked alongside ordinary calibration.

None of the above is legal advice or a substitute for site-specific
regulatory review — it's a description of *where in the app* that review's
outputs get configured and tracked.

---

## ISO 9001:2015 — Quality Management System

| Clause | Requirement | Application module(s) | Notes |
|---|---|---|---|
| 4.1 | Understanding the organization and its context | `ContextIssue` (§11.1) | Internal/external issues, category, impact, owner, periodic review. |
| 4.2 | Understanding the needs and expectations of interested parties | `InterestedParty` (§11.1) | Needs/expectations, compliance relevance, owner, review date. |
| 4.3 / 4.4 | Scope of the QMS; QMS and its processes | Organization Profile (§4.1) | Single-organization scope; no multi-tenant process map beyond the modules themselves. |
| 5.1–5.3 | Leadership, policy, roles/responsibilities/authorities | Users, roles, department heads (§4.2); Management Review (§9) | Roles are assignable per user; management review agenda item categories map directly to §9.3 inputs. |
| 6.1 | Actions to address risks and opportunities | `RiskOpportunity` (§11.2) | `kind: risk/opportunity`, scored, treatment actions, review history. |
| 6.2 | Quality objectives and planning to achieve them | `QualityObjective` / `ObjectiveAssignment` / `ObjectiveResult` (§8) | Target/comparator/frequency, periodic actual results, achievement calculation. |
| 6.3 | Planning of changes | `MocRequest` (§11.8, "Change management") | Impact/risk assessment, implementation plan, generic approvals, closure requires verification. |
| 7.1.5 | Monitoring and measuring resources (calibration) | `Asset` / `CalibrationRecord` / `MaintenanceRecord` (§11.7) | Overdue assets are visible and cannot be marked compliant without a record. |
| 7.1.6 / 7.2 | Organizational knowledge; competence | `CompetencyRequirement` / `TrainingSession` / `TrainingAttendance` (§11.5) | Renewal periods, expiring/expired competence dashboard. |
| 7.4 | Communication | `Notification` + email (§5.3); `SafetyMeeting` (§11.11); Management Review (§9) | Every assignment/approval/due-date event notifies via the same pipeline. |
| 7.5 | Documented information | `Document` / `DocumentVersion` (§10) | Sequential approvals, effective/current version, immutable approved revisions, controlled distribution/acknowledgement. |
| 8.1 | Operational planning and control | `MocRequest` (§11.8); `PssrReview` (§11.13); `HazopStudy` (§11.14) | Change, pre-startup, and hazard-study controls for planned/modified operations. |
| 8.4 | Control of externally provided processes, products, and services | `Supplier` / `SupplierEvaluation` (§11.6) | Approval status, risk rating, periodic evaluation; poor results can raise a Finding. |
| 8.5.1 | Control of production and service provision | `IncidentChemicalRelease` / process-related `Incident` records (§11.4) where relevant | Broader shop-floor process control (SOPs, work instructions) is document-managed via §10, not a separate production-execution module — out of scope for this app. |
| 8.6 | Release of products and services | `NonconformingOutput#disposition` (§11.16) | A "use as is" disposition is the release-with-concession case; every other disposition (rework/regrade/scrap/return-to-supplier/repair) implicitly withholds release until corrected. |
| **8.7** | **Control of nonconforming outputs** | **`NonconformingOutput`** (§11.16) | Detection stage, containment, disposition (with mandatory approval for "use as is"), verification/closure, optional linked Finding. |
| 9.1.1 | Monitoring, measurement, analysis and evaluation | Reports/dashboards (§15); `ObjectiveResult` (§8); `BbsObservation` trend report (§11.15) | Org-wide and "My Work" dashboards, CSV export, printable reports. |
| 9.2 | Internal audit | `Audit` / `AuditChecklist` / `AuditChecklistItem` (§6) | Full schedule → execute → report → approve workflow, team roles, findings raised from checklist items. |
| 9.3 | Management review | `ManagementReviewMeeting` / `MeetingAgendaItem` / `MeetingActionItem` (§9) | `STANDARD_AGENDA_POINTS` is the consolidated Quality/Environment/OH&S 16-point agenda, one item per §9.3.2 input. |
| 10.1 | General (improvement) | Cross-cutting — Findings, CAPA, objectives, all "opt-in raise a Finding" points across §11 | Improvement is the connective tissue between every module, not a standalone one. |
| 10.2 | Nonconformity and corrective action | `Finding` / `RootCauseAnalysis` / `CapaPlan` / `CapaAction` (§7) | Minor/major NC requires approved RCA and approved CAPA before closure. |
| 10.3 | Continual improvement | `SafetyRecognition` (§11.12); management review decisions (§9) | Recognition program plus the management-review decision/action-item loop. |

### ISO 9001 — Not yet covered

- **§8.3 Design and development** — no module models a design/development
  control gate (inputs, outputs, review, verification, validation,
  change control for a design). A manufacturer whose products are
  entirely to a fixed, externally-supplied specification may not need
  this; one that does its own product/process design does.
- **§9.1.2 Customer satisfaction** — no model captures customer feedback,
  satisfaction surveys, or complaint trend analysis as a distinct
  dataset. `Incident#incident_kind == "customer_complaint"` records an
  individual complaint through the incident workflow, but there is no
  aggregate customer-satisfaction monitoring yet.
- **§8.2 Requirements for products and services** (contract/order review)
  — no dedicated module; would sit naturally alongside a future
  Design & Development or Customer module.

---

## ISO 14001:2015 — Environmental Management System

| Clause | Requirement | Application module(s) | Notes |
|---|---|---|---|
| 4.1–4.2 | Context; interested parties | `ContextIssue` / `InterestedParty` (§11.1) | Shared with the QMS — this app does not duplicate context/interested-party registers per management system. |
| 6.1.1 | General (risks and opportunities) | `RiskOpportunity` with `domain: environment` (§11.2) | Same scoring/treatment/review mechanism as quality and OH&S risk. |
| **6.1.2** | **Environmental aspects and impacts** | **`EnvironmentalAspect`** (§11.9) | Likelihood × severity scoring, computed significance (`score >= threshold \|\| legal_requirement_linked?`) with a justified override, periodic review. |
| 6.1.3 | Compliance obligations | `ComplianceObligation` / `ComplianceEvaluation` (§11.3) | Shared register across all three standards — see "A note for manufacturing companies in India" above. |
| 6.2 | Environmental objectives | `QualityObjective` (§8), used generically | The objective model is not standard-specific; an environmental KPI is just an objective with an environmental metric. |
| 7.2 / 7.3 | Competence; awareness | `CompetencyRequirement` / `TrainingSession` / `TrainingAttendance` (§11.5) | Shared competence register, not environment-specific. |
| 8.1 | Operational control | `MocRequest` (§11.8); `EnvironmentalAspect#control_measures` (§11.9) | Change management covers planned changes with environmental impact; control measures are recorded directly on the significant aspect. |
| 8.2 | Emergency preparedness and response | `Incident` with `incident_kind: environmental_event` (§11.4); `SafetyMeeting` toolbox talks (§11.11) | No standalone emergency-drill/preparedness-plan model yet — see "Not yet covered" below. |
| 9.1 | Monitoring, measurement, analysis, evaluation | `EnvironmentalAspectReview` (§11.9); reports/dashboards (§15) | Periodic reassessment history mirrors `RiskReview`. |
| 9.2 | Internal audit | `Audit` (§6) | Same audit engine as the QMS — an EMS-scoped audit is an `Audit` tagged to the relevant department/clause set. |
| 9.3 | Management review | `ManagementReviewMeeting` (§9) | `environmental_performance` is one of the standard 16 agenda categories. |
| 10.2 | Nonconformity and corrective action | `Finding` / CAPA (§7) | `EnvironmentalAspectReview` can raise a Finding when a review finds existing controls inadequate. |
| 10.3 | Continual improvement | Management review decisions (§9) | Same mechanism as the QMS. |

### ISO 14001 — Not yet covered

- **§6.1.4 Planning action** as a distinct integrated action plan across
  aspects/obligations/risks — each of those three registers has its own
  treatment/action tracking today, but there is no single cross-register
  environmental action plan view.
- **§8.2 Emergency preparedness and response** as a standalone plan/drill
  register — emergency events are captured reactively via `Incident`;
  there is no proactive drill-scheduling or emergency-plan-document model
  beyond what `Document` (§10) can hold as an uploaded procedure.

---

## ISO 45001:2018 — Occupational Health & Safety Management System

| Clause | Requirement | Application module(s) | Notes |
|---|---|---|---|
| 4.1–4.2 | Context; interested parties | `ContextIssue` / `InterestedParty` (§11.1) | Shared register, as above. |
| 5.1–5.2 | Leadership and worker participation, OH&S policy | Roles (§4.2); `WorkerParticipationRecord` (§11.10) | `corporate_safety_head`/`corporate_safety_team` roles exist specifically for OH&S leadership. |
| **5.4** | **Consultation and participation of workers** | **`WorkerParticipationRecord`** (§11.10); Safety Observations & BBS (§11.15) | Hazard reports, suggestions, safety-committee input, risk-assessment involvement; BBS observations are a second, structured consultation channel. |
| 6.1.1 | General (risks and opportunities) | `RiskOpportunity` with `domain: ohs` (§11.2) | Shared risk register. |
| **6.1.2** | **Hazard identification, risk assessment** | `HazopStudy` (§11.14); `RiskOpportunity`; BBS observations (§11.15) | HAZOP is the structured process-hazard method; BBS covers behavioral/condition-level hazard identification. |
| 6.1.3 | Legal and other requirements | `ComplianceObligation` (§11.3) | Shared register — OH&S obligations (Factories Act, the OSH Code as it commences) are configured here. |
| 6.2 | OH&S objectives | `QualityObjective` (§8), used generically | As with EMS objectives. |
| 7.2 / 7.3 | Competence; awareness | `CompetencyRequirement` / `TrainingSession` / `TrainingAttendance` (§11.5); BBS observer competency (§11.15) | BBS additionally shows observer competency **as of the observation's own date**, not today's. |
| 7.4 | Communication | `Notification` + email; `SafetyMeeting` (§11.11) | Toolbox talks and safety committee meetings. |
| 8.1.1 | General operational planning and control | `PssrReview` (§11.13); `HazopStudy` (§11.14); `MocRequest` (§11.8) | Pre-startup review, hazard study, and change control together cover planned/modified operations. |
| 8.1.2 | Eliminating hazards and reducing OH&S risks | `BbsAction#control_hierarchy` (§11.15); `HazopDeviation` recommended actions (§11.14) | BBS actions force an explicit hierarchy-of-controls pick (elimination/substitution/engineering/administrative/PPE) rather than defaulting to retraining or PPE. |
| 8.2 | Emergency preparedness and response | `Incident` (§11.4); `IncidentMedicalTask`/Form B/B1/E (§11.4.2) | Reactive response and the full medical-treatment workflow; no proactive drill register yet (see EMS "Not yet covered," shared gap). |
| 9.1 | Monitoring, measurement, analysis, evaluation | BBS Coverage & Trends report (§11.15); reports/dashboards (§15) | Deliberately never ranks individuals or treats a quiet department as "well behaved" — every rate states its denominator. |
| 9.2 | Internal audit | `Audit` (§6) | Same audit engine, OH&S-scoped. |
| 9.3 | Management review | `ManagementReviewMeeting` (§9) | `ohs_performance` is one of the standard 16 agenda categories. |
| **10.2** | **Incident investigation, nonconformity, corrective action** | `Incident` / `IncidentInvestigation` / `Finding` / CAPA (§11.4, §7) | Full LOPC/Process Safety Event tiering (§11.4.1) and OHC Doctor/Medical workflow (§11.4.2) on top of the base investigation. |
| 10.3 | Continual improvement | `SafetyRecognition` (§11.12); management review decisions | Recognition program plus decision/action-item loop. |

### ISO 45001 — Not yet covered

- **§8.1.4 Procurement / contractors** as a distinct pre-qualification and
  on-site contractor-safety-control record — `Supplier`/`SupplierEvaluation`
  (§11.6) covers general supplier evaluation but nothing OH&S-specific
  (permit-to-work integration, contractor induction records beyond what
  `TrainingAttendance` can hold generically).
- **§8.2 Emergency preparedness** as a standalone plan/drill register —
  shared gap with ISO 14001, noted above.

---

## Keeping this document current

Update this file whenever a module in `architecture.md` §11 (or any future
section) changes its clause coverage — a new module, a renamed status, a
newly-added "opt-in raise a Finding" path, or a gap from "Not yet covered"
getting built. Treat a stale row here the same as a stale line in
`architecture.md`: fix it in the same change that touches the code.
