# Factories Act 1948 & OSHA — Occupational Safety Statutory Mapping

**Last reviewed: 2026-08-06 — update this alongside any new module.**

## Scope and how to read this document

This document maps two occupational safety and health legal frameworks to the
application module, model, or service that supports them:

- **The Factories Act 1948** (India) and the state Factory Rules made under it —
  the statute governing health, safety, welfare, working hours and statutory
  registers in a registered factory.
- **OSHA** (United States) — the Occupational Safety and Health Act's General
  Duty Clause, the injury and illness recordkeeping rule at **29 CFR Part 1904**,
  and the General Industry standards at **29 CFR Part 1910**, with **1910.119
  Process Safety Management** treated separately because it is where this
  application is strongest.

They are in one file because they impose the same duties in different words, and
because — unlike the [ICH Q7 / HACCP / HARPC mapping](ich-q7-haccp-harpc-mapping.md)
— **this is the area the application was actually built for.** A site running
this app has most of what either regulator would ask to see, and the honest gaps
are concentrated in a few specific places rather than spread across everything.

### Current overall position, stated plainly

**Substantially covered on the safety-management side; genuinely incomplete on
statutory registers, working hours, welfare facilities, and exposure
monitoring.**

The proactive and reactive safety machinery is real and, in places, better than
what a mid-size plant runs today: permit to work with an enforced approval
matrix and gas-test limits that block issue, HAZOP, pre-startup safety review
with a refuse-on-"No" authorization gate, incident investigation with process
safety event tiering, emergency scenarios and drills, occupational health with
statutory periodic medical examinations, first aid registers, contractor medical
clearance with gate-pass enforcement, behaviour-based safety, worker
participation, and safety committees.

What is missing is mostly **the clerical layer a labour inspector opens first**:
the prescribed registers and returns, the working-hours and leave records, the
welfare-facility evidence, and — the one substantive safety gap — **exposure
monitoring against permissible limits and a PPE programme**.

### How to read a status here, and what software can and cannot do

A statute imposes duties on an occupier or employer, and many of those duties are
**physical**: fence the machinery, provide a canteen, guard the hoist, supply
respirators that fit. No application satisfies those. What an application can do
is hold the register, the inspection record, the test certificate, the training
evidence and the corrective action that proves the physical duty was discharged
and is being maintained.

So **Covered** here means "the app can hold the record this section requires,
with the workflow and evidence an inspector would expect", never "the app makes
the factory compliant". Where a section is purely physical with no record
attached, it is marked **N/A — physical**.

### A note on the OSH Code 2020

The **Occupational Safety, Health and Working Conditions Code 2020** consolidates
the Factories Act 1948 with 12 other central labour laws, and is being brought
into force alongside state rules on a staggered timetable. Its substantive
duties — health, safety, welfare, hours, hazardous process controls, annual
health examinations, safety committees, accident notification — carry forward
from the Factories Act with changes to thresholds, definitions and form numbers
rather than to the underlying obligations. **Every row in Part 1 below survives
the transition; the section numbers do not.** Where a site has already
commenced under the Code, read the Factories Act section numbers here as the
subject-matter index they are.

Note also that the Act itself prescribes almost none of the actual forms — the
**state Factory Rules** do, and they differ. Form numbers are therefore
deliberately not asserted below; registers are named by what they contain.

---

## Part 1 — The Factories Act 1948

### Chapter III — Health (§§11–20)

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 11 | **Cleanliness** — factory kept clean and free from effluvia; floors cleaned, whitewashing/painting at prescribed intervals with a record of the dates | — | **Not covered** | There is no facility sanitation or housekeeping record anywhere in the app, and §11(2) explicitly requires the dates of whitewashing to be **entered in a prescribed register**. This is the same absence the food-safety mapping records as gap P5. See gap **S3**. |
| 12 | Disposal of wastes and effluents | `EnvironmentalAspect` (§11.9); `ComplianceObligation`/`ComplianceEvaluation` (§11.3) | **Partial** | Waste streams register as environmental aspects with scored significance, control measures and periodic review, and the consent conditions register as compliance obligations with an evaluation frequency and evidence. There is no effluent monitoring result record. |
| 13 | Ventilation and temperature | — | **Not covered** | No workplace environmental condition monitoring. See gap **S2**. |
| 14 | **Dust and fume** — effective measures where dust or fume is likely to be injurious; exhaust appliances near the point of origin | `EnvironmentalAspect` (§11.9); `SurveillanceProgram`/`HazardExposure` (§11.19 Slice 6) | **Partial** | The *people* side is genuinely covered: `EmployeeMedicalProfile::HAZARD_EXPOSURE_CATEGORIES` includes `dust` and `chemical`, workers are enrolled in a surveillance programme by recording a dated exposure, each exposure runs its own examination clock, and test-result trends are visible across successive examinations. The *atmosphere* side — measuring the dust concentration and comparing it to a limit — is absent. |
| 15 | Artificial humidification | — | **Not covered** | Textile-specific; needs the same workplace monitoring gap **S2**. |
| 16 | Overcrowding — minimum cubic space per worker | — | **N/A — physical** | A one-time design and licensing determination, not an ongoing record. |
| 17 | Lighting — sufficient and suitable lighting, prevention of glare and shadows | — | **Not covered** | Would be an inspection-round item under gap **S3**. |
| 18 | **Drinking water** — suitable points, legibly marked, cool water where over 250 workers | — | **Not covered** | Potability testing and point inspection have no record. |
| 19 | Latrines and urinals — prescribed number, separate for men and women, maintained clean and sanitary | — | **Not covered** | Sanitary facility inspection; gap **S3**. |
| 20 | Spittoons | — | **N/A — physical** | |

