# Shift Roster

Sidebar → **Operations → Shift Roster**. Factories Act §§51–62, and the OSH Code
2020 which carries the same duties forward.

Every hours provision in the Act is expressed in terms of who worked when: 48
hours a week (§51), a weekly holiday (§52), nine hours a day (§54), a
ten-and-a-half-hour spread-over (§56). The **register of adult workers** (§62)
*is* that record, and the **notice of periods of work** (§61) is its summary.

The shift master has existed since [permit to work](33-permit-to-work.md) needed
it — the approval matrix escalates by shift, so it had to know when shifts run.
Nobody was ever rostered onto one. This is that missing half.

## Contents

1. [The week board](#1-the-week-board)
2. [Rostering people](#2-rostering-people)
3. [Somebody already rostered is left alone](#3-somebody-already-rostered-is-left-alone)
4. [One shift per person per day](#4-one-shift-per-person-per-day)
5. [The hours limits are checked, never enforced](#5-the-hours-limits-are-checked-never-enforced)
6. [Overnight shifts](#6-overnight-shifts)
7. [Who is on duty](#7-who-is-on-duty)
8. [The registers this unlocks](#8-the-registers-this-unlocks)
9. [Who can do what](#9-who-can-do-what)
10. [What is deliberately out of scope](#10-what-is-deliberately-out-of-scope)

---

## 1. The week board

**Shift Roster** opens on the current week: shifts down the side, days across
the top, names in the cells. Today's column is highlighted.

It is a board rather than a list because that is how rostering is actually done
and read. A flat list of four hundred rows answers no question a supervisor has.

Navigate with **Previous week / This week / Next week**, or link straight to a
week. Anyone whose roster row crosses an Act limit is shown in red with the
limit named, and a summary panel below the board lists them.

## 2. Rostering people

**Roster people** takes a shift, a date range, an optional relay or group, an
optional department, and a list of people. It writes one row per person per day.

Rostering happens a week at a time, so the form is built for that: pick the
shift, accept the default Sunday-to-Saturday range, tick the crew, submit. Forty
individual rows is how a roster stops being kept.

Remove a single entry with the **×** in its cell.

## 3. Somebody already rostered is left alone

If a person is already rostered on a day, the bulk assignment **skips them —
it does not move them**, and the confirmation tells you how many were skipped.

Silently reassigning somebody would rewrite the record of who worked when, which
is precisely what §62's register exists to preserve. If they genuinely need
moving, remove the existing entry first. That way the change is deliberate.

## 4. One shift per person per day

The database refuses two roster rows for the same person on the same day, and
the error cites **§60** — the prohibition on double employment. Rostering
somebody onto two shifts of one day is the in-house version of the same mistake.

## 5. The hours limits are checked, never enforced

Four limits are computed and shown:

| Limit | Section |
|---|---|
| 48 hours in a week | §51 |
| A weekly holiday — no more than 10 consecutive days | §52 |
| 9 hours in a day | §54 |
| 10½-hour spread-over | §56 |

**An assignment always saves, even when it crosses one.** This is the single
most important thing to understand about this module.

A roster that refuses to record a 50-hour week does not prevent the week. It
prevents the *record* of it — which is the opposite of what a statutory register
is for, and would push the real roster into a spreadsheet where nothing checks
it at all. So the app records what happened and makes the breach loud: on the
board, in a breach panel, in the CSV, and on the printed register.

The one thing genuinely refused is §60's double-rostering, because that is a
data error rather than a management decision.

> Weeks run Sunday to Saturday, which is the convention §52's "first day of the
> week" assumes.

## 6. Overnight shifts

A 22:00–06:00 shift is an ordinary row, not a special case. Assignments anchor
to the date the shift **starts**, so a night shift rostered on the 4th runs into
the morning of the 5th.

That keeps one shift as one row, which is what §61's notice shows, and it means
"who is on duty at 02:00" correctly finds the person rostered for *yesterday* —
the case a naive same-day lookup gets wrong.

## 7. Who is on duty

**On duty now** answers who is actually on shift at a given moment. Change the
timestamp to ask about any other moment.

This is not a convenience screen. Two other parts of the app previously had to
approximate it by *role*:

- The **medical notification list** now also notifies the **first aiders who are
  on shift**, resolved by intersecting the
  [statutory appointment register](41-statutory-registers.md) (§45) with the
  roster. It is purely additive — a site with nobody rostered gets exactly the
  recipients it always did.
- Permit escalation can resolve the real duty-holder rather than every holder of
  a role.

## 8. The registers this unlocks

Two more registers become printable through the
[statutory form engine](37-designing-a-statutory-form.md) the moment a roster
exists:

**Register of adult workers (§62)** — every adult rostered in the period, the
shift group and relay they worked, days worked, total hours, and any limit
crossed.

It is a register of *adults*, so anyone the medical profile shows as a child or
adolescent is excluded and appears on the young persons register instead.
Somebody with **no date of birth on file is included** — most of a factory's
roster is adult, and dropping every unrecorded worker would produce a register
that quietly under-reports its own headcount.

**Notice of periods of work (§61)** — the periods each *group* works, not each
individual: shift, window, hours, relays, headcount and days operated. §61 is a
notice to be displayed, and it is about groups.

## 9. Who can do what

| Who | Can |
|---|---|
| Everyone | Read the roster — §61's notice of periods of work is a *notice*, displayed for everyone |
| Department heads | Roster their own department |
| IMS admin, top management, corporate safety head, corporate safety team, HSEF officer | Roster anybody |

## 10. What is deliberately out of scope

**Overtime and leave with wages.** That is payroll. This application should
integrate with an HR system rather than grow one, and a half-built payroll is
worse than none — say so explicitly in any proposal rather than leaving it
ambiguous.

The roster records **planned** shifts. It is not an attendance system: it does
not know who actually turned up, and the hours it reports are rostered hours.

---

## Related

- [Sites](31-sites.md) — where the shift master itself is configured
- [Designing a Statutory Form](37-designing-a-statutory-form.md) — printing the
  §62 and §61 registers in your state's format
- [Statutory Registers](41-statutory-registers.md) — the appointment register the
  on-duty first aider lookup reads
- [Permit to Work](33-permit-to-work.md) — the module that needed the shift
  master in the first place
- [Factories Act & OSHA mapping](standards/factories-act-osha-mapping.md) —
  gap S7
