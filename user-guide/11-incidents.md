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
location, and any immediate action already taken. **Report incident**
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
record:

![People added](images/incidents/06-people-tab-added.png)

## Investigation

While in **Investigation**, record the chronology, evidence summary,
immediate/direct/contributing/systemic causes, lessons learned, and
recommendations:

![Investigation form filled in](images/incidents/07-investigation-form-filled.png)

Submitting completes the investigation and moves the incident onward —
straight to **Closure Review** if no CAPA is required, or to **Actions In
Progress** if one is:

![Show, actions in progress](images/incidents/08-show-actions-in-progress.png)

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
must already be **Closed**. Try too early and you get told exactly what's
outstanding, same pattern as every other guarded transition in the app:

![Closure blocked pending the linked CAPA case](images/incidents/12-show-closure-blocked-by-capa.png)

Once every condition clears, **Request closure → Closure Review → Close
incident** works the same as Audits/CAPA Cases. A closed incident can be
**Reopened** with a reason; Draft/Reported/Investigation-stage incidents
can be **Cancelled** with a reason instead.

---
Previous: [Compliance Obligations](10-compliance-obligations.md) · Next: [Management of Change](12-management-of-change.md)
