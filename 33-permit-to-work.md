# Permit to Work

Sidebar → **Permit to Work**. The controlled authorisation of non-routine
work: an issuer prepares the equipment and raises a permit, a configurable
chain of approvers clears it, an acceptor takes it at the job location, gas
tests and shift renewals keep it honest while the work runs, and every
signing authority signs it closed.

The module ships configured against a real controlled format
(`DPLD-G00-HES-306-0001` guideline, `DPLD-G00-GEN-402-0033` form), but none
of that is hard-coded — see [Setting your permit format](#setting-your-permit-format).

## Who does what

| Role | What they get |
|---|---|
| Any signed-in employee | Raise a draft permit; read, edit, submit and cancel their own drafts |
| **Issuer** | Controls the whole permit process: submits, suspends/resumes, records gas tests, signs, and closes their own permits |
| **Acceptor** | Takes the permit at the job, completes the acceptor check sheets, gives the toolbox talk, signs |
| **Area executive / field operator** | Opens the permit at the equipment, records gas tests, signs the closure |
| Approvers (I, II, III) | Decide the permits routed to them, from their own **My Approval Requests** queue |
| **HSEF Officer** | Reads the whole register, and may stop any job — the guideline grants that to anyone with permit knowledge |
| **Permit Issuer** | Reads the whole register, issues and suspends |
| Department head | Manages every permit for their department |
| IMS Admin / Top Management | All of the above, plus all five configuration screens |

Two roles are new with this module: `hsef_officer` and `permit_issuer`.

## Raising a permit

**Permit to Work → Work Permits → Raise a Permit.**

**Permit form.** Pick the format this job is raised on. A plant runs more
than one — a hazardous-job format and a non-hazardous one are different
controlled documents asking different questions — and the choice decides which
sections the permit then shows you. It is **frozen once the permit exists**, so
reprinting it years later shows the questions it was actually issued against;
cancel and raise a new permit to change it. If only one form is published the
picker is filled in for you.

Three formats ship seeded, and an admin can add or edit any of them from
**Permit Forms**:

| Form | Document number |
|---|---|
| Permit to Work | `DPLD-G00-GEN-402-0033` |
| Permit to Work — Hazardous job (Category 1) | `HSE F 42/02/01.04.2026` |
| Permit to Work — Non-hazardous job (Category 2) | `HSE F 43/02/01.04.2026` |

**Section A — type of permit.** Tick every type that applies. Several types
may be clubbed onto one permit *for the same location, team and agency*, as
the form's own note allows. When you do, the permit takes the **shortest**
validity of them and **every** chosen type's requirements — so clubbing hot
work onto a 7-day cold work permit gives you a 12-hour permit that also
needs a gas test and a fire watcher, not a week-long one. The validity is
clamped automatically; you do not have to work it out.

Types cannot be changed after the permit is raised: they decide its
validity, its approvers and its check sheets. Cancel and raise a new permit
if they are wrong.

**Section B — job particulars.** Plant, area, equipment tag and name, job
description, planned headcount, the maximum number of people in a confined
space at one time, and the agency. If another job is running on the same
equipment or area, name it in **Conflicting work** — it prints on the permit.

**Signing authorities.** Issuer, area executive / field operator, and the
acceptor. The acceptor is usually contractor staff with no login, so you can
name them three ways and exactly one must be used:

- a **contractor worker** from the roster — checked against their gate pass
  and medical clearance;
- an **employee**;
- a **typed name** — allowed (a permit at 2 a.m. must not be blocked by a
  missing roster row) but recorded and printed as *unverified*.

**Sections C–E** are whatever your permit form asks — nature of work, hazard
considerations, the issuer's and acceptor's preparation checkpoints, the
initial gas test, PPE. Checkpoints are **Yes / No / N/A**, deliberately not a
tick box: "not applicable" and "not answered" are different facts on a safety
document.

The permit is created as a **draft**. Nothing is authorised yet.

## Submitting for approval

**Submit for approval** runs every pre-issue guard, because it is the last
moment before other people are asked to sign something. It refuses, with a
specific message, when:

- the issuer or acceptor is not named;
- a permit needing a fire watcher or a stand-by person has none;
- a **critical** checkpoint is answered No or left blank;
- a **critical** check sheet item is answered No;
- any roster-linked crew member's gate pass does not clear them to work;
- **no approver can be resolved.**

That last one is worth stating plainly: unlike visitor approvals, an
unconfigured chain here does **not** auto-approve. The message tells you
whether nothing is configured (an admin problem) or a level resolved to
nobody (a staffing problem — a department, its parent department or the site
has no head assigned).

Approvers decide from **My Approval Requests**, the same queue every other
approval in this app uses. A rejection returns the permit to draft so it can
be corrected and resubmitted; the rejected request stays in the history.

## Opening the permit at the job

Approval and activation are separate on purpose. Approval is a signature;
**Open permit at the job** is the acceptor physically taking the permit at
the location and the field operator opening it. The gap between the two is
where a paper system loses permits.

Opening refuses if the authorisation window has already passed, or — for a
permit whose types require one — if no gas test has been recorded or the
most recent one is outside limits.

## While the work runs

**Gas tests.** Recorded against the permit as a log, because "the gas test
passed" is a statement about a moment and re-testing is the control. O₂ and
LEL are range-checked against the limits your form defines. A reading outside
limits can still be recorded — suppressing it would be worse — but only with
an explicit note saying why it is being accepted, and **an out-of-limits
reading on an active permit suspends the permit automatically** and notifies
the issuer and field operator.

**Check sheets** attach themselves to the permit from the types you chose,
and their questions are *copied onto the permit* at that moment. Revising a
check sheet template next year never rewrites a sheet completed last year.
Eight ship seeded — hot work, confined space entry, work at height, excavation
clearance, LOTOTO, lifting & shifting, radiography work and line breaking — and
**Check Sheets** is where you edit them or add your own.

**The toolbox talk** is its own signed record with the crew-facing
checkpoints, not a tick box — "was a TBT given" is the first question asked
after an incident.

**Suspension** stops the job without ending the permit, and is deliberately
the most widely available action in the module: anyone named on the permit,
plus any HSEF officer, may suspend it. It needs a reason and is reversible.
Resuming returns the permit to *active*, not *approved* — the crew is
already at the job and should not have to open it twice.

**Renewals.** The guideline requires every signing authority again on
renewal, and a change of any signing authority forces one. So a renewal is
**re-approval, not a date edit**: it goes through the same approval chain as
the original permit, and the permit's validity only moves once the renewal
is fully approved. This is heavier than editing a date, and that is the
point.

**Expiry.** A sweep runs every 15 minutes. A permit past its window becomes
*expired* and everyone named on it is notified. An expired permit is
deliberately **not closed** — the job may be half-done with equipment still
isolated, and marking it closed would assert a restoration nobody performed.
It stays on the awaiting-closure list until the signatures arrive.

## Closing and cancelling

Closure needs four distinct signatures in the order the form prints them:
**acceptor → area executive / field operator → fire watcher (hot work only)
→ issuer**. The close action names exactly who is still outstanding rather
than closing early.

For hot work, **Record end of fire watch** starts the 30-minute observation
period the form's own declaration requires. The permit cannot close until it
has elapsed, and the screen tells you how many minutes are left.

**Cancellation** is one-way and needs a coded reason — the guideline's three
mandatory triggers plus *Other*:

| Reason code | When |
|---|---|
| `emergency_declared` | On-site/off-site emergency declared |
| `incident_at_work_area` | Any accident or incident at the permit work area |
| `permit_conditions_violated` | Permit conditions were violated |
| `other` | Anything else — a description is required |

Coding it is what lets the compliance report answer "how many permits were
cancelled for a conditions violation this quarter" without anyone reading
free text. A permit is never deleted; cancelled records that it existed and
why, which is exactly what an audit reads.

## The permit board

**Permit to Work → Permit Board** is the module's operational landing page,
with five count tiles over the same list: **awaiting my approval**, **active
now**, **expiring within the hour**, **awaiting closure**, and **suspended**.
Each tile filters the list below it, and every view has the full
search/filter/sort/pagination and CSV export the rest of the app has.

**My Work** additionally shows the permits naming *you* that are live, with
an expiring-or-expired panel above it.

## Printing

**Print** gives you the permit as the controlled format **it was raised on** —
each form prints as itself.

A form describes its own sheet: as well as its questions, it can carry its
particulars grid, its signature blocks (with the wording each authority signs
against), its attachments menu and its overleaf approval log. So a Category 1
permit prints "Name of the Permit Issuer" above HSE F 42's own declaration and
an overleaf with that form's normal and extension approval columns, while the
DPL form keeps its Sections A–H. A form that says nothing about its sheet
prints the built-in Sections A–H layout described below.

- **Sheet 1 (A4 portrait)** — the header block with your document number and
  the permit number, Section A's type boxes, Section B's job particulars, then
  every section of the form it was issued against: the nature-of-work and
  hazard tick grids, the issuer and acceptor preparation columns side by side,
  the clearing gas test, the PPE block and remarks. Sections F and H follow
  with every authority's declaration above its name/sign/date/time box, then
  the documents and check sheets menu.
- **Sheet 2 (A4 portrait, read sideways)** — the gas test log running
  alongside the shift extension block, then the closing confirmation that
  hazards and controls were ensured by the issuer, the area executive, the
  acceptor and the contractor. The table is twenty-eight columns wide, so it is
  **rotated within the page**: turn the sheet clockwise to read it. The log is
  ruled all the way down the sheet — rows already recorded print filled in and
  the rest print blank, because the permit is carried to the job and written on
  there. A permit renewed past the end of one sheet gets another.

The two sheets are the two sides of one piece of paper, which is why both are
portrait. Print **A4 portrait, double-sided, at 100% scale**, with "print
backgrounds" on so the section bands come out grey. Don't set the print dialog
to landscape — the overleaf table turns itself.

The toolbox talk is not printed. It is a signed record in its own right and is
read on the permit's screen; the paper form's overleaf has no room for it once
the shift log is laid out upright.

Each print is logged against the permit with who took it and when, because the
form is a controlled document issued in triplicate.

Two settings control how it prints, both under **Permit to Work**:

| Setting | Where | What it does |
|---|---|---|
| **Controlled document number** | Permit Forms → edit the form | Fills the "Doc. No." box at the head of every permit. |
| **Print cols** | Permit Forms → version builder → each section | How many items that section lays across the A4 sheet. Separate from **Cols**, which is the on-screen data-entry width — the paper form runs its hazard list seven across but the entry screen never should. Leave it blank to use the screen width. |

## Setting your permit format

This is the part most organisations need to change first. **The permit form
is data, not code** — you edit it from the app, and no developer is involved.

Five configuration screens, all under **Permit to Work** and all limited to
IMS Admin / Top Management:

| Screen | What it decides |
|---|---|
| **Permit Forms** | The sections and questions every permit asks |
| **Permit Types** | Which kinds of permit exist and what each one requires |
| **Approval Matrix** | Which approvers each type needs, and when |
| **Check Sheets** | The named check sheets that attach to a permit |
| **Shifts** | The working day, which the approval matrix escalates by |

### The form designer

> **Building or revising a form?** [Designing a Permit
> Form](34-designing-a-permit-form.md) walks the whole job through with
> screenshots — deciding your sections and who fills them, making a checkpoint
> critical and a gas limit enforceable, copying blocks between forms,
> signature declarations, publishing and revising. What follows here is the
> reference summary.

The version builder is a designer, not just a list of settings.

**The sheet it will print sits beside the form you are building**, rendered
through the print's own code rather than a lookalike — so what you see is what
comes out of the printer. Every edit redirects back to the builder, so the
preview is rebuilt on each change with no "refresh" to remember. A badge says
whether the form is using **its own layout** or the **built-in Sections A–H**
one, and a block with no questions on it is called out rather than silently
missing from the sheet.

**Copy a block from another form** is the fastest way to build a new format.
Your formats overlap heavily — the same PPE grid, the same hazard list, the
same declarations — so pick any block from any other form and it is copied in
whole, questions and all. It is a **copy, never a link**: change it here and
the form it came from is untouched. Copy the same block twice and the keys are
made unique for you.

### Editing the form itself

**Permit to Work → Permit Forms.** A form is **versioned**: a permit freezes
the version it was issued against, so a permit issued last month always
displays the questions it actually asked. That is why you never edit a live
form directly.

1. Open the form and click **Start a new draft**. The draft is a **full copy**
   of the published one — revising a form in practice means changing two
   checkpoints out of a hundred and thirty, and you should not have to retype
   the rest.
2. Edit the draft: add, edit, reorder or remove **sections** and **questions**.
3. Click **Publish this form**. The previous version is archived in the same
   moment. Permits already issued are untouched; the next permit raised uses
   the new version.

Only one draft may exist at a time, and only a draft can be discarded. A
form with no questions cannot be published — it would issue a blank permit.

**Sections** carry:

- a **title** and a stable **key**;
- **renders as** — `fields`, `tick list`, `checkpoints` (the Yes/No/N/A
  table), `gas test`, or `signatures`;
- **filled by** — issuer, acceptor, approver, field operator, or anyone.
  This is what reproduces the paper form's two-column E1-issuer /
  E2-acceptor split;
- **only for these permit types** — leave everything unselected and the
  section shows on *every* permit; select some and it shows only on permits
  carrying one of them. This is how "confined space only" rows work, with no
  rules engine;
- **columns** (1–4) for layout.

**Questions** carry a label, a **key**, a type, and:

- **required** — must be answered;
- **critical** — must be Yes or N/A before the permit can be issued. Only
  available on Yes/No/NA, checkbox, dropdown and number questions; a
  "critical" free-text box could never be evaluated as pass or fail;
- **minimum / maximum / unit** on a number question — this is what makes
  "O₂ 19.5–23.5%" and "%LEL 0% only" *enforceable* rather than advisory. A
  reading outside the range is refused;
- **only for these permit types**, same as sections;
- **filled by**, same as sections.

Question types are drawn from the paper form rather than from a generic form
builder: `text`, `textarea`, `number`, `date`, `datetime`, `select`,
`multiselect`, `checkbox`, `yes_no_na`, `signature_block`.

> **Keys are permanent.** A question's answers are stored keyed by its
> `field_key`, so the key cannot be changed once the question exists —
> renaming it would silently orphan every answer already recorded under the
> old one. The screen disables the field and says so. The same applies to a
> permit type's `key`, which sections, check sheets and approval rules all
> target by name.

### Adding or changing permit types

**Permit to Work → Permit Types.** Each type carries the conditions that
make it that type:

- **maximum validity (hours)** — a permit takes the shortest across its
  clubbed types;
- **gas test before work starts**;
- **fire watcher**, with the 30-minute observation before closure;
- **stand-by person / entry attendant**;
- **copy recorded with HSEF** for emergency response;
- a **badge colour** for the board.

A type that permits have already been issued against is deactivated rather
than deleted — the register has to keep saying what those permits authorised.

### Changing who approves what

**Permit to Work → Approval Matrix.** Rendered as the matrix itself — permit
types down, Approver I / II / III across — because that is the artifact an
HSEF manager reviews and signs, and a flat list of thirty rows hides the one
cell that is wrong.

Each cell is required **Every time**, **After a named shift**, or **Not
required**. `After a named shift` means "required once that shift has
ended", evaluated by the shift order you set on the Shifts screen — so a
permit starting in a shift ranked after it needs that approver.

Each level is filled by one of:

| Source | Resolves to |
|---|---|
| Head of the work executing department | Approver I — the sectional / area manager |
| Head of the parent department | Approver II — the plant HOD |
| Site head / factory manager | Approver III — `Site#head` |
| Every holder of a role | Everyone with that role, approving in parallel |

The seeded matrix reproduces the source guideline exactly — for example hot
work needs Approver I every time, Approver II after G-shift and Approver III
after B-shift; confined space inert and fire water isolation need all three
every time.

> **A time that matches no configured shift escalates.** It is treated as
> requiring the signature rather than skipping it — silently dropping an
> approver because the shift master is incomplete is the failure this matrix
> exists to prevent. Keep the Shifts screen complete.

### Shifts

**Permit to Work → Shifts.** Per site, because each plant runs its own
clock. Give each shift a code, a name, a start and end time, and an **order
within the working day** — that order is what "after G-shift" *means*, so an
organisation whose night shift is called something else still gets the right
escalation. A shift ending earlier than it starts is a night shift crossing
midnight and is handled normally.

Shifts referenced by an approval rule are deactivated rather than deleted.

### Check sheets

**Permit to Work → Check Sheets.** Each sheet has a name, a **filled by**
party, the **permit types** it applies to, and its questions — each of which
can be **critical** (a No blocks issue) or **comment required**.

Unlike a form section, a check sheet applying to *no* permit type applies to
nothing rather than everything: a check sheet is about a particular hazard,
so an unscoped one is a configuration mistake, not a universal sheet.

**Auto-attach** (on by default) attaches the sheet to every permit carrying
one of its types. **Duplicate** copies a sheet — inactive and not
auto-attaching — so you can revise it without touching the one already on
live permits.

Six check sheets ship seeded, built from the source guideline's own
Dos & Don'ts and role responsibilities: hot work, confined space entry, work
at height, excavation clearance, LOTOTO, and lifting & shifting. The
guideline *names* fifteen but reproduces the contents of none — they are
separate controlled documents — so the remaining nine are yours to enter
here from your own copies.

## Reports

**Reports → Work Permit Compliance** covers how the permit system is being
run rather than how many permits exist: permits by status, type and
department; **why permits were cancelled**; **permits expired without
closure**; permits **currently suspended**; **gas tests outside limits**; and
a count of people named by typed name only, whose gate pass could not be
checked.

## Notifications

Every permit event — opened, suspended, resumed, closed, cancelled, expired,
gas test failed, renewal requested, renewed — notifies the issuer, acceptor,
field operator and raiser in-app and by email, plus HSEF officers for the
types that require an HSEF copy. Nobody is notified about their own action.
The wording of every message is editable in the **Email Template Designer**
under the `work_permit.*` keys.
