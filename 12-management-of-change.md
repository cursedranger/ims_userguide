# Management of Change

Sidebar → **Operations → Management of Change**. Controlled evaluation of
planned (or emergency) changes to process, equipment, materials,
personnel, or documentation, from business case through implementation
and verified closure.

## List and create

![Index](images/moc/01-index.png)

**New** captures type, nature (planned/emergency), duration
(permanent/temporary — with an expiry date if temporary), title, business
reason, detailed description, department, location, process, owner,
sponsor, risk tier, pre-change risk score, proposed dates, benefits,
assumptions, and a **required rollback/contingency plan**:

![New form](images/moc/02-new-form.png)
![Show, draft](images/moc/03-show-draft.png)

## The lifecycle

**Draft → Assessment → Approval → Approved → Readiness → Implementing →
Verification → Closure Review → Closed**, with **Cancel** available
through Readiness, plus **Rollback** and (for temporary changes) **Extend
temporary change** while implementing.

**Submit for assessment** moves it out of draft:

![Show, assessment](images/moc/04-show-assessment.png)

### Impact assessments

Add a discipline-specific impact assessment (quality, environmental, OH&S,
operations, engineering, legal/compliance...) and assign an assessor:

![Assessment added](images/moc/05-affected-tab-assessment-added.png)

**Complete** it inline with an impact level, findings, and required
controls:

![Assessment completed](images/moc/06-affected-tab-assessment-completed.png)

### Requesting approval

**Request approval** is disabled until every impact assessment is
completed — pick one or more approvers, all of whom must approve:

![Request approval form](images/moc/07-request-approval-form.png)
![Show, pending approval](images/moc/08-show-approval.png)

Approvers decide from **My Approval Requests**, as everywhere else. Once
approved:

![Show, approved](images/moc/09-show-approved.png)

### Readiness and implementation

**Move to readiness**, then add pre-implementation actions (each
optionally **mandatory**) — **Start implementation** stays disabled until
every mandatory pre-implementation action is done:

![Pre-implementation action added](images/moc/11-actions-tab-pre-implementation-added.png)
![Show, implementing](images/moc/13-show-implementing.png)

While implementing, **Record deviation** logs anything that didn't go to
plan plus the decision taken — it doesn't block implementation, it just
keeps a record:

![Deviation recorded](images/moc/14-implementation-tab-deviation-recorded.png)

Add post-implementation actions the same way as pre-implementation ones;
a completed action gets a **Verify** control with an effective/ineffective
result:

![Post-implementation action verified](images/moc/15-actions-tab-post-implementation-verified.png)

### Verification and closure

**Mark implementation complete → Verification**: record success
criteria, measured outcome, unintended consequences, and a decision:

![Verification form filled in](images/moc/17-verification-form-filled.png)

An effective decision moves it to **Closure Review**, where **Close MOC**
stays disabled until every mandatory post-implementation action is done:

![Closure review](images/moc/18-show-closure-review.png)
![Closed](images/moc/19-show-closed.png)

---
Previous: [Incidents](11-incidents.md) · Next: [Competency & Training](13-competency-and-training.md)
