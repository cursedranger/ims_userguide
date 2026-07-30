# OHC / Employee Health Records

Sidebar → **Operations → OHC / Health Records**. This is a larger planned
Occupational Health Center platform being built as a sequence of slices —
this chapter covers what's built so far: an **Employee Medical Master**
profile per employee, **OPD/Clinic visit** records, basic **fitness
management**, and now **Pre-Employment and Periodic Medical Examinations**
with structured test results, auto-scheduled due dates, and fitness
certificates. Hazard-based surveillance programs, vaccination, pharmacy,
laboratory integration, contractor gate-pass compliance, health campaigns,
and role-specific dashboards are not built yet — see
[architecture.md](../architecture.md) §11.19 for the full roadmap.

**This is a confidential module.** Only the OHC doctor and org-wide admin
roles can create or edit a record; safety leadership can read but not
edit; an employee can only ever see their own profile and visit history.
Critically, a department head gets **no special access** here despite
managing that department everywhere else in this app — a genuine
deviation from every other module, because a manager should never see
their reports' medical findings.

## Creating a medical profile

![Index](images/ohc-employee-health-records/01-index.png)

**New medical profile** links an employee to their medical master record
— employment type, shift, a primary hazard exposure category, blood
group, and emergency contact details:

![New profile form](images/ohc-employee-health-records/02-new-form.png)
![Profile created](images/ohc-employee-health-records/03-show-created.png)

## Recording a visit

The **Visit History** tab records each OPD consultation — walk-in or
appointment, symptoms, vitals, diagnosis, treatment, prescription, and
optionally a **fitness result**: Fit, Fit With Restrictions, Temporarily
Unfit, or Permanently Unfit. Fit With Restrictions and Temporarily Unfit
both require restriction details. Checking **Notify supervisor** sends the
employee's department head a notification of the fitness outcome — the
one deliberate crack in this module's otherwise doctor/admin-only
visibility, and only ever the fact that a fitness decision was made, not
the underlying clinical notes:

![Visit recorded](images/ohc-employee-health-records/04-visit-recorded.png)

## Examination requirements

Sidebar → **Operations → OHC Examination Requirements**. A requirement
maps a hazard exposure category to how often a Periodic Medical
Examination (PME) is due, and which structured tests it should include —
"None" doubles as the baseline requirement that applies to everyone
regardless of hazard. A Pre-Employment requirement (no frequency, since
it's a one-time gate rather than recurring) can be configured the same
way:

![Examination requirements index](images/ohc-employee-health-records/06-requirements-index.png)
![New requirement form](images/ohc-employee-health-records/07-requirement-new-form.png)

## Recording an examination

The **Examinations** tab records a Pre-Employment or Periodic exam, with
up to four structured test results (Spirometry, Audiometry, Vision, ECG,
Chest X-ray, Lab Test, or Other — each with a Normal/Abnormal/Borderline
result and a free-text value) and an overall fitness decision:

![Examinations tab](images/ohc-employee-health-records/08-examinations-tab.png)

Recording a **Periodic** exam auto-schedules the next one: if an active
requirement exists for the employee's hazard exposure category, the next
due date is computed from the exam date and stored on the record — this
is "auto-scheduling" in the sense of computing when the next exam is due,
not a background job that creates future exam records on its own:

![Examination recorded, next due date computed](images/ohc-employee-health-records/09-examination-recorded.png)

Every examination has a **Download certificate** link — a one-page
fitness certificate (PDF) with the employee, exam, and test details plus
the fitness decision.

## Overdue tracking

[Reports](16-dashboards-and-reports.md) → **OHC Examinations Due** lists
every employee medical profile by their latest periodic exam's next due
date, with an Overdue badge once it's passed — the same "computed from an
actual record, never a manually toggled flag" convention
[Assets](15-assets.md)'s own calibration-due tracking already uses.
Recording a fresh periodic exam resolves the overdue status immediately,
the same way a new calibration record clears an overdue asset:

![OHC Examinations Due report](images/ohc-employee-health-records/10-examinations-due-report.png)

## Who can see what

`ohc_doctor`, `ims_admin`, and `top_management` manage every profile,
visit, examination, and examination requirement. `corporate_safety_head`/
`corporate_safety_team` can read profiles/visits/examinations but not edit
them, and not the examination requirement master at all (that's
configuration, not personal data) — the same read-only oversight they
already have on incident-medical records. Every employee can read only
their own profile, visits, and examinations, and cannot create or edit
one themselves — a deliberate break from this app's usual "anyone can
report" pattern, since a clinical record has to be doctor-authored:

![An employee viewing their own record](images/ohc-employee-health-records/05-employee-own-record.png)

Like every other module, a super admin can turn **Employee Health
Records** off entirely from Masters & admin → Module flags.

---
Previous: [Design & Development](27-design-and-development.md) · Back to [Getting Started](00-getting-started.md)
