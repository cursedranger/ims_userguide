# IMS Portal — User Guide

A screenshot-driven walkthrough of every module, written against a real
running copy of the app. To reproduce the data behind these screenshots:

```
bin/rails db:prepare
bin/rails sample_data:seed   # ~10-20 sample records per module, skips Documents
```

For larger context, see [ISO Mapping](iso-standards-mapping.md)



Sample/demo login credentials are printed by the seed tasks — see
[Getting Started](00-getting-started.md#signing-in).

## Contents

1. [Getting Started](00-getting-started.md) — signing in, forced password
   change, navigation, My Work, notifications, reports.
2. [Masters & Admin](01-masters-and-admin.md) — RailsAdmin master data,
   Document Workflow Designer, Email Template Designer, Access Control
   Matrix.
3. [Audits](02-audits.md) — scheduling, team, checklist, report approval,
   comments/attachments.
4. [Findings](03-findings.md) — raise, assign, RCA, CAPA plan/actions,
   effectiveness, close/reject.
5. [CAPA Cases](04-capa-cases.md) — the standalone triage → investigation
   → RCA → plan → effectiveness → closure workflow.
6. [Quality Objectives](05-quality-objectives.md) — targets, assignments,
   periodic results, review.
7. [Management Reviews](06-management-reviews.md) — agenda, minutes,
   decisions, action items.
8. [Documents](07-documents.md) — controlled revisions, review/publish
   approval, Control/Master copies, distribution & acknowledgement, Master
   Document Register with MR-only approval and automatic Pending Update.
9. [Context & Interested Parties](08-context-and-interested-parties.md)
10. [Risks & Opportunities](09-risks-and-opportunities.md) — scoring,
    treatment, monitoring, review history.
11. [Compliance Obligations](10-compliance-obligations.md) — evaluations,
    auto-raised findings.
12. [Incidents](11-incidents.md) — triage, investigation, linked CAPA,
    external notifications, closure.
13. [Management of Change](12-management-of-change.md) — the full
    assessment → approval → implementation → verification workflow.
14. [Competency & Training](13-competency-and-training.md)
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
