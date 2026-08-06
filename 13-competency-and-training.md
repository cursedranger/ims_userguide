# Competency & Training

Sidebar → **Operations → Competency Requirements** and **Training**.

## Competency Requirements

The reference list of competencies the organization expects — each
optionally scoped to a role and/or department, with a required level and
a renewal period:

![Index](images/training/01-competency-index.png)
![New form](images/training/02-competency-new-form.png)
![Show](images/training/03-competency-show.png)

These are reference data (like a skills matrix), not individually
workflow-tracked — actual training against them is recorded on Training
Sessions below.

## Training Sessions

List, filter, create:

![Index](images/training/04-training-index.png)

**New** captures the topic, provider, trainer, date, location, and
status, plus the pass mark and number of attempts to use *if* an
assessment is run later:

![New form](images/training/05-training-new-form.png)
![Show, scheduled](images/training/06-training-show-scheduled.png)

There are two trainer fields, and they do different jobs. **Trainer
(external)** is free text, for a third-party trainer with no account here.
**Trainer (internal)** picks a real user — that person can then validate
attendance and run the assessment for this session without holding any
special role. The **Training Coordinator** role can do the same across
every session.

## The attendance register

A session runs as a roster, not a single form. **Add attendees** books
people on; each is notified and starts as **Not marked**:

![Roster, nobody marked yet](images/training/10-roster-invited.png)

### The attendee marks themselves present

**On the day of the training** (and only that day) each attendee sees
their own panel at the top of the session page:

![Attendee's check-in panel](images/training/11-attendee-check-in.png)

**Mark me present** records their claim — it does not confirm it. The
badge moves to **Awaiting validation** until the trainer rules on it:

![Attendance marked, awaiting validation](images/training/12-attendee-marked.png)

The same prompt appears in the attendee's **My Work** on the day, so they
don't have to go looking for the session:

![My Work training panels](images/training/23-my-work-panels.png)

### The trainer confirms or rejects it

The trainer (or coordinator) sees every claim and decides:

![Trainer's view of an unvalidated claim](images/training/13-trainer-awaiting-validation.png)

**Confirm present** marks them **Present** and records who confirmed it.
It also works for someone who never logged in at all, so nobody is
penalised for forgetting.

**Mark absent** requires a reason — it overrides what the attendee
recorded about themselves and costs them the certificate, so the record
has to say why. The attendee is notified of the reason:

![Presence confirmed](images/training/14-presence-confirmed.png)

A session **cannot be completed** while any claim is still awaiting a
decision — the trainer is told exactly whose.

## The assessment (optional)

Most training needs no test. The **Assessment** tab starts out saying so,
and a toolbox talk can be completed on attendance alone:

![No assessment](images/training/15-assessment-optional.png)

**Add an assessment** puts it in **draft** — the paper is built but
nothing is visible to attendees yet:

![Assessment draft](images/training/16-assessment-draft.png)

### Building the question paper

**Add question** takes the question, the points it is worth, and its
answer type:

![Question builder](images/training/17-question-builder.png)

- **Single answer** — the classic MCQ. Exactly one option must be ticked
  as correct.
- **Multiple answers** — several correct options, marked
  **all-or-nothing**: the attendee has to select the whole correct set, so
  ticking every box scores zero.

Tick every correct option; leave an option's text blank to drop it. The
paper shows what you've built, with correct answers ticked (only ever
visible to whoever can edit it):

![Question paper](images/training/18-question-paper.png)

### Triggering it

**Trigger assessment** releases the paper. It refuses if any question
would be unanswerable — fewer than two options, no correct option, or a
single-answer question with two correct ones — and tells you which
question and why.

It goes **only to attendees already confirmed present**. Someone still
awaiting validation is told the assessment opens to them once their
attendance is confirmed. Questions lock once it's open:

![Assessment open with results](images/training/19-assessment-open.png)

### Sitting it

The attendee starts from the session page or **My Work**. Radio buttons
for single-answer questions, checkboxes for multiple:

![Sitting the assessment](images/training/20-sitting-the-assessment.png)

Answers are graded the moment they submit, and **that attempt can't be
changed afterwards**. The result shows the score against the pass mark,
with every answer marked up and the correct ones shown:

![Assessment result](images/training/21-assessment-result.png)

Attempts are capped by the session's **attempts allowed**. Somebody who
has already passed can't sit it again; somebody who hasn't can retake
until they run out. The **best** score across attempts is the one that
counts.

**Close assessment** stops further attempts. An attempt still in progress
at that moment is submitted and graded as it stands, rather than being
thrown away.

## Completing the session, and certificates

**Mark completed** settles the whole roster in one action and issues
certificates:

![Session completed](images/training/22-session-completed.png)

Each attendee's **result** is worked out from presence and, where there
was one, the assessment:

| Presence | Assessment | Result | Certificate |
|---|---|---|---|
| Confirmed present | None required | Attended | Yes |
| Confirmed present | Passed | Passed | Yes |
| Confirmed present | Not passed | Failed | No |
| Confirmed present | Never sat it | Failed | No |
| Marked absent, or never marked | — | No show | No |

Two things worth knowing:

- A result you set **by hand** before completing is never overwritten.
- Someone confirmed present who never sat a required assessment is
  recorded as **Failed**, not Attended — "Attended" counts as a valid
  qualification everywhere else in the app, and they haven't earned it.

Certificates carry a `TRC-YYYY-NNNN` number and are downloadable as a PDF
by the attendee (from their own panel) and from the register. Nothing is
issued by hand.

Expiring/expired attendance records feed the **Training Expiry** report
and the dashboard's Operations Alerts — see
[Dashboards & Reports](16-dashboards-and-reports.md). Set an attendee's
**expiry date** on their row to drive that; **Expiring soon** and
**Expired** badges then appear automatically.

The same MCQ machinery is used for controlled documents — see
[Documents → Read & Understood](07-documents.md#read--understood-assessments).

---
Previous: [Management of Change](12-management-of-change.md) · Next: [Suppliers](14-suppliers.md)
