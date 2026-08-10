# 21 CFR Part 11 — Electronic Records & Electronic Signatures Mapping

**Last reviewed: 2026-08-06 — update this alongside any new module.**

## Scope and how to read this document

This document maps **21 CFR Part 11** (US FDA, *Electronic Records; Electronic
Signatures*) to the specific application module, model, or service that supports
it — the same job [iso-standards-mapping.md](iso-standards-mapping.md) does for
ISO 9001/14001/45001.

It is not the same *kind* of document, and it should not be read as one. Three
differences matter:

1. **ISO clauses describe a management system; Part 11 describes a computer
   system.** Every row below is a statement about this codebase itself, not
   about a process the codebase tracks. A gap here is a defect in the software,
   not a missing register.
2. **Part 11 is not standalone.** It applies only to records that a *predicate
   rule* (21 CFR 210/211, 820, 606, 58 …) already requires you to keep. If a
   record in this app is not required by a predicate rule, Part 11 does not
   reach it. Deciding which of this app's records are predicate-rule records is
   the regulated organization's call, not the software's — see "Deciding what is
   in scope" below.
3. **Roughly half of Part 11 cannot be satisfied by software at all.** §11.10(i),
   (j), §11.100(b)(c), and most of §11.300's operational controls are written
   procedures, identity-verification practice, and a signed letter to the FDA.
   Those rows are marked **Procedural** and will never become "Covered" by
   shipping code.

**Current overall position, stated plainly: this application is not yet 21 CFR
Part 11 compliant, but it is no longer missing the core of it.** As of
2026-08-06 the electronic signature itself is built (gap G1): approvals of
predicate-rule records require a two-component signing challenge and carry a
frozen declaration naming the meaning of the signature. What remains blocking is
record protection and retention (G2 — 44 controllers still expose `destroy`) and
the validation package (G7), neither of which is a signature problem. The
audit-trail gaps (G4) sit just behind them.

**As of 2026-08-10 the EBMR module (architecture.md §11.23) brings the two
highest-value predicate-rule records into that signing scope**:
`MasterBatchRecordVersion` (§211.186) and `ProductionBatch` (§211.188/§211.192).
Both route their decision through `Approvals::Approve`, which is what makes it a
signature rather than a click — a direct `released_by` write would have been
simpler and would not have been one.

**Gap G11 was closed on the same day it was opened.** `Part11::Sign` gained a
second entry point, `for_decision`, which signs a single-party decision without
an `ApprovalStep` and writes an `ElectronicSignature`. Four EBMR decisions are
now signed that way — **material lot release**, **material lot rejection**,
**deviation batch-impact assessment** and **batch record review** — alongside
the two that go through the approval engine (master batch record approval and
batch release). All six take password re-entry, render a §11.50 declaration
frozen at signing time, and record a `SignatureAttempt` whether they succeed or
fail.

Two EBMR actions remain unsigned by deliberate choice, and a site should know
which: **placing a lot or a batch on hold**, and **closing a deviation**. A hold
is reversible, precautionary and not a predicate-rule decision — making every
cautious act cost a password discourages caution, which is the wrong incentive
to build. Deviation closure follows the assessment that was already signed, by
the same quality unit, and adds no new determination. Both are attributable,
timestamped and under PaperTrail.

Every row below was checked against the current models/services, not against
what was planned. Cross-reference [architecture.md](../../../architecture.md) for the
technical description of each module.

### Deciding what is in scope

Part 11 scope is per-record, not per-application. In this app the records most
likely to be predicate-rule records for an FDA-regulated site are:

- **`Document` / `DocumentVersion` / `DocumentAcknowledgement`** (§10) — SOPs,
  specifications, batch record templates. Almost certainly in scope under
  §211.100/§211.180 or §820.40.
- **`MasterBatchRecordVersion`** (§11.23) — the master production and control
  record under §211.186, which explicitly requires a second, independent
  signature by the quality unit.
- **`ProductionBatch`** (§11.23) — the batch production and control record and
  its review under §211.188 / §211.192. This is the record an FDA inspector
  asks for by name.
- **`Finding` / `RootCauseAnalysis` / `CapaPlan` / `CapaAction`** (§7) and
  **`CapaCase`** — the CAPA record under §820.100 / §211.192.