### Chapter IV — Safety (§§21–41)

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 21 | Fencing of machinery | `Asset` (§11.7); `HazopStudy` (§11.14); `BbsObservation` (§11.15) | **Partial** | The guard itself is physical. What the app holds well is the *finding* that a guard is missing — a BBS observation records an unsafe condition with contributing factors and raises a `BbsAction` that forces an explicit hierarchy-of-controls pick, and a HAZOP deviation records the hazard with a scored risk and an owned recommendation. There is no machine guarding inspection checklist. |
| 22 | Work on or near machinery in motion — by trained adult male workers in tight clothing, with a register of such workers | `WorkPermit` (§11.22); `TrainingAttendance` (§11.5) | **Partial** | A permit authorises the non-routine work and names the crew; training evidences the competence. The **register of persons authorised to examine machinery in motion** that §22(2) requires does not exist as such. |
| 23 | Employment of young persons on dangerous machines — only after training and under supervision | `TrainingAttendance` (§11.5) | **Partial** | Training is evidenced with validated presence and a frozen assessment score. There is no age or young-person attribute on `User`, so the prohibition cannot be enforced. See gap **S4**. |
| 24 | Striking gear and devices for cutting off power | `WorkPermit` LOTOTO checklist template (§11.22) | **Partial** | Seeded checklist templates include LOTOTO; the isolation itself is a permit checklist item whose `critical` flag blocks issue when answered "No". No energy-isolation point register. |
| 25–27 | Self-acting machines; casing of new machinery; cotton openers | — | **N/A — physical** | |
| 28 | **Hoists and lifts** — of good construction, thoroughly examined **at least once every six months** by a competent person, with a **register of examination** | `Asset`/`MaintenanceRecord` (§11.7) | **Partial** | `Asset` carries a `maintenance_frequency_months`, and `MaintenanceRecord` a due date, completed date, provider, result and evidence, with an overdue report and a daily reminder job. That is a workable statutory examination register *in substance*. What it lacks is the competent-person identity and certification, the prescribed register format, and any distinction between a maintenance visit and a statutory examination. See gap **S1**. |
| 29 | **Lifting machines, chains, ropes and lifting tackles** — thoroughly examined at prescribed intervals by a competent person, with a register | `Asset`/`MaintenanceRecord` (§11.7) | **Partial** | As §28. Chains and slings are typically not individually registered as `Asset` rows today, and there is no safe-working-load attribute. |
| 30 | Revolving machinery — maximum safe working peripheral speed notified | — | **Not covered** | |
| 31 | **Pressure plant** — safe working pressure, examined at prescribed intervals, register maintained | `Asset`/`MaintenanceRecord` (§11.7); `PssrReview` (§11.13) | **Partial** | As §28. `PssrReview`'s standard checklist domains explicitly include "mechanical integrity" and "safety/relief devices & interlocks", which is the right structure for a pressure-vessel examination but is scoped to a startup event, not a recurring statutory examination. |
| 32 | Floors, stairs and means of access | `BbsObservation` (§11.15) | **Partial** | Condition findings only. |
| 33–34 | Pits, sumps, openings in floors; excessive weights | `WorkPermit` (§11.22) | **Partial** | Openings are typically a permit condition. Manual handling limits are not modelled. |
| 35 | Protection of eyes | — | **Not covered** | Part of the PPE gap **S5**. |
| 36 / 36A | **Precautions against dangerous fumes and gases** — nobody to enter a confined space unless it is certified safe, with a certificate in writing by a competent person; portable electric light restrictions | **`WorkPermit`** confined space type + `WorkPermitGasTest` (§11.22) | **Covered** | This is one of the strongest matches in the document. A confined space permit carries the confined-space type (with its own conditional checklist rows and its own approval levels — confined space inert requires all three approver levels every time), the crew, the stand-by person, and a gas test log where **O₂ and %LEL readings are validated against template `min_value`/`max_value` and `WorkPermits::RecordGasTest` refuses a reading outside the configured range without an explicit override note**. An unacceptable reading on an active permit suspends it and notifies the issuer and field operator. The permit itself is the §36(3) certificate in writing, signed by the issuer and every required approver. |
| 37 | **Explosive or inflammable dust or gas** — precautions before hot work on any plant that has contained such substances | **`WorkPermit`** hot work type + gas test + fire watch (§11.22) | **Covered** | Hot work requires Approver I every time, escalating to Approver II and III by shift; the fire watch is a `requires_fire_watch` type condition; closure requires four distinct signatures **and** `WorkPermit#fire_watch_release_at` (`fire_watch_completed_at + 30.minutes`) is a computed guard, not a suggestion. |
| 38 | **Precautions in case of fire** — safe means of escape, fire-fighting equipment, and workers **familiar with the means of escape and trained in the routine to be followed** | `EmergencyScenario`/`EmergencyResponseTeamMember`/`EmergencyDrill`/`EmergencyPlanReview` (§11.20) | **Covered** | Identified emergency situations with a scored risk rating, the planned response, required resources, external agencies and a named response team; announced and unannounced drills with response and evacuation timings, effectiveness evaluation and an opt-in Finding raise; and a periodic/post-drill/post-emergency plan review loop that parks a plan at `under_review` until it is actually revised. A drill due reminder job runs daily. |
| 39–40 | Power to require specifications of defective parts or tests of stability; safety of buildings and machinery | `Finding`/CAPA (§7); `NonconformingOutput` (§11.16) | **Partial** | An inspector's written order can be tracked as a `Finding` with RCA and CAPA. There is no building stability certificate record. |
| 40A | **Maintenance of buildings** — the Inspector may require specified repairs where a building is in a state prejudicial to health or safety | `MaintenanceRecord` (§11.7) | **Partial** | Equipment maintenance only; buildings are not `Asset` rows by convention. |
| 40B | **Safety Officers** — required where 1,000 or more workers are employed, or where hazardous processes are carried on | `Role::KEYS` — `corporate_safety_head`, `corporate_safety_team`, `hsef_officer` | **Covered** | Three safety-specific roles exist and carry real, distinct abilities: `hsef_officer` reads the whole permit register and may issue and suspend; `corporate_safety_head`/`corporate_safety_team` are resolved by `MedicalNotifications::Recipients` as mandatory stakeholders on injury and process-safety events. The **statutory appointment record** — the notice to the Chief Inspector naming the Safety Officer and their qualifications — has no field. |

### Chapter IVA — Provisions relating to hazardous processes (§§41A–41H)

This chapter is the closest the Factories Act comes to a process safety
management regime, and it is where this app is strongest.

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 41A | Site Appraisal Committees | — | **N/A — external** | A state government committee constituted before a hazardous factory is sited. |
| 41B | **Compulsory disclosure of information by the occupier** — the nature and hazards of the process disclosed to workers, the Chief Inspector, the local authority and the general public; a **written on-site emergency plan** with detailed disaster control measures, made known to workers and rehearsed; a **safety policy** | `EmergencyScenario`/`EmergencyPlanReview` (§11.20); `Document` (§10); `SafetyMeeting` (§11.11); `Chemical` master | **Partial — strong on the plan, weak on the disclosure** | The on-site emergency plan, its rehearsal (drills with timings and effectiveness evaluation) and its periodic review are properly modelled, and the safety policy is an ordinary controlled document with approval, effective dating and controlled distribution with acknowledgement. What is missing is the **disclosure record itself**: no register of what was disclosed to which authority and when, and no material safety data sheet holding. `Chemical` is a bare master (`name`, `cas_number`, `active`) used for incident release reporting — it holds no hazard classification and no SDS. See gap **S6**. |
| 41C(a) | **Maintain accurate and up-to-date health records of workers exposed to hazardous substances**, accessible to those workers | `EmployeeMedicalProfile`/`OhcExamination`/`OhcExaminationTest`/`SurveillanceProgram`/`HazardExposure` (§11.19) | **Covered** | A medical master with a hazard exposure category, dated exposure periods per hazard, structured examination tests (`spirometry`, `audiometry`, `vision`, `ecg`, `chest_xray`, `lab_test`), exposure history timelines, test-result trends across successive examinations, and a printable Medical Examination Register. The module's confidentiality boundary is deliberately the app's one exception to every other ability shape — **no department-scoped read for anyone**, so a department head cannot see their reports' medical records. |
| 41C(b) | **Appoint persons with qualifications and experience in handling hazardous substances**, competent to supervise the process | `Competency`/`CompetencyRequirement`/`UserCompetency` (§11.5.3); `TrainingAttendance` (§11.5); `Role` | **Covered** | Competence is evidenced *and* now enforced: a `CompetencyRequirement` set to `blocking` refuses the role assignment while the person does not hold the competency, validated on `UserRole` so RailsAdmin cannot bypass it. `UserCompetency` is the appointment record — who, what, from when, until when, granted by whom, on what evidence. The statutory **notice to the Chief Inspector** naming the appointed person still has no field; see gap **S4**. |
| 41C(c) | **Medical examination of every worker before employment and thereafter at intervals not exceeding twelve months**, with records maintained | `OhcExamination` (`pre_employment`/`periodic`/`surveillance`); `OhcExaminationRequirement` frequency master; `Ohc::ExaminationDueReminderJob`; `Ohc::MedicalExaminationRegisterPdf` (§11.19 Slice 9) | **Covered** | The single cleanest statutory match in this document. A configurable frequency-by-hazard-category master drives each employee's next due date; a two-tier reminder job notifies the employee then escalates to the OHC ahead of the date and again once overdue; and the printable register calls out overdue and never-examined rows separately. Fitness certificates are issued from the examination. |
| 41D–41E | Central Government inquiry committee; emergency standards | — | **N/A — external** | |
| 41F | **Permissible limits of exposure of chemical and toxic substances** — as specified in the Second Schedule | — | **Not covered — the substantive safety gap** | Nothing in the app holds an occupational exposure limit or a workplace measurement to compare against one. The people-side surveillance is built; the atmosphere-side monitoring is not. See gap **S2**. |
| 41G | **Workers' participation in safety management** — a Safety Committee with equal representation of workers and management, meeting as prescribed | `SafetyMeeting` (`safety_committee` type) / `SafetyMeetingParticipant` / `SafetyMeetingActionItem` (§11.11); `WorkerParticipationRecord` (§11.10) | **Covered** | `SafetyMeeting#meeting_type` includes `safety_committee` specifically; participants are internal users or external names with a `chair`/`attendee` role and an attendance status; action items carry an assignee, due date and status; scheduling sends a real notification and email to every internal participant. `WorkerParticipationRecord` is the separate log of individual participation events — hazard reports, suggestions, safety-committee input, risk-assessment involvement — where any signed-in user may submit and **always retains read access to their own submission's progress regardless of department**, which is what makes the consultation real rather than nominal. The composition rule (equal worker/management representation) is not validated. |
| 41H | **Right of workers to warn about imminent danger** — workers may bring it to the notice of the occupier, who must take remedial action and inform the Inspector | `WorkerParticipationRecord` (`hazard_report`) (§11.10); `BbsAction` stop-work (§11.15 Slice 4); `WorkPermits::Suspend` (§11.22) | **Covered** | Three independent channels, and the stop-work one has teeth: `BbsAction` carries `stop_work` with declared/lifted timestamps and actors, `DeclareStopWork` notifies the programme owner, and the observation's show page renders a red banner while any stop-work is active. `WorkPermits::Suspend` is deliberately **authorised more widely than approval is** — any signatory plus `hsef_officer` — because the guideline lets any employee with permit knowledge stop a job. A `hazard_report` can raise a Finding for formal RCA/CAPA. The statutory *notification to the Inspector* is not tracked. |

