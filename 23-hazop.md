# HAZOP (Hazard and Operability Study)

Sidebar → **Operations → HAZOP**. IEC 61882's structured technique for
identifying hazards in a process or operation: a facilitated team
divides the process into "nodes," then systematically applies guide
words (No, More, Less, As Well As, Part Of, Reverse, Other Than, Early,
Late) to process parameters (flow, pressure, temperature, etc.) at each
node to find credible deviations, their causes/consequences, existing
safeguards, a risk ranking, and any follow-up action. A HAZOP commonly
happens *before* [Management of Change](12-management-of-change.md) or
[PSSR](22-pssr.md) — it identifies the hazards a design change must
address.

## Create and set up the team

![Index](images/hazop/01-index.png)

**New study** picks a department, an optional related asset and MOC, a
title, and a scope/objective describing what the study will cover:

![New study form filled in](images/hazop/02-new-form-filled.png)
![Draft study, before scheduling](images/hazop/03-show-draft.png)

Add the study team on the **Team** tab — internal users or external
subject-matter experts — before scheduling. Exactly one team member can
hold the Facilitator role per study:

![Team member added](images/hazop/04-team-added.png)

## The lifecycle: Draft → Scheduled → In Progress → Completed → Closed

**Schedule** requires a study date and at least one team member.
Scheduling emails every internal team member immediately, the same
notification pipeline Safety Meetings use:

![Study scheduled](images/hazop/05-show-scheduled.png)

**Start study** is a single click once the session begins:

![Study in progress](images/hazop/06-show-in-progress.png)

## Nodes and deviations

On the **Nodes & Deviations** tab, add a node — the process section under
examination, with a reference and description:

![Node added](images/hazop/07-node-added.png)

For each node, pick a guide word and parameter to add a deviation (e.g.
"More Pressure"). Add as many as the walkdown finds:

![Deviation added](images/hazop/08-deviation-added.png)

**Edit assessment / action** captures the full picture: causes,
consequences, existing safeguards, a likelihood × severity risk score
(the same admin-configurable risk matrix Risks & Opportunities and
Environmental Aspects use), a recommendation (left blank if safeguards
are judged adequate), an action owner and due date, and an action status.
A genuine safety gap can optionally raise a Finding for formal RCA/CAPA,
the same opt-in mechanism [PSSR](22-pssr.md) rejections use:

![Deviation fully assessed](images/hazop/09-deviation-assessed.png)

**Complete study** requires at least one node with a recorded deviation:

![Study completed](images/hazop/10-show-completed.png)

**Close study** is blocked while any deviation still has an Open or
In Progress action — the study can't be closed with loose ends, mirroring
how Management of Change can't close early. Once every action is
Completed or marked Not Required, closing succeeds:

![Study closed](images/hazop/11-show-closed.png)

A Draft or Scheduled study can also be **Cancelled** with a reason.
Closed and Cancelled are terminal.

## HAZOP report (PDF)

Once a study is Completed (or Closed), a downloadable PDF report is
generated automatically in the background — no extra button to click.
The **Overview** tab shows a card with **View report** and **Download
report** once it's ready (a badge shows "Generating…" for the few
seconds it takes, or a failure reason if generation ever fails):

![HAZOP report card on the Overview tab](images/hazop/12-report-tab.png)

The report itself opens with the study's identification and team, then a
**diagrammatic node overview** — every node as its own colored panel,
banded red/orange/green by that node's highest deviation risk score, so
the whole study's structure is visible at a single glance before drilling
into any one node's detail:

![HAZOP report PDF — study details, team, and node overview](images/hazop/13-report-pdf-page-1.png)

Every node then gets its own full worksheet page: each deviation's
causes, consequences, existing safeguards, risk score, recommendation,
and action owner/status.

---
Previous: [PSSR](22-pssr.md) · Back to [Getting Started](00-getting-started.md)
