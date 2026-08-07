# Incidents

Sidebar → **Operations → Incidents**. Incidents, near misses, and
complaints across quality/environmental/OH&S domains, from first report
through investigation to closure — reusing the same central CAPA
structure as everything else instead of a separate incident-only
RCA/CAPA.

## List and create

![Index](images/incidents/01-index.png)

**New** captures the kind (quality incident / environmental incident /
injury-illness / near miss / unsafe act / unsafe condition / other),
domain, when it occurred, a title, initial description, department,
location, any immediate action already taken, and whether this was a
**Loss of Primary Containment (LOPC)** — defaults to No, and for every
incident that isn't one, nothing below in this chapter applies: no extra
tab, no extra validation, no extra notification. **Report incident**
submits it directly; **Save as draft** (shown for a new record only) lets
you hold it before it's officially reported:

![New form](images/incidents/02-new-form.png)
![Show, reported](images/incidents/03-show-reported.png)

## Triage

**Reported → Investigation**: set actual/potential severity,
confidentiality, responsible manager, lead investigator, the
reportability decision, and whether a CAPA will be required:

![Triage form filled in](images/incidents/04-triage-form-filled.png)

An incident that crosses into high/critical severity for the first time
here automatically notifies everyone holding the **Incident Manager**
role — no separate step needed.

## People & Witnesses

Record everyone involved — internal users or external people by name —
each with a role (affected person, injured person, witness, reporter,
supervisor, other) and an involvement summary. A separate
**sensitive/medical notes** field keeps that detail apart from the general
record. For an **Injured Person**, an extra **Injury** column lets you
pick a classification — FAC, MTC, RWC, LTI-NR, LTI-R, or Fatality — right
from the row; setting one is what reveals the Medical tab below,
independent of whether this incident is an LOPC:

![People added](images/incidents/06-people-tab-added.png)
![Injury classification set on an injured person](images/incidents/17-people-injury-classified.png)

## Process Safety (LOPC)

Once **LOPC** is set to Yes on an incident, a **Process Safety** tab
appears. **Edit Process Safety details** captures Fire/Explosion damage
(₹) — entering an amount also tags the incident as **Fire** — and
Community/Onsite evacuation. **Chemical release** supports adding as many
entries as needed, each a chemical (from the Chemical Master, managed
under Administration like any other master) and a quantity released in
kg:

![Process Safety tab with chemical release recorded](images/incidents/20-process-safety-chemical-release.png)

The app classifies the incident automatically every time any of this
changes — you never set the Tier yourself. **Tier 1**: a Fatality among
the incident's injured people, a Community Evacuation, or Fire/Explosion
damage over ₹1 Crore. **Tier 2**: a Lost Time Injury (Reportable), an
Onsite Evacuation, Fire/Explosion damage between ₹10 Lakhs and ₹1 Crore,
or a large enough total chemical release. Any other LOPC incident is
**Tier 3** — an LOPC is always at least a Process Safety Event. The
highest matching tier always wins, and the classification card shows
exactly which trigger(s) fired:

![PSE Tier 2 with triggers and reason shown](images/incidents/19-process-safety-tier2.png)

The LOPC and Tier badges also appear right in the incident header, and on
the Dashboard's new **Process Safety** card (LOPC incidents, PSE Tiers 1-3,
Fire incidents, Process Safety Events, and chemical releases this year —
each linking straight to the filtered incident list).

## Medical (OHC Doctor workflow)

Whenever one or more people are classified as injured (see **People &
Witnesses** above — this runs independent of LOPC), a **Medical** tab
appears and a Medical Task is assigned to the OHC Doctor. It runs fully
in parallel with Investigation/RCA/CAPA — it never replaces or gates
them, only the incident's own closure at the end:

![Medical task created for the OHC Doctor](images/incidents/21-medical-tab-task-created.png)

**Form B** is mandatory for every classification, including a First Aid
Case. If **Hospital referral** is Yes, **Follow-up required** is
automatically set to Yes too:

![Form B submitted, follow-up and hospital referral both Yes](images/incidents/22-medical-form-b-submitted.png)

Whenever follow-up is required, **Form B1** becomes available — submit as
many as needed until one is marked **Further follow-up required: No**,
which closes the chain:

![Form B1 follow-up chain closed](images/incidents/23-medical-form-b1-closed.png)

