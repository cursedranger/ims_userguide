# Workplace Exposure Monitoring

Sidebar → **Operations → Exposure Monitoring / Exposure Limits**. Factories Act
§41F and the Second Schedule; 29 CFR 1910.95 (noise), 1910.1000 and the
Subpart Z substance-specific standards.

Health surveillance already knew **who** is exposed to what and examined them
on a clock ([OHC](28-ohc-employee-health-records.md)). It could not answer
**how much**. That is the gap this module closes, and it is the one an
occupational hygienist asks about first: an exposure register that records who
works with silica but never measures the silica is a list, not a control.

## Contents

1. [The two halves: limits and samples](#1-the-two-halves-limits-and-samples)
2. [The limit register](#2-the-limit-register)
3. [Why limits are shared across sites](#3-why-limits-are-shared-across-sites)
4. [The action level, and the noise exception](#4-the-action-level-and-the-noise-exception)
5. [Recording a sample](#5-recording-a-sample)
6. [Personal or area — and why it matters](#6-personal-or-area--and-why-it-matters)
7. [What happens when a limit is exceeded](#7-what-happens-when-a-limit-is-exceeded)
8. [Automatic enrolment into health surveillance](#8-automatic-enrolment-into-health-surveillance)
9. [Why a past verdict never changes](#9-why-a-past-verdict-never-changes)
10. [The exceedance report](#10-the-exceedance-report)
11. [Who can see and do what](#11-who-can-see-and-do-what)
12. [What this module does not do](#12-what-this-module-does-not-do)

---

## 1. The two halves: limits and samples

| Half | What it is | Where |
|---|---|---|
| **Exposure limit** | The permissible number a sample is judged against — agent, averaging period, value, unit, source | **Exposure Limits** |
| **Exposure measurement** | One sample: where, on whom, for how long, by what method, and what it read | **Exposure Monitoring** |

A measurement is always judged against a limit. You cannot record a sample
without choosing the limit it is measured against, because a number with no
limit behind it answers nothing.

## 2. The limit register

**Exposure Limits** ships with 14 limits already configured — the common Indian
statutory values plus the noise limits — each carrying its citation so the
number can be checked rather than trusted. Every seeded row says in its notes:
*confirm against the current gazette notification before use.*

Each limit holds:

- **Agent** — free text, because noise, heat and radiation are agents with
  permissible limits and are not chemicals. An optional link to the
  [chemical register](43-chemical-register-and-sds.md) ties a chemical agent to
  its substance record.
- **Averaging period** — 8-hour TWA, 15-minute STEL, or ceiling. A substance
  legitimately holds **more than one row**: an 8-hour TWA and a 15-minute STEL
  are different limits judged against differently-collected samples, so
  uniqueness is on agent *plus* period.
- **Limit value and unit** — mg/m³, ppm, dB(A).
- **Action level** — see §4.
- **Source** — Factories Act Second Schedule, OSHA PEL, ACGIH TLV, NIOSH REL,
  or an internal limit, with a citation field.
- **Hazard type** — which kind of health surveillance an exceedance implies.

## 3. Why limits are shared across sites

Almost every operational record in this application belongs to one site. The
limit register deliberately does not.

A permissible limit is set by a statute or a standards body, not by a plant.
The Second Schedule value for lead is the same in every factory in the
country. Copying it per site would create three rows that can silently
disagree — and the one that disagrees is the one somebody edited by accident.

A site that wants a **stricter** internal limit than the statute requires sets
the source to *Internal limit* and records its own number, which is a
deliberate decision rather than a drift.

## 4. The action level, and the noise exception

The action level is stored as a **fraction of the limit** rather than a second
absolute number, because that is how the regulators express it — OSHA's action
level is 50% of the PEL — and because a fraction cannot drift out of order with
the limit it is a fraction of.

**Noise is the exception, and it is seeded correctly.** Decibels are
logarithmic, so "half the limit" is meaningless: the real action level is
85 dB(A) against a 90 dB(A) limit, which is 0.944 of it, not 0.5. The seeded
noise rows carry that fraction explicitly. An impulse-peak ceiling carries 1.0,
because a peak has no meaningful "approaching it" band — you are either under
it or you are not.

> This is a known wrinkle in the model rather than a triumph of it. A fraction
> is the wrong abstraction for a logarithmic unit; it lands on the right number
> here because the seeded fraction was chosen to. If you add more logarithmic
> agents, check the arithmetic.

## 5. Recording a sample

**Exposure Monitoring → Record a sample.** You need:

| Field | Notes |
|---|---|
| Agent and limit | The limit this sample is judged against |
| Sample type | Personal or area — see §6 |
| Worker sampled | Required for a personal sample |
| Sampling point | "Bagging line 2, operator breathing zone" |
| Sampled at | A contemporaneous record; a future timestamp is refused |
| Measured value | In the unit of the chosen limit |
| Duration, method | 480 minutes, NIOSH 7500, gravimetric |
| Instrument used | Optional link to the [asset register](15-assets.md), which ties the result back to that pump's calibration status |
| Controls in place | What was protecting people at the time |

## 6. Personal or area — and why it matters

An **area** sample says something about a place. A **personal** sample — a pump
clipped to a collar, a dosimeter — says something about one identified person's
exposure.

The distinction is not cosmetic. Only a personal sample can enrol somebody in
health surveillance (§8), because enrolling a worker off an area reading would
put the wrong people in front of the wrong tests. A personal sample with nobody
attached is refused by a validation *and* a database constraint: it is an area
sample that has lost track of what it was measuring.

## 7. What happens when a limit is exceeded

Three verdicts, computed on save and stored:

| Verdict | When |
|---|---|
| **Within limits** | Below the action level |
| **Action level** | At or above the action level, below the limit |
| **Over limit** | **At** the limit or above |

Note the last one. A permissible limit is a value **not to be reached**, so a
reading exactly at the limit is an exceedance, not a pass.

An over-limit sample **raises a major nonconformity without asking.** There is
no tick box. This is a stronger version of the rule
**environmental condition monitoring** (Monitored Conditions) already follows:
that one records a number outside a limit the laboratory set for itself; this
records a number outside a limit set by statute to protect somebody's lungs. There is no
discretion left to exercise, and an opt-in would make a permissible limit
advisory. It is raised as a **major** NC rather than a minor one for the same
reason.

Reaching the **action level** raises nothing. Approaching a limit is not
breaching it, and it is carried as a notification to the department head and
the recorder.

## 8. Automatic enrolment into health surveillance

When a **personal** sample reaches the action level or above, the worker is
enrolled in their own site's surveillance programme for that hazard —
automatically. Remembering to do this is exactly the step that gets missed.

Three details worth knowing:

- **The programme is resolved at the worker's own site**, not named on the
  limit. The limit is shared across sites (§3) and surveillance programmes are
  per-site, so a limit naming one programme directly would enrol a Plant 2
  worker into Plant 1's programme.
- **Ambiguity is not guessed at.** A site can run several programmes for one
  hazard at different exposure intensities. If exactly one active programme
  matches, the worker is enrolled; if none or several do, enrolment is left to
  a human. The nonconformity is already raised, so nothing goes unnoticed.
- **Exposure level is set from the verdict** — over limit becomes *high*,
  action level becomes *medium*.

If the worker has no medical profile, nothing is enrolled and the sample still
saves.

## 9. Why a past verdict never changes

The limit value, the action level, the unit **and** the verdict are all copied
onto the measurement when it is taken.

That is deliberate and it matters: revising a permissible limit next year must
never retroactively turn last year's exceedance into a pass. Edit a limit and
the register says so — *"Measurements already recorded keep the limit they were
judged against."* It also means a printed report can state the number the
verdict was actually reached against without consulting today's master.

## 10. The exceedance report

**Reports → Exposure Exceedances** lists every sample that reached an action
level or breached a limit, with the finding raised against it, filterable by
verdict, sample type and agent, and exportable.

## 11. Who can see and do what

This module's permissions are **narrower than most**, because a personal sample
names one identified person's health exposure.

| Who | Can |
|---|---|
| Everyone | Read the **limit register** — a worker is entitled to know the limit that applies to their job |
| Everyone | Read **their own** measurements — withholding somebody's own exposure result from them is not defensible under §41B's disclosure duty or 29 CFR 1910.1020 |
| Department members | Read their department's measurements |
| Department heads | Manage their department's measurements |
| IMS admin, top management, corporate safety head | Manage the **limit register** |
| Those plus corporate safety team, HSEF officer | Manage all measurements |

## 12. What this module does not do

**Noise dosimetry does not yet drive a standard threshold shift
determination.** 1910.95 requires successive audiograms to be compared for a
shift in hearing threshold. Dosimetry-driven *enrolment* works; the comparison
does not, because `OhcExaminationTest` stores its value as free text and there
is no frequency-by-frequency audiogram data to compare. Structuring audiometry
is its own piece of work.

This module also does not manage sampling *schedules* — it records what was
sampled, not what is due to be sampled next.

---

## Related

- [OHC / Employee Health Records](28-ohc-employee-health-records.md) — the
  surveillance programmes an exceedance enrols people into
- [Chemical Register & SDS](43-chemical-register-and-sds.md) — the substances
  these limits apply to
- [PPE & Respiratory Protection](40-ppe-and-respiratory-protection.md) — the
  controls an exceedance usually turns on
- [Factories Act & OSHA mapping](standards/factories-act-osha-mapping.md) —
  gap S2, and what remains open
