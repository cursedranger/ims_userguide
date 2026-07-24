# Findings

Sidebar → **Findings & CAPA → Findings**. A finding can be **Open Point**,
**Observation**, **Opportunity for Improvement**, **Minor NC**, or **Major
NC** — the last two are nonconformities and always require a Root Cause
Analysis; the other three can optionally bypass RCA with a justified,
approved exception. Findings are usually raised from an [Audit](02-audits.md)
(as shown there) but can also arrive from a noncompliant Compliance
Evaluation or a serious Incident, reusing this exact same workflow.

## List and filter

Search by title/reference, and filter by status, kind, or department:

![Findings index](images/findings/01-index.png)
![Filtered to open](images/findings/02-index-filtered-open.png)

## The lifecycle

**Open → Assigned**: reassign the owner from the Overview tab at any time
(not just while open):

![Finding, open, overview](images/findings/03-show-open-overview.png)
![Finding assigned](images/findings/04-show-assigned.png)

### Root Cause Analysis

**Assigned/RCA Required → RCA Review**: from the **Root Cause Analysis**
tab, pick a method (5 Whys, Fishbone, Fault Tree, Bow-Tie, Barrier
Analysis, or Other), write the problem statement, analysis, identified
root cause, contributing causes, and immediate correction, and pick an
approver:

![RCA form filled in](images/findings/06-rca-form-filled.png)

Submitting moves the finding to **RCA Review** and creates an approval
request — visible on the RCA tab itself alongside the form (now hidden
until the decision comes back):

![RCA pending approval](images/findings/08-rca-tab-pending-approval.png)

The approver reviews and approves it from **My Approval Requests** exactly
as shown in [Audits](02-audits.md#approvals) — once approved, the finding
automatically advances to **CAPA Required**.

*(Open points/observations/opportunities only: while status is Assigned or
RCA Required, an update-permitted user can instead submit a **Bypass RCA**
request from the Overview tab with a justification and an approver —
nonconformities can never skip RCA.)*

### CAPA plan and actions

**CAPA Required → CAPA Review**: the **CAPA** tab's form takes a target
date, approver, plan summary, effectiveness criteria, verification method,
and up to three initial actions (type, description, assignee, due date,
priority) — the first action row is required:

![CAPA plan form filled in](images/findings/11-capa-plan-form-filled.png)

Submitting moves the finding to **CAPA Review** pending the approver's
decision. Once approved, the finding moves to **Implementation** and the
plan/actions become visible with **Mark complete** per action (with a
completion note):

![CAPA tab, approved plan and actions](images/findings/14-capa-tab-approved-plan.png)

You can also **Add action** to the plan at any time while the finding is
in Implementation, beyond the initial set.

### Effectiveness and closure

**Implementation → Effectiveness Review**: once every action is completed,
**Request effectiveness review** appears on the Overview tab (it's hidden
— and the underlying transition guarded server-side — until every action
reports `completed`):

![Effectiveness review requested](images/findings/16-show-effectiveness-review.png)

The verifier records a decision — **Effective** or **Ineffective** (a note
is required if ineffective):

- **Effective** clears the finding to close.
- **Ineffective** sends it back to **Implementation** for more corrective
  work — the same actions panel reopens for further action.

![Effectiveness decision recorded](images/findings/17-show-after-effective-decision.png)

**Close finding** only appears once the finding is actually closable
(effectiveness confirmed, or RCA bypass approved for non-nonconformities):

![Finding closed](images/findings/18-show-closed.png)

## Rejecting a finding

Anyone with verify permission can reject a finding at any point before
closure, with a required reason — this is a terminal state, same as
Closed, and cannot be undone:

![Finding rejected](images/findings/19-show-rejected.png)

## Comments, attachments, aging

Findings carry the same [Comments and Attachments](02-audits.md#comments-and-attachments)
panels as every other record. **Age** (days since raised) and **overdue**
(past due date) are both shown on the Overview tab and roll up into the
**Finding Aging & CAPA Effectiveness** report — see
[Dashboards & Reports](16-dashboards-and-reports.md).

---
Previous: [Audits](02-audits.md) · Next: [CAPA Cases](04-capa-cases.md)