### Chapter V — Welfare (§§42–50)

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 42–44 | Washing facilities; facilities for storing and drying clothing; facilities for sitting | — | **Not covered** | Facility provision and inspection; gap **S3**. |
| 45 | **First-aid appliances** — a first-aid box per 150 workers, readily accessible, in charge of a trained person; an ambulance room where over 500 workers | **`FirstAidKit`/`FirstAidKitInspection`/`FirstAidCase`** (§11.19 Slice 5); `PharmacyItem`/`PharmacyStockBatch`/`PharmacyDispensation` | **Covered** | A first aid box register with periodic inspections, an overdue report and a daily reminder job; a first aid case register covering employees, contractors and visitors alike, with emergency response, ambulance and hospital-referral detail and a computed response time; and a batch-tracked medicine inventory with expiry and stock alerts supporting the dispensary stocking requirement. The "in charge of a trained person" link between a kit and a certified first aider is not modelled. |
| 46 | Canteens | — | **Not covered** | Where a canteen exists it is also a food business — see the [HACCP mapping](ich-q7-haccp-harpc-mapping.md). |
| 47 | Shelters, rest rooms and lunch rooms | — | **Not covered** | |
| 48 | Creches | — | **Not covered** | |
| 49 | **Welfare Officers** — required where 500 or more workers are employed | `Role`/`Department` | **Not covered** | No welfare officer role or statutory appointment record. |

### Chapter VI — Working hours of adults (§§51–66)

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 51, 54, 56 | Weekly hours (48); daily hours (9); spread over (10½) | `Shift` (§11.22) | **Not covered** | `Shift` exists and is a real site master — `code`, `name`, `starts_at_minute`, `ends_at_minute`, `position`, `general`, `active`, with `Shift#covers?` correctly handling an overnight wrap and `Shifts::At.call(time)` resolving a timestamp. But it exists to make "after G-shift" computable for the permit approval matrix; **nobody is rostered onto it**. There is no attendance, no hours worked, and no limit check. See gap **S7**. |
| 52–53 | Weekly holidays; compensatory holidays | — | **Not covered** | |
| 55 | Intervals for rest | — | **Not covered** | |
| 57–58 | Night shifts; prohibition of overlapping shifts | `Shift` | **Not covered** | The masters exist; the roster does not. |
| 59–60 | Extra wages for overtime; restriction on double employment | — | **Not covered — out of scope** | Payroll. Not something this app should grow into. |
| 61 | **Notice of periods of work for adults** — displayed and correctly maintained | `Shift` | **Not covered** | |
| 62 | **Register of adult workers** — name, nature of work, group, shift, with the register available to the Inspector | `User`; `Shift`; `DepartmentMembership` | **Not covered** | The identity data largely exists on `User`; the statutory register form and its shift/group columns do not. See gap **S7**. |
| 63 | Hours of work to correspond with the notice and register | — | **Not covered** | |
| 66 | Further restrictions on the employment of women (night work) | — | **Not covered** | No sex attribute on `User`, and no roster to check against. |

### Chapters VII–VIII — Young persons and annual leave (§§67–84)

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 67–71 | Prohibition of employment of young children; certificate of fitness for a young person; working hours for children; register of child workers | `OhcExamination` (`pre_employment`) (§11.19) | **Partial** | The **certificate of fitness** is genuinely close — a pre-employment examination with a fitness verdict and a printable certificate already exists. The age attribute, the register of child workers, and the restricted hours do not. See gap **S4**. |
| 78–84 | Annual leave with wages; register of leave | — | **Not covered — out of scope** | HR/payroll territory. |

### Chapter IX — Special provisions (§§85–91A)

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 87 | **Dangerous operations** — state rules may require prior medical examination, prohibition of certain persons, PPE, exhaust appliances and periodical examinations | `SurveillanceProgram`/`HazardExposure`/`OhcExamination` (§11.19) | **Partial** | The examination and surveillance half is covered; the PPE and exhaust half is not (gaps **S5**, **S2**). |
| 87A | Power to prohibit employment on account of serious hazard | `Incident`; `WorkPermits::Suspend`; `BbsAction` stop-work | **Partial** | Internal stop-work is well modelled; an external prohibition order has no record. |
| 88 | **Notice of certain accidents** — where an accident causes death or bodily injury preventing the person from working for 48 hours or more, notice in the prescribed form to the prescribed authorities; and a **register of such accidents** | `Incident` + `IncidentPerson#injury_classification` + `IncidentNotification` + `Incident#reportability_decision` (§11.4) | **Covered** | `Incident` carries a `reportability_decision` (`pending`/`not_reportable`/`reportable`/`not_applicable`) so the determination itself is a recorded, filterable decision rather than an assumption, and `IncidentNotification` holds the `recipient_authority` and a `required_decision` — the external notification is tracked as part of the incident record rather than hardcoded as a rule, which is the right call for a requirement that differs by state. `IncidentPerson#injury_classification` (`fac`/`mtc`/`rwc`/`lti_nr`/`lti_r`/`fatality`) distinguishes reportable lost-time injury from non-reportable, which is exactly the §88 threshold. The incidents index with its search, filter, sort and CSV export is the register. |
| 88A | **Notice of certain dangerous occurrences** — whether or not any bodily injury results | `Incident` (`near_miss`, `fire`, `property_damage`); LOPC / Process Safety Event tiering (§11.4.1) | **Covered** | A dangerous occurrence with no injury is a near miss or a property damage incident, and if it is a loss of primary containment the `ProcessSafety::Classify` engine assigns a Tier — Tier 1 on a fatality, community evacuation, or fire/explosion damage over ₹1 Crore; Tier 2 on a reportable lost time injury, onsite evacuation, damage between ₹10 Lakhs and ₹1 Crore, or a chemical release at or above 500 kg — with the triggers and a human-readable classification reason stored. That is a more rigorous dangerous-occurrence taxonomy than the statute asks for. |
| 89 | **Notice of certain diseases** — a medical practitioner attending a person suffering from a Third Schedule occupational disease must report it; the occupier must send notice | `OhcExamination`/`OhcExaminationTest`; `IncidentNotification` (§11.19, §11.4) | **Partial** | An abnormal surveillance test result is captured and trended, and the notification mechanism exists. There is **no occupational disease notification record** linking a diagnosis to the Third Schedule and to the notice sent. See gap **S6**. |
| 90 | Power to direct enquiry into cases of accident or disease | `IncidentInvestigation` + `RootCauseAnalysis` (§11.4) | **Covered** | A structured investigation with a team, a fishbone/5-Whys root cause analysis on a draft-then-submit-for-approval workflow, and CAPA where warranted. |
| 91 | Power to take samples | — | **Not covered** | Needs gap **S2**. |
| 91A | **Safety and occupational health surveys** — the Chief Inspector may direct a survey; workers must present themselves for medical examination | `SurveillanceProgram`/`HazardExposure`/`OhcExamination` (§11.19) | **Covered** | Hazard-based surveillance programmes with named protocols, per-hazard examination frequencies, multi-hazard enrolment by dated exposure, and test-result trends are precisely what a survey would draw on. |

