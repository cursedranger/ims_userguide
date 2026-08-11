# Factories Act 1948 & OSHA — Occupational Safety Statutory Mapping

**Last reviewed: 2026-08-11 — update this alongside any new module.**

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

What is missing is now the **prescribed formats** rather than the underlying
records: the state-prescribed registers and returns (gap S8), and the
working-hours and leave records (gap S7). Exposure monitoring against
permissible limits was the one substantive safety gap and is **closed**
(§11.7.4); the recurring welfare and housekeeping inspection is **closed**
(§11.25); and the **PPE programme with respirator fit testing** is **closed**
(§11.26). What remains is clerical: prescribed formats (S8), working hours
(S7), and the smaller registers in S4, S6, S9 and S10.

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
| 11 | **Cleanliness** — factory kept clean and free from effluvia; floors cleaned, whitewashing/painting at prescribed intervals with a record of the dates | `WorkplaceInspectionRound`/`WorkplaceInspection` (§11.25) | **Covered** | Housekeeping is a scheduled round per area with an owner, a frequency and an overdue report; an area never walked counts as overdue. §11(2)'s whitewashing register is a standard question carrying the statutory reference and a **required note** for the date, held against an immutable form version. The facility *sanitation-schedule* half that food safety asks for (gap P5) is still narrower than this. |
| 12 | Disposal of wastes and effluents | `EnvironmentalAspect` (§11.9); `ComplianceObligation`/`ComplianceEvaluation` (§11.3) | **Partial** | Waste streams register as environmental aspects with scored significance, control measures and periodic review, and the consent conditions register as compliance obligations with an evaluation frequency and evidence. There is no effluent monitoring result record. |
| 13 | Ventilation and temperature | `ExposureLimit`/`ExposureMeasurement` (§11.7.4); `MonitoredCondition` (§11.7.2) | **Partial** | Airborne agents can now be sampled against a permissible limit with an automatic verdict and a mandatory Finding on exceedance. Temperature and general ventilation as *comfort* parameters are `MonitoredCondition`'s territory and need a condition configuring per area. |
| 14 | **Dust and fume** — effective measures where dust or fume is likely to be injurious; exhaust appliances near the point of origin | `EnvironmentalAspect` (§11.9); `SurveillanceProgram`/`HazardExposure` (§11.19 Slice 6) | **Partial** | The *people* side is genuinely covered: `EmployeeMedicalProfile::HAZARD_EXPOSURE_CATEGORIES` includes `dust` and `chemical`, workers are enrolled in a surveillance programme by recording a dated exposure, each exposure runs its own examination clock, and test-result trends are visible across successive examinations. The *atmosphere* side — measuring the dust concentration and comparing it to a limit — is absent. |
| 15 | Artificial humidification | `MonitoredCondition`/`ConditionReading` (§11.7.2) | **Partial** | Humidity is configurable as a monitored condition with a limit band, an action band and a mandatory Finding on breach. The textile-specific prescribed register format is not rendered — gap **S8**. |
| 16 | Overcrowding — minimum cubic space per worker | — | **N/A — physical** | A one-time design and licensing determination, not an ongoing record. |
| 17 | Lighting — sufficient and suitable lighting, prevention of glare and shadows | `WorkplaceInspection` (§11.25) | **Covered** | Two standard questions — general lighting and glare, and emergency/escape-route lighting, the latter **critical** so a No raises a nonconformity automatically. |
| 18 | **Drinking water** — suitable points, legibly marked, cool water where over 250 workers | `WorkplaceInspection` (§11.25) | **Covered** | Points, marking and separation from washing areas are standard questions; potability testing is a second question with a **required note** so the last result and date are recorded rather than asserted. |
| 19 | Latrines and urinals — prescribed number, separate for men and women, maintained clean and sanitary | `WorkplaceInspection` (§11.25) | **Covered** | A standard question covering number, separation, cleanliness, lighting and ventilation, walked on the round's own frequency. |
| 20 | Spittoons | — | **N/A — physical** | |

### Chapter IV — Safety (§§21–41)

