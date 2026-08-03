# Audit Programme (Planning)

Sidebar → **Audits → Audit Programme**. ISO 9001:2015 §9.2.2 a) requires an
organization to *plan, establish, implement and maintain an audit
programme* — and ISO 19011:2018 §5 describes what that programme is: the
set of audits agreed up front for a period, covering what will be audited,
where, when, and by whom. The same requirement appears at §9.2 of ISO
14001:2015 and ISO 45001:2018.

That is a different record from the audits themselves. [Audits](02-audits.md)
already covered *running* an audit. This module covers **deciding a year's
worth of audits in advance and then having them appear on their own**, which
is the half auditors ask to see evidence of: not "here are the audits you
did", but "here is the plan you agreed, and here is every audit it produced".

Three record types, in order:

| Record | What it is |
|---|---|
| **Audit programme** | The plan for a period — the year, the window, who owns it, and how far ahead audits should open. |
| **Planned audit** (an entry in the programme) | One intended audit: title, kind, scope, departments, locations, dates, lead auditor, checklist template. Nothing executable. |
| **Audit** | The real record, opened automatically from the planned audit, which then runs its normal `draft → planned → scheduled → …` lifecycle. |

## The programme list

The index lists every programme with its year, period, owner, how many
audits are planned, how many have been opened, and status — with the usual
search, year/status/owner/period filters, result count, CSV export and an
empty state.

The CSV adds a **Coverage %** column: opened audits as a share of the
programme's non-cancelled entries. That single number is the "is the
programme actually being implemented?" evidence §9.2.2 asks for, and it is
also shown as a progress bar on the programme's Overview tab.

## Creating a programme

**New programme** captures the period and the automation:

- **Title**, **year**, and **programme owner** (normally the audit manager).
- **Period start / period end** — every planned audit has to fall inside
  this window, so a programme's own dates are what bound its schedule.
- **Open audits this many days ahead** (`auto_create_lead_days`, default
  14) — the lead time. Once the programme is approved, each planned audit
  turns into a real audit record this many days before its start date. Set
  it to how long your auditors need to assign the team and confirm dates.
- **Programme objective** and **programme scope** — what the year's auditing
  is meant to achieve and what it covers (standards, processes, sites).

The programme is created as a **Draft** with an auto-generated reference
(`APG-2026-0001`). Comments, attachments and full PaperTrail history work
here exactly as they do everywhere else in the app.

## Adding the planned audits

**Add planned audit** (Draft only) captures one intended audit:

- **Audit title**, **kind** (internal/external — external requires an
  external party, the certification body), and an optional **external party**.