### Statutory registers and returns

The Act delegates the actual forms to the **state Factory Rules**, so numbering
differs between Gujarat, Maharashtra, Tamil Nadu and the rest. Registers are
therefore named here by content.

| Register / return | Application position | Status |
|---|---|---|
| Register of accidents and dangerous occurrences (§88/§88A) | `Incident` index with search, filter, sort, pagination and CSV export | **Covered in substance**, not in prescribed format |
| Health register for workers in dangerous operations (§87, §41C) | `Ohc::MedicalExaminationRegisterPdf` — last examination, fitness verdict, next due date, with overdue and never-examined called out | **Covered in substance** |
| Register of examination — hoists, lifts, lifting tackle, pressure plant (§§28, 29, 31) | `MaintenanceRecord` per `Asset`, with due dates, an overdue report and a daily reminder | **Partial** — no competent-person identity or certificate structure |
| Register of adult workers, with shift and group (§62) | — | **Not covered** |
| Register of leave with wages; muster roll; register of compensatory holidays | — | **Not covered — out of scope** (payroll) |
| Register of whitewashing / lime washing dates (§11) | — | **Not covered** |
| Annual return and half-yearly return | — | **Not covered** | 
| On-site emergency plan and its rehearsal record (§41B) | `EmergencyScenario`/`EmergencyDrill`/`EmergencyPlanReview` | **Covered** |
| Safety Committee minutes (§41G) | `SafetyMeeting` (`safety_committee`) with participants and action items | **Covered** |

**None of the above prints in a prescribed statutory format.** Every one of them
is a general-purpose index or a PDF designed for this app, not for an inspector's
form. That is gap **S8**, and it is cheap relative to its inspection value —
the app already has five hand-written Prawn renderers to copy from.

---

## Part 2 — OSHA (United States)

### The General Duty Clause and recordkeeping

| Reference | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| OSH Act §5(a)(1) | **General Duty Clause** — furnish a place of employment free from recognized hazards causing or likely to cause death or serious physical harm | `HazopStudy` (§11.14); `RiskOpportunity` (§11.2); `BbsObservation` (§11.15); `Finding`/CAPA (§7) | **Covered** | The General Duty Clause is cited when a recognized hazard has no specific standard. The defence is documented hazard recognition and abatement, which is exactly what a scored HAZOP deviation with an owned recommendation, a BBS observation with a hierarchy-of-controls action, and a Finding with RCA and effectiveness verification produce. |
| 1904.4, 1904.7 | **Recording criteria** — record every work-related injury or illness involving death, days away from work, restricted work or transfer to another job, medical treatment beyond first aid, loss of consciousness, or a significant injury diagnosed by a licensed healthcare professional | `IncidentPerson#injury_classification` (§11.4.2) | **Covered — and unusually well** | The enum `fac`/`mtc`/`rwc`/`lti_nr`/`lti_r`/`fatality` maps almost one-to-one onto 1904.7's own decision tree: first aid case (not recordable), medical treatment case (recordable), restricted work case, lost time injury, fatality. Setting a classification is what triggers the OHC medical workflow (`IncidentPeople::UpdateInjury`), so the recordability determination and the medical response are the same act rather than two disconnected ones. |
| 1904.8–1904.12 | Specific recording criteria — needlesticks, medical removal, **standard threshold shift in hearing**, tuberculosis | `OhcExaminationTest` (`audiometry`); `SurveillanceProgram` (§11.19 Slice 6) | **Partial** | Audiometry is a first-class test type with results trended across successive examinations, which is the data a standard threshold shift determination needs. The **determination itself** — comparing against a baseline audiogram with age correction — is not computed. |
| 1904.29 | **Forms** — OSHA 300 Log, 300A Annual Summary, 301 Incident Report (or equivalent) | `Incident` index + CSV export (§15) | **Partial** | Every field the 300 Log needs exists somewhere on `Incident`/`IncidentPerson` — case number, date, department, description, classification, days away. **Nothing renders it as a 300 Log**, and "or equivalent form" in 1904.29(b)(4) means the substitute must contain the same information and be as readable. See gap **S8**. |
| 1904.32 | **Annual summary** — review the log, certify the 300A by a company executive, and post it from February 1 to April 30 | — | **Not covered** | The certification is a signature by a named executive, which lands squarely on the Part 11 signature gaps. |
| 1904.35 | Employee involvement — a procedure for employees to report injuries, and access to their own records | `WorkerParticipationRecord` (§11.10); `Incident` reporting; `EmployeeMedicalProfile` (§11.19) | **Covered** | Any signed-in user may report, and retains read access to their own submission and their own medical records. |
| 1904.39 | **Reporting fatalities and severe injuries to OSHA** — a fatality within **8 hours**; an in-patient hospitalization, amputation or loss of an eye within **24 hours** | `IncidentNotification` (§11.4); `Incidents::MedicalTaskReminderJob` | **Partial** | The notification record exists with a recipient authority and a required decision, and the medical task reminder job escalates to the full stakeholder list once overdue. **Nothing enforces the 8-hour or 24-hour clock**, and that clock is the whole point of the rule. Adding a due-at on `IncidentNotification` driven by the injury classification would be a small change with real value. See gap **S6**. |
| 1904.41 | Electronic submission of 300A (and 300/301 for larger high-hazard establishments) to the ITA | — | **Not covered** | Depends on the 300 Log existing first. |

### 29 CFR 1910.119 — Process Safety Management of Highly Hazardous Chemicals

The standout section of this document. PSM has 14 elements; this app substantially
covers ten of them, and architecture.md cites 1910.119(i) by name as the basis
for the PSSR module.

