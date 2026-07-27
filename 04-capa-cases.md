# CAPA Cases

Sidebar → **Findings & CAPA → CAPA Cases**. Where [Findings](03-findings.md)
gives every audit finding its own lightweight RCA/CAPA, a **CAPA Case** is
the heavier-weight workflow for a systemic problem — one that didn't
originate from a single audit finding, spans multiple sources, or needs a
dedicated team, formal containment, and revisioned RCA/plan documents with
multi-approver sign-off. The two share the same underlying RCA/CAPA-action
building blocks but the case workflow has more stages:

**Draft → Triage → (Containment) → Investigation → RCA Review → Plan
Review → Implementation → Effectiveness Monitoring → Closure Review →
Closed**, with **Cancel** available through Plan Review and **Reopen for
recurrence** available after Closed.

## List and create

![CAPA Cases index](images/capa-cases/01-index.png)

**New CAPA Case** captures the problem statement, domain, department,
process, owner/coordinator/sponsor, priority, actual/potential severity,
target closure date, and whether containment is required:

![New CAPA case form](images/capa-cases/02-new-form.png)

Saving creates it in **Draft**, with its own reference number
(`CAPA-2026-NNNN`):

![Show, draft](images/capa-cases/03-show-draft.png)

## Triage, containment, investigation

**Triage** (Overview tab) assigns the real owner/coordinator, sets
priority and target closure date, and decides whether containment is
required:

![Triage form](images/capa-cases/04-triage-form.png)

If containment was required, the case moves to **Containment** and the
Overview tab's action becomes a single required note — recording it moves
straight to **Investigation**:

![Investigation, after containment recorded](images/capa-cases/06-show-investigation.png)

## Team, scope, and linked sources

The **Scope, Team & Sources** tab is unique to CAPA Cases: add team
members with a specific role (owner, coordinator, investigator, action
owner, approver, verifier, observer), scope the case to extra
departments/locations beyond the primary one, and link related source
records (a Finding, Incident, etc.) so the case shows what triggered it:

![Team and scope tab](images/capa-cases/07-team-scope-tab.png)

## Root Cause Analysis (revisioned, multi-approver)

**Investigation → RCA Review**: the RCA form is richer than a Finding's —
immediate/direct/underlying/systemic cause fields — and takes **multiple
approvers**, each of whom must approve in sequence. It also supports the
same structured **Fishbone**/**5 Whys** entry a Finding's RCA does
(expand the matching section to add causes or a branching why-step tree)
— since a CAPA case's RCA has no draft state, the whole diagram is built
in this one submit form rather than added incrementally afterward:

![RCA form filled in, two approvers selected](images/capa-cases/08-rca-form-filled.png)
![Fishbone entered on a CAPA case's RCA, expanded inline on the submit form](images/capa-cases/28-rca-fishbone-form-filled.png)
![Fishbone diagram rendered on the RCA revisions list](images/capa-cases/29-rca-fishbone-chart.png)

The Approvals tab shows every step's position — later approvers stay
**Queued** until the one ahead of them decides:

![Two-step approval, one pending one queued](images/capa-cases/10-approvals-tab-two-pending-steps.png)

If an approver ever rejects an RCA/plan revision, the next submission
creates a new **revision** rather than editing the rejected one — the RCA
and Plan tabs both show full revision history, not just the current one.

## CAPA plan and actions

**RCA approved → Plan Review**: draft the plan (summary, target date,
effectiveness criteria, verification method, baseline/target/measurement
method/monitoring period):

![Plan draft form filled in](images/capa-cases/12-plan-draft-form-filled.png)

Add one or more actions to the draft, then submit the whole plan (again
with one or more approvers). When the RCA identified structured causes or
why-steps, each action can optionally be linked to the specific one it
addresses via a **Root cause addressed** select — left blank for actions
broader than a single identified cause:

![Plan action added, ready to submit](images/capa-cases/14-plan-action-added.png)
![Root cause addressed selected on a new action](images/capa-cases/30-plan-action-with-root-cause-selected.png)
![Action list showing which root cause it addresses](images/capa-cases/31-plan-action-addresses-root-cause.png)

Once approved, the case moves to **Implementation** and each action gets a
**Complete** button; a completed action then gets a **Verify** button
(with a result of effective/ineffective) — actions must be verified, not
just completed, before the case can move on:

![Plan tab, action completed and awaiting verification](images/capa-cases/18-plan-action-completed.png)
![Plan tab, action verified](images/capa-cases/19-plan-action-verified.png)

## Effectiveness monitoring

**Implementation → Effectiveness Monitoring** only unlocks once every
mandatory action is verified — schedule the first check with a planned
review date right from the Overview tab:

![Start effectiveness monitoring form](images/capa-cases/20-start-effectiveness-monitoring-form.png)

The **Effectiveness** tab lists every scheduled check; **Record** opens an
inline decision form (effective / partially effective / ineffective, plus
an actual result and comments):

![Effectiveness check recorded](images/capa-cases/23-effectiveness-check-recorded.png)

An **ineffective** decision sends the case back to **Investigation** for
another RCA revision instead of forward — recording it never overwrites a
prior check, it always creates the next one. An **effective** decision
moves the case to **Closure Review**.

## Closing, cancelling, reopening

**Closure Review → Closed** requires a closure rationale and (server-side
guarded) every mandatory action verified:

![Case closed](images/capa-cases/25-show-closed.png)

A closed case can be **Reopened for recurrence** at any time with a
reason — it goes to a distinct **Reopened** status (flagging
`recurrence_flag`) and needs **Resume investigation** to go back into the
normal flow.

Any case from Draft through Plan Review can be **Cancelled** with a
required reason instead of running to completion:

![Case cancelled](images/capa-cases/26-show-cancelled.png)

## Filtering by status

Like every list in the app, filters combine with search and stay visible
so it's clear why the list narrowed:

![Filtered to closed](images/capa-cases/27-index-filtered-closed.png)

---
Previous: [Findings](03-findings.md) · Next: [Quality Objectives](05-quality-objectives.md)
