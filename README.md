# IMS Portal — User Guide

A screenshot-driven walkthrough of every module, written against a real
running copy of the app. To reproduce the data behind these screenshots:

```
bin/rails db:prepare
bin/rails sample_data:seed   # ~10-20 sample records per module, skips Documents
```

`sample_data:seed` loads everything for a single site (the first one), so on
a multi-site install the demo lives at one plant — switch site in the topbar
to see the boundary, as [Sites](31-sites.md) describes.

Sample/demo login credentials are printed by the seed tasks — see
[Getting Started](00-getting-started.md#signing-in).

## Contents

1. [Getting Started](00-getting-started.md) — signing in, forced password
   change, navigation, My Work, notifications, reports.
2. [Masters & Admin](01-masters-and-admin.md) — RailsAdmin master data,
   Document Workflow Designer, Email Template Designer, Access Control
   Matrix.
3. [Audits](02-audits.md) — scheduling, team, checklist, report approval,
   comments/attachments. (Planning a year of them up front is
   [Audit Programme](32-audit-programme.md).)
4. [Findings](03-findings.md) — raise, assign, RCA, CAPA plan/actions,
   effectiveness, close/reject.
5. [CAPA Cases](04-capa-cases.md) — the standalone triage → investigation
   → RCA → plan → effectiveness → closure workflow.
6. [Quality Objectives](05-quality-objectives.md) — targets, assignments,
   periodic results, review.
7. [Management Reviews](06-management-reviews.md) — agenda, minutes,
   decisions, action items.
8. [Documents](07-documents.md) — controlled revisions, review/publish
   approval, Control/Master copies, distribution & acknowledgement,
   Read & Understood assessments pinned to a revision, Master Document
   Register with MR-only approval and automatic Pending Update.
9. [Context & Interested Parties](08-context-and-interested-parties.md)
10. [Risks & Opportunities](09-risks-and-opportunities.md) — scoring,
    treatment, monitoring, review history.
11. [Compliance Obligations](10-compliance-obligations.md) — evaluations,
    auto-raised findings.
12. [Incidents](11-incidents.md) — triage, investigation, linked CAPA,
    external notifications, closure.
13. [Management of Change](12-management-of-change.md) — the full
    assessment → approval → implementation → verification workflow.
14. [Competency & Training](13-competency-and-training.md) — the
    attendance register (self check-in, trainer validation), the optional
    MCQ assessment, and auto-issued participation certificates.
15. [Suppliers](14-suppliers.md) — evaluations, auto-raised findings.
16. [Assets](15-assets.md) — calibration and maintenance records.
17. [Dashboards & Reports](16-dashboards-and-reports.md) — the
    organization dashboard and every printable/exportable report.
18. [How the Access Control Matrix works](17-access-control-matrix.md) —
    role vs. user rows, the four scopes, resolution order, a worked
    before/after example, and the non-model tab-toggle rows.
19. [Environmental Aspects & Impacts](18-environmental-aspects.md) —
    ISO 14001 aspect/impact register, significance determination
    (computed + overridable), periodic review, and auto-raised findings.
20. [Worker Participation](19-worker-participation.md) — ISO 45001 §5.4
    consultation log: hazard reports, suggestions, and safety committee
    input, with an auto-raised finding for confirmed hazards.
21. [Safety Meetings](20-safety-meetings.md) — toolbox talks and safety
    committee meetings: schedule, participants, actions, no formal
    minutes approval.
22. [Award & Reward](21-award-and-reward.md) — nominate, approve, award,
    individual/team recognitions, optional link to a worker
    participation record, notification on award.
23. [PSSR (Pre-Startup Safety Review)](22-pssr.md) — standard 12-point
    checklist, draft/in-review/authorized/started lifecycle, optional
    MOC/asset linkage, reject with auto-raised finding.
24. [HAZOP (Hazard and Operability Study)](23-hazop.md) — nodes, guide
    word/parameter deviations, risk ranking, recommended actions tracked
    to closure, optional auto-raised finding.
25. [Safety Observations & BBS](24-safety-observations-and-bbs.md) —
    Behavior-Based Safety programmes; a learning and prevention system,
    not a disciplinary one. Programmes, versioned checklist templates,
    observations with per-item results and contributing factors, actions
    with the hierarchy of controls, segregation-of-duties verification,
    and stop-work, cross-module links to MOC/Incident/Training/Document,
    a coverage & trends report, and now observer competency shown as of
    the observation's own date.
26. [Nonconforming Output](25-nonconforming-output.md) — ISO 9001 §8.7:
    containment, disposition (with approval required for Use As Is),
    verification/closure, and an optional linked Finding for RCA/CAPA.
27. [Customer Satisfaction](26-customer-satisfaction.md) — ISO 9001 §9.1.2:
    customer feedback/surveys, an optional linked Finding for a poor
    result, and a trends report.
28. [Design & Development](27-design-and-development.md) — ISO 9001 §8.3:
    inputs, outputs, review, verification, validation, and approval for a
    design project; a design change is raised as an MOC request rather
    than a duplicate mechanism.
29. [OHC / Employee Health Records](28-ohc-employee-health-records.md) —
    a larger planned OHC platform built as slices, now with its own
    sidebar section: employee medical master, OPD/clinic visits, basic
    fitness management, Pre-Employment and Periodic Medical Examinations
    with auto-scheduled due dates and fitness certificates,
    vaccination/immunisation tracking, pharmacy & inventory (medicine
    stock, batch tracking, dispensing), the first aid & emergency
    register with first aid kit inspections, hazard-exposure surveillance
    programs with exposure timelines and test trends, contractor medical
    clearances with gate-pass compliance, health campaigns, and statutory
    examination reminders with a printable Medical Examination Register —
    all rolled into one dashboard card. A department head gets no special
    access here — the one deliberate deviation from every other module's
    ability shape.
30. [Emergency Preparedness & Response](29-emergency-preparedness.md) —
    ISO 14001/45001 §8.2: the register of potential emergency situations
    and their planned response, the named response team, announced and
    unannounced drills with response/evacuation timings and an
    effectiveness verdict, and the review-and-revise loop that keeps a
    plan honest after a drill or a real emergency.
31. [Visitor Management](30-visitor-management.md) — site gate control:
    pre-registering a future visitor, admin-defined dynamic fields on the
    visitor form, a configurable multi-level approval chain (including the
    built-in reporting-manager level), the security Gate Console with
    walk-in registration and mandatory photo capture, check-in/check-out,
    the 12-hour automatic close, and the drag-and-drop visitor pass
    designer with QR codes.
32. [Sites (Corporate & Plants)](31-sites.md) — the multi-site layer: how a
    site, a location and a department differ, what a site user can and
    cannot see, corporate access and the topbar site switcher, the two
    things that deliberately cross the boundary (documents shared with all
    sites, audits spanning all sites), and what happens when an existing
    single-site installation is upgraded.
33. [Audit Programme (Planning)](32-audit-programme.md) — ISO 9001 §9.2.2 a)
    / ISO 19011 §5: the year's audits agreed and approved up front, and the
    lead-time automation that opens each real audit from its planned entry,
    with the checklist, team, departments and dates already filled in.
    Repeating audits (monthly through annual) expand into one dated audit per
    interval on approval; standards and clauses are linked as records so
    programme coverage is a filter, not a reading exercise; and the lead
    auditor can be handed over on the live audit with the change recorded.