| 1910.119 | Element | Application module(s) | Status | Notes |
|---|---|---|---|---|
| (c) | **Employee participation** — a written plan; employees consulted on the conduct of PHAs and given access to PHA and PSM information | `WorkerParticipationRecord` (§11.10); `HazopParticipant` (§11.14); `SafetyMeeting` (§11.11) | **Covered** | HAZOP studies name their team by role with invitation and attendance tracking; worker participation is its own register with guaranteed read-back for the submitter. |
| (d) | **Process safety information** — chemical hazards (SDS), technology of the process (block flow, chemistry, safe upper and lower limits), and equipment (materials of construction, P&IDs, relief system design, safety systems) | `Chemical` master; `Asset` (§11.7); `Document` (§10) | **Not covered** | `Chemical` holds `name`, `cas_number` and `active` and nothing else; `Asset` holds no material of construction, design pressure, or relief system data. P&IDs and design basis can be uploaded as controlled documents, which is how most sites would use this today, but nothing is structured or queryable. See gap **S6**. |
| (e) | **Process hazard analysis** — by a team with process and PHA-methodology expertise, using a recognized methodology; findings addressed with a documented resolution, an action schedule and completion; **revalidated at least every 5 years** | **`HazopStudy`/`HazopNode`/`HazopDeviation`/`HazopParticipant`** (§11.14) | **Covered** | IEC 61882 HAZOP with the 9 standard guide words and 11 process parameters, nodes with a design intent, deviations with causes, consequences, existing safeguards and a `likelihood` × `severity` score through the configurable `RiskMatrixLevel`, and a recommendation with an owner, due date and action status. **`Close` refuses while any deviation still has an open or in-progress action** — the actual governance gate 1910.119(e)(5) asks for. The report PDF renders a colour-banded node overview plus a full worksheet per node. The 5-year revalidation clock is not tracked. |
| (f) | Operating procedures — written, covering steps for each phase, operating limits, safety and health considerations; **certified annually as current** | `Document`/`DocumentVersion` (§10) | **Partial** | Controlled documents with sequential approval, immutable approved revisions, effective dating, controlled distribution, acknowledgement and Read & Understood MCQ assessments pinned to one revision. `Document#review_frequency`/`next_review_date` with reminders covers the annual certification clock adequately. The procedure's *content* — operating limits, consequences of deviation — is an opaque file. |
| (g) | **Training** — initial training, refresher **at least every 3 years**, and a record with the means used to verify the employee understood the training | `TrainingSession`/`TrainingAttendance`/`AssessmentQuestion`/`AssessmentAttempt` (§11.5); `CompetencyRequirement` renewal periods | **Covered** | "The means used to verify that the employee understood the training" is 1910.119(g)(3)'s own wording, and it is exactly what the optional MCQ assessment provides — with the score **frozen at submission**, so editing the paper later cannot retroactively move a past result. Certificates require validated presence plus a pass. `CompetencyRequirement#renewal_period` covers the 3-year refresher. |
| (h) | **Contractors** — evaluate the contractor's safety performance and programmes, inform them of hazards, train, and maintain an injury and illness log for contractor employees | `ContractorWorker`/`ContractorMedicalClearance` (§11.19 Slice 7); `WorkPermit`/`WorkPermitWorker` (§11.22); `Supplier`/`SupplierEvaluation` (§11.6); `FirstAidCase` | **Partial — strong on control, weak on pre-qualification** | On-site control is genuinely enforced: `WorkPermits::Issue` **refuses a permit whose acceptor or roster-linked crew does not hold a `gate_pass_status` of `cleared`**, so an unfit or lapsed contractor worker cannot be signed onto a job; a free-text worker is allowed but surfaced and printed as unverified and counted as such on the compliance report. The toolbox talk is a separate signed record with the acceptor's and contractor supervisor's names. `FirstAidCase` covers contractors alongside employees. What is missing is **contractor safety pre-qualification and performance evaluation** — `SupplierEvaluation` scores quality, delivery and service, with no safety criteria. This is the same gap `iso-standards-mapping.md` records as ISO 45001 §8.1.4 "Not yet covered". |
| (i) | **Pre-startup safety review** — for new facilities and modified facilities where the change requires a change in process safety information; confirms construction and equipment are per design, procedures are in place, a PHA has been performed and recommendations resolved, and training is complete | **`PssrReview`/`PssrChecklistItem`** (§11.13) | **Covered** | Built against this clause by name. Twelve standard checklist domains covering as-built construction vs. design, mechanical integrity, safety/relief devices and interlocks, operating procedures, maintenance procedures, emergency preparedness, PHA/risk-assessment closure, MOC closure, training, environmental compliance, housekeeping and startup-plan communication. `AuthorizeStartup` guards that every item is answered **and that none is still answered "No"**; an item answered "No" can raise its own Finding, and the only way off "No" is that Finding resolving — enforced by `PssrChecklistItems::MaybeResolve`, with every response transition paper-trailed with an actor and timestamp. |
| (j) | **Mechanical integrity** — written procedures, training, inspection and testing on equipment following recognized engineering practice, documented with the date, the person, the equipment identifier, the test performed and the results; deficiencies corrected before further use | `Asset`/`MaintenanceRecord`/`CalibrationRecord` (§11.7) | **Partial** | Due dates, completed dates, provider, result, evidence, next due date, an overdue report and a daily reminder job — a real preventive maintenance and calibration programme. Missing for (j): the recognized engineering practice reference, the acceptance criteria, the inspector's identity as a person rather than a provider string, and any **use-blocking** consequence of a deficiency. Nothing prevents equipment with an overdue statutory inspection from being permitted for work — which is a natural and valuable tie between `Asset` and `WorkPermits::Issue`, given `Issue` already refuses on an uncleared gate pass. See gap **S1**. |
| (k) | **Hot work permit** — issued for hot work on or near a covered process, documenting that fire prevention requirements were implemented, with the dates authorized and the object identified | **`WorkPermit`** (§11.22) | **Covered — exceeds the requirement** | Hot work is a seeded permit type with `requires_gas_test`, `requires_fire_watch` and a shift-escalating approval matrix; validity is capped at the **minimum** `max_validity_hours` across every clubbed type; the permit's template version is frozen at issue so reprinting a closed permit renders the questions it was actually issued against; closure requires four signatures in the order the form prints them, with the fire watcher's signature gated behind a computed 30-minute fire watch release time; and `WorkPermitDownloadLog` records who printed which permit and when. |
| (l) | **Management of change** — written procedures for changes to process chemicals, technology, equipment and procedures; the technical basis, impact on safety and health, modifications to operating procedures, the authorization required, and updating of PSI and procedures | **`MocRequest`/`MocImpactAssessment`/`MocRiskAssessment`/`MocChecklist`/`MocAction`/`MocVerification`** (§11.8) | **Covered** | `moc_type`, `nature`, `duration` with a mandatory `temporary_expiry_date` and an expiry reminder job for temporary changes, a `risk_tier`, structured impact and risk assessments, checklists, affected records and documents, multi-approver sequential approval, implementation actions, a `readiness` status that pairs naturally with a PSSR, and closure that **requires verification**. `PssrReview` optionally links the MOC whose readiness it verifies. |
| (m) | **Incident investigation** — initiated within 48 hours, by a team including someone knowledgeable in the process, with a report addressing findings and recommendations, resolutions documented, and the report reviewed with affected personnel | `Incident`/`IncidentInvestigation`/`IncidentInvestigationTeamMember`/`RootCauseAnalysis` (§11.4); `Incidents::TriageReminderJob` | **Covered** | A named investigation team, a structured fishbone/5-Whys RCA with draft-then-approve, CAPA where the incident warrants a full case, and a daily triage reminder job that chases an untriaged incident — which is the 48-hour clock in practice, though not enforced as one. LOPC and Process Safety Event tiering (§11.4.1) is a genuine PSM-grade addition the standard does not itself require. |
| (n) | **Emergency planning and response** — an emergency action plan per 1910.38 | `EmergencyScenario`/`EmergencyResponseTeamMember`/`EmergencyDrill`/`EmergencyPlanReview` (§11.20) | **Covered** | See 1910.38 below. |
| (o) | **Compliance audits** — the employer certifies it has evaluated compliance with PSM **at least every 3 years**, by at least one person knowledgeable in the process; a report is developed, responses documented, deficiencies corrected, and **the two most recent audit reports retained** | `AuditProgram`/`AuditProgramEntry`/`Audit` (§6.0, §6) | **Covered** | An audit programme with a period, an owner, an approved schedule, per-entry frequency that expands into one dated audit per interval across the period, standard and clause tagging copied through to the audit, a lead auditor, coverage %, and a close guard that refuses while any planned audit is neither opened nor cancelled-with-a-reason. Findings run the full RCA → CAPA → effectiveness-verification loop. The 3-year interval is expressible as a programme frequency. **Note the retention caveat**: "retain the two most recent reports" runs into the app having no retention concept at all — see Part 11 gap G2. |
| (p) | **Trade secrets** — information must be made available to those conducting PHAs, audits and investigations, subject to confidentiality agreements | `Incident#confidentiality` (`normal`/`restricted`/`highly_restricted`); `Document#confidentiality` (`public`/`internal`) | **Partial** | `Incident` has a real three-tier confidentiality with a neutral label for restricted records. `Document#confidentiality` is only `public`/`internal` — there is no restricted document tier and no confidentiality agreement record. |

