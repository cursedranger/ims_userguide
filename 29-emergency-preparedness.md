# Emergency Preparedness & Response

Sidebar → **Operations → Emergency Preparedness** (and **Emergency
Drills**). ISO 14001:2015 §8.2 and ISO 45001:2018 §8.2 — the same clause
number in both standards — require an organization to identify the
potential emergency situations it could face, plan how it will respond,
**periodically test that response**, and review and revise the plan, in
particular after a drill or after a real emergency.

This module is the proactive half of that clause. The reactive half was
already built and is unchanged: [Incidents](11-incidents.md) records the
emergency that actually happened, and the [first aid &
emergency register](28-ohc-employee-health-records.md) records who was
treated. A response plan here is what those are measured against.

## The register

The index lists every response plan with its risk score, status and
**drill readiness** — Drill current / Drill due soon / Drill overdue,
computed from the plan's own drill history rather than stored. Alongside
the usual search, type, status and department filters there is a
**Readiness** filter for the three states worth chasing (drill overdue,
drill due soon, plan review overdue), and CSV export of exactly what is
on screen.

A plan that is in force and has **never been drilled at all** counts as
overdue. That is deliberate: an untested emergency plan is precisely
what the periodic-testing requirement exists to catch. (First aid kits
and hazard-exposure surveillance treat "never done" the same way.)

## Creating a response plan

**New response plan** captures two things: the potential emergency
situation, and the planned response to it.

The situation half is a title, a scenario type (fire, explosion,
chemical spill, gas leak, environmental release, natural disaster,
medical emergency, utility failure, structural collapse, security
threat, other), the owning department and location, the plan owner, a
description of what could happen, the potential environmental/OH&S
impacts, and a **likelihood × severity** rating using the same
admin-configurable risk matrix [Risks &
Opportunities](09-risks-and-opportunities.md) and [Environmental
Aspects](18-environmental-aspects.md) use. If the plan responds to an
environmental aspect assessed under the **emergency** operating
condition, link it here — that is the ISO 14001 §6.1.2 → §8.2 trail an
auditor follows.

The response half is the prevention/mitigation measures, the **planned
response actions** (required — a plan with no planned response is the
one thing this record must never be), the required resources and
equipment, the external agencies and mutual-aid arrangements, an
optional link to the controlled document holding the full written
procedure, and the **drill and review frequencies** in months, which
drive both clocks.

## Naming the response team

On the **Response Team** tab, add the responders: internal users, or
external responders such as the district fire station or the nearest
hospital, each with a contact number and their responsibilities during
the response. Roles run from Incident Controller and Deputy through
Fire Warden, Floor Marshal, First Aider, Spill Responder, Rescue Team,
Communication Coordinator, Security Coordinator and External Agency
Liaison.

Exactly one person can hold **Incident Controller** on a plan.

## Putting the plan into force

**Activate plan** moves a draft to *Active*. It refuses until the team
exists *and* names an incident controller — the guard that stops a plan
being "in force" while nobody is actually accountable for running the
response. Activation emails every internal responder telling them which
role they hold, and starts the review clock from the plan's own review
frequency.

Anyone named on the plan can read it from then on, whatever department
they belong to — a response plan its own responders cannot open is not a
plan.

**Retire plan** takes a plan out of force with a required reason. It
refuses while a drill is still open (cancel the drill first), and it
never deletes anything: the drill and review history stays on file, the
plan simply drops off both clocks.

## Drills

**Plan drill** (from the plan, or its Drills tab) creates a drill
against a plan that is in force. Pick the drill type — mock drill,
tabletop, evacuation, full scale, equipment test, notification test —
the coordinator, when it is scheduled for, the objectives, and the
simulated situation participants will be told has happened. Tick or
untick **Announced**, and record whether an external agency is taking
part.

Add named participants on the drill's **Participants** tab (internal
users, or external agency responders). A site-wide evacuation does not
need everyone named — the headcount is recorded with the evaluation
instead.

**Schedule drill** notifies the plan's response team. For an
**unannounced** drill it notifies *only* the response team, never the
drill's own participant list — telling the participants in advance is
exactly what an unannounced drill is testing against.

### Recording the evaluation

Once the drill has run, **Record the drill evaluation** captures when it
was actually conducted, the headcount, the response and evacuation times
in minutes, what was observed (required), the weaknesses identified, and
an **effectiveness** verdict of Effective / Partially Effective /
Ineffective. Two optional decisions sit alongside it:

- **The response plan needs revising** parks the plan at *Under Review*.
  It stays there until a plan review is recorded — so "under review"
  always means a revision is genuinely outstanding, not just a label.
- **Raise a finding for RCA/CAPA** creates a linked
  [Finding](03-findings.md) from the drill, the same opt-in escalation
  path PSSR rejections and BBS reviews use. It is never automatic: not
  every partially effective drill warrants formal corrective action, and
  that judgment is the coordinator's.

A drill cannot be recorded as conducted in the future, or before the
date it was scheduled for. A completed or cancelled drill is no longer
editable; only attendance can still be marked.

The standalone **Emergency Drills** list covers every drill across every
plan, filterable by type, status, effectiveness, department and
scheduled date range, with CSV export.

## Plan reviews

On the plan's **Plan Reviews** tab, **Record a plan review** logs what
prompted the review — Periodic, Post Drill, Post Emergency or Change
Driven — optionally linking the drill or the incident behind it, the
review findings, whether the planned response was actually revised (and
what changed), and the next review date. Left blank, the next date comes
from the plan's own review frequency, so the clock never silently stops.

Recording a review returns an *Under Review* plan to *Active*. A review
that concludes the planned response is inadequate can raise a Finding
for RCA/CAPA, the same opt-in checkbox the drill evaluation offers.

## Reminders, report and dashboard

A daily job watches both clocks. While a drill or review is merely due
soon it reminds the plan owner; once either is genuinely overdue it
escalates to the owner, the department head and safety leadership. It
stays quiet about a drill that is already on the calendar — the clock is
being acted on.

**Reports → Emergency Preparedness** shows plans by scenario type and
status, drill coverage and effectiveness over a date range, the plans
overdue for a drill or a review, and every drill conducted in range,
with CSV export.

The organization dashboard carries a card with plans whose drill is
overdue, plan reviews overdue, and drills currently scheduled.

## Who can do what

| Role | Access |
|---|---|
| IMS Admin, Top Management, Corporate Safety Head, Incident Manager | Manage plans, teams, drills and reviews |
| Corporate Safety Team | Manage drills; **read** plans — they run and evaluate the test without authoring, activating, retiring or reviewing what is being tested |
| Audit Manager, Auditor | Read the whole register organization-wide |
| Department head | Manage their own department's plans and drills |
| Department member | Read their department's plans and drills |
| Plan owner | Read and update their own plan, any department |
| Named responder / drill participant | Read the plan or drill they are named on, any department |

The module is gated by the **Emergency Preparedness & Response** module
flag; with it off, the sidebar links, the report and every URL are
blocked for everyone.