- **`NonconformingOutput`** (§11.16) — §820.90 / §211.192 nonconformance.
- **`MocRequest`** (§11.8) — change control under §211.100(b) / §820.30(i).
- **`Asset` / `CalibrationRecord`** (§11.7) — §211.68 / §820.72.
- **`CompetencyRequirement` / `TrainingSession` / `TrainingAttendance`** (§11.5)
  — §211.25 / §820.25 training records.
- **`Supplier` / `SupplierEvaluation`** (§11.6) — §820.50 supplier controls.
- **`Audit` / `AuditProgram`** (§6) — §820.22 quality audits (note: FDA does
  *not* review internal audit reports, but the records must still exist).
- **`Complaint`-kind `Incident`** (§11.4) — §820.198 complaint files.

- **`DesignProject`** (§11.18) — §820.30 design controls. Added to the enforced
  registry on 2026-08-06: design review, verification and validation approvals
  are among the most-inspected signatures in a device QMS, and omitting it
  would have been the larger error.

Records with no predicate-rule hook — BBS observations, safety recognition,
visitor passes, worker participation — are outside Part 11 even though they live
in the same database. That distinction matters, because the remediation cost
below scales with how many models you put in scope. **Do not put everything in
scope by default.**

The nine models actually enforced today are declared in
[`app/models/part11_scoped.rb`](../../../app/models/part11_scoped.rb), each with the
predicate rule that put it there. Of the records listed above, `Asset` /
`CalibrationRecord`, `TrainingSession`, `SupplierEvaluation` and complaint-kind
`Incident` are **not** enforced — not because they are out of scope, but because
they have no approval workflow for a signature to attach to. Giving them one is
a separate piece of work.

### A note for pharmaceutical and medical-device manufacturers in India

An Indian site is not subject to 21 CFR Part 11 by virtue of being in India — it
becomes subject when it manufactures, tests, or holds product for the US market
and its electronic records support that product. For a site exporting to the US,
the FDA's jurisdiction runs to the records, wherever the server is.

The Indian and international equivalents this app should be read against at the
same time:

- **Schedule M, Drugs and Cosmetics Rules 1945** (revised 2024, phased
  enforcement) now carries explicit computerised-system and data-integrity
  expectations — audit trails, access control, backup, and validation. A site
  that satisfies Part 11 will generally satisfy revised Schedule M; the reverse
  is not reliably true, because Schedule M does not impose Part 11's specific
  two-component signing requirement.
- **EU GMP Annex 11 (Computerised Systems)** and **MHRA "GXP Data Integrity
  Guidance and Definitions"** are the practical drafting source for most of the
  gaps below, and are stricter than Part 11 on risk assessment, supplier
  assessment, and periodic review.
- **PIC/S PI 041** (data integrity) and **WHO TRS 1033 Annex 4** are what an
  Indian regulator or a customer audit will most likely cite. **ALCOA+**
  (Attributable, Legible, Contemporaneous, Original, Accurate, + Complete,
  Consistent, Enduring, Available) is the vocabulary; the gap list below is
  organised so each item can be traced to an ALCOA+ attribute.
- **CDSCO** inspection findings increasingly mirror FDA 483 observations on
  audit trail review — note that *having* an audit trail is §11.10(e), but
  *reviewing* it is a separate, procedural expectation that this app currently
  gives you no tooling for (see gap G4).

None of this is legal or regulatory advice. It is a description of where in the
app a regulatory assessment's outputs get configured, and — more importantly
here — where the app currently cannot hold them.

---

## Subpart B — Electronic Records