34. [Permit to Work](33-permit-to-work.md) — ISO 45001 §8.1.2/§8.1.4: the
    controlled authorisation of non-routine work. A **versioned, admin-editable
    permit form** (sections, Yes/No/NA checkpoints, enforceable numeric ranges,
    per-permit-type visibility) rather than a hard-coded one; permit types that
    can be clubbed onto a single permit and fold to the shortest validity; a
    configurable permit-type x approver-level matrix that escalates by shift; an
    acceptor who is usually contractor roster staff, gate-pass checked; a gas
    test log where an out-of-limits reading suspends the job; renewal as
    re-approval rather than a date edit; four-signature closure with the
    30-minute fire watch; coded cancellation reasons; the permit board, the
    printable permit with a download log, and a compliance report.
35. [Designing a Permit Form](34-designing-a-permit-form.md) — the form
    designer in depth, for the administrator who has to make the system ask
    what *your* permit sheet asks: sections and who fills them, the five
    section behaviours, question types, what makes a checkpoint **critical**
    and a gas limit **enforceable**, per-permit-type visibility, copying a
    block out of another form, reading the printed sheet as you build,
    signature declarations, publishing and revising. Ends with a worked hot
    work permit and a pre-publish checklist.
36. [Batch Manufacturing Records (EBMR)](35-batch-manufacturing-records.md) —
    ICH Q7 §6.4–6.7/§8, 21 CFR 211.184–211.192, Schedule M Part I: the
    electronic batch record. One **material master** covering raw material
    through finished product; goods receipt with quarantine and quality-unit
    lot release; a structured, versioned, **immutable-once-effective Master
    Batch Record** with a bill of materials, process steps and enforceable
    in-process limits; batch execution with FEFO dispensing, **lot genealogy**,
    second-person checks the database itself enforces, blocking limits and
    yield reconciliation; deviations that name the batch they affected and
    block its release; quality-unit review and release under 21 CFR Part 11
    electronic signature; and the printable batch record an inspector asks for
    by name.