### 29 CFR 1910 — other General Industry standards

| Reference | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 1910.28, 1910.30 | **Fall protection** and training on walking-working surfaces | `WorkPermit` work-at-height type + checklist templates (§11.22) | **Partial** | Work at height is a seeded permit type with its own checklist template, and the rescue plan check sheets are seeded too. Anchor point inspection and harness inspection records do not exist. |
| 1910.38 | **Emergency Action Plans** — written, with evacuation procedures, procedures for employees who remain to operate critical operations, accounting for employees, rescue and medical duties, means of reporting, and an alarm system; employees trained | `EmergencyScenario`/`EmergencyResponseTeamMember`/`EmergencyDrill`/`EmergencyPlanReview` (§11.20) | **Covered** | Identified scenarios with a scored risk rating, planned response, required resources, external agencies and a named response team; announced and unannounced drills with response and evacuation timings and effectiveness evaluation; and a periodic/post-drill/post-emergency review loop with a daily drill-due reminder job. |
| 1910.39 | Fire prevention plans — list of major fire hazards, handling and storage procedures, ignition source controls | `EmergencyScenario`; `Document` (§10); `WorkPermit` hot work | **Partial** | The plan holds as a document and the ignition source control is the hot work permit. No fire hazard inventory. |
| 1910.95 | **Occupational noise exposure** — monitoring, a hearing conservation programme where exposure equals or exceeds an 8-hour TWA of 85 dB, **audiometric testing (baseline and annual)**, hearing protectors, training, and recordkeeping | `EmployeeMedicalProfile` (`noise` hazard category); `SurveillanceProgram`/`HazardExposure`; `OhcExaminationTest` (`audiometry`) (§11.19) | **Partial — the people half only** | A noise surveillance programme with its own examination frequency and required test set, workers enrolled by dated exposure, annual audiometry, and test-result trends across successive examinations is genuinely a hearing conservation programme's clinical half. What is missing is the **noise dosimetry** — the exposure measurement that determines who must be enrolled — and the standard threshold shift determination. See gap **S2**. |
| 1910.119 | Process Safety Management | *(see the dedicated table above)* | **Covered** | |
| 1910.120 | HAZWOPER — emergency response to hazardous substance releases | `EmergencyScenario`/`EmergencyDrill` (§11.20); `IncidentChemicalRelease` (§11.4.1) | **Partial** | Response planning and drills are covered; the release itself is recorded with the chemical and quantity released and drives PSE tiering. HAZWOPER's tiered responder training levels are not modelled. |
| 1910.132 | **PPE — hazard assessment, written certification of that assessment, selection, training, and certification of training** | — | **Not covered** | 1910.132(d)(2) requires a **written certification** of the workplace hazard assessment identifying the workplace, the person certifying, the date, and identifying the document as a certification. There is no PPE module at all — no PPE catalogue, no issue record, no hazard assessment certification, and no inspection. `BbsAction#control_hierarchy` includes `ppe` as one of the five hierarchy levels, which is the only mention of PPE anywhere in the schema. See gap **S5**. |
| 1910.134 | **Respiratory protection** — a written programme, **medical evaluation before fit testing or use**, fit testing initially and annually, training, and cartridge change schedules | `EmployeeMedicalProfile`/`OhcExamination` (§11.19) | **Partial** | The medical evaluation half is well served — a pre-employment and periodic examination with structured tests including spirometry and a fitness verdict, which is exactly what a respirator medical evaluation needs. **Fit testing has no record**, and it is the single most commonly cited element of this standard. Fit test records are the clearest, cheapest addition on this list: a dated record per person per respirator make/model/size with a pass/fail and an annual clock, reusing the `ContractorMedicalClearance` expiry-and-reminder shape verbatim. See gap **S5**. |
| 1910.146 | **Permit-required confined spaces** — a written programme, a space inventory and signage, entry permits with atmospheric testing, attendants, entry supervisors, rescue services, and permit cancellation and retention for one year | **`WorkPermit`** confined space types + `WorkPermitGasTest` + stand-by person + rescue plan checklists (§11.22) | **Covered — with one real gap** | The entry permit is fully modelled: the confined space and confined-space-inert types, a stand-by person as a named `WorkPermitWorker` role, gas tests validated against configured O₂ and %LEL ranges that refuse an out-of-range reading without an explicit override note, seeded rescue plan check sheets, suspension on an unacceptable reading, and a cancellation reason code vocabulary (`emergency_declared`, `incident_at_work_area`, `permit_conditions_violated`, `other`) so the register can answer "how many permits were cancelled for a violation this quarter" without reading free text. **The gap is the space inventory** — 1910.146 requires the employer to identify and evaluate permit spaces; there is no register of confined spaces as such. `Asset` or `Location` is the natural home. |
| 1910.147 | **Lockout/Tagout** — energy control procedures per machine, annual periodic inspection by an authorized employee with a certification identifying the machine, the date, the employees included and the inspector, and training | `WorkPermit` LOTOTO checklist template (§11.22) | **Partial** | LOTOTO is one of the seeded check sheets and its `critical` items block permit issue. What is missing is the **machine-specific energy control procedure** as a record and the **annual periodic inspection certification**, which is what an inspector asks for. |
| 1910.151 | Medical services and first aid; eyewash and safety showers | `FirstAidCase`/`FirstAidKit`/`FirstAidKitInspection`; `OhcVisit` (§11.19) | **Covered** | Kit register with periodic inspections, overdue report and daily reminder; case register with response time, ambulance and hospital referral. Eyewash and safety shower inspection would be a new `FirstAidKit`-shaped register. |
| 1910.157, 1910.159 | Portable fire extinguishers — **monthly visual inspection and annual maintenance check, with the date and the inspector's initials recorded**; sprinkler system maintenance | `FirstAidKitInspection` as a *pattern* | **Not covered** | Structurally identical to the first aid kit inspection register that already exists, with a shorter clock. Cheap to build by copying it. |
| 1910.178 | **Powered industrial trucks** — operator training and evaluation, with **re-evaluation at least every 3 years**, and certification of the training | `TrainingSession`/`TrainingAttendance`; `CompetencyRequirement#renewal_period` (§11.5) | **Covered** | A renewal period on the competency requirement, an expiring/expired competence dashboard, and a certificate issued only on validated presence plus a pass. `TrainingAttendance#valid_as_of?(date)` answers "was this person qualified on the date they actually did the work", checking both bounds — which is precisely the question after a forklift incident. |
| 1910.212, 1910.217 | Machine guarding; mechanical power presses | `BbsObservation`; `Asset` | **Partial** | Condition findings only; no guarding inspection checklist and no press inspection register. |
| 1910.252 | Welding, cutting and brazing — fire prevention, hot work authorization | `WorkPermit` hot work (§11.22) | **Covered** | See 1910.119(k). |
| 1910.331–335 | Electrical safety-related work practices — qualified persons, energized work permits | `WorkPermit` electrical type (§11.22); `TrainingAttendance` | **Partial** | An electrical permit type exists with its own checklist template. Arc flash boundary and PPE category determination do not. |
| 1910.1020 | **Access to employee exposure and medical records** — retained for the duration of employment plus 30 years, with employee access on request | `EmployeeMedicalProfile`/`OhcExamination`/`HazardExposure` (§11.19) | **Partial** | Employee access to their own records is enforced by the ability rules, and the OHC module's confidentiality boundary is the strictest in the app. **The 30-year retention is not enforceable** — there is no retention period concept anywhere in the schema, and 44 controllers expose `destroy`. This is Part 11 gap G2 surfacing in an OSHA context. |
| 1910.1030 | Bloodborne pathogens — exposure control plan, hepatitis B vaccination offer and declination, post-exposure evaluation, sharps injury log | `OhcVaccination`; `FirstAidCase`; `IncidentPerson` (§11.19, §11.4) | **Partial** | Vaccination tracking with batch, expiry and booster compliance genuinely covers the hepatitis B offer; a needlestick is recordable as a first aid case or an incident. The **declination record** and a distinct **sharps injury log** do not exist. |
| 1910.1200 | **Hazard Communication** — a written programme, labels, **safety data sheets readily accessible**, and employee training on the hazards of chemicals in their work area | `Chemical` master; `TrainingSession` (§11.5) | **Not covered** | `Chemical` holds `name`, `cas_number` and `active`. **There is no SDS holding, no GHS classification, no hazard or precautionary statements, no chemical inventory by location, and no label management.** Training can be evidenced generically. HazCom is consistently among the most-cited OSHA standards, and this is the largest single OSHA gap in the app. See gap **S6**. |
| 1910.1000 & Subpart Z substance-specific standards | **Permissible exposure limits**; exposure monitoring, medical surveillance and recordkeeping for specific substances | `SurveillanceProgram`/`HazardExposure`/`OhcExamination` (§11.19) | **Partial** | Same position as Factories Act §41F: the medical surveillance half is built and the exposure measurement half is absent. See gap **S2**. |

