# IMS Portal — User Guide

A screenshot-driven walkthrough of every module, written against a real
running copy of the app. To reproduce the data behind these screenshots:

```
bin/rails db:prepare
bin/rails sample_data:seed   # ~10-20 sample records per module, skips Documents
```

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
   approval, Control/Master copies, distribution & acknowledgement.
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
