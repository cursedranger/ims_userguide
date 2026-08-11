# Chemical Register & SDS

Sidebar → **Operations → Chemical Register**. 29 CFR 1910.1200 (hazard
communication) and 1910.119(d) (process safety information); Factories Act §41B
(compulsory disclosure by the occupier), §§88, 88A and 89 (notification); the
Manufacture, Storage and Import of Hazardous Chemicals Rules 1989.

Three questions this module answers that a substance name alone cannot: **how
dangerous is it**, **where is it and how much**, and — for an incident —
**how long have we got to tell the authority**.

## Contents

1. [The substance master](#1-the-substance-master)
2. [The SDS review clock](#2-the-sds-review-clock)
3. [Why everyone can read this register](#3-why-everyone-can-read-this-register)
4. [Inventory by location](#4-inventory-by-location)
5. [The MSIHC threshold determination](#5-the-msihc-threshold-determination)
6. [Exposure limits point one way](#6-exposure-limits-point-one-way)
7. [Statutory notification clocks](#7-statutory-notification-clocks)
8. [What triggers what](#8-what-triggers-what)
9. [Why the reminder runs hourly](#9-why-the-reminder-runs-hourly)
10. [The two reports](#10-the-two-reports)
11. [Who can do what](#11-who-can-do-what)
12. [Known limitations](#12-known-limitations)

---

## 1. The substance master

**Chemical Register** holds every hazardous substance the site keeps. Each entry
carries:

- **Identity** — name, CAS number, UN number, physical state.
- **GHS classification** and **signal word** (Danger / Warning), with the
  **hazard statements** (H-codes) and **precautionary statements** (P-codes).
  These are free text rather than a fixed list, because a substance carries
  several classifications and the GHS list is revised.
- **Incompatibilities** and **storage requirements** — what it must not be
  stored next to.
- **First aid** and **firefighting** measures, for the people who need them
  under pressure.
- The **safety data sheet itself**, as an attachment.

Non-hazardous substances can sit on the register too — untick **Hazardous** and
they are not chased for an SDS.

## 2. The SDS review clock

Two fields drive it: the **SDS revision date** (the supplier's, from the sheet)
and a **review interval** in months, defaulting to 36. The next review date is
derived and stored, so the register can filter and sort on it.

**A hazardous substance with no revision date counts as overdue, not unknown.**
An undated sheet cannot be shown to be the current revision, and in practice
that is the usual finding rather than an edge case. The register says *"No SDS on
file"* and the report lists it first.

A substance inside 60 days of its review date shows as due soon.

## 3. Why everyone can read this register

Read access is open to every signed-in user, and that is not a convenience
decision.

29 CFR 1910.1200(g)(8) requires safety data sheets to be **readily accessible to
employees in their work area**. Factories Act §41B requires the occupier to
disclose the nature and hazards of the process **to the workers**. A register
only the safety team can open fails both duties.

Writing is restricted: a substance whose hazard statements anybody could edit is
not a hazard communication.

## 4. Inventory by location

From a substance's page, record what is held: the location, the quantity and
unit, an optional **licensed maximum**, the container type and the stocktake
date. One row per substance per location — a second row for the same pair is
refused and points you at the existing one.

**A quantity over the maximum is recorded and flagged, never refused.** Same rule
as the [shift roster](42-shift-roster.md) and the hours limits: exceeding the
licensed maximum is exactly the fact the register exists to surface, and refusing
to write it down would hide the breach rather than prevent it.

**Chemical Inventory** lists every holding across the site, filterable by
substance and location.

## 5. The MSIHC threshold determination

A site holding more than a listed substance's **threshold quantity** becomes a
Major Accident Hazard installation under the MSIHC Rules, which brings its own
duties — an on-site emergency plan, a safety report, notification to the
authority.

Record the threshold and its unit on the substance. The app then sums that
site's holdings **across all its locations** — because the threshold is a site
total, not a per-tank figure — and compares.

Two things to note:

- With **no threshold recorded**, the answer is **not "under the threshold"** —
  it is *"not determined"*. The report says so. Those are very different answers
  to an inspector, and conflating them would let an undetermined substance read
  as a safe one.
- The determination is **per site**, so the report asks you to switch to a
  specific site in the header rather than showing a meaningless cross-site
  total.

## 6. Exposure limits point one way

A substance links to the
[permissible exposure limits](38-workplace-exposure-monitoring.md) it is sampled
against — and the link lives on the **limit**, read from the substance.

That is because a substance has *several* limits: an 8-hour TWA and a 15-minute
STEL are different limits on the same agent. A single link on the substance could
only ever hold one of them.

## 7. Statutory notification clocks

When an incident hurts somebody, several authorities have to be told, on clocks
measured in **hours**. This part of the module derives which and when, from the
injury classifications actually recorded on the incident.

The clock runs from when the employer **knew** — the moment the incident was
recorded — not from when it happened. For an incident reported days later those
differ, and the reported time is the honest starting point.

Two things the derivation deliberately does *not* do:

- It never **duplicates** a notification that already exists.
- It never **overrules a human**. An officer who has judged something *not
  required* keeps that judgement. This proposes; it does not decide.

## 8. What triggers what

The mapping is not uniform between jurisdictions, and the differences matter:

| Recorded on the incident | Factories Act §88 / §88A | 29 CFR 1904.39 |
|---|---|---|
| Fatality | Reportable, shorter clock | **8 hours** |
| Lost-time injury | Reportable accident | 24 hours |
| Medical treatment / restricted work | Reportable accident | **Nothing** — it belongs on the 300 log, but is not separately reportable |
| First aid only | **Nothing** — not a §88 accident | Nothing |
| Nobody injured | §88A dangerous occurrence | Nothing |

Raising a clock for a first-aid case would train people to ignore the clock, so
it raises none.

> **The Factories Act hours are defaults, not authority.** §88 leaves the
> timeframe to the State Rules, and several states are shorter than the 24 and 48
> hours seeded here. They exist so a fresh install chases something rather than
> nothing. Check your State Rules and adjust.

## 9. Why the reminder runs hourly

Every other reminder in this application runs once a day at 7am. This one runs
**every hour**, and that is the entire point: 1904.39 gives **eight hours** for a
fatality. A reminder arriving next morning arrives after the deadline it was
reminding about.

It warns two hours out and keeps chasing while the clock is live, keyed by the
hour so it does not fall silent after the first notification.

## 10. The two reports

**Reports → Chemical Register & SDS** — sheets missing or stale, sheets due for
review within 60 days, and the MSIHC threshold determination for the selected
site.

**Reports → Statutory Notifications** — what is outstanding and how long is
left, sorted by time remaining, because that is the only ordering that matters
here. Anything past its deadline is called out loudly: late notification is
itself an offence under most of these rules.

## 11. Who can do what

| Who | Can |
|---|---|
| Everyone | Read the register, the SDS and the inventory — see §3 |
| Department heads | Keep their own area's holdings current |
| IMS admin, top management, corporate safety head, HSEF officer | Manage the substance master |
| Those plus corporate safety team and the store keeper | Manage the inventory |

## 12. Known limitations

**The §89 occupational disease notification has its fields and its clock, but
nothing derives one.** A notifiable Third Schedule disease has a place to be
recorded against a notification, and a 96-hour clock — but the app cannot yet
raise it automatically, because that needs a diagnosis recorded against the
Third Schedule, which is a shape the OHC module does not have yet. Record it by
hand for now.

The register does not track **batch or lot level** chemical movements. It records
what is held, not each drum's history.

---

## Related

- [Workplace Exposure Monitoring](38-workplace-exposure-monitoring.md) — the
  limits these substances are sampled against
- [PPE & Respiratory Protection](40-ppe-and-respiratory-protection.md) — the
  controls the hazard statements imply
- [Incidents](11-incidents.md) — where the notification clocks come from
- [Statutory Registers](41-statutory-registers.md) — the hazardous area register,
  for the places these substances are kept
- [Factories Act & OSHA mapping](standards/factories-act-osha-mapping.md) —
  gap S6
