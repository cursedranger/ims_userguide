# Access Control: how authorization works, end to end

This is the precise, current-state reference for how this application decides
who can see and do what. It exists because access control is not one
mechanism here — it's two layers that compose — and getting the composition
rule wrong is the one mistake that would actually be dangerous (silently
over-granting) or actually be disruptive (silently under-granting for a role
that used to work). Read this before touching anything in
`app/models/concerns/abilities/`, `app/models/access_control_rule.rb`,
`app/models/access_controllable.rb`, or `app/services/access_control_rules/`.

For the admin-facing UI walkthrough of the matrix specifically (screenshots,
click-by-click), see
[the user guide's Access Control Matrix deep dive](user-guide/17-access-control-matrix.md).
This document covers the whole system, including the code-level rules that
guide doesn't touch.

## 1. The one-sentence model

Every permission in this app is decided by **CanCanCan**, and every single
rule that contributes to that decision is a `can` grant — there is not one
`cannot` anywhere in the codebase (verified: `grep -rn "cannot" app/models/ability.rb app/models/concerns/abilities/` returns nothing). That means:

> **The final answer to "can this user do X to this record?" is always the
> logical OR of every rule that matches — from code, and from the matrix.
> Nothing in this system can take away permission that something else
> granted.**

There are exactly two sources of `can` rules:

1. **Built-in rules** — plain Ruby, in `app/models/concerns/abilities/*.rb`, one module per business area. Fixed at deploy time; changing them means changing code and shipping.
2. **The Access Control Matrix** — `AccessControlRule` rows, editable at runtime from Masters & admin → Access Control Matrix. Purely additive on top of (1); it only ever widens.

Both layers are evaluated for every request. If either grants it, it's
granted.

This isn't just an assertion — it's enforced by
`spec/abilities/access_control_matrix_invariants_spec.rb`, which fails the
build if it ever stops being true. See §8 for exactly what it checks.

## 2. Where this happens: `Ability#initialize`

`app/models/ability.rb` is the single entry point. For every signed-in user:

```ruby
def initialize(user)
  return if user.nil?

  if user.super_admin?
    can :manage, :all
    return                      # <- nothing else runs; total bypass
  end

  apply_masters_abilities(user)
  apply_shared_workflow_abilities(user)
  apply_audit_abilities(user)
  apply_finding_abilities(user)
  apply_objective_abilities(user)
  apply_meeting_abilities(user)
  apply_document_abilities(user)
  apply_risks_compliance_abilities(user)
  apply_operations_abilities(user)
  apply_capa_abilities(user)
  apply_moc_abilities(user)
  apply_access_control_matrix_abilities(user)   # <- always last, always additive
end
```

Two things to internalize from this method alone:

- **`super_admin` is a total bypass.** It returns before any other module —
  including the matrix — ever runs. A `super_admin` account is not
  "granted everything by rules that happen to always match"; it genuinely
  never touches the rest of this system. There is no way to restrict a
  super_admin from inside the app.
- **Every other module runs for every user, unconditionally**, including
  the matrix. Each module method internally decides what (if anything) to
  grant based on the user's roles/departments/assignments — a user with no
  relevant role just accumulates zero rules from that module and moves on.
  Order between modules 2–11 doesn't matter (they're independent `can`
  additions into the same rule set); the matrix running *last* doesn't mean
  it takes priority, it just means it's additive on top of a rule set
  that's already fully built — consistent with "matrix only ever widens."

Every controller loads records through this same `Ability`:
`Model.accessible_by(current_ability)` for lists, `authorize! :action,
record` for single-record actions — see `architecture.md` §"Authorization
& query patterns." **Never** `Model.all` or a bare `where(...)` for
anything user-facing; that's how a permission gets bypassed even if the
`Ability` rule for it is written correctly.

## 3. Layer 1 — built-in rules, module by module

