# Management Reviews

Sidebar → **Management Review → Management Reviews**. Formal management
review meetings: agenda, minutes, decisions, and trackable action items.

## List and create

![Meetings index](images/meetings/01-index.png)

**New meeting** sets the title, purpose, chair, secretary, location,
venue, and an optional meeting link:

![New meeting form](images/meetings/02-new-form.png)

Saves in **Draft**:

![Show, draft](images/meetings/03-show-draft.png)

## Participants and agenda

Add participants with a role (chair/secretary/attendee/invitee — internal
user or an external guest by name/email/company):

![Participants tab](images/meetings/04-participants-tab.png)

Build the agenda before or during the meeting — each item has a title,
category, and optional presenter:

![Agenda item added](images/meetings/05-agenda-tab-item-added.png)

**Add standard IMS agenda** fills in the standard, ISO-aligned 16-point
integrated review agenda in one click — consolidated from the actual
review-input clauses of ISO 9001:2015 §9.3.2, ISO 14001:2015 §9.3, and
ISO 45001:2018 §9.3 (previous actions, changes, interested parties,
customer feedback, objectives, process/environmental/OH&S performance,
NC/CAPA, monitoring & measurement, audit results, supplier performance,
resources, risks & opportunities, communications, and improvement
opportunities). Each item shows which standard clause(s) it comes from:

![Empty agenda with the "Add standard IMS agenda" button](images/meetings/30-agenda-tab-before-standard.png)
![Standard 16-point agenda populated, with clause badges](images/meetings/31-agenda-tab-standard-populated.png)

It's additive and safe to click more than once — a category you've
already added an item for (by hand, or from a previous click) is left
alone rather than duplicated. You can still add your own one-off items
alongside the standard ones, or skip it entirely and build the agenda
by hand as before.

## Scheduling and running the meeting

**Draft → Scheduled** needs a start/end time (a chair, secretary, and at
least one participant are expected first):

![Schedule form filled in](images/meetings/06-schedule-form-filled.png)
![Show, scheduled](images/meetings/07-show-scheduled.png)

**Scheduled → In Progress** is a single click. While in progress, each
agenda item becomes directly editable — record **Notes**, **Discussion**,
and **Decision** right on the item as the meeting happens:

![Agenda item, discussion and decision recorded](images/meetings/09-agenda-item-recorded.png)

## Action items

Add trackable action items at any point, each with an assignee,
department, and due date — independent of the agenda items they came from:

![Action added](images/meetings/10-actions-tab-item-added.png)

Each gets its own reference number (`ACT-2026-NNNN`) and a **Complete**
button with a completion note once assigned, same pattern as CAPA actions.

## Minutes and approval

**In Progress → Minutes Review**: summary, conclusions, decisions, and an
approver:

![Submit minutes form filled in](images/meetings/11-submit-minutes-form-filled.png)
![Show, minutes review](images/meetings/12-show-minutes-review.png)

The approver decides from **My Approval Requests**, exactly as in
[Audits](02-audits.md#approvals). Once approved:

![Minutes approved](images/meetings/13-show-minutes-approved.png)

**Close meeting** then appears — guarded server-side: every open action
item needs an assignee and due date first, or closing is blocked with a
clear message:

![Meeting closed](images/meetings/14-show-closed.png)

Draft, Scheduled, or In Progress meetings can be **Cancelled** with a
required reason (same pattern as every other cancellable record in the
app).

## Duplicating an agenda

**Duplicate agenda for a new meeting** copies the title, purpose, and full
agenda item list into a fresh draft meeting — useful for a recurring
quarterly review — leaving chair/secretary/location/date blank for you to
set on the new one:

![Duplicated meeting, ready to edit](images/meetings/15-duplicated-meeting.png)

---
Previous: [Quality Objectives](05-quality-objectives.md) · Next: [Documents](07-documents.md)