---

## What we are missing — prioritized

Ordered by regulatory exposure. Unlike the [ICH Q7 / HACCP / HARPC gaps](ich-q7-haccp-harpc-mapping.md),
none of these is a foundational absence — every one is an addition to a module
that already exists, and several are a few columns and a reminder job.

### S1 — Statutory equipment examination as a distinct record
**Factories Act §§28, 29, 31, 87; 29 CFR 1910.119(j), 1910.147 · High, both**

`MaintenanceRecord` conflates a preventive maintenance visit with a statutory
examination by a competent person, and neither one blocks anything.

- An **examination type** on the record distinguishing statutory examination
  from routine maintenance, with the **competent person's identity and
  qualification**, the certificate, the acceptance criteria and the verdict.
- Safe working load, design pressure and last-examination date as first-class
  `Asset` attributes rather than free text.
- **A use-blocking consequence.** `WorkPermits::Issue` already refuses a permit
  whose crew has an uncleared `gate_pass_status`; the same guard should refuse a
  permit against an `Asset` with an overdue statutory examination. This is the
  cheapest high-value change in this document — the pattern, the service and the
  guard-error convention all already exist.
- A LOTO **energy control procedure per machine** with an annual periodic
  inspection certification (1910.147(c)(6) names the four fields it must carry).

### S2 — Workplace exposure monitoring against permissible limits
**Factories Act §§13–15, 41F, 91; 29 CFR 1910.95, 1910.1000, Subpart Z · High, both**

The single substantive safety gap. The app knows who is exposed to what and
examines them on a clock; it does not know **how much** they are exposed to.

- An **occupational exposure limit master** — substance or agent, limit value,
  averaging period (8-hour TWA, STEL, ceiling), and the source (Factories Act
  Second Schedule, OSHA PEL, ACGIH TLV), configurable like `RiskMatrixLevel`
  rather than hardcoded.
- A **workplace monitoring record** — sampling point, area or personal, date,
  duration, method, measured value, and an automatic comparison against the
  limit in force **at the time of measurement** (the `WorkPermitGasTest`
  precedent: `acceptable` is computed and stored precisely because the limit is
  configuration that can change).
- Noise dosimetry driving `SurveillanceProgram` enrolment automatically, and a
  **standard threshold shift** determination on successive audiograms.
- An exceedance raises a Finding through the existing `Findings::RaiseFromSource`
  path — but **mandatorily**, not opt-in, since this is a statutory limit.
- This is also gap P13 in the [food/pharma mapping](ich-q7-haccp-harpc-mapping.md);
  build the sample-and-result engine once and point it at both.

### S3 — Workplace inspection rounds and facility condition records
**Factories Act §§11, 17, 18, 19, 42–48; 29 CFR 1910.141, 1910.157 · High, both**

A whole class of recurring, scheduled, checklist-driven inspection that the app
does not have, despite having four different checklist engines.

- A **general workplace inspection** with a versioned checklist, a schedule per
  area, an executor, findings per item and an overdue report — housekeeping,
  lighting, drinking water, sanitary facilities, welfare facilities, fire
  extinguishers, eyewash and safety showers, emergency exits, and signage.
- Build it by **generalizing `FirstAidKitInspection`**, which already has the
  register-plus-periodic-inspection-plus-overdue-report-plus-daily-reminder-job
  shape working, rather than adding a fifth bespoke checklist engine. Use
  `BbsChecklistTemplateVersion`'s immutable-once-published versioning for the
  form so a past inspection is never re-read against wording that did not exist
  yet.
- The whitewashing date register (§11(2)) falls out of this for free.

### S4 — Person attributes and the statutory appointment register
**Factories Act §§23, 40B, 41C(b), 49, 66, 67–71; 29 CFR 1910.119(g), 1910.178 · Medium, both**

**Partially closed (2026-08-06).** Competency-gated role assignment is built —
architecture.md §11.5.3. A `blocking` `CompetencyRequirement` refuses a role
assignment while the person does not hold the competency, `UserCompetency` is
the appointment record, and the Competency Gaps report catches an authorization
that lapses *after* assignment. That closed the shared gap this document
previously listed alongside ICH Q7 §3.1, 21 CFR 117.180 and Part 11 §11.10(i).

What remains:

- Date of birth, sex and employment category on `User` or a person profile,
  without which the young-person and women's-night-work restrictions cannot even
  be expressed. Handle this carefully: these are sensitive attributes and belong
  behind the OHC module's confidentiality shape, not the ordinary
  department-scoped one.
- **Gate permit signing authority**, not only role assignment.
  `WorkPermits::Issue` already refuses a permit whose crew has an uncleared
  contractor gate pass, so the guard shape exists; pointing the same guard at
  `User#holds_competency?` for a permit type's issuer, acceptor and approvers is
  a small, high-value extension of what now exists.
