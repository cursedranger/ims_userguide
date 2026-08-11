# Statutory Registers

Sidebar → **Operations → Statutory Appointments / Hazardous Areas**. Factories
Act §§7, 36, 40B, 49, 66–71; 29 CFR 1910.146 (permit-required confined spaces)
and 1910.147 (energy isolation).

Two registers an inspector asks for by name, and neither is about a process.
One is about **people the occupier must appoint** — and, for several posts,
must have told the Chief Inspector about. The other is about **places that are
dangerous by virtue of what they are**, which 1910.146 requires you to have
identified *before* anybody raises a permit to enter one.

## Contents

1. [Statutory appointments](#1-statutory-appointments)
2. [Why the notice matters more than the qualification](#2-why-the-notice-matters-more-than-the-qualification)
3. [Recording an appointment](#3-recording-an-appointment)
4. [Ending one](#4-ending-one)
5. [The appointment report, and unfilled posts](#5-the-appointment-report-and-unfilled-posts)
6. [Hazardous areas](#6-hazardous-areas)
7. [Reclassifying a confined space](#7-reclassifying-a-confined-space)
8. [Linking an area to a permit](#8-linking-an-area-to-a-permit)
9. [Age and employment category](#9-age-and-employment-category)
10. [Who can do what](#10-who-can-do-what)

---

## 1. Statutory appointments

[Competency requirements](13-competency-and-training.md) already record that
somebody *holds* a qualification, and refuse a role assignment when they do
not. This register records something different: that the occupier has **put
them in a named statutory post**.

That is a separate fact. It carries a date, it ends, and for several posts it is
only effective once the Chief Inspector has been notified.

Ten post types ship configured:

| Post | Reference | Notice to Chief Inspector? |
|---|---|---|
| Occupier | §2(n), §7 | **Yes** |
| Factory Manager | §7 | **Yes** |
| Safety Officer | §40B | **Yes** |
| Welfare Officer | §49 | **Yes** |
| Competent person (examinations) | §§28, 29, 31 | No |
| Trained first aider | §45 | No |
| Confined space entry supervisor | 1910.146(j) | No |
| Authorized employee (LOTO) | 1910.147(c)(7) | No |
| Fire marshal / emergency co-ordinator | §38 | No |
| Other statutory appointment | — | No |

## 2. Why the notice matters more than the qualification

A Safety Officer appointment nobody told the Chief Inspector about is **not a
valid §40B appointment**, however well-qualified the person is. That
notification half is the thing `UserCompetency` has never had a field for, and
it is the whole reason this register exists.

So the register distinguishes posts that need a notice from those that do not.
It flags an unsent notice for an Occupier, Manager, Safety Officer or Welfare
Officer, and stays quiet about a first aider — who needs none. Nagging about
every row would train people to ignore the flag.

## 3. Recording an appointment

**Statutory Appointments → New appointment.** Choose the post (the statutory
reference fills in from the catalogue), the person, the department if it is not
site-wide, the qualification the appointment rests on, and the date.

The **competency** field links to a [competency](13-competency-and-training.md)
where the site tracks one. It is optional, because an appointment is frequently
made on an external certificate the competency master has no row for.

When the post needs a notice, the confirmation says so. Record
**Notice to Chief Inspector** and its reference once sent, and the warning
clears.

## 4. Ending one

**End appointment** closes it and stamps the end date. The record stays on the
register — an appointment that has ended is history, and history is what a
register is for.

## 5. The appointment report, and unfilled posts

**Reports → Statutory Appointment Register** lists active appointments,
notice-outstanding ones first, and adds something the register itself cannot:
**posts with nobody currently appointed.**

It reads that list from the post catalogue, so adding a post type surfaces there
for free. Note the wording carefully — the report says the post is *unfilled*,
not that it is *required*. Whether your factory needs a Safety Officer depends
on headcount and whether a hazardous process runs there (§40B), and the register
reports the absence rather than asserting the duty.

## 6. Hazardous areas

**Hazardous Areas** is the register 1910.146(c)(1) requires **before any entry
permit is raised**: the places on site that are dangerous by virtue of what they
are.

One register covers three kinds, because all three answer the same questions —
where is it, what makes it dangerous, and what must be true before anyone works
on it:

| Kind | Example | Extra field |
|---|---|---|
| **Confined space** | Reactor manway, effluent sump | Permit required? |
| **Classified area** | Solvent store | Area classification (Zone 1, Class I Div 2) |
| **Energy isolation point** | MCC panel feeder | Isolation method |

Each entry carries its hazards, its entry requirements, an owner, an optional
link to the [asset](15-assets.md) it belongs to, and a review clock. Overdue
reviews are flagged on the index and the page.

## 7. Reclassifying a confined space

**Permit required** is ticked by default on a confined space. Unticking it
reclassifies the space as non-permit — and the app then **requires a written
basis**, enforced by a validation *and* a database constraint.

That is not friction for its own sake. 1910.146(c)(7) makes reclassification a
decision the employer has to document and defend. An unexplained
reclassification is not a decision; it is a space that quietly stopped needing a
permit.

The reclassified page displays the basis prominently, so anybody looking at the
space can see why it is not a permit space.

## 8. Linking an area to a permit

A [permit to work](33-permit-to-work.md) can name the hazardous area it governs
entry to, in the job particulars section. Doing so brings the area's recorded
hazards and entry requirements onto the job, and the area's page lists every
permit raised against it.

The link is optional, the same way the asset link is: a permit against a space
that is not on the register still works. Refusing those would block every permit
at a site that has not finished building its register, which is how a control
gets switched off wholesale rather than complied with.

## 9. Age and employment category

§§66–71 turn on whether somebody is an adult, an adolescent or a child, and §66
on whether a woman may be employed at night. Neither restriction is even
expressible without a date of birth.

Those attributes live on the **employee medical profile**, not on the user
record — deliberately. They are sensitive, and the
[OHC module](28-ohc-employee-health-records.md) already has the confidentiality
shape for them: no department-scoped read for anybody. Putting them on the user
record would expose every employee's age and sex to anybody who can list users.

Two things follow:

- The **statutory category** is *derived* from the date of birth, not trusted
  from the typed field. Where the two disagree, the app says so rather than
  silently believing one.
- With no date of birth on file, the derived category is **nil**, not "adult".
  "We do not know" and "adult" are very different answers to a labour inspector.

The [register of young persons](37-designing-a-statutory-form.md) prints from
this.

## 10. Who can do what

Both registers are **read-open, write-restricted**, and for the same reason:
they are the registers everyone relies on and nobody should be able to edit
quietly. A worker about to enter a vessel is entitled to know it is a permit
space; a supervisor is entitled to know who the Safety Officer is. Neither
should be able to reclassify the vessel or appoint themselves.

| Who | Can |
|---|---|
| Everyone | Read both registers |
| Area owners | Update their own area's entry requirements |
| IMS admin, top management, corporate safety head | Manage **appointments** |
| Those plus corporate safety team, HSEF officer | Manage **hazardous areas** |

Appointing somebody to a statutory post sits with the narrowest set, not with
the safety team generally — it is the occupier who is prosecuted when a post is
unfilled.

---

## Related

- [Permit to Work](33-permit-to-work.md) — the permit that governs entry to a
  registered space
- [Competency & Training](13-competency-and-training.md) — the qualification an
  appointment rests on
- [Designing a Statutory Form](37-designing-a-statutory-form.md) — printing the
  young persons register in your state's format
- [Factories Act & OSHA mapping](standards/factories-act-osha-mapping.md) —
  gaps S4, S9 and S10
