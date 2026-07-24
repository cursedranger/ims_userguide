# Audits

Sidebar → **Audits**. Covers both internal and external (certification
body) audits, from scheduling through report approval.

## List, filter, export

The index is search/filter/sort/paginate like every list in the app, with a
result count and CSV export:

![Audits index](images/audits/01-index.png)

Filtering by status (here, `status = in_progress`) narrows the list and
keeps the filter visible so it's obvious why fewer rows are showing:

![Filtered to in-progress](images/audits/02-index-filtered-in-progress.png)

A filter that matches nothing gets a plain empty state instead of a blank
table — never a dead end:

![No matches empty state](images/audits/03-index-empty-state.png)

## Creating an audit

**New Audit** asks for the kind (internal/external — external also expects
an external certification body), title, objective, scope, and criteria:

![New audit form](images/audits/04-new-form.png)

Saving creates it in **Draft** with an auto-generated reference number
(`AUD-2026-NNNN`) and opens its tabbed detail page — Overview, Schedule &
Team, Departments & Locations, Checklist, Findings, Report, Approvals,
Comments & Activity, and Attachments:

![Show, draft status](images/audits/05-show-draft.png)

## The lifecycle: Draft → Planned → Scheduled → In Progress → Report Review → Completed

Each stage's action lives on the **Overview** tab and only appears to users
who can update the audit.

**Draft → Planned** is a single click:

![Move to Planned](images/audits/06-show-planned.png)

**Planned → Scheduled** requires more than just dates — the app enforces
this before it lets you schedule: at least one department tagged, at least
one location tagged, exactly one **lead auditor**, and at least one **lead
auditee**. Try to schedule too early and you get told exactly what's
missing instead of a generic error:

![Schedule blocked — missing team and tags](images/audits/08-show-scheduled.png)

So the real order is: add your team first —

![Adding a lead auditor / lead auditee](images/audits/10-team-tab-participant-added.png)

— tag departments and locations —

![Departments & Locations tab, saved](images/audits/12-tags-tab-saved.png)

— *then* fill in the schedule dates and submit:

![Schedule form filled in](images/audits/07-schedule-form-filled.png)

Once every precondition is met, scheduling succeeds and notifies every
internal participant:

![Show, scheduled](images/audits/08-show-scheduled.png)

**Scheduled → In Progress** is another single-click action once you're
ready to start (also stamps `actual_start_at`).

## Checklist

From **Checklist**, add a checklist from an active template matching the
audit's kind (internal/external). Each item can be answered **Conforming /
Partially Conforming / Nonconforming / Not Applicable**, with an optional
note, evidence note, and file attachments per item:

![Checklist added from template](images/audits/14-checklist-tab-added.png)

Answering an item stamps who answered it and when:

![Checklist item answered](images/audits/15-checklist-item-answered.png)

Mark an item **Nonconforming** and a **Raise finding** shortcut appears
right on that item, pre-linking the new finding to it.

## Raising a finding from an audit

The **Findings** tab always has its own **Raise finding** button too (not
only from a nonconforming checklist item):

![Findings tab, empty](images/audits/17-findings-tab-empty.png)
![Raise finding form](images/audits/19-raise-finding-form-filled.png)

The finding is created and immediately linked back to this audit — see
[Findings](03-findings.md) for its own lifecycle from here.

## Submitting the report

**In Progress → Report Review**: fill in the **Report** tab's summary and
conclusion and pick an **Approver**. This is also gated — every checklist
item across every checklist on the audit must be answered (or marked N/A)
before the report can be submitted:

![Report blocked — checklist incomplete](images/audits/26-show-report-review.png)

Once the checklist is complete, the same form submits successfully and
creates an approval request:

![Submit report form, filled](images/audits/25-submit-report-form-filled.png)

## Approvals

The **Approvals** tab shows the full approval history for the audit —
here, one pending step:

![Approvals tab, pending](images/audits/27-approvals-tab-pending.png)

The approver sees it queued on their own **My Approval Requests** page:

![My Approval Requests, pending review](images/audits/28-my-approval-requests-pending.png)

**Review** opens the request with the same approval-history view plus
**Approve** (optional comment) and **Reject** (comment required) forms:

![Approval request detail](images/audits/29-approval-request-show.png)

Approving records the decision on the step and, since this was the only
approver, completes the request — which moves the audit itself straight to
**Completed** and stamps `actual_end_at`:

![Audit completed](images/audits/31-show-completed.png)

*(A finding raised from this audit is still open independently — closing
an audit doesn't close its findings; see [Findings](03-findings.md).)*

## Downloadable audit report (PDF)

Once an audit is **Completed** *and* every finding it raised is closed or
rejected, the **Report** tab grows a third card with a generated PDF —
identification, team, checklist responses, the summary/conclusion, every
finding's RCA/CAPA resolution, and the approval history, all in one
document:

![Report tab with the generated PDF and its download log](images/audits/33-report-tab-with-pdf.png)

The PDF is generated automatically in the background the moment both
conditions are met — completing the audit, or closing/rejecting the last
open finding, whichever happens second — nobody has to click a "generate"
button. **View report** opens it inline; **Download report** downloads it
and logs the download (who, when, from which IP) in the card right below,
and in the full **Report Download Log** under Audits in the sidebar:

![Audit Report Download Log, filterable and exportable](images/audits/34-report-download-log-index.png)

If the audit isn't there yet, the card explains exactly what's still
outstanding (audit not completed yet, or how many findings remain open)
instead of just hiding the button.

## Comments and attachments

Every major record in the app (not just audits) has the same **Comments**
and **Attachments** panels — shown here once as the canonical example.
Comments support an **Internal** flag (visible only to internal roles) for
audit managers/top management, and attachments accept multiple files at
once:

![Comment posted](images/audits/22-comment-posted.png)
![Attachments, empty state](images/audits/23-attachments-empty.png)

## Cancelling an audit

Draft, Planned, Scheduled, or In Progress audits can be cancelled with a
required reason — Report Review and Completed audits can't (a report
already under approval or finished has to run its course):

![Audit cancelled](images/audits/32-show-cancelled.png)

---
Previous: [Masters & Admin](01-masters-and-admin.md) · Next: [Findings](03-findings.md)