### §11.10 Controls for closed systems

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 11.10(a) | Validation of systems to ensure accuracy, reliability, consistent intended performance, and the ability to discern invalid or altered records | RSpec/FactoryBot/Capybara suite (§20); DB constraints, FKs, check indexes (§17) | **Not covered** | The test suite is *evidence a validation effort could cite*, not validation. There is no validation plan, no URS/FS/DS, no IQ/OQ/PQ protocol, no requirements traceability matrix, and no documented release/change-control procedure for the software itself. This is the single largest body of work and it is mostly documentation, not code. See gap G7. |
| 11.10(b) | Ability to generate accurate and complete copies in both human-readable and electronic form for FDA inspection | Printable reports and PDF services (`Audits::ReportPdf`, `HazopStudies::ReportPdf`, `Ohc::MedicalExaminationRegisterPdf`, document Master/Control copy PDFs, §15.3); CSV export (§15) | **Partial** | Human-readable copies exist for the major modules. What is missing is (i) a *complete* record export — the record plus its full audit trail plus its signatures plus its attachments, as one artifact — and (ii) an electronic-form export in a non-proprietary structured format. Today an inspector asking "give me this CAPA and everything that ever happened to it" cannot be served from the UI. See gap G3. |
| 11.10(c) | Protection of records to enable accurate and ready retrieval throughout the retention period | Approved/effective `DocumentVersion` immutability (§10.2); `Finding`/CAPA state guards (§7.2); Active Storage; PaperTrail | **Partial** | Controlled-document immutability is genuinely good. But 44 controllers expose `destroy`, `dependent: :destroy` cascades physically remove child rows (and their PaperTrail versions are orphaned rather than preserved as deletions of a retained parent), there is no retention period concept anywhere in the schema, and no archival tier. CLAUDE.md's "archive rather than hard-delete" rule is honoured by convention in workflow states, not enforced structurally. See gap G2. |
| 11.10(d) | Limiting system access to authorized individuals | Devise (`database_authenticatable`, `lockable`, `timeoutable`, `trackable`, `confirmable`); CanCanCan on every controller/view/query/export (§12); `SiteScoped`/`SiteScopable::REGISTRY` (§3A); `User#active`; `must_change_password` | **Covered** | This is the strongest area. 30-minute session timeout, 10-attempt lockout with 1-hour unlock, per-site isolation enforced by registry and regression-tested in `spec/abilities/site_isolation_spec.rb`, and an active-user check. RailsAdmin is authorization-gated too. |
| 11.10(e) | Secure, computer-generated, time-stamped audit trails recording operator entries and actions that create, modify, or delete records; changes must not obscure previous values; retained as long as the record and available for review and copying | PaperTrail 17.0 on 196 of 241 models (every EBMR model carries it); `set_paper_trail_whodunnit` in `ApplicationController`; `DocumentDownloadLog` / `AuditReportDownloadLog` / `WorkPermitDownloadLog` | **Partial — and this is the highest-risk partial** | `object_changes` is stored so prior values are not obscured, and whodunnit is the acting user. But: **(1)** there is no audit-trail viewer anywhere in the UI — the "Comments & Activity" tabs are comments, not PaperTrail, and only two views (`audits/_team`, `pssr_checklist_items/_item`) read `.versions` at all, for one field each; **(2)** the `versions` table has no reason-for-change column, so *why* a GxP record changed is never captured; **(3)** no IP address or user agent is recorded on the version despite §17 claiming "PaperTrail records actor and request context"; **(4)** `whodunnit` is a bare user-id string with no name snapshot, so the trail degrades if a user is renamed or removed; **(5)** the only index is on `[item_type, item_id]` — an audit-trail review by user or by date range will table-scan; **(6)** nothing prevents an application-level or DBA-level delete of a `versions` row. See gaps G1 and G4. |
| 11.10(e) — coverage holes | *(same requirement, enumerated)* | Models **without** `has_paper_trail` | **Not covered for these** | `AccessControlRule`, `Setting`, `ModuleFlag` (security and configuration changes — Part 11 and Annex 11 care about these *specifically*), `EnvironmentalAspectReview`, `RootCauseAnalysisCause`, `RootCauseAnalysisWhyStep`, `IncidentInvestigationTeamMember`, `NumberSequence`, `AuditDepartment`, `AuditLocation`, `DocumentStandard`, `DocumentClause`. The RCA cause/why-step omissions matter most: the reasoning inside a CAPA's root cause analysis is exactly what an investigator reads, and it is currently unversioned. |
| 11.10(f) | Use of operational system checks to enforce permitted sequencing of steps and events | Service objects for every state transition (§3, §5.2); `Approvals::Approve` step-position guard; `WorkPermits::Issue` gas-test and gate-pass guards (§11.22); `DocumentVersion::EDITABLE_STATUSES`; `MasterDocumentRegisters::GateForRelease` (§10.4); "no workflow-changing callbacks" rule | **Covered** | Sequencing is enforced in transactional services with explicit guard errors rather than in callbacks or the UI, which is the right architecture for this clause. `Approvals::Approve` refusing anything but the current pending step is a textbook §11.10(f) control. |
| 11.10(g) | Use of authority checks to ensure only authorized individuals can use the system, sign a record, access the operation or device, or perform the operation at hand | CanCanCan abilities per module (§12); `Approvals::Approve` `SelfApprovalError`; permit-type × approver-level matrix that *refuses* rather than auto-approves when unconfigured (§11.22); `can_download_master_copy` / `can_download_control_copy`; `document_access_level` | **Covered** | Segregation of duties is real: the submitter cannot approve their own item, and the approval chain refuses to proceed when its authority matrix is unconfigured rather than silently degrading. |
| 11.10(h) | Device checks to determine the validity of the source of data input or operational instruction | — | **Not covered / mostly N/A** | No instrument or terminal is an authorized data source in this app — all input is human, through the browser. If instrument data is ever imported (e.g. calibration results from a device, LIMS results into `OhcExamination`), this row becomes live and needs a source-device check. `rails_admin_import` is a bulk input path with no device-level provenance and is restricted to master/reference models, which is the correct mitigation. |
| 11.10(i) | Persons who develop, maintain, or use the system have the education, training, and experience to perform their assigned tasks | `Competency` / `CompetencyRequirement` / `UserCompetency` / `TrainingSession` / `TrainingAttendance` / `AssessmentAttempt` (§11.5, §11.5.3) | **Partial — Procedural** | The app holds the training records, including validated attendance and a frozen assessment score, and **it now gates itself**: a `blocking` `CompetencyRequirement` refuses a role assignment while the person does not hold the competency, enforced as a validation on `UserRole` so RailsAdmin cannot bypass it, with a Competency Gaps report for authorizations that lapse afterwards. What remains is a **system-specific training curriculum** — no `Competency` row today says "trained on this application" — and nothing covers the developers/maintainers of the system, which this clause explicitly includes. |
| 11.10(j) | Written policies holding individuals accountable for actions initiated under their electronic signatures | — | **Not covered — Procedural** | An organizational SOP with individual acknowledgement. The app could hold it as a controlled `Document` with a mandatory `DocumentAcknowledgement` (§10) and a Read & Understood assessment (§10.5), which is a good fit — but the policy itself has to be written and the acknowledgement made a precondition of account activation. |
| 11.10(k)(1) | Adequate controls over the distribution of, access to, and use of documentation for system operation and maintenance | `architecture.md`, `docs/`, git history; `Document`/`DocumentVersion` (§10) | **Not covered** | The system's own documentation lives in the repository under git, not under the controlled-document controls this app implements for everything else. For Part 11, system operating documentation should itself be a controlled document with approvals, effective versions, and distribution. |
| 11.10(k)(2) | Revision and change control procedures maintaining an audit trail of time-sequenced development and modification of systems documentation | git; `CLAUDE.md` build rules | **Partial** | Git provides the time-sequenced trail, and commits are attributable. It is not tied to an approval workflow or a change-control record, and there is no link from a deployed release to the `MocRequest` (§11.8) that authorised it. |