| § | Requirement | Application module(s) | Status | Notes |
|---|---|---|---|---|
| 21 | Fencing of machinery | `Asset` (§11.7); `HazopStudy` (§11.14); `BbsObservation` (§11.15) | **Partial** | The guard itself is physical. What the app holds well is the *finding* that a guard is missing — a BBS observation records an unsafe condition with contributing factors and raises a `BbsAction` that forces an explicit hierarchy-of-controls pick, and a HAZOP deviation records the hazard with a scored risk and an owned recommendation. There is no machine guarding inspection checklist. |
| 22 | Work on or near machinery in motion — by trained adult male workers in tight clothing, with a register of such workers | `WorkPermit` (§11.22); `TrainingAttendance` (§11.5) | **Partial** | A permit authorises the non-routine work and names the crew; training evidences the competence. The **register of persons authorised to examine machinery in motion** that §22(2) requires does not exist as such. |
| 23 | Employment of young persons on dangerous machines — only after training and under supervision | `TrainingAttendance` (§11.5) | **Partial** | Training is evidenced with validated presence and a frozen assessment score. There is no age or young-person attribute on `User`, so the prohibition cannot be enforced. See gap **S4**. |
| 24 | Striking gear and devices for cutting off power | `WorkPermit` LOTOTO checklist template (§11.22) | **Partial** | Seeded checklist templates include LOTOTO; the isolation itself is a permit checklist item whose `critical` flag blocks issue when answered "No". No energy-isolation point register. |
| 25–27 | Self-acting machines; casing of new machinery; cotton openers | — | **N/A — physical** | |
| 28 | **Hoists and lifts** — of good construction, thoroughly examined **at least once every six months** by a competent person, with a **register of examination** | `Asset`/`MaintenanceRecord` (§11.7.3); `WorkPermits::Submit` (§11.22) | **Covered** | `MaintenanceRecord#kind` distinguishes the statutory examination from a maintenance visit and carries the competent person's name, qualification and employer, the statutory reference, the acceptance criteria, the certificate number and a `fit`/`fit_with_conditions`/`unfit` verdict. `Asset#statutory_examination_state` is derived nightly, treats never-examined as overdue, and — the part that matters — **refuses any work permit raised against the equipment** once it lapses. `ReportsController#statutory_examinations_due` is the register, with a due/overdue reminder job. The one thing still missing is the *state-prescribed register format*, which is gap **S8**, not this row. |
| 29 | **Lifting machines, chains, ropes and lifting tackles** — thoroughly examined at prescribed intervals by a competent person, with a register | `Asset`/`MaintenanceRecord` (§11.7.3) | **Covered** | As §28, and `Asset#safe_working_load` is now a first-class attribute with its unit rather than free text. Note a practical caveat that is a data-entry matter rather than a software gap: chains and slings have to be registered as individual `Asset` rows to be examined individually. |
| 30 | Revolving machinery — maximum safe working peripheral speed notified | — | **Not covered** | |
| 31 | **Pressure plant** — safe working pressure, examined at prescribed intervals, register maintained | `Asset`/`MaintenanceRecord` (§11.7.3); `PssrReview` (§11.13) | **Covered** | As §28, with `Asset#design_pressure` now a first-class attribute. The recurring statutory examination this row asks for is what §11.7.3 added; `PssrReview`'s "mechanical integrity" and "safety/relief devices & interlocks" checklist domains remain the *startup-event* counterpart, and the two are deliberately separate records. |
| 32 | Floors, stairs and means of access | `BbsObservation` (§11.15) | **Partial** | Condition findings only. |
| 33–34 | Pits, sumps, openings in floors; excessive weights | `WorkPermit` (§11.22) | **Partial** | Openings are typically a permit condition. Manual handling limits are not modelled. |
| 35 | Protection of eyes | `PpeHazardAssessment`/`PpeItem`/`PpeIssue` (§11.26) | **Covered** | Eye and face hazards are assessed and certified per workplace, the goggles selected carry their conformity standard, and the issue register records who received which size. The screen itself is physical; the evidence that suitable protection was selected and provided is not. |
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
| 41F | **Permissible limits of exposure of chemical and toxic substances** — as specified in the Second Schedule | `ExposureLimit`/`ExposureMeasurement` (§11.7.4); `SurveillanceProgram`/`HazardExposure` (§11.19) | **Covered** | The Second Schedule values are a configurable register with their citations, and a workplace sample — personal or area — is judged against the limit in force and stores the verdict alongside it. An exceedance raises a major NC without asking, and a personal sample at or above the action level enrols the worker in their own site's surveillance programme. Both halves of the duty now exist: who is exposed, and how much. |
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
| 1910.132 | **PPE — hazard assessment, written certification of that assessment, selection, training, and certification of training** | `PpeHazardAssessment`/`PpeItem`/`PpeIssue`/`PpeInspection` (§11.26); `TrainingSession` (§11.5) | **Covered** | (d)(2)'s four required fields — workplace evaluated, person certifying, date, and identification as a certification — are each a column, with a check constraint refusing a certified row missing any of them. Selection is recorded per hazard against a catalogue carrying conformity standards, and a hazard with no equipment selected against it is flagged rather than hidden. Certifying supersedes the previous assessment for that workplace and fixes the record. PPE training is an ordinary training session. |
| 1910.134 | **Respiratory protection** — a written programme, **medical evaluation before fit testing or use**, fit testing initially and annually, training, and cartridge change schedules | `RespiratorFitTest`/`PpeIssue` (§11.26); `EmployeeMedicalProfile`/`OhcExamination` (§11.19) | **Covered** | The medical evaluation half was already well served. **Fit testing is now built** exactly as this row proposed — a dated record per person per make/model/size with a pass/fail and an annual clock, reusing `ContractorMedicalClearance`'s expiry-and-reminder shape verbatim. Two things are enforced rather than trusted: (f)(7)'s fit-factor minimum on a quantitative pass, and withdrawal of the respirator on a failed or revoked test. Issuing a respirator to somebody with no valid fit test is flagged at the moment of issue. Cartridge change schedules are not modelled. |
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
| 1910.1000 & Subpart Z substance-specific standards | **Permissible exposure limits**; exposure monitoring, medical surveillance and recordkeeping for specific substances | `ExposureLimit`/`ExposureMeasurement` (§11.7.4); `SurveillanceProgram`/`HazardExposure`/`OhcExamination` (§11.19) | **Covered** | Same position as Factories Act §41F above — both halves now exist. `ExposureLimit#source` records a PEL as a PEL, so an OSHA limit and an ACGIH TLV for the same agent are distinguishable rather than conflated. 1910.95's **standard threshold shift** remains open, since audiograms are not structured data. |