- A **statutory appointment register** — Safety Officer (§40B), Welfare Officer
  (§49), competent persons, first aiders, entry supervisors, authorized LOTO
  employees — recording the qualification, the appointment date and **the notice
  sent to the Chief Inspector**. `UserCompetency` holds the qualification half
  today; the statutory notification half has no field.

### S5 — PPE programme and respirator fit testing
**Factories Act §35, §87; 29 CFR 1910.132, 1910.133, 1910.134, 1910.138 · Medium, both — and cheap**

No PPE module exists. `BbsAction#control_hierarchy`'s `ppe` value is the only
mention of PPE in the entire schema.

- A **PPE hazard assessment with written certification** — 1910.132(d)(2) names
  the four fields it must carry (workplace, certifier, date, and identification
  as a certification), which makes this an unusually well-specified build.
- A PPE catalogue, issue record per person, and inspection for reusable items.
- **Respirator fit test records** — person, make, model, size, method, date,
  pass/fail, annual clock. Copy `ContractorMedicalClearance` verbatim: dated
  clearance, printable certificate, revocation, computed status and an expiry
  reminder job. It is the same record with different columns, and fit testing is
  the most-cited element of 1910.134.

### S6 — Chemical hazard information, SDS, and external notification clocks
**Factories Act §41B, §89; 29 CFR 1910.1200, 1910.119(d), 1904.39 · Medium, both**

Three related things the `Chemical` master is too thin to hold.

- Extend `Chemical` into a real hazardous substance master: GHS classification,
  hazard and precautionary statements, **SDS attachment with a revision date and
  a review clock**, exposure limit reference (S2), incompatibilities, and
  storage requirements. `Chemical` is already paper-trailed and already master
  data in RailsAdmin, so this is additive.
- A **chemical inventory by location** with quantities — which serves 1910.1200
  and 1910.119(d), and also the Factories Act §41B public disclosure and the
  MAH/threshold-quantity determination under the Manufacture, Storage and Import
  of Hazardous Chemicals Rules.
- **Statutory notification clocks.** `IncidentNotification` holds a recipient
  authority and a required decision but no due-at. Add one, derived from the
  injury classification and the applicable rule — OSHA's 8-hour fatality and
  24-hour hospitalization/amputation clocks, and the state-prescribed Factories
  Act §88 timeframe — and wire it into the existing overdue-reminder job pattern
  every other module already uses. An **occupational disease notification**
  record (§89, Third Schedule) belongs here too.

### S7 — Working hours, shift roster and the register of adult workers
**Factories Act §§51–66, 62; OSH Code 2020 · Medium, India-specific**

`Shift` is a real, well-designed site master that nobody is rostered onto — it
exists only so the permit approval matrix can compute "after G-shift".

- A **shift roster** assigning users to shifts by date, which turns the existing
  master into a register of adult workers (§62), a periods-of-work notice (§61),
  and the basis for weekly-hours, daily-hours and spread-over checks.
- Overtime and leave are deliberately **out of scope** — that is payroll, and
  this app should integrate with an HR system rather than grow one. Say so
  explicitly in any proposal rather than leaving it ambiguous.
- Worth noting the incidental benefit: a roster would let
  `MedicalNotifications::Recipients` and the permit escalation resolve *who is
  actually on shift now*, which both currently approximate by role.

### S8 — Prescribed statutory formats and standard log renderers
**Factories Act registers and returns; 29 CFR 1904.29, 1904.32 · Medium, both — and mostly mechanical**

The data largely exists; nothing prints in the format the regulator expects.

- **OSHA 300 Log, 300A Annual Summary and 301 Incident Report** rendered from
  `Incident`/`IncidentPerson`. 1904.29(b)(4) permits an equivalent form provided
  it carries the same information and is as readable, so this is a rendering
  problem, not a data problem.
- State-prescribed Factories Act registers and the annual and half-yearly
  returns, driven from a per-state configuration rather than hardcoded — form
  numbers and columns differ by state, and hardcoding one state's forms is how
  this becomes unmaintainable.
- The app already has five hand-written Prawn renderers to copy from
  (`Audits::ReportPdf`, `HazopStudies::ReportPdf`,
  `Ohc::MedicalExaminationRegisterPdf`, `Documents::CoverSheet`,
  `MasterDocumentRegisters::IndexPdf`), and `Ohc::MedicalExaminationRegisterPdf`
  is already a statutory register in all but name.
- The 300A **certification by a company executive** lands on Part 11 gap G1 —
  it is a signature with a stated meaning, which the approval engine cannot yet
  produce.

### S9 — Contractor safety pre-qualification
**Factories Act contractor provisions; 29 CFR 1910.119(h) · Medium, both**

On-site control is genuinely strong — gate-pass-enforced crews, toolbox talks as
signed records, medical clearance with expiry reminders. **Pre-qualification is
not**: `SupplierEvaluation` scores quality, delivery and service, with no safety
criteria at all.

- Safety criteria on supplier evaluation, or a contractor-specific
  pre-qualification record: injury rates, safety programme review, insurance,
  statutory registrations, and a periodic re-qualification clock.
- A contractor injury and illness log — 1910.119(h)(2)(vi) requires it by name,
  and `FirstAidCase` and `IncidentPerson` already cover contractors, so this is
  a report over existing data rather than a new model.
- This is the same gap `iso-standards-mapping.md` records as ISO 45001 §8.1.4
  "Not yet covered" — closing it closes both.

### S10 — Confined space and hazardous location inventories
**Factories Act §36; 29 CFR 1910.146 · Low, both**

1910.146 requires the employer to identify and evaluate permit spaces before a
permit is ever raised. There is no register of confined spaces, hazardous area
classifications, or energy isolation points. `Location` or `Asset` is the natural
home, and each entry should be linkable from the permit that governs entry to it.

---

## The standout, stated so it is not lost in the gap list

Permit to Work (§11.22) is the module that most directly satisfies a regulator
in either jurisdiction, and it does so for reasons worth restating:

- **The form is data, not schema** — sections, fields and declaration wording
  live in a versioned template, so a second site adopts it without a migration.
- **The template version is frozen at issue**, so reprinting a closed permit
  renders the questions it was actually issued against — which is the single
  property that makes a permit a legal record rather than a screenshot.
- **Limits are enforced, not advisory** — `min_value`/`max_value` on a numeric
  field make "%LEL (0% only)" and "O₂ 19.5–23.5%" block issue; an unacceptable
  gas reading on an active permit suspends it.
- **`yes_no_na` is a distinct field type** because "not applicable" and "not
  answered" are different facts on a safety document, and collapsing them is how
  a missed check becomes invisible.
- **When no approval level resolves, the permit is refused, not auto-approved.**
  A visitor stuck at the gate is an inconvenience; unapproved hot work
  proceeding because nobody was configured is the accident the module exists to
  prevent.
- **Signatures store the declaration text as rendered at signing time**, because
  a signature means the words the signer actually read — which is also the design
  [cfr21-part11-mapping.md](cfr21-part11-mapping.md) gap G1 wants lifted into the
  generic approval engine.

---

## Keeping this document current

Update this file whenever a module changes its posture against either framework —
a new inspection register, a new statutory record, an exposure or PPE module, a
gap from the list above getting built, or a new permit type or checklist
template that changes what a permit enforces. Treat a stale row here the same as
a stale line in `architecture.md`: fix it in the same change that touches the
code.

When the OSH Code 2020 commences for a given site, the subject-matter rows here
carry forward unchanged — revise the section numbers in place rather than
starting a new file, and record the change.
