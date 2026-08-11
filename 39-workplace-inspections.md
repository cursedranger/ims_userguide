# Workplace Inspections

Sidebar → **Operations → Inspection Rounds / Workplace Inspections**. Factories
Act §§11, 17, 18, 19, 38, 42–48; 29 CFR 1910.141 (sanitation) and 1910.157
(portable fire extinguishers).

This is the inspection a labour inspector opens first, and the one most systems
have nowhere to put: housekeeping, lighting, drinking water, latrines, washing
and welfare facilities, the canteen, the creche, fire extinguishers, eyewash
stations and escape routes. None of it is glamorous and all of it is
prosecutable.

The module is built from two shapes this application already had working — the
register-plus-periodic-execution of [first aid kit inspections](28-ohc-employee-health-records.md)
and the immutable versioned form of [BBS checklists](24-safety-observations-and-bbs.md)
— rather than a fifth bespoke checklist engine.

## Contents

1. [Three things, in order](#1-three-things-in-order)
2. [Build the form](#2-build-the-form)
3. [The Factories Act standard set](#3-the-factories-act-standard-set)
4. [Critical questions](#4-critical-questions)
5. [Publishing, and why it can't be edited afterwards](#5-publishing-and-why-it-cant-be-edited-afterwards)
6. [Schedule a round](#6-schedule-a-round)
7. [Walk it](#7-walk-it)
8. ["Not applicable" is not "unanswered"](#8-not-applicable-is-not-unanswered)
9. [Completing, and what raises a nonconformity](#9-completing-and-what-raises-a-nonconformity)
10. [The overdue report and reminders](#10-the-overdue-report-and-reminders)
11. [The whitewashing register](#11-the-whitewashing-register)
12. [Who can do what](#12-who-can-do-what)

---

## 1. Three things, in order

| Thing | What it is | Where |
|---|---|---|
| **Template** | The form, versioned | **Inspection Templates** |
| **Round** | What gets inspected, where, how often, by whom | **Inspection Rounds** |
| **Inspection** | One walk, answered question by question | **Workplace Inspections** |

You need them in that order: a round cannot be scheduled against a template
with no published version, and an inspection cannot start without a round.

## 2. Build the form

**Inspection Templates → New template.** A draft v1 is created with it, so you
are never left with a template that has nothing in it.

Add questions with a **category** (which groups them on the walk), the question
text, an optional **statutory reference**, and two flags:

- **Critical** — a "No" here raises a nonconformity automatically. See §4.
- **Needs note** — a note is required even when the answer is satisfactory.
  This is what turns a question into a *register entry* rather than a tick. See
  §11.

## 3. The Factories Act standard set

**Add Factories Act standard set** puts 22 questions on the draft in one
action, drawn from the Act itself:

| Category | Covers |
|---|---|
| Cleanliness (§11) | Floors and walls, drainage, **whitewashing dates** |
| Ventilation & temperature (§13) | Openings, fans, workroom temperature |
| Dust & fume (§14) | Local exhaust running, hoods positioned — **critical** |
| Lighting (§17) | Sufficiency and glare; emergency lighting — **critical** |
| Drinking water (§18) | Points marked, sited away from latrines; potability testing |
| Sanitary facilities (§19) | Number, separation, cleanliness |
| Washing & welfare (§§42–48) | Washing, clothing storage, seating, canteen, shelters, creche |
| First aid (§45) | Boxes present at ratio, sealed, in date |
| Fire precautions (§38) | Extinguishers, escape routes, alarm — all **critical** |
| Emergency equipment | Eyewash and safety showers — **critical** |
| Machinery & access (§§21, 32) | Guards, floors, stairs, handrails — **critical** |
| Signage & housekeeping | Mandatory signage, stacking, gangways |

It is idempotent — running it twice adds nothing — and you can add your own
questions alongside.

## 4. Critical questions

Marking a question **critical** is the strongest thing you can do on this form.
A critical item answered **No** raises a nonconformity when the inspection is
completed, without anybody deciding to.

That is deliberate. The template declared the item critical *in advance*, in
the calm of designing a form. A blocked fire exit is not somebody's judgement
that something might be wrong — it is a prohibition. Letting an inspector tick
past it on a wet Tuesday would make `critical` decorative.

A **non-critical** failure raises nothing. It is carried as a deficiency on the
inspection. That is a judgement call, and drowning a housekeeping round in
nonconformities is how inspections stop being done at all.

## 5. Publishing, and why it can't be edited afterwards

**Publish this version** makes it effective and supersedes the previous one.
After that the version is **immutable** — you cannot add, edit or remove a
question, and the app refuses at the form, at the controller and in its tests.

This is the single property that makes a completed inspection a record rather
than a screenshot. Reprint a walk from two years ago and it renders the
questions that were actually asked, not today's wording.

To change the form, **start a new draft**. It copies the effective version's
questions forward, so changing one line does not mean retyping forty.

## 6. Schedule a round

**Inspection Rounds → New round**: a reference, a name, the **area**
(location), the form, an owner, and a **frequency in days**.

Only templates with a published version appear in the picker.

**An area that has never been inspected counts as overdue.** Not pending —
overdue. The burden is on the record, the same rule calibration and statutory
examination already follow, and an area nobody has ever walked is precisely
what the register exists to surface.

## 7. Walk it

**Start** on the round, or **Start inspection** from its page. The inspection
pins the template version at that moment and creates one answer row per
question.

Answer each question with a response and, where needed, a note. The page groups
questions by category and shows the statutory reference and the critical flag
against each. A count of unanswered questions sits at the top.

## 8. "Not applicable" is not "unanswered"

Four responses: **unanswered**, **satisfactory**, **unsatisfactory**, **not
applicable**.

`not_applicable` and `unanswered` are deliberately distinct. On a safety
document, "does not apply here" and "nobody looked" are different facts, and
collapsing them is exactly how a missed check becomes invisible. The permit form's
`yes_no_na` field type makes the same distinction for the same reason.

So: if your area has no creche, mark that question **Not applicable**. Do not
leave it blank.

## 9. Completing, and what raises a nonconformity

**Complete inspection** refuses while anything is unanswered, and tells you how
many are left.

On completion:

- The **result** is set — *satisfactory* if nothing was unsatisfactory,
  *deficiencies found* otherwise.
- The **next due date** is computed from the round's frequency and **stored**.
  Changing the frequency later does not rewrite when past inspections were due.
- Every **critical** failure raises a nonconformity, whose description carries
  the round, the area, the date, the inspector, the statutory reference and the
  inspector's own note.
- The round's owner is notified if anything was found.

## 10. The overdue report and reminders

**Reports → Workplace Inspections Due** lists rounds worst-first, with
never-inspected areas flagged. It reads the **register**, not the inspections —
which is the point, because an area with no inspection has no inspection row to
find.

`WorkplaceInspections::DueReminderJob` runs daily: the owner is reminded when a
round comes due, and an **overdue** one escalates to the department head.

## 11. The whitewashing register

§11(2) requires the dates of whitewashing, colour-washing or painting to be
entered in a prescribed register. This module *is* that register, and it needed
no separate model.

The standard set includes the question *"Whitewashing, colour-washing or
painting has been carried out at the prescribed interval — record the date
carried out"*, carrying the statutory reference §11(2) and flagged **needs
note**. The date goes in the note; the immutable form version behind it is what
makes it a register rather than a comment.

It prints through the [statutory form engine](37-designing-a-statutory-form.md)
in your state's own format.

## 12. Who can do what

| Who | Can |
|---|---|
| Everyone | Read the forms, the rounds and the completed inspections — a worker is entitled to know what their area is inspected against |
| Everyone | **Carry out** an inspection and answer its questions |
| Round owners | Manage their own round |
| Department heads | Manage their department's rounds and inspections |
| IMS admin, top management, corporate safety head, HSEF officer | Manage the templates |
| Those plus corporate safety team | Manage all rounds and inspections |

**Doing the walk is deliberately open.** Gating who may carry out an inspection
is how inspections stop happening. Defining the *form* is not open: a checklist
whose wording anybody could edit would not be a controlled checklist.

---

## Related

- [Designing a Statutory Form](37-designing-a-statutory-form.md) — printing
  these records in your state's prescribed format
- [Findings](03-findings.md) — where a critical failure ends up
- [OHC / Employee Health Records](28-ohc-employee-health-records.md) — first aid
  kit inspections, the shape this module generalises
- [Factories Act & OSHA mapping](standards/factories-act-osha-mapping.md) —
  gap S3