---

## What we are missing — prioritized

Ordered by regulatory exposure. Unlike the [ICH Q7 / HACCP / HARPC gaps](ich-q7-haccp-harpc-mapping.md),
none of these is a foundational absence — every one is an addition to a module
that already exists, and several are a few columns and a reminder job.

### S1 — Statutory equipment examination as a distinct record
**Factories Act §§28, 29, 31, 87; 29 CFR 1910.119(j), 1910.147 · High, both**

**Mostly closed (2026-08-11).** See architecture.md §11.7.3.

`MaintenanceRecord#kind` now separates a preventive maintenance visit from a
statutory examination by a competent person, carrying the competent person's
name, qualification and employer (plus an optional user link, since the
examiner is often external), the statutory reference, the acceptance criteria,
the certificate number and a `fit`/`fit_with_conditions`/`unfit` verdict.
`fit_with_conditions` requires the conditions to be recorded and does not bar
use — the conditions govern. Safe working load and design pressure are
first-class `Asset` columns with units.

`Asset#statutory_examination_state` is derived by
`StatutoryExaminations::RefreshState` on the same terms as `calibration_state`
(nightly sweep, worst-true-statement precedence, never-examined counts as
overdue), and **`WorkPermits::Submit` now refuses a permit against equipment
whose examination has lapsed or which was declared unfit** — the use-blocking
consequence this gap asked for, built on the `guard_gate_passes!` shape that
already existed. `work_permits.asset_id` is the new optional register link that
makes the check possible; a permit naming its equipment only in free text is
still allowed rather than blocked, so a half-built asset register cannot
switch the control off wholesale.

What remains:

- A LOTO **energy control procedure per machine** with an annual periodic
  inspection certification (1910.147(c)(6) names the four fields it must
  carry). This is a distinct model against a distinct regulation with its own
  clock, and was deliberately left out of the examination slice rather than
  bolted onto it.