**Form E** (medical fitness) is required for every classification except
a First Aid Case, and records the doctor's fitness determination before
the incident can close:

![Form E fitness result recorded](images/incidents/24-medical-form-e-submitted.png)

Injury and Process Safety/Medical notifications reach a wider audience
than a typical incident: the OHC Doctor, Corporate Safety Head/Team, Top
Management, the incident's own department head, and the heads of the
Safety/HR/Production/Engineering departments — with daily reminders to
the assigned doctor and escalation to that same wider list once a Medical
Task is overdue.

## Investigation

While in **Investigation**, record the chronology, evidence summary,
immediate/direct/contributing/systemic causes, lessons learned, and
recommendations:

![Investigation form filled in](images/incidents/07-investigation-form-filled.png)

Submitting completes the investigation and moves the incident onward —
straight to **Closure Review** if no CAPA is required, or to **Actions In
Progress** if one is:

![Show, actions in progress](images/incidents/08-show-actions-in-progress.png)

### When no investigation is warranted

Not every incident earns one. A housekeeping near miss corrected on the
spot produces paperwork, not learning. From the Actions panel of a triaged
incident, **No investigation required** records that decision with a
mandatory reason; it is stamped with who decided it and when, appears on
the Overview and Investigation tabs, and is written to the comment thread.

It is only available to the people who hold full control of the incident
(Incident Manager, IMS Admin, Top Management, or the head of the
incident's department) — not to the reporter or the lead investigator of
that incident. It is not offered at all for a **high or critical severity**
incident, when triage decided CAPA was **required**, or once an
investigation has been completed.

The waiver does not close anything. It removes exactly one closure
condition — the completed investigation — so the incident can go
**Request closure → Closure Review → Close incident** with no
investigation, no finding and no CAPA case. Every other closure check
below still applies. **Reinstate investigation** withdraws the waiver
while the incident is still open, and reopening a closed incident
reinstates it automatically.

## Root Cause Analysis

Once an investigation is recorded, the **Root Cause Analysis** tab offers
the same structured RCA a [Finding](03-findings.md#root-cause-analysis)
gets — pick a method (5 Whys, Fishbone, Fault Tree, Bow-Tie, Barrier
Analysis, or Other), write the problem statement, and either save as an
editable draft or submit it for approval (the **Approver** field is the
draft/submit switch, same as Findings). This is a separate, lighter RCA
from the one a linked CAPA Case gets if one is opened — useful for an
incident that doesn't rise to needing a full CAPA case, or as an earlier
working analysis before one is opened. Approving or rejecting this RCA
doesn't itself move the incident's own status; that stays governed by the
investigation:

![Incident RCA form filled in, fishbone expanded](images/incidents/13-rca-form-filled.png)
![Fishbone diagram rendered on the RCA tab](images/incidents/14-rca-fishbone-chart.png)

## Linked CAPA

If CAPA was marked required, **Create linked CAPA case** appears once —
it opens a brand-new [CAPA Case](04-capa-cases.md) already linked back to
this incident, ready to run its own full triage → investigation → RCA →
plan → effectiveness → closure workflow:

![Linked CAPA tab](images/incidents/09-linked-capa-tab.png)

## External notifications

Track regulator/authority notifications required by the incident —
authority, decision (pending/required/not required/sent), due date, sent
date, method, reference, and acknowledgement:

![Notification added](images/incidents/10-notifications-tab-added.png)

## Closure

**Request closure** is guarded on three things at once: the investigation
must be completed, every notification decision must be resolved (none
left **Pending**), and — if a CAPA was required — its linked CAPA case
must already be **Closed**. If any injuries were recorded, closure adds
three more checks, per person: Form B must be submitted; Form E must be
submitted unless the classification was a First Aid Case; and if a Form
B1 follow-up chain was ever required, it must be closed (a submission
with **Further follow-up required: No**). Try too early and you get told
exactly what's outstanding, same pattern as every other guarded
transition in the app:

![Closure blocked pending the linked CAPA case](images/incidents/12-show-closure-blocked-by-capa.png)

Once every condition clears, **Request closure → Closure Review → Close
incident** works the same as Audits/CAPA Cases. A closed incident can be
**Reopened** with a reason; Draft/Reported/Investigation-stage incidents
can be **Cancelled** with a reason instead.

---
Previous: [Compliance Obligations](10-compliance-obligations.md) · Next: [Management of Change](12-management-of-change.md)
