# PPE & Respiratory Protection

Sidebar → **Operations → PPE Issued / PPE Hazard Assessments / Respirator Fit
Tests / PPE Catalogue**. Factories Act §35 (protection of eyes) and §87
(dangerous operations); 29 CFR 1910.132, .133, .134 and .138.

PPE is the last line of defence and the one most often treated as a stationery
problem. This module holds the four things a regulator actually asks for: that
the hazard was **assessed and certified**, that the equipment was **selected**
against it, that it was **issued** to a named person, and — for respirators —
that it was **tested to seal on that particular face**.

## Contents

1. [Four records, four duties](#1-four-records-four-duties)
2. [The catalogue](#2-the-catalogue)
3. [The hazard assessment, and its written certification](#3-the-hazard-assessment-and-its-written-certification)
4. [Hazards with no equipment selected](#4-hazards-with-no-equipment-selected)
5. [Certifying, and what it fixes](#5-certifying-and-what-it-fixes)
6. [Issuing equipment](#6-issuing-equipment)
7. [Inspecting reusable equipment](#7-inspecting-reusable-equipment)
8. [Respirator fit testing](#8-respirator-fit-testing)
9. [The fit factor is enforced](#9-the-fit-factor-is-enforced)
10. [A failure withdraws the respirator](#10-a-failure-withdraws-the-respirator)
11. [Revoking a fit test](#11-revoking-a-fit-test)
12. [The compliance report](#12-the-compliance-report)
13. [Who can do what](#13-who-can-do-what)

---

## 1. Four records, four duties

| Record | Answers | Rule |
|---|---|---|
| **Hazard assessment** | Was the workplace assessed, and by whom? | 1910.132(d) |
| **Catalogue** | What is this equipment certified to? | 1910.132(b), §35 |
| **Issue** | Who was actually given it, in what size? | 1910.132(a) |
| **Fit test** | Does the respirator seal on this face? | 1910.134(f) |

## 2. The catalogue

**PPE Catalogue** is what the site issues. Each item carries a **conformity
standard** — IS 6994, EN 166, NIOSH N95 — which is what turns "gloves" into a
specification. Without it, the name is a shopping note and nothing can be
assessed against it.

Two flags change behaviour:

- **Reusable** — the item is inspected periodically while issued, and an
  inspection interval in days becomes mandatory. Reusable equipment that never
  says how often it is examined cannot be reported as overdue, which would make
  the flag decorative.
- **Requires a respirator fit test** — issuing this item to somebody with no
  valid fit test is flagged at the moment of issue.

## 3. The hazard assessment, and its written certification

29 CFR 1910.132(d)(2) is unusually specific. It does not merely require an
assessment; it requires a **written certification** that identifies four
things. All four are fields on the record, not prose:

| The rule wants | The field |
|---|---|
| The workplace evaluated | **Location** |
| The person certifying it was performed | **Certified by** |
| The date(s) of the assessment | **Assessed on** |
| That the document *is* a certification | **Certification statement** |

A database constraint refuses a certified assessment missing any of them, so
the record cannot claim to be a certification while lacking what makes it one.

**PPE Hazard Assessments → New assessment**, then record the hazards.

## 4. Hazards with no equipment selected

Each hazard row carries its category, the hazard itself, and the PPE selected
against it — and the PPE link is **optional**.

That is on purpose. An assessment legitimately identifies a hazard before the
equipment for it has been sourced, and recording the gap is more useful than
refusing to record the hazard. Rows with nothing selected are highlighted, and
counted on the index, because that gap is exactly what an assessment exists to
surface.

Selecting equipment from the wrong category is refused: hearing protection
against an eye hazard would read as a controlled hazard when it is not.

## 5. Certifying, and what it fixes

**Certify** stamps the four fields, supersedes whichever assessment previously
covered that workplace, and **fixes the record** — it cannot be edited
afterwards, and neither can its hazards.

A changed workplace gets a **fresh assessment**. The old one stays on file as
superseded rather than being rewritten, which is the same rule this application
applies to controlled documents and to statutory examinations.

The certified page prints as the certification itself, with the statement and
the four fields laid out, ready to be filed.

## 6. Issuing equipment

**PPE Issued → Issue PPE**: the item, the person, the date, a size, a quantity.
The replacement date is derived from the catalogue item's interval if you leave
it blank.

**If the item requires a fit test and the wearer has no valid one, the
confirmation says so** — at the moment of issue, not in a report nobody opens.
That is the exact gap 1910.134 exists to close: a respirator handed over to
somebody it has never been shown to fit.

Equipment comes back with **Mark returned**.

## 7. Inspecting reusable equipment

On a reusable item's issue page, record an inspection: the date, a result
(**serviceable** or **withdrawn**) and, to withdraw it, the defects.

Two behaviours worth knowing:

- **Withdrawing takes the equipment off the wearer automatically.** An
  inspector who has just written down that a harness is frayed should not also
  have to remember to un-issue it, and leaving it issued would let the register
  report somebody as protected by equipment that failed.
- **Withdrawing without recording the defects is refused** — by a validation
  and a database constraint. "Withdrawn" with no reason is not an inspection
  record.

The next inspection date is computed once from the catalogue interval and
**stored**, so changing that interval later never rewrites when a past
inspection was due. Equipment that has **never** been inspected counts as
overdue.

## 8. Respirator fit testing

**Respirator Fit Tests → Record fit test.** Fit testing is the most-cited
element of 1910.134, and this record is deliberately built as a **clearance**,
not a working document — the same shape as a contractor medical clearance:

- dated, and **immutable once issued** (there is no edit action anywhere);
- **revocable**, with a reason, which leaves the original on file;
- with a **computed status** — valid, expiring soon, expired, failed, revoked;
- on an **annual clock**, defaulting to the 12 months 1910.134(f)(2) requires.

A mistake is corrected by testing again, not by editing what was recorded.

Record the person, the respirator (from the catalogue, ideally), make, model,
facepiece size, the method — **qualitative (QLFT)** or **quantitative
(QNFT)** — the protocol, and the result.

## 9. The fit factor is enforced

1910.134(f)(7) sets a minimum fit factor of **100** for a half mask on a
quantitative test. A quantitative **pass** recorded below that is refused with
the rule cited.

A pass under the threshold is either a typo or a pass that should not have been
given, and neither should be stored quietly. Qualitative tests produce no fit
factor at all — they are pass/fail on the wearer's response to a challenge
agent — so they are exempt rather than forced to invent a number. A quantitative
*failure* is not held to the minimum, obviously.

## 10. A failure withdraws the respirator

Recording a **fail** withdraws any issue of that model to that person, and says
so. A failed test means that specific facepiece does not seal on that specific
face; leaving the issue standing would let the register report somebody as
protected by a respirator known not to fit them.

Try a different size or model and test again.

## 11. Revoking a fit test

Use **Revoke** when a valid test should stop counting — a changed facepiece,
significant weight change, facial scarring, dental work. A reason is required.

Revoking also withdraws any respirator issued on that test. The record stays on
file, because it is evidence of what was decided and when.

## 12. The compliance report

**Reports → PPE Compliance** answers the two questions an inspector asks:

1. **Who is wearing a respirator without a valid fit test** — the 1910.134 gap.
2. **What reusable equipment is overdue inspection** — including anything never
   inspected.

Plus replacements past due, and fit tests expiring within 30 days.
`Ppe::ExpiryReminderJob` runs daily: the **wearer** is told about their own fit
test, because they are the person who cannot work without it, and the **issuer**
about the inspection, because they are the person who can do something about it.

## 13. Who can do what

This module follows the clearance shape rather than the open-reporting shape,
because a fit test is evidence about one person.

| Who | Can |
|---|---|
| Everyone | Read the **catalogue** — a worker is entitled to know what their PPE is certified to |
| Everyone | Read the hazard assessments |
| Everyone | Read **their own** issues and **their own** fit tests — 1910.1020 gives an employee access to records about their own protection |
| Department heads | Issue and inspect PPE for their own people |
| IMS admin, top management, corporate safety head, HSEF officer | Manage the catalogue |
| Those plus corporate safety team | Manage assessments and all issues |
| Those plus the OHC doctor | Record and revoke **fit tests** |

A fit test is issued only by those qualified to conduct one — **never by the
wearer**. Certifying a hazard assessment is not granted to department heads
either: that is the written certification the rule puts on the employer.

## What this module does not do

There is **no Prawn certificate renderer** for a fit test. The fit test page is
print-friendly, which is a printable certificate in the sense the rule asks for,
but it is not a designed one-page certificate like the contractor medical
clearance has.

---

## Related

- [Workplace Exposure Monitoring](38-workplace-exposure-monitoring.md) — an
  exceedance usually turns on the RPE this module tracks
- [Chemical Register & SDS](43-chemical-register-and-sds.md) — where the hazards
  PPE is selected against are described
- [OHC / Employee Health Records](28-ohc-employee-health-records.md) — the
  medical evaluation that must precede respirator use
- [Factories Act & OSHA mapping](standards/factories-act-osha-mapping.md) —
  gap S5
