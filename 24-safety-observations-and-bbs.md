# Safety Observations & BBS

Sidebar → **Operations → Safety Observations & BBS**. A Behavior-Based
Safety (BBS) programme for proactive safety learning — planned and ad hoc
observations, recognition of safe practices, and identification of
at-risk practices and unsafe conditions, without ever defaulting to
"worker error." This is a **learning and prevention system**, not a
disciplinary or surveillance one — see
[architecture.md](../../architecture.md) for the full standards mapping
(ISO 45001/45002/45003/45004) and non-negotiable design principles behind
it.

Seven slices are built so far: the **programme**, **checklist templates**
with a lightweight versioning system that protects a checklist's history
once it's been used, **observations** — the actual walkdown, with per-item
results, contributing factors, and a coordinator review step — **actions**,
with a configurable priority, the hierarchy of controls, a
segregation-of-duties verification step, and stop-work — **cross-module
links** to a related change, incident, training session, or document — a
**coverage & trends report** — and now **observer competency**, shown as of
the observation's own date. This chapter will keep growing alongside the
module.

## List and create

![Programmes index](images/bbs/01-index-empty.png)

**New Programme** picks a department (and optional location), a BBS
Coordinator (the programme owner) and optional management sponsor, an
observation frequency, and the programme's privacy defaults — whether
anonymous reporting is allowed, and whether an observed person's identity
is required — which will govern the observations created under it once
that capability ships:

![New programme form](images/bbs/02-new-form.png)

## The lifecycle: Draft → Active → Suspended/Closed

A new programme starts **Draft**. **Activate** makes it live; an active
programme can be **Suspended** (and later **Reactivated**) or **Closed**
directly. Closing is available from Draft, Active, or Suspended — there's
no need to activate a programme just to close out one that was set up in
error:

![Active programme](images/bbs/03-show-active.png)

Comments are available on every programme (`shared/_comments.html.erb`,
the same partial used throughout this app), for the informal worker- and
sponsor-review discussion a programme's design and scope typically need
before activation.

## Checklist templates and versioning

Sidebar → **Operations → BBS Checklist Templates**. A checklist template
belongs to one programme and holds the questions a future observation will
be scored against. No checklist in this app — PSSR, HAZOP, MOC — protects
a checklist's history from a later edit; BBS is the first to, because an
observation must always be traceable to the exact wording it was answered
against, even after the template changes:

![Checklist templates index](images/bbs/05-checklist-templates-index.png)

**New Template** picks a programme, a name, and whether it's still active.
A brand-new template has no version yet — **Start New Draft Version**
creates one, optionally linking a governing controlled document's current
revision (only documents with a published, effective revision can be
picked, the same rule the [Master Document Register](07-documents.md)
uses):

![A draft version with no items yet](images/bbs/06-checklist-template-draft-v1.png)

While a version is **Draft**, add checklist items one at a time — a
question, an optional category, and whether the item is critical, needs a
comment, or permits a photo:

![Two items added to the draft](images/bbs/07-checklist-template-items-added.png)

**Activate** requires at least one item. Once active, the version becomes
**Effective** and its items are locked — they can no longer be edited or
removed, and any version that was previously effective is marked
**Superseded**:

![Version 1 effective and read-only](images/bbs/08-checklist-template-v1-effective.png)

To make a change, **Start New Draft Version** again — it copies every item
from the current effective version forward as a starting point, so editors
aren't starting from a blank page. The version history at the bottom of
the page keeps every past version visible:

![Version 2 draft, items copied forward from version 1](images/bbs/09-checklist-template-v2-draft-copied.png)

## Recording an observation

Sidebar → **Operations → BBS Observations**. Anyone signed in can report an
observation against any active programme's published checklist —
reporting is deliberately not restricted to a programme's own department,
because consultation only works if reporting is open:

![Observations index](images/bbs/10-observations-index-empty.png)

**New Observation** picks a checklist template (only templates with a
published, effective version are offered), when it happened, and
optionally who or what was observed — a specific person, or a group/area
like "Loading dock team" when the observation isn't about one individual.
Creating it snapshots every item from that checklist's current effective
version, each starting **Unanswered**:

![New observation, items snapshotted and unanswered](images/bbs/11-observation-new-form.png)
![Observation created](images/bbs/12-observation-submitted-unanswered.png)

Each item is answered independently: a result (**Safe**, **At Risk**,
**Not Observed**, **Not Applicable**), three flags that are never folded
into the result — an unsafe condition present, a system contributor
present, a positive practice worth recognizing — a comment (required when
the item calls for one), an optional photo, and one or more **contributing
factors** drawn from a fixed, categorized master list (equipment/design,
procedure/planning, resources/competence, organizational/psychosocial,
individual/task interaction). The factor list exists specifically so an
at-risk finding is never left to default to "worker error":

![An at-risk item with a flag, comment, and contributing factor](images/bbs/13-observation-item-at-risk.png)

## Observer competency

If the observer has any training attendance records, an **Observer
competency** card shows each one against the observation's own date — not
today's date. A record completed and still unexpired *as of when the
observation actually happened* is marked **Valid as of observation date**;
one that had already lapsed by then (even if it looks fine at a glance
today) is marked **Not valid as of observation date**:

![Observer competency, one valid record and one that had already expired by the observation date](images/bbs/24-observer-competency.png)