### §11.30 Controls for open systems

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 11.30 | Additional measures — document encryption and appropriate digital signature standards — where records are transmitted through an open system | Secure session cookies (§17); TLS at the deployment layer; `Dockerfile` / deployment config | **Partial / deployment-dependent** | Whether this is a closed or open system is a deployment decision, not a code one. A single-organization on-premise install behind a VPN, with access controlled by the system owner, is a **closed system** and §11.30 does not apply. The moment it is reachable from the public internet by contractors, auditors, or corporate users on untrusted networks, it is an **open system** and needs documented encryption in transit and at rest plus digital-signature-grade record binding (see gap G6). The app does not currently record which posture it is deployed in, and that determination belongs in the validation package. |

### §11.50 Signature manifestations

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 11.50(a)(1) | Signed record shows the **printed name** of the signer | `ApprovalStep#signed_name` / `#signed_title`; `WorkPermitSignature#signatory_name`; `shared/_approval_history` | **Covered for in-scope records** | `signed_name`/`signed_title` are snapshots written at signing time, not joins to the live `User` row, so renaming a user no longer rewrites their historical signatures. Steps decided before this shipped still fall back to the association, and are visibly unsigned. |
| 11.50(a)(2) | Signed record shows the **date and time** of signing | `ApprovalStep#acted_at`; `WorkPermitSignature#signed_at` (server-set via `Time.current`) | **Covered** | Server-generated, never client-supplied — correct. `config.time_zone` defaults to UTC with an ENV override; the validation package should state the time source and that it is NTP-synchronised. |
| 11.50(a)(3) | Signed record shows the **meaning** of the signature (review, approval, responsibility, authorship) | `Part11::SignatureMeaning`; `ApprovalStep#signature_meaning` / `#declaration`; `WorkPermitSignature#declaration` (§11.22) | **Covered for in-scope records** | Five meanings (authorship / review / approval / verification / responsibility), resolved per model and stage from `Part11Scoped`, rendered into a declaration and **frozen onto the step at signing time** — the PTW design generalized, for the same reason: a signature means the words the signer actually read. A rejection is signed with withheld wording rather than treated as a non-event. |
| 11.50(b) | The above items are subject to the same controls as electronic records, and are included in any human-readable form of the record (display and print) | `shared/_approval_history` partial; `Documents::CoverSheet`; report PDFs (§15.3) | **Covered for in-scope records** | The name, timestamp, meaning and declaration render wherever approval history renders, which is on screen and in every report PDF that embeds the partial. |