### S2 — Workplace exposure monitoring against permissible limits
**Factories Act §§13–15, 41F, 91; 29 CFR 1910.95, 1910.1000, Subpart Z · High, both**

**Mostly closed (2026-08-11).** See architecture.md §11.7.4.

`ExposureLimit` is the permissible-limit register — agent, averaging period
(8-hour TWA / 15-minute STEL / ceiling), value and unit, action level, and the
source (Factories Act Second Schedule, OSHA PEL, ACGIH TLV, NIOSH REL or an
internal limit) with its citation. Configurable rather than hardcoded, and
deliberately **not** site-scoped: a statutory limit is set by the statute, not
by the plant. A starting register of the common Indian values plus the noise
limits is seeded in every environment, each row flagged for a competent person
to confirm against the current gazette.

`ExposureMeasurement` is the sample — personal or area, sampling point,
duration, method, the instrument used, and the value — with the limit, the
action level, the unit **and** the verdict all snapshotted onto the row, so
revising a limit never rewrites a past result. An exceedance raises a
**major_nc Finding without asking**, and a personal sample at or above the
action level enrols the worker in their own site's surveillance programme for
that hazard, which is the step that otherwise gets missed.

`ReportsController#exposure_exceedances` is the register of everything that
reached an action level or breached a limit.

What remains:

- **Noise dosimetry driving a standard threshold shift determination** on
  successive audiograms. Dosimetry-driven *enrolment* is built; the STS
  comparison is not, and cannot be until audiograms are structured data —
  `OhcExaminationTest` stores its value as free text, so there is no
  frequency-by-frequency threshold to compare. That is its own slice.
- This gap was previously noted as also closing **P13** in the
  [food/pharma mapping](ich-q7-haccp-harpc-mapping.md). It does not: P13 is
  utility and *microbiological* monitoring (water, compressed air, organisms)
  and depends on P2's `Sample`/`TestResult` engine. The sampling-and-verdict
  shape built here is a good precedent for it, and makes it cheaper, but P13
  stays open.

### S3 — Workplace inspection rounds and facility condition records
**Factories Act §§11, 17, 18, 19, 42–48; 29 CFR 1910.141, 1910.157 · High, both**

**Closed (2026-08-11).** See architecture.md §11.25.

`WorkplaceInspectionTemplate` / `…Version` / `…Item` is the versioned form,
immutable once effective; `WorkplaceInspectionRound` is the register of what
must be inspected, where and how often; `WorkplaceInspection` is one walk,
pinning the form version at start so it always renders the questions actually
asked.

Built exactly as this gap asked — by generalizing `FirstAidKitInspection`'s
register-plus-periodic-execution shape and borrowing
`BbsChecklistTemplateVersion`'s immutable versioning — rather than adding a
fifth bespoke checklist engine.

`PopulateStandardItems` seeds 22 questions drawn from the Act in one action,
covering cleanliness, ventilation, dust and fume, lighting, drinking water,
sanitary facilities, washing and welfare, creche, first aid, fire precautions,
eyewash and safety showers, machine guarding and signage. A round nobody has
ever walked counts as **overdue**, and a **critical** item answered No raises a
nonconformity without asking.

**§11(2)'s whitewashing date register falls out of it for free**, exactly as
predicted: a standard question carrying the statutory reference, a required
note for the date, and an immutable form version behind it *is* that register.

### S4 — Person attributes and the statutory appointment register
**Factories Act §§23, 40B, 41C(b), 49, 66, 67–71; 29 CFR 1910.119(g), 1910.178 · Medium, both**

**Closed (2026-08-11).** See architecture.md §11.5.3 and §11.27.

Competency-gated role assignment was built 2026-08-06. The remainder is now
built too:

- **Person attributes** — date of birth, sex and employment category sit on
  `EmployeeMedicalProfile`, behind the OHC confidentiality boundary exactly as
  this gap insisted, never on `User`. `#statutory_category_as_of` derives
  adult/adolescent/child from the date of birth and returns nil when none is on
  file, and `#employment_category_mismatch?` surfaces a typed category that
  disagrees with it.