- **Frequency** — `one time`, `monthly`, `quarterly`, `half yearly` or
  `annual`. See [Repeating audits](#repeating-audits) below.
- **Planned start / planned end** — validated against the programme's own
  period, and the date inputs are already bounded by it.
- **Lead auditor** (optional at planning time — "Decide later" is a real
  answer in January) and a **checklist template**, copied into the audit
  when it opens.
- **Departments** and **locations** — multi-select. At least one of each is
  required before the programme can be submitted for approval, because an
  audit cannot be scheduled without them.
- **Standards audited against** and **clauses in scope** — see
  [Standards and clauses](#standards-and-clauses) below.
- **Objective**, **scope** (required), **criteria** and **notes**.

Scope is required here, not just on the audit: an audit cannot move from
Draft to Planned without one, so a planned audit with no scope could never
open. The module refuses to let you plan something that could not happen.

## Repeating audits

Most programmes are not a list of one-offs. "Audit Production every quarter"
is one decision, and typing it four times is how a quarter gets forgotten.

Set a **frequency** on a planned audit and it becomes a *series*. While the
programme is a draft, the Planned Audits tab tells you what it will become
("1 repeating audit will expand into 4 dated audits on approval") without
cluttering the list yet. On approval, the series expands into one dated
planned audit per interval:

| Frequency | Interval |
|---|---|
| One time | No repeat — a single audit. |
| Monthly | Every 1 month |
| Quarterly | Every 3 months |
| Half yearly | Every 6 months |
| Annual | Every 12 months |

A quarterly audit starting 5 January, in a January–December programme,
becomes four planned audits: 5 Jan, 5 Apr, 5 Jul, 5 Oct — each numbered
(`#1`, `#2`…) and each keeping the length you planned. **The series stops at
the programme period end**; an occurrence that would run past it is never
created, so a quarterly audit starting in November produces one audit, not
four spilling into next year.

Everything is copied to each occurrence: title, scope, criteria, departments,
locations, standards, clauses, lead auditor and checklist template. From that
moment each occurrence is an ordinary planned audit — open it early, cancel
just Q3 with a reason, or let each one open on its own lead time. Cancelling
one quarter does not touch the others.

Expansion happens at **approval**, not when you type the frequency, so you
can still move the first date while drafting without leaving stale
occurrences behind. It is also safe to repeat: a series that has already
expanded is never expanded again.

## Standards and clauses

Free-text criteria are fine to read and useless to search. A planned audit
also links to the **standards** it is run against (ISO 9001, ISO 14001,
ISO 45001 — whatever is in your Standards master) and optionally the specific
**clauses** in scope. Leave clauses blank to mean the whole standard.

Both are copied onto the audit when it opens, shown on the audit's Overview
tab, and — the point of doing it this way — the **Audits index filters by
standard and by clause**, and the CSV export includes both columns. "Show me
every audit this year that covered ISO 9001 §8.4" becomes a filter instead of
a reading exercise, which is exactly the question a certification auditor
asks about programme coverage.

## Approval — what makes the plan binding

`Draft → Submitted → Active`. **Submit for approval** picks the approver
(normally top management) and raises the same `ApprovalRequest` used
everywhere else in the app, visible on the Approvals tab.

Submission is guarded. It refuses unless:

- at least one planned audit exists;
- every planned audit has at least one department and one location tagged;
- every external planned audit names an external party.

Those are exactly the conditions an audit needs to reach **Scheduled**, so
approving the programme means every audit it will open is genuinely
schedulable.

When the approver approves, the programme becomes **Active** and generation
starts immediately. If the approver rejects it, the programme goes back to
**Draft** for reworking, with the rejection recorded on the Approvals tab.

**Entries can only be added, edited or removed while the programme is a
Draft.** Once it is approved, the agreed schedule changes by *cancelling* a
planned audit with a reason — never by quietly rewriting it. That is the
whole point of approving a plan.

## How audits get created automatically

This is the part the module exists for.

A background job runs **every day at 06:00** (`config/recurring.yml` →
`AuditPrograms::GenerateDueAuditsJob`) and, for every **active** programme,
opens the real audit for any planned audit that has reached its lead time —
`planned start date − lead days`. The Planned Audits tab shows that date in
its **Opens** column, with a **Due** badge once it has arrived.

06:00 is deliberately before the 07:00 reminder jobs, so an audit opened
this morning is already visible to today's reminders.

When an audit is opened, it inherits from the planned audit:

- title, kind, objective, scope, criteria and external party;
- scheduled start/end dates taken from the planned window;
- every tagged department and location;
- the standards and clauses it is audited against;
- the lead auditor, added as a participant (who is notified, as usual);
- the checklist template, copied into the audit's **own** items — so later
  edits to the template never alter an audit already opened.

The new audit is created as a draft and then taken to **Planned** through
the same `Audits::Plan` guard a hand-raised audit goes through. It stops
there on purpose: **scheduling it is still a human decision.** Automation
opens the file and fills it in; it does not commit your auditors to dates or
tell an auditee the audit is confirmed. The programme owner gets an in-app
notification and email for each one ("Planned audit opened…").

Two manual overrides exist for when you do not want to wait:

- **Open any due audits now** (Overview tab) runs the same sweep for this
  programme only.
- **Open audit now** (on a planned audit row) opens one audit immediately,
  ahead of its lead time — the "the customer moved the date forward" case.

Both are safe to click twice: a planned audit can only ever open one audit,
enforced by a row lock and a database unique index, so a double-click, a
retried job and the nightly sweep cannot produce duplicates.

Once opened, the two records stay linked in both directions: the planned
audit row shows **Opened as AUD-…** with its live status, and the audit's
own Overview tab shows **Planned under APG-…**.

## Changing the lead auditor after the audit opens

A name chosen in January goes stale — leave, resignation, or a conflict of
interest nobody foresaw. On the audit's **Schedule & Team** tab, **Change
lead auditor** picks the replacement and requires a reason.

It is deliberately *not* "remove the participant, add another". The lead
auditor seat keeps its identity, so the tab grows a **Lead auditor history**
showing every handover — who changed it, when, from whom to whom, and why.
Nothing about who was originally responsible is lost, which is the whole
reason this is a distinct action.

Both people are notified: the incoming lead auditor that they now hold the
audit, the outgoing one that they have handed it over. Anyone already on the
audit in another role (say, an auditor being promoted to lead) vacates that
seat automatically. The lead auditor of a **completed or cancelled** audit
cannot be changed — that record is closed.

## Cancelling and closing

- **Cancel a planned audit** (reason required) — for an audit that will not
  happen. Only possible while it is still waiting: once the audit has been
  opened it is a live record with its own cancellation, and rewriting the
  plan to say it was never planned would destroy the trail.
- **Cancel the programme** (reason required) — cancels the programme and
  every planned audit that had not yet opened. Audits already opened are
  deliberately left alone.
- **Close programme** — the end-of-period close. It **refuses while any
  planned audit is still waiting**: an entry that was never opened and never
  cancelled would otherwise vanish silently, and "why didn't this audit
  happen?" is precisely the question a programme review has to answer. Open
  it or cancel it with a reason first.

## Who can do what

| Role | Access |
|---|---|
| `audit_manager`, `top_management` | Manage programmes and planned audits — the audit function owns the plan. |
| `auditor`, `ims_admin` | Read every programme and planned audit. |
| Department head | Read any programme that plans an audit of **their** department. |
| Programme owner | Always read/update their own programme. |
| Named lead auditor | Always read the programme they are planned into, whatever their role. |

Everything else is read-only by design: an auditee finding out *when* they
are due to be audited is the point; changing the plan is not.

`AuditProgram` also appears in the [Access Control
Matrix](17-access-control-matrix.md) so an admin can widen access further.
It has no **Department** scope option there — a programme reaches
departments only through its planned audits' tags, which is not a column
that system can express — so it offers **Own** (programmes you created) and
**All**.

The whole module sits behind the `audit_programs` module flag, so a super
admin can switch it off entirely; audits then work exactly as before.

## Gotchas

- **Nothing generates until the programme is approved.** A Draft programme
  with perfectly good dates produces no audits. This is intentional — an
  unapproved plan is a proposal.
- **A programme approved late catches up immediately.** Approving in March a
  programme whose first audit was due in February opens that audit at once
  rather than waiting for the next nightly run, so a late approval never
  silently skips the audits it was late for.
- **Lead time is per programme, not per audit.** If one audit genuinely
  needs eight weeks of notice and the rest need two, plan its start date
  eight weeks early or open it by hand.
- **Planned dates become the audit's *scheduled* dates, but the audit is
  still only Planned.** Someone has to schedule it for participants to be
  notified.
- **A cancelled programme does not cancel audits already opened.** Those
  are live records; cancel each one on the audit itself if that is what you
  mean.
- **A repeating audit expands once, at approval.** Changing the frequency
  after approval does not re-expand the series — entries are draft-only
  editable by design. Cancel the occurrences you no longer want, or plan the
  next period properly in next year's programme.
- **A series never runs past the programme period.** If you want a quarterly
  audit to continue into next year, that is next year's programme — which is
  what a programme *is*.
- **Changing the lead auditor on the audit does not change the plan.** The
  programme records who was planned; the audit records who actually led it,
  and the history explains the difference. That is intentional.