### §11.70 Signature/record linking

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 11.70 | Electronic signatures shall be linked to their respective electronic records to ensure they cannot be excised, copied, or transferred to falsify a record | `ApprovalRequest`/`ApprovalStep` polymorphic `approvable`; `WorkPermitSignature` polymorphic `signable`; FK constraints; approved-version immutability (§10.2) | **Partial** | The link is a relational foreign key, which is a real link but a mutable one — a DBA or a defect can repoint it, and nothing in the record would show that it moved. Part 11 does not mandate cryptography here, and referential linking is broadly accepted for closed systems, but there is no content binding: nothing captures *what the signed content was* at signing time, so a signature cannot be shown to have covered the version of the record that exists now. `DocumentVersion` is the exception — its file is immutable once approved, so the binding holds. See gap G5. |

---

## Subpart C — Electronic Signatures

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 11.100(a) | Each electronic signature is unique to one individual and never reused by or reassigned to anyone else | `users.email` unique index; `users.employee_code` unique index | **Partial** | Uniqueness is enforced at the database level, which is the mechanical half. The procedural half — that a departed employee's identity is never reissued, and that the account is deactivated rather than recycled — is not enforced: `User#active` exists but nothing prevents an admin from editing a former employee's row into a new hire, which would silently reassign every historical signature. A guard against changing identity fields on a user with signature history is a cheap, high-value fix. |
| 11.100(b) | The organization verifies the identity of an individual before establishing, assigning, or certifying their electronic signature | — | **Not covered — Procedural** | An HR/IT identity-verification procedure plus a record of it. `User` has no field asserting that identity verification occurred, who performed it, or when. A small addition (`identity_verified_at`, `identity_verified_by_id`, evidence attachment) would let the app hold this evidence. |
| 11.100(c) | Certification to the FDA, before or at the time of use, that electronic signatures are intended to be the legally binding equivalent of handwritten signatures — in paper form, signed with a handwritten signature, to the Office of Regional Operations | — | **Not covered — Procedural** | A one-time letter from the organization to the FDA, plus the ability to produce additional certification on request. Nothing to build. Nothing to skip either — using e-signatures without it is a violation regardless of how good the software is. |
| 11.200(a)(1) | Non-biometric signatures employ **at least two distinct identification components** (e.g. ID code and password) | `Part11::Sign` (user ID + password re-entry); `Part11Scoped` registry | **Covered for in-scope records** | Deciding an in-scope approval now requires re-entering the user ID and password. Enforcement is inside `Approvals::Approve`/`Reject` rather than the controller, so an unsigned decision is not reachable by writing a new caller. Out-of-scope approvals are untouched by design — see "Deciding what is in scope". |
| 11.200(a)(1)(i) | When several signings are executed during a **single continuous period of controlled system access**, the first signing uses all components; subsequent signings use at least one component executable only by the signer | `SigningPeriod` controller concern; `Part11::Sign::CONTINUOUS_PERIOD` (15 min) | **Covered for in-scope records** | The first signature of a period demands both components; inside the period the password alone completes a signature. The window is deliberately shorter than the 30-minute Devise `timeout_in`, and only an actual executed signature opens it — an ordinary unsigned approval does not. |
| 11.200(a)(1)(ii) | Signings **not** performed in a single continuous period use all signature components | `SigningPeriod#within_signing_period?` | **Covered for in-scope records** | The period lapses on idle and on sign-out (session-backed), after which the identity component is required again. An unparseable session value fails closed to the full challenge. |
| 11.200(a)(2) | Signatures are used only by their genuine owners | §11.10(d) access controls; `SelfApprovalError`; `Part11::Sign` identity check; §11.10(j) accountability policy | **Partial — Procedural** | The technical side is now built. What remains is procedural: password sharing is defeated by the §11.10(j) policy and its acknowledgement, not by code. |
| 11.200(a)(3) | Administered and executed such that attempted use by anyone other than the genuine owner requires collaboration of two or more individuals | Devise `lockable` (10 attempts / 1 hour), now also counting failed *signing* attempts; `must_change_password`; no admin password-set-in-the-clear path | **Partial** | Password reset goes through an emailed token to the account owner rather than an admin setting a known password, which is the important control and it is correct. A wrong password at the signing prompt now increments the same failed-attempt counter and locks the account, so the prompt is not an unthrottled guessing oracle. Weak points remain: `config.password_length = 6..128` is far below any current expectation (gap G8), and there is no separate signing credential, so an admin with database access is still a single individual who can act. |
| 11.200(b) | Biometric signatures designed to ensure they cannot be used by anyone other than their genuine owner | — | **N/A** | No biometric signing implemented, and none needed. |
| 11.300(a) | Maintaining the uniqueness of each combined identification code and password, so no two individuals have the same combination | Unique index on `users.email`; bcrypt via Devise | **Covered** | Unique ID guarantees a unique combination. |
| 11.300(b) | Ensuring that identification code/password issuances are periodically checked, recalled, or revised (e.g. password aging) | — | **Not covered** | No password expiry, no password history / reuse prevention, no periodic access review. The schema has no `password_changed_at`. This is a small, well-understood addition (`devise-security` or a bespoke `password_changed_at` + `PasswordArchive`) and closes a commonly-cited 483 observation. See gap G8. |
| 11.300(c) | Loss management procedures to electronically deauthorize lost, stolen, missing, or otherwise potentially compromised tokens/cards/devices, and to issue temporary or permanent replacements using suitable rigorous controls | `User#active` flag; Devise `lockable`, `unlock_strategy = :both`; `reset_password_token` | **Partial — Procedural** | Deauthorization mechanics exist (deactivate the user, lock the account, invalidate the token). What is missing is the *procedure*, an auditable record of the deauthorization event, and — because `User` carries no `has_paper_trail`-backed security-event view — any easy way to show an inspector when access was revoked and by whom. Note `AccessControlRule` is unversioned entirely. |
| 11.300(d) | Transaction safeguards to prevent unauthorized use of passwords/codes, and to detect and report in an immediate and urgent manner any attempted unauthorized use to system security, and as appropriate to organizational management | `SignatureAttempt` + the Electronic Signature Log (`/signature_attempts`); Devise `lockable` + `failed_attempts` + `locked_at`; `trackable` | **Partial** | Detection and **review** are now built: every signature and every failed attempt is recorded with its outcome, record, and IP, and is searchable/filterable/exportable — an IMS admin sees the whole log, everyone else sees signatures executed under their own account. **Push reporting is still missing**: nothing notifies anyone at the moment an account locks or a signature fails, and the `Notification` pipeline (§5.3) is still not wired to security events. "Immediate and urgent manner" is explicit in the regulation. See gap G8. |
| 11.300(e) | Initial and periodic testing of devices (tokens, cards) to ensure they function properly and have not been altered in an unauthorized manner | — | **N/A** | No hardware tokens or cards are used for signing. Becomes live only if hardware 2FA is introduced. |