Each file below is included into `Ability` and contributes independently.
"Manage" means create+read+update+destroy. Department scoping almost always
comes in two flavors: **head** (`DepartmentMembership.membership_role ==
"head"`, via `can :manage`) and **member** (any `DepartmentMembership`, via
`can :read`) — a head can edit; a plain member can only see.

| File | Resource(s) | Built-in access |
|---|---|---|
| `masters.rb` | Department, Location, Standard, Clause, DocumentCategory, OrganizationProfile, Role, RiskMatrixLevel, RootCauseCategory, DocumentWorkflowRole, EmailTemplate, AccessControlRule, and more | Everyone: read-only, plus read/update their own `User` record. `ims_admin` only: manage all of the above, manage `User`, access RailsAdmin, import the safe reference subset. |
| `shared_workflow.rb` | Notification, ApprovalRequest | Everyone: read/update their own notifications; read an `ApprovalRequest` they requested or are a decision step on. |
| `audits.rb` | Audit, AuditParticipant, AuditChecklist(Item), Finding | `audit_manager`/`top_management`: manage everything. Everyone: read/update an audit they're a participant on, manage its checklist, create a finding on it, read/update findings they own or raised. |
| `findings.rb` | RootCauseAnalysis, CapaPlan, CapaAction (finding-owned) | `audit_manager`/`top_management`: manage RCA/CAPA for any finding, verify any finding. `auditor`: verify any finding. Everyone: manage RCA/CAPA on findings they own; read/update a CAPA action assigned to them; read an RCA/CAPA plan they're an approval step on. |
| `objectives.rb` | QualityObjective, ObjectiveAssignment, ObjectiveResult | `ims_admin`/`top_management`: manage everything. Department heads: manage their department's objectives. Department members: read. Assigned users: read/update the objective; owners/contributors: manage results; reviewers: read + review results. |
| `meetings.rb` | ManagementReviewMeeting, MeetingParticipant/AgendaItem/ActionItem | `top_management`/`ims_admin`: manage everything. Participants: read/update their meeting and manage its sub-records. Action item assignees: read/update their action **and** read the parent meeting, even after the meeting closes or if they weren't a participant. |
| `documents.rb` | Document, DocumentVersion, DocumentDistribution, DocumentAcknowledgement, DocumentDownloadLog | See §3a below — the most layered of any module. |
| `risks_compliance.rb` | ContextIssue, InterestedParty, RiskOpportunity, RiskTreatmentAction, RiskReview, ComplianceObligation, ComplianceEvaluation | `ims_admin`/`top_management`: manage everything. Everyone: read Context/Interested Party; owners: manage their own. Department heads: manage their department's risks/obligations; members: read. Owners: manage/read their own risk or obligation regardless of department. |
| `operations.rb` | Incident, CompetencyRequirement, TrainingSession/Attendance, Supplier, Asset, CalibrationRecord, MaintenanceRecord | `incident_manager`/`ims_admin`/`top_management`: manage incidents. Department heads: manage **normal**-confidentiality incidents in their department only — restricted/highly-restricted incidents stay limited to reporter/responsible manager/lead investigator/assigned investigators, regardless of department headship. `ims_admin`/`top_management`: manage competency/training/suppliers/assets. Department heads: manage their department's competency requirements/assets; members: read. Owners: read/update their own asset. |
| `capa.rb` | CapaCase, CapaLink, CapaScope, CapaTeamMember, RootCauseAnalysis/CapaPlan/CapaAction/CapaEffectivenessCheck (CapaCase-owned) | `capa_manager`/`ims_admin`/`top_management`: manage everything. Department heads: manage their department's CAPA cases; members: read. Team members (any of 7 team roles): read/update the case and manage its RCA/CAPA/actions. Action assignees: read/update their action. |
| `moc.rb` | MocRequest, MocAffectedRecord, MocImpactAssessment, MocRiskAssessment, MocAction, MocImplementationEvent, MocVerification | `moc_manager`/`ims_admin`/`top_management`: manage everything. Department heads: manage their department's requests; members: read. Everyone: create a request; requester/owner: read/update it and manage its sub-records. Action assignees / impact assessors: read/update their own piece. |
| `access_control_matrix.rb` | Every non-synthetic `AccessControllable` model | See §4. |

