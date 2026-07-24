# Dashboards & Reports

## Organization Dashboard

The landing page after sign-in (sidebar → **Dashboard**) — every module's
current numbers in one screen: audits scheduled/overdue, findings by
kind, RCA/CAPA approvals pending, objectives, management review actions,
documents awaiting approval/review/acknowledgement, risk/compliance
overdue counts, and operational alerts (serious incidents, training
expiring, calibration overdue):

![Organization dashboard](images/dashboards/01-org-dashboard.png)

Everything shown is scoped through `accessible_by(current_ability)` —
what you see reflects only what you're authorized to see, same as every
list in the app.

## My Work

Covered in [Getting Started](00-getting-started.md#my-work--your-personal-queue)
— your own cross-module queue of assigned/owned items.

## Reports

Sidebar → **Reports**. One card per cross-module report, each opening a
focused, printable page with its own **Export CSV**:

![Reports index](images/dashboards/02-reports-index.png)

- **Audit Schedule & Report** — the full audit list with filters and
  print/CSV export (same as [Audits](02-audits.md)' own index).
- **Finding Aging & CAPA Effectiveness** — open findings bucketed by age
  (0-7 / 8-14 / 15-30 / 31-60 / 60+ days) with a drill-down **Show**, plus
  CAPA effectiveness check outcomes:

  ![Finding aging report](images/dashboards/03-finding-aging-report.png)

- **Department Objective Performance** — achievement breakdown by
  department:

  ![Objective performance report](images/dashboards/05-objective-performance-report.png)

- **Management Review Minutes & Action Tracker** — meetings list plus
  every action item across every meeting.
- **Document Master List** — controlled documents with current version,
  effective date, and history via each document's own page.
- **Risk Register** — every risk/opportunity with scoring and treatment
  status.
- **Compliance Evaluation Status** — obligations with evaluation
  frequency and overdue status.
- **Incident Trends** — incidents by type, domain, department, and
  severity:

  ![Incident trends report](images/dashboards/06-incident-trends-report.png)

- **Training Expiry** — attendance records with upcoming or past expiry.
- **Supplier Status** — approval status, risk rating, and evaluation
  history.
- **Calibration Due** — assets requiring calibration, with overdue
  status:

  ![Calibration due report](images/dashboards/07-calibration-due-report.png)

Every report and every module index supports **Export CSV** for
offline/spreadsheet use, and every printable page respects the same
authorization scope as its interactive counterpart — there's no
report-only backdoor to data you couldn't otherwise see.

---
Previous: [Assets](15-assets.md) · Back to [Getting Started](00-getting-started.md)