37. [Recalls & Traceability](36-recalls-and-traceability.md) — ICH Q7 §10.2/§15,
    21 CFR 117.139, Codex §7, FSSAI (Food Recall) Regulations 2017: the
    despatch register that answers *who has it*, recalls and market actions
    whose affected lots are resolved by **walking the genealogy chain** rather
    than from memory, consignee notification built from despatch records,
    recovery reconciled against a stated target, and **mock recall exercises**
    that run the identical procedure and are timed — which is what a GFSI
    auditor actually asks you to demonstrate.
38. [Designing a Statutory Form](37-designing-a-statutory-form.md) — Factories
    Act §112 and 29 CFR 1904: the registers and returns an inspector asks for,
    printed in **your state's** prescribed format. The Act prescribes almost
    none of its own forms — the state Rules do — so the format is data you
    design in the app: pick the register it draws from, tick and label the
    columns your form has, print it, then mark it **verified** once you have
    checked it against the published Rules. Explains why no Indian state ships
    pre-filled, how to bulk-load many jurisdictions, and states plainly which
    forms cannot yet be produced and why.

## Standards & regulatory mapping

Clause-by-clause mappings of what this app covers, for nine frameworks across
five documents — ISO 9001/14001/45001, Factories Act 1948, OSHA, ISO/IEC 17025,
21 CFR Part 11, ICH Q7, HACCP and HARPC.

**[Start at the standards index →](standards/README.md)** — a one-screen summary
of where the app stands against each, which document answers which question, and
the cross-cutting gaps that appear in several at once.

These are working gap analyses rather than a compliance claim: every row states
plainly what is missing as well as what is built.

## Conventions used throughout

- Every screenshot is a real screen from the running app — none are
  mocked up.
- **Guide Demo …** titled records were created live for these
  screenshots and are safe to ignore/delete in a real environment.
- Server-side guard rails (e.g. "at least one lead auditor is required
  before scheduling") are shown as they actually appear — this guide
  documents the real validation messages, not an idealized happy path.
- Comments, Attachments, and the Approval history panel look and behave
  identically everywhere they appear; they're documented in full once
  (in [Audits](02-audits.md#comments-and-attachments)) rather than
  repeated on every page.