---

## What we are missing — prioritized

Ordered by regulatory exposure, not by effort. G1 and G2 are the two that make
the difference between "not compliant" and "remediable".

### G1 — Electronic signature: two-component signing and signature meaning
**§11.200(a)(1), §11.50(a)(3), §11.50(a)(1) · ✅ Built — 2026-08-06**

Delivered as a vertical slice. `Part11Scoped` declares which approvables are
predicate-rule records (nine models, each citing its rule); `Part11::Sign`
verifies the components and returns the manifestation; `Approvals::Approve` and
`Approvals::Reject` call it between their guards and their transaction, so an
unsigned decision on a regulated record is unreachable regardless of caller.

- Five signature meanings resolved per model and stage, rendered into a
  declaration and frozen onto `ApprovalStep` at signing time — the
  `WorkPermitSignature` design generalized rather than reinvented.
- `signed_name` / `signed_title` / `signed_ip` / `signed_user_agent` snapshots,
  so a later rename cannot rewrite a signature already executed.
- Continuous signing period (15 min, session-backed) distinct from the Devise
  session timeout; only an executed signature opens one.
- `SignatureAttempt` records every signing and every failure; wrong passwords
  count toward the Devise lockout.
- Out-of-scope approvals are entirely unchanged — no password prompt on a BBS
  action or a visitor pass.