- **Statutory appointment register** — `StatutoryAppointment` records the post,
  the qualification, the appointment date and **the notice sent to the Chief
  Inspector**, which is the half `UserCompetency` never had. Posts requiring a
  notice are distinguished from those that do not, and the report lists posts
  nobody currently holds.

What remains, deliberately deferred: **gate permit signing authority on
competency**. `WorkPermits::Submit` now carries three guards of exactly this
shape (gate pass, statutory examination, and the checklist gate), so pointing
one at `User#holds_competency?` for a permit type's issuer and acceptor is a
small extension — but it needs a competency to be nominated per permit type,
which is a configuration decision rather than a build.

### S5 — PPE programme and respirator fit testing
**Factories Act §35, §87; 29 CFR 1910.132, 1910.133, 1910.134, 1910.138 · Medium, both**

**Closed (2026-08-11).** See architecture.md §11.26.

`PpeHazardAssessment` is 1910.132(d)(2)'s written certification, with all four
fields the section names modelled as columns and a check constraint refusing a
certified row missing any of them. `PpeItem` is the catalogue with conformity
standards, `PpeIssue` the issue register per person, and `PpeInspection` the
periodic examination of reusable equipment.

`RespiratorFitTest` is `ContractorMedicalClearance` with different columns,
exactly as this gap asked: a dated, immutable, revocable clearance with a
computed status and an annual clock. Two rules are enforced rather than
trusted — a quantitative pass below 1910.134(f)(7)'s fit-factor minimum is
refused, and a failed or revoked test withdraws the respirator the person was
issued.

Not built: a Prawn certificate renderer. The fit-test page is print-friendly,
which is a printable certificate in the sense asked for; a dedicated renderer
would be a mechanical copy of `ContractorMedicalClearances::CertificatePdf`.

### S6 — Chemical hazard information, SDS, and external notification clocks
**Factories Act §41B, §88, §88A, §89; 29 CFR 1910.1200, 1910.119(d), 1904.39 · Medium, both**

**Closed (2026-08-11).** See architecture.md §11.30.

`Chemical` is now a real hazardous substance master — GHS classification and
signal word, hazard and precautionary statements, incompatibilities, storage,
first aid and firefighting measures, and the SDS as an attachment with a
revision date and a review clock. A hazardous substance with **no revision
date counts as overdue**, because an undated sheet cannot be shown to be
current.

`ChemicalInventory` records what is held where, and the per-site total drives
the **MSIHC Major Accident Hazard determination** — which returns *nil* rather
than false when no threshold is recorded, because "not determined" and "under
the threshold" are different answers.

`IncidentNotifications::DeriveClocks` derives the statutory clocks from the
injury classifications on the incident, with the jurisdictions mapped
separately: 1904.39's **8 hours** for a fatality and 24 for a hospitalisation,
§88's reportable-accident clock for anything above first aid, §88A for an
occurrence with nobody hurt, and **nothing at all for a first-aid-only case**.
The reminder job runs **hourly**, because a daily 7am reminder arrives after an
eight-hour deadline.

**A correction to this gap's own description**, which said
`IncidentNotification` had "no due-at". It had `due_date` — but a date cannot
express eight hours, and nothing derived or chased it. `due_at` is now a real
timestamp and `due_date` is left untouched.

**Residual:** the §89 occupational disease notification has its field
(`third_schedule_disease`) and its clock, but nothing yet *derives* one — that
needs a diagnosis recorded against the Third Schedule, which is an OHC data
shape rather than a notification one.

### S7 — Working hours, shift roster and the register of adult workers
**Factories Act §§51–66, 62; OSH Code 2020 · Medium, India-specific**

**Closed (2026-08-11).** See architecture.md §11.29.

`ShiftAssignment` rosters people onto the existing `Shift` master by date.
That turns it into the **register of adult workers (§62)** and the **notice of
periods of work (§61)** — both now register kinds in the statutory form engine
(S8), printable in any jurisdiction's format.

The hours limits are **checked and surfaced, never enforced**: §51 (48 hours a
week), §54 (9 a day), §56 (10.5-hour spread-over) and §52 (weekly holiday) are
computed and shown on the board, in a breach panel and on the register, but an
assignment always saves. Refusing the record does not prevent the hours. The
one refusal is rostering somebody twice on a day, citing §60.

