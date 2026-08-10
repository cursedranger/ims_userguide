# Recalls & Traceability

Sidebar → **Manufacturing (EBMR) → Despatches / Recalls**. ICH Q7 §10.2 and
§15, Codex CXC 1-1969 §7, 21 CFR 117.139, and the FSSAI (Food Recall)
Regulations 2017 — which make a documented recall plan and periodic mock
recall a **licence condition** for many Indian food businesses, independent of
any export requirement.

Recall is the one capability nobody wants to use and everybody is audited on.
The expensive half of it — knowing which lots are affected — is genealogy, and
[batch records](35-batch-manufacturing-records.md) already build that. This
module adds the other half: where the material went, and the exercise itself.

## Contents

1. [Despatches — the forward half](#1-despatches--the-forward-half)
2. [Opening a recall](#2-opening-a-recall)
3. [The trace](#3-the-trace)
4. [Telling the consignees](#4-telling-the-consignees)
5. [Reconciling and closing](#5-reconciling-and-closing)
6. [Mock recalls](#6-mock-recalls)
7. [What a mock recall proves](#7-what-a-mock-recall-proves)

---

## 1. Despatches — the forward half

**Manufacturing (EBMR) → Despatches** is the register; individual despatches
are recorded from a lot's own page, under **Despatched to**.

Genealogy answers *what went into this lot*. A despatch answers *where this
lot went*. A recall needs both, and the second is near-impossible to
reconstruct after the fact — which is why it is recorded by whoever ships,
not only by the quality unit.

Each despatch records the consignee, the quantity, the date, an invoice or
delivery-challan reference, and a destination. The consignee is either a
registered **Customer** or a **free-text name** — a one-off buyer, an internal
transfer to another plant, a sample to a laboratory. Recording the name rather
than refusing the despatch is deliberate: a lot that left the site with no
record of where it went is the single failure a recall cannot survive.

> **A despatch is not a consumption.** Dispensing into a batch draws the lot's
> balance down; despatching does not. They are different events, and
> conflating them would make "how much is still on our shelf" wrong at exactly
> the moment it matters. The system will not let total despatches exceed what
> the lot ever held.

A despatch already named on a recall notification cannot be deleted.

---

## 2. Opening a recall

**Manufacturing (EBMR) → Recalls → New Recall** (or **New Mock Recall**).

| Field | What it is for |
|---|---|
| **Type** | Recall, market withdrawal, or other market action |
| **Class** | Class I (reasonable probability of serious harm), II (temporary or reversible harm), III (unlikely to cause harm) — the FDA's three, which Codex and FSSAI mirror closely |
| **Why** | The reason, in writing |
| **Seed lot / seed batch** | Where the trace starts |
| **Recovery target** | The percentage you expect to get back — defaults to 100% |
| **Triggered by** | An optional link to the deviation or incident that set it off |
| **Decision authority** | Who authorised it; recorded at closure if left blank |

Reference numbers make the two kinds obvious at a glance: real recalls are
`RCL-2026-0001`, mock exercises `MRC-2026-0001`.

Reading is deliberately wide — production, stores, despatch and sales all need
to know which lots are in scope, and a recall list only some people can see is
how a recalled lot ships anyway. **Reconciling and closing** are quality unit
decisions.

---

## 3. The trace

**Trace affected lots** walks the genealogy chain forward from the seed and
records every lot it reaches. The chain it follows is:

```
seed lot ──consumed by──▶ batch ──produced──▶ lot ──consumed by──▶ batch ──▶ lot …
```

Each affected lot is recorded with its **trace depth** — how many
manufacturing generations downstream of the seed it sits — plus how much was
made, how much was despatched, and how much is still on your own shelf.

The walk is breadth-first with a visited set, so a **rework loop** (a lot fed
back into a batch that produced it) terminates instead of spinning. Rework is
a normal GMP operation, not an error.

Seeding from a **batch** rather than a lot starts from every lot that batch
produced.

### Adding and re-running

A lot the trace could not reach — despatched before this system existed, or
identified by a customer — can be **added by hand**. It is marked as such,
because an auditor asks "why is this lot on the list" and *the system traced
it* is a materially different answer from *somebody decided*.

**Re-running the trace** rebuilds the traced lots and keeps the ones you added
by hand. That way, correcting a despatch record and tracing again gives the
right answer rather than the old answer plus the new one.

A traced lot cannot be removed by hand — it is what the genealogy says.
Correct the genealogy and re-trace.

### What the recall already knows

Once traced, the recall page shows the **complaints and nonconforming outputs
already recorded against those lots**. That panel exists because incidents and
nonconformities can now name the lot they concern; before that, a complaint
about a recalled lot was a paragraph of prose that nothing could join to. It
is worth reading before deciding the classification.

The panel obeys your own permissions — running a recall does not become a way
to read incidents you otherwise could not.

---

## 4. Telling the consignees

**Build the consignee list** assembles the notification list from the
**despatch records** of every lot the trace found. Not from memory, and not
typed — a coordinator working from recollection of who bought what is the
failure forward traceability exists to remove.

Each consignee gets a row with the lot and the quantity they received.
Recording a response sets one of four states:

- **Pending** — not yet contacted
- **Notified** — contacted, no reply yet
- **Acknowledged** — replied; requires you to record what they said
- **Unreachable** — you could not reach them after reasonable effort

**Unreachable is a real outcome**, and it is settled rather than pending.
Folding it into "pending" would leave every honest recall permanently
unclosable.

A confirmed return quantity rolls straight up into the affected lot's
recovered figure, so the consignee list and the effectiveness percentage
cannot disagree.

Building the list on a **real** recall notifies the quality unit. On a mock it
does not — see below.

---

## 5. Reconciling and closing

**Start reconciliation**, then fill in what came back against each affected
lot, with a disposition: returned, destroyed, reworked, released after
investigation, or not recovered.

**Effectiveness** is arithmetic, not judgement:

```
recovered ÷ despatched × 100
```

Material still on your own shelf is not counted as needing recovery — it never
reached the market, so it needed finding, and the trace already found it.

Closing requires:

- every consignee responded or marked unreachable (real recalls only)
- a written **effectiveness conclusion**
- **corrective actions**, if recovery fell short of the target

That last one is the governance gate. Product genuinely goes unrecovered, and
missing the target is allowed — but it has to be said out loud, with the
number recorded next to the words. This is the same shape as releasing a batch
on an out-of-range yield.

---

## 6. Mock recalls

A mock recall is **the same record with a flag**, not a separate module. What
a GFSI auditor actually tests is "run your recall procedure and time it", and
running a different procedure for the drill than for the real thing is exactly
what that test exists to catch.

Two things differ, and only two:

1. **Nobody outside the site is contacted.** The consignee list is built and
   left unsent, and the quality unit is not paged. A drill must not put a
   false alarm in front of a real customer.
2. **Closing does not wait on consignee responses**, because there are none to
   wait for.

Everything else — the trace, the depth, the quantities, the reconciliation,
the effectiveness arithmetic, the closure conclusion — is identical.

---

## 7. What a mock recall proves

The headline number is the **trace time**: the clock starts when you press
*Trace affected lots* and stops when the walk finishes, shown in minutes on
the recall and in the register.

Most schemes expect a site to identify every affected lot within two to four
hours. That expectation assumes a person reading despatch registers. This
records the actual elapsed time of the system doing it, and the register keeps
every past exercise, so "we run a mock recall annually and here are the last
five, with their times and their recovery percentages" is a filter rather than
a folder.

Run one at least annually, and after any material change to how you record
despatches. An exercise whose trace comes back with fewer lots than you
expected is telling you something about your despatch records, not about the
software.

---

## Related

- [Batch Manufacturing Records](35-batch-manufacturing-records.md) — the
  genealogy this module walks
- [Nonconforming Output](25-nonconforming-output.md) — now able to name the
  lot it concerns
- [Incidents](11-incidents.md) — a customer complaint can name its lot, which
  is the first thing a complaint investigation asks
- [ICH Q7, HACCP & HARPC mapping](standards/ich-q7-haccp-harpc-mapping.md) —
  gap P4, and what is still missing around it