**Not yet done in this area**: the approval engine is the only signing surface
covered. Any future direct-signature action outside `ApprovalRequest` — a
disposition signed inline, a result signed on a form — needs to route through
`Part11::Sign` too, and nothing currently forces that.

### G2 — Record protection, retention, and deletion
**§11.10(c), §11.10(e) · Blocking**

44 controllers expose `destroy` and `dependent: :destroy` physically removes
child rows. For a predicate-rule record this is a data-integrity failure
regardless of who did it: the deletion must be recorded and the data retained.

- Soft-delete (`archived_at` / `archived_by_id` / `archive_reason`) for every
  in-scope model, with hard `destroy` removed from those controllers and
  abilities, mirroring the existing `SiteScopable::REGISTRY` pattern so a new
  in-scope model cannot be added without declaring its retention behaviour.
- A retention period per record class and an enforced hold — no purge path
  before it elapses, and an authorized, audit-trailed one after.
- Preserve child PaperTrail versions when a parent is archived.
- Documented, tested backup and restore, with restore actually exercised — an
  untested backup is not §11.10(c) coverage.

### G3 — Inspection-ready complete record export
**§11.10(b) · High**

An FDA or CDSCO inspector asks for one CAPA and everything that ever happened to
it. Today that cannot be produced from the UI.

- A per-record "regulatory export" producing the record, its full audit trail,
  its signatures with declarations, its comments, its approval history, and its
  attachments — as a paginated PDF (human-readable) *and* a structured archive
  (electronic form), with a generation timestamp and the exporting user.
- Every export logged, as `DocumentDownloadLog` already does for documents.

### G4 — Audit trail: completeness, review tooling, and reason for change
**§11.10(e) · High**

PaperTrail is installed but effectively invisible and incomplete.

- A per-record audit trail viewer, authorization-scoped, with the searchable /
  paginated / filterable treatment CLAUDE.md requires of every listing —
  filter by user, date range, field, and event.
- A **reason for change** captured at edit time on in-scope records, stored on
  the version, mandatory for records in an approved or effective state.
- Add `ip_address`, `user_agent`, `whodunnit_name`, and `reason` columns to
  `versions`; index `whodunnit` and `created_at` (an audit-trail review by user
  or date currently table-scans).
- `has_paper_trail` on the 21 models that lack it — **`AccessControlRule`,
  `Setting`, and `ModuleFlag` first**, because security and configuration
  changes are what Annex 11 and PIC/S PI 041 reviewers open with, and
  `RootCauseAnalysisCause` / `RootCauseAnalysisWhyStep` next, because
  unversioned RCA reasoning inside a CAPA is indefensible.
- A periodic audit-trail *review* record — who reviewed which trail, over what
  period, with what conclusion. This is the observation Indian and US
  inspectors are currently writing most often, and it needs a place to live.

### G5 — Signature/record content binding
**§11.70 · Medium**

Signatures link relationally but do not bind to content. Store a hash of the
signed payload (the serialized record state, or the attachment checksum) on the
signature row, and surface a "signature covers the record as it stood at
signing" check in the record view and in the G3 export. `DocumentVersion` is
already sound here because its file is immutable; the rest are not.

### G6 — Deployment posture: closed vs. open system
**§11.30, §11.10(d) · Medium**

The app does not record, and the docs do not state, whether an installation is a
closed or open system. Decide it per deployment, document it in the validation
package, and if open: TLS enforcement with HSTS, encryption at rest for the
database and Active Storage, and the §11.70 content binding of G5 upgraded to a
digital-signature standard.

### G7 — Validation package
**§11.10(a) · Blocking, but not a code change**