The incidental benefit this gap predicted is delivered:
`ShiftAssignment.on_duty_at` answers who is actually on shift, and
`MedicalNotifications::Recipients` now notifies the **on-duty first aiders**
by intersecting the §45 appointment register with the roster — additively, so
a site with no roster behaves exactly as before.

**Overtime and leave with wages remain deliberately out of scope** — that is
payroll, and this app should integrate with an HR system rather than grow one.
Say so explicitly in any proposal rather than leaving it ambiguous.

### S8 — Prescribed statutory formats and standard log renderers
**Factories Act registers and returns; 29 CFR 1904.29, 1904.32 · Medium, both**

**Engine closed; per-state content is a data task (2026-08-11).** See
architecture.md §11.28.

`StatutoryFormDefinition` makes the format **configuration**: a jurisdiction,
a form number, a citation, and the columns that jurisdiction wants. Adding a
state is a row, not a class — which is exactly what this gap asked for and the
only design that survives 36 Indian jurisdictions.

Built and verifiable: **OSHA 300 and 300A**, rendered from
`Incident`/`IncidentPerson` under 1904.29(b)(4)'s equivalent-form allowance,
with the classification mapping enforced (`fac` excluded per
1904.7(b)(5)(ii)). Built as an unverified baseline: the Model Rules accident
and young-persons registers.

**Every seeded format ships `verified: false`** and says so on screen and in
print. No individual Indian state is seeded, deliberately — a wrong form
number that looks authoritative is worse than an empty register. Filling in
Maharashtra, Gujarat and the rest from their published Rules is a data task
for somebody with those Rules to hand, and `rails_admin_import` will load a
spreadsheet of them.

What remains:

- **Forms requiring man-days, hours worked or periods of work** — the register
  of adult workers (§62) and the annual return's headcount figures — depend on
  the shift roster in gap **S7**. The engine will carry them once that data
  exists.
- **Total days away / days restricted** on the 300A are not derivable from the
  schema, and print as "Not recorded".
- **The 300A executive certification** is Part 11 gap **G1**; it prints a
  signature block.

### S9 — Contractor safety pre-qualification
**Factories Act contractor provisions; 29 CFR 1910.119(h) · Medium, both**

**Closed (2026-08-11).** See architecture.md §11.27.

`Supplier` carries a `contractor` flag and the four things 1910.119(h)(2)(i)
asks an employer to obtain and evaluate before selecting a contractor — injury
rates (LTIFR/TRIR), safety programme review, insurance currency and statutory
registrations — with a re-qualification clock.
`Supplier#prequalification_gaps` names *which* piece is missing rather than
returning a bare false. `SupplierEvaluation` gains a `safety_score` on the same
scale as the commercial criteria, nullable so a non-contractor is not scored on
a criterion that does not apply, and a score below halfway demands findings.

`ReportsController#contractor_safety` is the register plus the injury log.

**A correction to this gap's own description.** It stated that "`FirstAidCase`
and `IncidentPerson` already cover contractors, so this is a report over
existing data". That is true of `FirstAidCase`, which carries `person_type`. It
is **not** true of `IncidentPerson`, which has no person type at all — a
contractor there is an external person with no linked user, indistinguishable
from a visitor or a member of the public. The report therefore shows a genuine
contractor first aid log and a separately, honestly labelled "external persons
injured" panel. **Residual:** adding a person type to `IncidentPerson` would
close the incident half properly.

This also closes what `iso-standards-mapping.md` records as ISO 45001 §8.1.4.

### S10 — Confined space and hazardous location inventories
**Factories Act §36; 29 CFR 1910.146 · Low, both**

**Closed (2026-08-11).** See architecture.md §11.27.

`HazardousArea` is one register covering confined spaces, electrically
classified areas and energy isolation points — one table with a `kind` rather
than three, following the precedent `calibration_records` set. Each entry
carries its hazards, entry requirements and review clock, and
`work_permits.hazardous_area_id` links the permit to the space it governs entry
to, so 1910.146(c)(1)'s "identify and evaluate before a permit is raised" is
literal rather than aspirational.

Reclassifying a confined space to non-permit requires a written basis, enforced
by a validation and a check constraint — that is precisely the decision
1910.146(c)(7) makes the employer document.

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