This is supporting evidence, not a gate — nothing here blocks anyone from
observing or reviewing. It's only visible to someone who could already see
the observer's training records on their own terms (their own records, or
an elevated role) — a department head, for instance, doesn't gain visibility
into someone else's training history just because they can see this
observation.

## Review and closure

Once submitted, a coordinator can review the observation — notes, and a
new status of **Reviewed** or **Closed** — and optionally raise a
**Finding** for formal RCA/CAPA, the same opt-in mechanism PSSR, HAZOP, and
Worker Participation already use. Items lock the moment the observation is
no longer **Submitted**:

![Closed observation with review notes and a linked finding](images/bbs/14-observation-closed-with-finding.png)
![Observations index with a mix of statuses](images/bbs/15-observations-index-listed.png)

The observer can edit their own submission up until it's reviewed. The
observed person — if one was named — can always see the observation
recorded about them, so BBS's feedback loop runs both ways, not just to
the coordinator.

## Actions and stop-work

An observation's **Actions** panel tracks the corrective/preventive work
it generates — one or more actions, each optionally tied to a specific
checklist item. Adding one requires a description, an assignee, a due
date, a **priority** (Low/Medium/High/Critical), and, deliberately, a
**hierarchy of controls** — Elimination, Substitution, Engineering,
Administrative, or PPE — so "retrain the worker" or "issue more PPE" is
never the reflexive, unexamined answer:

![Action added, with a control hierarchy and priority](images/bbs/16-action-added.png)

For anything urgent enough to halt work immediately, **Declare stop-work**
on the action — it flags the action, timestamps and attributes the
declaration, and shows a red banner across the whole observation page
until someone **Lifts** it:

![Stop-work declared, banner shown, second action still open](images/bbs/17-stop-work-declared.png)

**Complete** is available to the assignee (or anyone who can manage the
observation). Completing moves an action to **Completed**, ready for
**Verify** — a genuine segregation-of-duties step: the assignee cannot
verify their own action, even if they otherwise have manage rights:

![Assignee blocked from verifying their own action](images/bbs/18-self-verify-blocked.png)

A different user verifies with an **Effective**/**Ineffective** result:

![Action verified by a different user](images/bbs/19-action-verified.png)

Lifting a stop-work removes the banner as soon as every active order on
the observation is cleared:

![Stop-work lifted](images/bbs/20-stop-work-lifted.png)

Assigning an action always emails the assignee and creates an in-app
notification; completing one notifies the observation's reviewer (or its
observer, if no one has reviewed it yet); declaring a stop-work notifies
the programme owner — the one BBS event urgent enough to alert someone
beyond the assignee.

## Cross-module links

Sometimes an observation genuinely belongs to a wider context — it was
made during an MOC implementation walkdown, it followed up on a reported
incident, it's evidence a training requirement was met, or it references a
specific controlled document. The **Links** panel records that connection
without duplicating data: pick a source type (Management of Change,
Incident, Training Session, or Document), the record's ID, and a role
(Related To, Follow-Up Of, Resulted In):

![A link added, showing its snapshot text and a working link to the source](images/bbs/21-link-added.png)

Anyone who can read the linked record can click through to it directly.
If the source is a confidential incident, the link shows only its neutral
label (e.g. "Near Miss (#42)"), never its title or description — the same
protection CAPA's own cross-module links already give a confidential
source:

![Following a link through to the related incident's own page](images/bbs/22-followed-link-to-incident.png)

The same four record types (and a BBS observation itself) can also link
the other way — a CAPA case or a management review agenda item can
reference a BBS observation as one of its own sources.

## Coverage & trends report

[Reports](16-dashboards-and-reports.md) → **BBS Coverage & Trends**. A
single page of unranked breakdowns — checklist items by result, observations
by programme and by department, observations by status, actions by status,
and the top recorded contributing factors — plus two actionable lists: open
high-potential observations, and any active stop-work orders. Every
breakdown is grouped by a stable key and left in that order rather than
sorted by count, and nothing is ever broken down by individual worker — a
BBS dashboard that ranked people, or that treated a quiet department as "well
behaved," would defeat the module's own purpose:

![BBS Coverage & Trends report](images/bbs/23-coverage-trends-dashboard.png)

The positive-practice count is always shown against the total number of
answered items, not as a standalone percentage, so it's never read out of
context. Like every other report, the counts only ever reflect what the
signed-in viewer can already read — a department member sees their own
department's numbers, not the whole organization's.

## Who can see what

A department head can manage their own department's programmes; a plain
department member can see them but not edit them; the programme's owner
and sponsor can always see it regardless of department. Checklist
templates, versions, and items follow the same rule through their
programme — whoever can manage a programme can manage its templates.
Observations follow it too for department-scoped review, but reporting
itself is open to anyone regardless of department, and every observer
always sees their own submissions. Actions follow the observation's own
scope, plus an assignee can always read and complete their own action —
and the observation it lives on — no matter which department it belongs
to. `ims_admin`, `top_management`,
`incident_manager`, and `corporate_safety_head` can
manage every programme org-wide — the same department/head/all-tier
pattern already used by
[Worker Participation](19-worker-participation.md) and
[Award & Reward](21-award-and-reward.md).

Like every other module, a super admin can turn **Safety Observations &
BBS** off entirely from Masters & admin → Module flags.

---
Previous: [HAZOP](23-hazop.md) · Next: [Nonconforming Output](25-nonconforming-output.md)