Nothing in the repository constitutes validation. Required: validation plan,
user/functional/design specifications traceable to the requirements in
`architecture.md`, risk assessment (GAMP 5 category and criticality per module),
IQ/OQ/PQ protocols with executed evidence, a traceability matrix mapping each
Part 11 clause to the test that demonstrates it, change control tying deployed
releases to authorised changes, and a periodic review schedule. The existing
RSpec suite is genuinely useful raw material for OQ — it should be *cited* by
the protocol, not mistaken for it.

### G8 — Credential lifecycle and security-event reporting
**§11.300(b), §11.300(d), §11.200(a)(3) · Medium, cheap**

- Raise `config.password_length` from `6..128` to a modern minimum, with
  complexity or a breached-password check.
- `password_changed_at`, expiry, and password-history reuse prevention.
- Periodic access review: a scheduled report of active accounts, roles, and
  last sign-in, with a recorded review decision.
- Wire security events into the existing `Notification` pipeline (§5.3):
  account lockout, repeated failed signing attempts, role/ability change,
  deactivation, and admin actions on user records — "immediate and urgent"
  is the regulation's own wording.

### G9 — Training gate and identity verification
**§11.10(i), §11.100(b) · Low, procedural with a small code component**

- `identity_verified_at` / `identity_verified_by_id` / evidence on `User`.
- ~~Role assignment gated on holding the required competency.~~ **Built
  2026-08-06** (architecture.md §11.5.3): `Competency` is now a real master,
  `UserCompetency` records who holds what and until when, and a `blocking`
  `CompetencyRequirement` refuses a role assignment on `UserRole` — a
  validation rather than a service, so RailsAdmin cannot bypass it. What is
  still missing is a **system-training curriculum**: no `Competency` row today
  represents "trained on this application", so the machinery is in place but
  nothing yet requires it of a user before granting them a role in it.
- The §11.10(j) accountability policy as a controlled `Document` with mandatory
  acknowledgement (§10) and a Read & Understood assessment (§10.5), made a
  precondition of account activation.

### G10 — System documentation under document control
**§11.10(k) · Low**

Bring `architecture.md` and the operating/maintenance documentation under the
app's own controlled-document workflow, or under an equivalent externally
controlled system, and link deployed releases to their authorising `MocRequest`
(§11.8).

### G11 — Single-party decisions could not be signed — **CLOSED (2026-08-10)**
**§11.50, §11.200; predicate §211.84 · was Medium**

`Part11::Sign` could originally only sign an `ApprovalStep`, so a signature was
reachable only through a multi-party approval chain. That is the right shape for
a document revision and the wrong one for a material lot release: ICH Q7 §2.2
gives that decision to the quality unit alone, and a busy receiving day is dozens
of them, so an approval request per lot with a single step and no second party
would have been ceremony rather than control.

Option (a) from the original entry was taken: `Part11::Sign.for_decision` signs a
decision named in `Part11Scoped::DIRECT_DECISIONS` and writes an
`ElectronicSignature` — polymorphic `signable`, append-only (`update` and
`destroy` both raise), with the §11.50 declaration frozen as rendered. The same
verification core serves both entry points, so the two-component rule, the
15-minute continuous signing period, the lockout on a wrong password and the
`SignatureAttempt` log are identical whichever path a signature takes.

`material_lot.release`, `material_lot.reject`, `deviation.assess` and
`production_batch.review` are in that registry. A cascade — the output lot
leaving quarantine because the batch release was signed — passes `cascade_from:`
and is not signed twice; the upstream signature is named in the lot's release
note instead.

---

## What a Part 11 assessment will not find here

Stated so nobody goes looking: there is **no** batch record, no electronic batch
record review, no LIMS, no instrument interface, no chromatography data system,
and no production execution. This is a management-system application. Part 11
applies to the QMS records it holds — documents, CAPA, change control,
calibration, training, complaints, supplier controls — and those are exactly the
records listed under "Deciding what is in scope". A site's manufacturing and
laboratory records are a different system's Part 11 problem.

---

## Keeping this document current

Update this file whenever a module changes its Part 11 posture — a new signing
path, a new `has_paper_trail`, a new export, a gap from the list above getting
built, or a new model added to the in-scope list. Treat a stale row here the
same as a stale line in `architecture.md`: fix it in the same change that
touches the code.

When a gap is closed, move it from the prioritized list into the tables above
with its status changed, and record the change — a Part 11 gap analysis with no
history of its own is a poor advertisement for the argument it is making.