### 3a. Documents — worked example of a fully layered module

Documents is the richest built-in module and worth reading in full as the
template for "how many independent sources can legitimately stack on one
resource." A document's `:read` (and by extension `:read_control_copy`) can
come from **any** of these, independently:

1. `document_controller`/`ims_admin` role → unconditional manage.
2. `top_management` role → unconditional read.
3. A `DocumentWorkflowRole` (creator/reviewer/publisher) for that document's department — configured per department in the Document Workflow Designer, not a global role.
4. `document_access_level` tier (`view_department` / `view_all` / `admin_department` / `admin_all`) — a **per-user column**, independent of the Role/UserRole system entirely. `admin_*` tiers additionally see the Master Copy and every tab except Version Control.
5. Confidentiality: `public` documents are readable by everyone; `internal` documents are department-scoped unless the viewer's tier ends in `_all`.
6. An ad hoc approver named on a specific revision's approval request, even outside their normal department.

None of these six sources knows about the other five. A user can fail
(1)-(3) and (6) entirely and still read a document purely through (4)+(5).
That's not a bug — every module in this app is built this way; Documents
just has the most sources on one resource, so it's the clearest illustration
of the OR-composition rule from §1.

## 4. Layer 2 — the Access Control Matrix

### What it is

`AccessControlRule` rows, one per `(Role, resource_type)` or `(User,
resource_type)`, resolved by `AccessControlRules::Resolver` and applied by
`Abilities::AccessControlMatrix` — the **last** module to run in
`Ability#initialize`, but "last" only means "added on top of," not
"overrides." Full mechanics, screenshots, and a live before/after
demonstration are in
[the user guide's matrix deep dive](user-guide/17-access-control-matrix.md);
this section restates the resolution algorithm precisely because this
document needs to be correct standing alone.

### The resolution algorithm

For a given user, resource type, and action (View/Edit/Delete):

1. If that user has their **own** `AccessControlRule` row for that resource
   type, with a non-nil value for that action → **that value wins**, full
   stop. (A user row's field can be left on "Inherit from role," which is
   nil in the database — that's what lets one user's Delete override sit
   next to their Edit still inheriting from their role.)
2. Otherwise, collect every value from **every role the user holds** that
   has a rule for that resource type and action, and take the single
   **most permissive** (`all` > `department` > `own` > `none`).
3. **No matching rule anywhere** (not the user, not any of their roles) →
   `none`. `none` here means "the matrix contributes nothing" — it is not a
   revocation of anything the built-in rules already granted.

```ruby
# AccessControlRules::Resolver — the whole algorithm, verbatim
def scope_for(resource_type, action)
  column = ACTIONS.fetch(action)
  user_value = @user_rules[resource_type]&.public_send(column)
  return user_value if user_value.present?

  role_values = (@role_rules[resource_type] || []).filter_map(&column)
  return "none" if role_values.empty?

  AccessControllable::SCOPES.reverse.find { |scope| role_values.include?(scope) } || "none"
end
```

`Abilities::AccessControlMatrix` turns whatever scope comes out of that into
a `can` grant — `all` → unconditional, `department`/`own` → a hash
condition scoped by the resource's registered `department_condition`/
`own_condition` lambda, `none` → nothing added. It never emits `cannot`.

### `ims_admin`'s matrix-layer bypass

`ims_admin` gets `can :manage, resource_type.constantize` for every
non-synthetic resource **before** the resolver even runs for them — matching
`super_admin`'s blanket bypass in spirit, but scoped to what the matrix
knows about rather than truly everything (an `ims_admin` still goes through
normal resolution for the two synthetic rows, and for every module in §3
that doesn't mention `ims_admin`, like `audits.rb` — see §6 for why that
matters).

### Synthetic (non-model) rows

Two rows aren't real ActiveRecord models — `Documents — Version Control
tab` and `Documents — Approval/Activity/Download Log tabs` — used for
feature/tab-level toggles that don't fit "whole record" View/Edit/Delete.
Only their View column does anything; they resolve into named abilities
(`:read_version_history`, `:read_document_admin_tabs`) instead of a generic
`can <action>, Model`. Full explanation in the user guide's matrix deep
dive and in `docs/user-guide/07-documents.md`.

### The four scopes

| Scope | Meaning | Requires |
|---|---|---|
| `none` | Matrix adds nothing for this action | always available |
| `own` | Records this user created/owns | only if the resource registers an `own_condition` |
| `department` | Records in any department this user belongs to | only if the resource registers a `department_condition` |
| `all` | Every record of this type, company-wide | always available |

A handful of resources (`TrainingSession`, `Supplier`) have neither
condition — for those, only `none`/`all` are ever offered, because there's
no column to scope `own`/`department` by.

## 5. Worked examples across modules

Each example below states the **complete** set of sources that could grant
the permission, built-in and matrix, to make the OR-composition concrete
rather than abstract.

### Example A — reading a Finding

`can?(:read, finding)` is true if **any** of:

- Built-in, `audits.rb`: the user is `audit_manager`/`top_management` (unconditional), or is a participant on the finding's audit, or owns the finding, or raised it.
- Matrix: a role or user-level `AccessControlRule` for `Finding` resolving to `all` (any finding, any department), `department` (their departments only, via `RiskOpportunity`-style `department_id` condition), or `own` (`owner_id`).

A `viewer`-role user (no built-in grants of their own anywhere in the
codebase — see §6) sees **zero** findings until a matrix row for `Finding`
exists for the `viewer` role or for them specifically. That's the matrix
doing exactly its job: giving a role that the built-in code never
anticipated a way to see something, without writing a new `Abilities`
module for it.

### Example B — downloading an Audit's PDF report (the newest example in the app)

This is deliberately the freshest built feature, because it shows the same
principle applied correctly on day one rather than retrofitted:

- **Availability** is a pure business-logic gate on the record itself, nothing to do with authorization: `Audit#report_ready?` (`completed? && findings_resolved?`). No matrix row and no ability controls *whether the PDF exists* — only whether a given user may fetch it once it does.
- **Who may fetch it** reuses plain `:read` on the `Audit` — deliberately no separate `:download_audit_report` ability, unlike Documents' Control/Master Copy split. That split exists in Documents because of a real, distinct per-user download-restriction requirement; nothing in the audit report request asked for that, so it wasn't added (see CLAUDE.md: no abstraction beyond what's needed).
- **Who may browse the download *log*** (`AuditReportDownloadLog`) is its own, narrower ability: `audit_manager`/`top_management` unconditionally, plus a participant scoped to `audit_id: participant_audit_ids` — and that scoping rule is only added `if participant_audit_ids.any?` (see §7's first pitfall for why that guard is load-bearing, not decorative).
- No matrix row exists for either `Audit` or `AuditReportDownloadLog` today. Nothing stops one being added later — an admin could grant a `viewer` role `all` on `AuditReportDownloadLog` from Masters & admin without any code change, the same way Example A's `Finding` row would work.

### Example C — the "matrix `None` doesn't restrict" pitfall, made concrete

Suppose an admin, worried that too many people can see Assets, sets the
`auditor` role's Assets row to **View: None** in the matrix, expecting this
to lock auditors out of assets outside their own department.

**It will not do that.** `operations.rb` already grants `can :read, Asset,
department_id: member_department_ids` to *every* user unconditionally —
that's a built-in rule, not a matrix rule, and the matrix cannot touch it.
Setting the `auditor` row to `None` only guarantees the matrix itself adds
nothing on top; auditors keep their built-in department-scoped read exactly
as before. If the actual goal was "auditors should never see another
department's assets," that requires changing `operations.rb` — a code
change and a deploy, not a matrix edit. This is the single most important
practical consequence of §1's OR-composition rule: **the matrix is a
one-way ratchet, only ever turning access up.**

## 6. Roles with no built-in grants at all

Grep `role?(:viewer)` and `role?(:employee)` and `role?(:approver)` across
`app/models/concerns/abilities/` — none of them appear. Those three roles
(along with any future role added the same way) get **no code-level
permissions whatsoever** beyond what every user gets regardless of role
(their own records, their own notifications, approval steps they're on).
Everything a `viewer` can see is either the handful of universal rules in
§3, or something an admin explicitly granted via the matrix.

This is intentional, not an oversight — it's what makes the matrix a real
alternative to writing a bespoke `Abilities` module for a role whose access
pattern doesn't fit any of the existing ones. It also means: **if a report
comes in that "a Viewer/Employee/Approver can't see something they should,"
check the matrix first, not the ability files** — there's a very good
chance nothing in code was ever meant to grant it.

## 7. Correctness pitfalls (learned the hard way in this codebase)

These are real mistakes made and caught while building features on this
system — worth knowing before you touch an ability file or a matrix
condition.

1. **A hash-conditioned `can` rule with an empty-array condition still
   "exists" for CanCan's class-level `can?(:read, SomeModel)` check.**
   CanCan can't evaluate a hash condition without an instance, so a
   class-level check (used by `authorize!` on an index action, and by
   `<% if can? :read, SomeModel %>` nav-link guards) returns true if *any*
   rule for that class exists at all — even one scoped to `id: []`. Adding
   `can :read, AuditReportDownloadLog, audit_id: participant_audit_ids`
   unconditionally for every user (instead of gating it behind
   `if participant_audit_ids.any?`) silently let every signed-in user past
   the index controller's `authorize!`, even though the *instance*-level
   scoping (`accessible_by`) was correctly returning zero rows for them.
   **Always guard a conditioned `can` with a presence check on the
   collection driving its condition**, or test the class-level `can?` case
   explicitly, not just the instance-level one.

2. **Attaching an Active Storage file bumps `lock_version` outside the
   in-memory record.** Not an authorization bug, but it'll look like one
   from a stack trace (`ActiveRecord::StaleObjectError`) right next to
   `update!(status: ...)` calls that run immediately after an attach. Call
   `reload` between the attach and the next `update!`.

3. **Ransack + `accessible_by` compose correctly by default, but only if
   every index action actually starts from `accessible_by(current_ability)`
   before calling `.ransack`.** A `.ransack(params[:q])` on an unscoped
   `Model.all` bypasses every rule in this document. Every controller in
   this app follows the `scope = Model.accessible_by(current_ability)...;
   @q = scope.ransack(...)` pattern — if you add a new index action, copy
   that pattern exactly, don't write a new one.

4. **The matrix's `available_scopes_for` silently narrows the dropdown, not
   the data.** If a resource has no `own_condition`/`department_condition`
   registered in `AccessControllable::REGISTRY`, the admin UI simply won't
   offer `Own`/`Department` as options for it — it is not possible to
   accidentally under-scope a resource that has no column to scope by. If
   you add a new resource to the registry and forget its condition lambda,
   the failure mode is "the option isn't offered," not "the option is
   offered but does nothing" — safe by construction.

5. **Role-page matrix saves persist a `None` row for every resource type,
   even ones you didn't touch**, because the role edit page has no "leave
   blank" option (every select always has a real value, defaulting to
   "None"). This is harmless — a persisted `None` row and no row at all
   resolve identically — but don't be alarmed seeing a role's "configured
   item types" count jump to the full registry size after a single save;
   it's not a sign anything was over-granted.

## 8. How to verify a permission change is actually correct

Given how much rides on this, don't trust a code read alone — verify with
one of these three, every time:

**Standing proof — `spec/abilities/access_control_matrix_invariants_spec.rb`.**
This file exists specifically to make §1's two claims ("untouched matrix =
built-in only" and "matrix can only widen") *mechanically checked on every
test run*, not just asserted in prose here. It runs against every real
registered resource, not a hand-picked one or two, so it can't go stale the
way a single worked example could:

- Zero `cannot` calls anywhere in `Ability`/`Abilities::*` — the structural
  precondition the whole guarantee rests on.
- A `viewer`-role user (confirmed in §6 to have zero built-in grants) gets
  exactly the app's unconditional-for-everyone reads and nothing else, with
  zero matrix rows in the database.
- For **every** entry in `AccessControllable::MODELS` (checked against the
  registry directly, so adding a new resource without updating this file
  fails loudly): granting that `viewer` role an `all` matrix row for it
  makes a bare record of that type readable.
- For 8 resources with a built-in department-head grant (QualityObjective,
  RiskOpportunity, ComplianceObligation, Incident, CompetencyRequirement,
  Asset, CapaCase, MocRequest): setting an explicit **user-level `None`**
  override for that exact head, on that exact resource, does **not** take
  away their built-in `:manage`.
- Same for a Finding's owner.
- A role-level `None` row behaves identically to no row at all.

Run it on its own — it's fast, no server or browser needed:
`bundle exec rspec spec/abilities/access_control_matrix_invariants_spec.rb`.
If you ever doubt the additive-only guarantee still holds after a change
to `Ability`, an `Abilities::*` module, the resolver, or the registry, this
is the one command that answers it directly rather than by inspection.

**Fastest — a live console check**, against real data, no server needed:

```ruby
user = User.find_by(email: "someone@example.com")
record = Finding.find(123)
ability = Ability.new(user)
ability.can?(:read, record)              # instance-level — the real answer
ability.can?(:read, Finding)             # class-level — only "could this ever match"
Finding.accessible_by(ability).to_sql    # the actual SQL scoping used by index pages
```

**Durable — add it to `spec/abilities/ability_spec.rb`.** Every context in
that file follows the same shape: build a user with a specific
role/department/ownership/assignment, assert `be_able_to`/`not_to
be_able_to` against a real factory-built record. When you add or change a
rule (built-in or matrix), add the positive case *and* the negative case in
the same context — the negative case is what actually catches an
over-broad hash condition or a missing guard like pitfall #1 above. Run
just that file fast: `bundle exec rspec spec/abilities/ability_spec.rb`.

For anything involving the matrix specifically, there's a second angle:
edit the row for real (dev server, `/access_control_rules`) and reload the
affected page as that user, before *and* after — a live before/after is
what caught pitfall #1, a spec alone would have needed to already know to
check the class-level case.

## 9. Deciding where a new rule belongs

```
Does this decision depend on something an admin should be able to
reconfigure without a deploy (which role/user, how widely)?
  │
  ├─ No, it's inherent business logic (ownership, department, workflow
  │  stage, confidentiality) → built-in rule, in the relevant
  │  app/models/concerns/abilities/*.rb file.
  │
  └─ Yes → does it fit "View/Edit/Delete on a whole record type"?
       │
       ├─ Yes, and the model isn't in AccessControllable::REGISTRY yet
       │  → add it there (with department_condition/own_condition if the
       │  model has the columns for them), no ability code needed beyond
       │  that — Abilities::AccessControlMatrix picks it up generically.
       │
       └─ No, it's a feature/tab/sub-permission that isn't "the whole
          record" (like Documents' two tab-toggle rows) → add a synthetic
          entry (synthetic: true, actions: %i[view], conditions: nil) and
          one line in Abilities::AccessControlMatrix#apply_synthetic_matrix_abilities
          resolving it to a named ability.
```

Either way: **never add a `cannot`.** If a change appears to require taking
access away from someone, the actual fix is almost always narrowing an
existing built-in `can`'s condition (or, if it's matrix-driven, simply not
creating/removing that role's or user's matrix row) — not introducing the
one kind of rule this entire system has been deliberately built without.

---
See also: [Access Control Matrix — the UI walkthrough](user-guide/17-access-control-matrix.md) · [Masters & Admin](user-guide/01-masters-and-admin.md#access-control-matrix) · `architecture.md` §"Authorization & query patterns."
