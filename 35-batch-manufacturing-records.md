# Batch Manufacturing Records (EBMR)

Sidebar → **Manufacturing (EBMR)**. The electronic batch manufacturing
record: what you make, the approved instruction for making it, the record of
how one batch was *actually* made, and the quality unit's decision to release
it. ICH Q7 §6.4–6.7 and §8, 21 CFR 211.184–211.192, Schedule M Part I.

This is the module a Schedule M, ICH Q7 or FDA inspection asks for by name.
It replaces the paper batch record — not the laboratory. If you run a LIMS,
test results and certificates of analysis stay there; this module holds the
manufacturing record and attaches the CoA to the lot.

> **Three module flags.** *Materials & Lots*, *Batch Manufacturing Records
> (EBMR)* and *Deviations* switch on independently, so a site can run the
> material and lot register without batch records if that is all it needs.

## Contents

1. [Who does what](#who-does-what)
2. [Materials](#1-materials) — the master everything else points at
3. [Goods receipt and lots](#2-goods-receipt-and-lots)
4. [Master Batch Records](#3-master-batch-records) — the approved instruction
5. [Running a batch](#4-running-a-batch)
6. [Deviations](#5-deviations)
7. [Batch review and release](#6-batch-review-and-release)
8. [The printed batch record](#7-the-printed-batch-record)
9. [Genealogy](#8-genealogy)
10. [Electronic signatures](#9-electronic-signatures)
11. [Reports](#10-reports)

---

## Who does what

Four roles matter here, and the separation between them is the point rather
than an inconvenience. ICH Q7 §2.2 requires the quality unit to be
independent of production.

| Role | Does |
|---|---|
| **Store Keeper** | Books deliveries in, records despatches |
| **Production Manager** | Writes batch records, opens and runs batches |
| **Quality Unit (QA)** | Releases material and batches, assesses deviations, gives the independent second signature on an instruction |
| **Recall Coordinator** | Runs recalls and mock recalls (see [Recalls](36-recalls-and-traceability.md)) |

Anyone posted to a batch's own department can execute that batch — an
operator on the line needs no special role, because requiring one would mean
every operator is a production manager.

**A permission is not an authority.** Being able to open the release screen
is not the same as being allowed to release. The quality-unit decisions are
enforced in the service layer, so they hold whatever the permission matrix
says and whoever writes the next caller.

---

## 1. Materials

**Manufacturing (EBMR) → Materials.** One master covers all four kinds:

- **Raw** — what you buy and consume
- **Packaging** — foils, cartons, labels
- **Intermediate** — something you make and then consume
- **Finished product** — what you ship

They are one master on purpose. A finished product of one batch is an input
to the next, and an intermediate is both — keeping them in one place is what
makes traceability a single unbroken chain instead of two half-chains that
meet nowhere.

### What to fill in

**Unit of measure** is the single unit every quantity of this material is
expressed in, everywhere. There is deliberately no unit conversion: ICH Q7
asks for quantities to reconcile, and a conversion layer is where
reconciliation quietly goes wrong. If you buy in drums and dispense in kg,
record kg.

**Shelf life** and **retest period** are the two dating conventions, and a
material may use either or both:

- *Shelf life* — the finished-product convention. The lot expires.
- *Retest period* — the API convention. The lot is re-tested and its use
  extended.

Give either one and the system works the lot's dates out for you at goods
receipt when the supplier supplies only a manufacture date.

**Hazard attributes** — hazardous, allergen, sensitising, controlled
substance — travel with the material onto every batch record that uses it. An
allergen must say *what* the allergen is; the form will not save without it.

**Receipt controls** decide what happens when a lot of this material arrives:

- *Received lots are quarantined until the quality unit releases them* —
  leave this on unless you have a justified reason not to. It is the material
  master that decides, not whoever is on the receiving dock under time
  pressure.
- *Sampling required on receipt* — when on, the quality unit cannot release a
  lot until a sample has been recorded against it.

**Specification document** points at the controlled document the material is
tested against. It is a document link rather than structured limits — this
module does not hold numeric specifications, and does not try to.

> **Materials are deactivated, never deleted.** An inactive material stays
> readable on every historical lot and batch that names it, and simply stops
> being offered for new work.

---

## 2. Goods receipt and lots

### Booking a delivery in

**Manufacturing (EBMR) → Goods Receipts → New Goods Receipt.** One receipt
per delivery. The header carries the commercial and transport facts — the
supplier (only *approved* suppliers are offered), the invoice and delivery
note, the vehicle, the transport condition and arrival temperature, and
whether the vehicle inspection was satisfactory.

A cold-chain material arriving warm is a receiving decision, and recording
the temperature at the gate is what makes it one rather than a laboratory
mystery three days later.

Then add the lots that came on it. For each: the supplier's lot number, the
quantity, the container count, and the manufacture / expiry / retest dates.
Leave expiry and retest blank and give a manufacture date, and the system
works them out from the material master.

Each lot also gets its own **internal reference** (`LOT-2026-0001`). Two
identifiers on purpose: the supplier's lot number is what a recall notice
will name, and the internal one is unique across every material, so a scan or
a URL is never ambiguous when two suppliers both ship "A-17".

### Recording the receipt

**Record receipt** closes the paperwork and hands the quarantined lots to the
quality unit. After this the receipt's lots and quantities are frozen — which
is what makes the balances trustworthy, because a lot already dispensed into
a batch cannot have its arrival quantity quietly corrected afterwards.

A receipt can be **cancelled** while it is still a draft, but not once any of
its lots has been issued: the material has moved, and pretending the delivery
never happened would leave it unaccounted for.

### Releasing a lot

**Manufacturing (EBMR) → Material Lots** is the register. A lot moves:

```
quarantine ──▶ released ──▶ consumed
     │            │  ▲
     │            ▼  │
     │         on_hold
     ▼            │
  rejected ◀──────┘
```

Only the quality unit may release, reject or hold. Release requires an
[electronic signature](#9-electronic-signatures). The system refuses to
release a lot that has expired, or one whose material requires sampling and
has none recorded.

**Hold** is its own state rather than a return to quarantine, because "never
assessed" and "assessed, then stopped" are different facts about a lot and a
reviewer needs to tell them apart.

### What "released" does and does not mean

A lot is dispensable only when *all* of these hold:

- it is **released**, and
- it has stock remaining, and
- it has not expired, and
- it is not past its retest date

The lot page says so plainly at the top, in green or amber, with the reason
when the answer is no. Trusting the status alone is exactly how expired
material reaches a batch, so the dispensing screen asks the full question.

---

## 3. Master Batch Records

**Manufacturing (EBMR) → Master Batch Records.** The approved instruction for
how one batch of a product is made — ICH Q7 §6.4, 21 CFR 211.186.

The record itself holds only identity: a code, a title, the product, the
production department and an owner. Everything a regulator would want frozen
lives on a **revision**, and that split is what lets an approved instruction
stay immutable while the recipe still evolves.

Only intermediates and finished products can have a batch record — you do not
manufacture a raw material.

### Writing a revision

**New Revision** starts one. If a revision is already effective, the whole
recipe is **copied** into the new one, so a change to one line of a fifty-line
instruction does not mean retyping the other forty-nine. The previous revision
is untouched — every batch already made against it depends on that.

A revision carries:

**Batch size and yield.** The batch size the instruction is written for, the
theoretical yield, and a minimum/maximum yield as *percentages*. Percentages
rather than absolute quantities so the range survives a change of batch size.

**Bill of materials.** One line per material: quantity, unit, and an optional
**overage** percentage for expected losses — the screen shows the resulting
"to dispense" figure. Tick **Critical** on a line whose weighing must be
witnessed by a second person (ICH Q7 §8.1).

The unit must match the material's own unit of measure. Quantities only
reconcile if everyone agrees what the number means.

**Process steps.** One per instructed operation: a step number, a phase
(dispensing / manufacturing / packaging / quality control / other), the
instruction the operator will actually read, the equipment, and expected and
maximum durations. Three flags matter:

- **Critical step** — a second person must sign it off
- **Requires a second-person check** — same effect; a critical step is
  checked whether or not you also tick this
- **Requires line clearance** — the step will not start until clearance is
  recorded (ICH Q7 §9.4)

**In-process controls** hang off a step. A control has a name, a type
(numeric / yes-no / text) and a limit:

- A **numeric** control needs a minimum, a maximum, or both. One with neither
  is refused by the database, because it would silently pass every reading put
  into it — which is worse than having no control.
- A **yes-no** control must say which answer is acceptable.
- A **text** control records an observation for a human to judge at batch
  review, and never blocks the operator.

**Blocking** is the important flag. A blocking control that fails stops the
step. A non-blocking one records the reading and lets work continue.

### Approval

ICH Q7 §2.2 and 21 CFR 211.186(a) both require an instruction to be prepared
by one person and **independently checked** by the quality unit. Submitting a
revision therefore builds two stages:

1. **Production review** — one or more reviewers you name
2. **Quality approval** — one or more quality unit members

The system refuses a quality stage with nobody from the quality unit on it,
and refuses the same person on both stages. Both decisions are 21 CFR Part 11
electronic signatures.

A revision cannot be submitted without a bill of materials *and* at least one
step.

### Effective is not the same as approved

Approval is the signature. **Effective** is the date the floor is told to use
it — and the gap between them is real: training, printing, changeover. A batch
can only be opened against an **effective** revision.

When a revision becomes effective, the previous one is superseded. Batches
already made against the old one are untouched; each pins its own revision.

> **An effective revision can never be edited.** Not by an administrator, not
> by its author. A batch made last March was made against the words that
> existed last March, and editing them rewrites the evidence rather than the
> instruction. Start a new revision instead.

---

## 4. Running a batch

**Manufacturing (EBMR) → Batch Records → New Batch.** Pick the instruction
and give the batch a number. The batch opens against whichever revision is
effective *today*, and pins it permanently.

The revision's steps and in-process controls are **copied onto the batch**,
not merely linked. §6.5 asks for a batch record "prepared from" the current
master, and the copy is the honest answer to "show me what this operator read
on 14 March".

A batch moves:

```
planned ──▶ dispensing ──▶ in_production ──▶ manufactured ──▶ qa_review ──▶ released
                                                                    └──▶ rejected
                        (on_hold and cancelled sit off to the side)
```

### Dispensing

The batch page shows the bill of materials against what has actually been
dispensed. Each line offers only lots that are genuinely dispensable, ordered
**first expiry first** (FEFO), showing the remaining quantity and the expiry
date.

Dispense, and the system:

- refuses a lot that is quarantined, held, rejected, expired, past re-test or
  empty, and says which
- refuses to issue more than the lot holds
- locks the lot row before moving the balance, so two operators dispensing the
  same drum cannot both succeed
- writes the **genealogy row** linking this batch to that lot

**A critical line will not dispense without a witness**, and the witness
cannot be the person dispensing (ICH Q7 §8.1). The database enforces the
second part as well as the service.

If a dispensing fails, no genealogy row is left behind. A record claiming
material that was never issued is worse than no record.

Unused material can be **returned** to its lot with a reason. The return
amends the original row rather than adding a negative second one — the record
should say what the batch actually consumed, not require a reviewer to add
two numbers.

### Executing steps

Each step is worked in order:

1. **Record line clearance** where the step requires it. Until then the step
   will not start.
2. **Start step** — records who is performing it and when. The batch moves to
   *in production*.
3. **Record each in-process result.** The result is judged immediately against
   the limit *frozen on that row*, so amending the instruction later can never
   retroactively move a verdict.
4. **Complete step** — refused while a mandatory result is missing or a
   blocking limit has failed.
5. **Confirm as second person** where required — and the person who performed
   the step cannot be the one who checks it.

**A failed blocking limit stops the step and keeps the reading.** You will see
the failure on screen with the value and the limit. That is deliberate: "we
measured 112 and it failed" is precisely the fact the record must keep. The
step stays blocked until the quality unit has dispositioned a deviation.

A step the batch did not perform is **skipped** with a reason, and stays
visible in the record. "We skipped step 7 because the material arrived
pre-milled" is a fact the reviewer must see, not an absence.

### Completing manufacturing

**Complete manufacturing** is refused while any step is open, any critical
step is unchecked, or any critical dispensing is unwitnessed — the panel
lists exactly what is outstanding.

You give the **actual yield** and an output lot number. The system:

- calculates the yield percentage against the *snapshotted* theoretical yield
- creates the output lot **in quarantine**
- notifies the quality unit that a batch record is waiting for review

The output lot is created in quarantine because that is the honest state: the
batch has been made, the material physically exists and must be accounted for,
and nobody has yet said it may be used. Creating it only at release would
leave real material with no record at all for as long as review takes.

**A yield outside the approved range does not block completion** — the
material exists either way. It blocks *release* until it is justified in
writing.

---

## 5. Deviations

**Manufacturing (EBMR) → Deviations**, or **Raise deviation** from a batch.
ICH Q7 §2.16–2.18 and §6.7, 21 CFR 211.192.

A deviation is any departure from the approved instruction, named against the
batch, step, in-process control or lot it departed from. **Anyone may raise
one** — an operator who spots a departure and has nowhere to record it will
record it nowhere, which is the failure the whole record exists to prevent.

*Planned* deviations are agreed before the work; *unplanned* ones are
discovered. A planned deviation cannot be raised against a batch that has
already been manufactured, because that is not what planned means.

### The quality unit's determination

Only the quality unit assesses a deviation, and the assessment is an
electronic signature. It records:

- **Batch impact** — none / under evaluation / affects this batch / affects
  other batches
- **The assessment itself**, in writing
- **A disposition** — release, release with justification, rework, reprocess,
  reject, or quarantine pending investigation

*Release with justification* requires the justification. The system will not
save it otherwise.

### Two halves, two homes

The **batch** half — what this means for the material — lives on the
deviation. The **systemic** half — why it happened and what stops it
recurring — goes to the existing Findings / RCA / CAPA engine via
**Escalate for RCA / CAPA**, rather than building a second investigation
tool beside a good one. Severity maps automatically: critical → major
nonconformity, major → minor nonconformity, minor → observation.

**A major or critical deviation cannot be closed until it has been
escalated** (ICH Q7 §2.18).

### Deviations block release

An open deviation, or one whose disposition holds the batch, prevents that
batch being released. A deviation assessed with a permitting disposition does
not. This is ICH Q7 §6.7 — "deviations investigated and closed first" — as a
guard rather than a habit.

---

## 6. Batch review and release

Once a batch is *manufactured*, the quality unit sees a **Batch record review
& release** panel listing everything standing in the way. The blockers are:

- steps not complete
- steps still needing a second-person check
- in-process results not recorded
- in-process limits that failed
- critical dispensing that was not witnessed
- a bill-of-materials line nothing was dispensed against *(an operator turning
  two pages at once is the commonest real defect in a paper batch record)*
- deviations not assessed and dispositioned
- a yield outside the approved range with no justification

When the list is empty the panel says so, and offers release.

**Record batch record review** captures the review itself — signed, and
distinct from the release. A reviewer who has read a forty-step record and
found nothing wrong has done something worth recording even before the
release decision.

**Send for release signature** routes the batch to a named quality unit
member, who signs it through the standard approval flow. The approver cannot
be the person who submitted it.

At the moment of signature the blocker list is **checked again**. Things
change between submission and signature — a deviation reopens, a lot is held —
and releasing on a stale check is exactly what the re-ask prevents.

Release carries the output lot out of quarantine with it. Rejection carries
the lot into *rejected* rather than deleting it: rejected material still
physically exists and somebody has to dispose of it.

**Hold** stops a batch and its output lots together, because leaving the lot
released while the batch is held is how held material gets shipped.

---

## 7. The printed batch record

**Download record (PDF)** on any batch produces the complete batch
manufacturing record: identification, the full genealogy of what was
dispensed from which lot by whom and witnessed by whom, every step with its
performer and checker and timings, every in-process result against its limit
(failures in red), the yield reconciliation, the deviations, the release
decision, and the electronic signature declarations exactly as they were
signed.

It is **rendered fresh every time** rather than stored. A cached PDF can
silently disagree with the database it came from; regenerating makes 21 CFR
Part 11 §11.10(b)'s "accurate and complete copies" structurally true rather
than true at the moment somebody last pressed a button.

Downloads are logged with who and when. Inline views are not — the log is for
copies that leave the building.

---

## 8. Genealogy

**Genealogy** on any batch answers the two questions a recall or a complaint
investigation asks:

- **Upstream** — every lot consumed by this batch, with its supplier and
  origin
- **Downstream** — every lot this batch produced, and every later batch that
  consumed one

That chain is what makes [recall and mock recall](36-recalls-and-traceability.md)
possible, and it is why raw material, intermediate and finished product share
one master.

---

## 9. Electronic signatures

Four decisions in this module are 21 CFR Part 11 electronic signatures, not
authenticated clicks:

| Decision | Meaning | Predicate rule |
|---|---|---|
| Approving a master batch record revision | Review / verification | 21 CFR 211.186 |
| Releasing or rejecting a material lot | Verification | 21 CFR 211.84 |
| Assessing a deviation's batch impact | Responsibility | 21 CFR 211.192 |
| Reviewing and releasing a batch | Review / responsibility | 21 CFR 211.188, 211.192 |

Signing asks for your **user ID and password**. Within a continuous 15-minute
signing period the password alone is enough — which is what makes signing
forty lots on a busy receiving day workable rather than punitive.

The declaration you read is **frozen onto the record** at the moment you
sign, and is reproduced verbatim afterwards on screen and in the PDF. A
signature means the words the signer actually read, and those words are
software that can change.

Every signature and **every failed attempt** is recorded in the Electronic
Signature Log (sidebar → **Electronic Records → Signature Log**), with the
outcome and the IP address. A wrong password at a signing prompt counts
toward the same account lockout as a wrong password at the login form, so the
prompt is not an unthrottled place to guess.

**What is not signed:** placing a lot or a batch on hold. A hold is
reversible, precautionary, and not a predicate-rule decision — and making
every cautious act cost a password would discourage caution.

---

## 10. Reports

**Reports** →

- **Batch Release Status** — what is awaiting quality review with the exact
  blockers per batch, what is on hold, what is in production, and batches
  released outside their approved yield range. The blocker list is generated
  by the same code the release gate uses, so this report can never tell a
  reviewer a batch is ready when the gate would refuse it.
- **Material Lot Status** — lots in quarantine, expiring within 30 days,
  already expired, past retest, or on hold.
- **Deviation Aging** — how long deviations have been open by age band,
  category and severity; which are blocking a release; and which serious ones
  still have no CAPA behind them.

The dashboard carries a **Manufacturing** card with the same numbers at a
glance.

---

## What this module does not do

Stated plainly, because a gap you know about is manageable and one you
discover during an inspection is not:

- **No specifications or test results.** There is no `Specification` with
  numeric acceptance criteria, no sample or test result record, and no
  generated Certificate of Analysis. A CoA is a file attached to a lot. If
  you run a LIMS, that is where the verdict lives, and the batch record does
  not re-check it.
- **No equipment qualification or process validation** records.
- **No cleaning or changeover record** independent of a batch step.
- **No warehouse locations** — a lot records where it is stored as a single
  location, with no bin management or stock movement.
- **No label reconciliation** (issued / used / returned / destroyed).

See [the ICH Q7, HACCP and HARPC mapping](standards/ich-q7-haccp-harpc-mapping.md)
for the full, current gap analysis.
