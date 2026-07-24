# Masters & Admin

Master/reference data — the things every other module points at (users,
departments, locations, standards, and so on) — is managed in one place:
**RailsAdmin**, mounted at `/admin` and opened from the sidebar's
**Administration → Masters & admin** link (it opens in a new tab). Three
purpose-built screens sit alongside it for the workflows RailsAdmin isn't a
good fit for: the **Document Workflow Designer**, the **Email Template
Designer**, and the **Access Control Matrix**.

Only roles CanCanCan grants `:access, :rails_admin` (or the matching
`:manage` ability for the three standalone screens) see this section at all
— it's absent from the sidebar for ordinary users, and the controllers
enforce the same rule even if you have the URL.

Everything under **Operations** (Audits, Findings, CAPA, Objectives,
Meetings, Documents, and the rest) is deliberately **excluded** from
RailsAdmin — those records only exist to be created and moved through their
workflow by the app's own screens, never edited as raw data, so you won't
find them here even as a super admin.

## The RailsAdmin dashboard

Every master model gets a row: record count, when the last one was created,
and quick-action icons (list, add, export, bulk-delete, and — key for
day-to-day config work — a **history** shortcut, since every change here is
PaperTrail-audited with who made it). The bottom half is a live feed of the
most recent changes across every master model:

![RailsAdmin dashboard](images/masters-admin/01-rails-admin-dashboard.png)

## Users

**Users → list** supports RailsAdmin's built-in search/sort/filter and
pagination like every model here:

![Users index](images/masters-admin/02-users-index.png)

**Add new** creates an account. Every new user starts with **Must change
password** forced on — see [Getting Started](00-getting-started.md) for
what that looks like from the user's side:

![New user form](images/masters-admin/03-user-new-form.png)

**Show** is the read-only detail view (with the sensitive Devise columns —
encrypted password, tokens — excluded, never displayed anywhere):

![User show](images/masters-admin/04-user-show.png)

**Edit** exposes exactly the fields an admin should be touching:
active/locked state, name/email/title, **Super admin**, per-user
**Document access level** overrides, the two document-download toggles, and
**Must change password** — each with inline help text explaining what it
does:

![User edit form](images/masters-admin/05-user-edit-form.png)

**Reset password** (its own tab next to Edit/Delete) generates a new
temporary password for a locked-out user and forces the change-password
screen on their next sign-in — the same state a brand new account starts
in:

![Reset password action](images/masters-admin/06-user-reset-password.png)

**User roles** and **Department memberships** (visible from the Users list
navigation) are where multi-role, multi-department assignment lives — a
single user can hold several roles and belong to several departments
(exactly one marked primary).

## Roles

The fixed list of role keys the app understands (`super_admin`,
`ims_admin`, `auditor`, `department_head`, and so on — see
[Getting Started](00-getting-started.md)). What each role can actually
*do* is controlled by CanCanCan abilities in code plus the
[Access Control Matrix](#access-control-matrix) below, not by anything
editable here:

![Roles index](images/masters-admin/07-roles-index.png)

## Departments and locations

**Departments** support a parent department (Production is a child of
Operations in the sample data) and carry the `code` every reference number
and report groups by:

![Departments index](images/masters-admin/08-departments-index.png)
![New department form](images/masters-admin/09-department-new-form.png)

**Locations** are the physical sites audits and assets get tagged with:

![Locations index](images/masters-admin/10-locations-index.png)

## External parties

Certification bodies, suppliers, customers, and regulators all live in one
polymorphic table, distinguished by **Party type** — a **Supplier** record
(Operations → Suppliers) always points back to one of these:

![External parties index](images/masters-admin/11-external-parties-index.png)

## Standards and clauses

**Standards** (ISO 9001, 14001, 45001 in the sample data) each own an
ordered list of **Clauses** — shown inline on the standard's own **Show**
page rather than as a separate cross-referenced list:

![Standards index](images/masters-admin/12-standards-index.png)
![Standard show with clauses](images/masters-admin/13-standard-show-clauses.png)

Audits are scored against these clauses via **Audit checklist templates**
(also RailsAdmin-managed, since a template is reference data — only a
scheduled audit's actual checklist is operational).

## Document categories

The default review-frequency policy (Policy/Procedure/Work
Instruction/Form/Record, each annual by default in the sample data) new
documents inherit unless overridden:

![Document categories index](images/masters-admin/14-document-categories-index.png)

## Module flags

One row per sidebar module. Flip **Active** off and that module disappears
from every user's sidebar and its routes redirect away — the emergency
switch for "we're not using Management of Change yet, hide it" without
touching code. A module with no row at all defaults to **on**:

![Module flags index](images/masters-admin/15-module-flags-index.png)

## Organization profile

A singleton — one row, editable, never creatable/deletable again once it
exists — holding the org name, timezone, locale, and contact email used
throughout the app's headers, footers, and printable reports:

![Organization profile show](images/masters-admin/16-organization-profile-index.png)
![Organization profile edit](images/masters-admin/17-organization-profile-edit.png)

## Bulk import

Every master model's list page has an **Import** action for bulk-loading
data from a spreadsheet (`rails_admin_import`). It's only wired up for
these safe reference models — never for operational records, and never for
anything holding a password or approval history:

![CSV import form](images/masters-admin/18-import-form.png)

Upload a CSV/XLS, map its columns to model fields, and preview before
committing — the same validations as the normal form apply to every row.

## Document Workflow Designer

This is the one workflow-shaped screen that lives in Masters & admin
rather than RailsAdmin, because it's really configuring three other
models (`DocumentWorkflowRole` rows) per department in one page instead of
three separate raw tables. For each department you set who may **create**
new document revisions, who must **review** one (all reviewers must
approve before it moves to publishing), and who must **publish** it (all
publishers must approve before it becomes effective) — plus an **Allow
inline approvers** toggle per department letting anyone with a pending
decision loop in extra approvers on the fly:

![Document Workflow Designer](images/masters-admin/20-document-workflow-designer.png)

Add a user from the dropdown + **Add**, or **Remove** one — changes save
immediately per department via the department's own **Save** button (the
inline-approvers toggle) or instantly on Add/Remove (the role lists).

## Email Template Designer

One editable template per notification category the app can send
(`finding.assigned`, `audit.scheduled`, `capa_action.overdue`, and so on —
21 in the sample seed). Leaving a template **inactive** doesn't stop the
email — the recipient still gets the event's plain default wording; the
template only overrides subject/body when turned on:

![Email templates index](images/masters-admin/21-email-templates-index.png)

Edit subject and body with merge fields (`{{recipient_name}}`, `{{title}}`,
`{{body}}`, `{{actor_name}}`, `{{action_url}}`) — the right-hand panel
documents each one, and they're substituted as plain text, never executed:

![Email template editor](images/masters-admin/22-email-template-edit.png)

## Access Control Matrix

Fine-grained, per-model **view / edit / delete** scoping layered on top of
CanCanCan's role abilities — each action can be scoped to **None / Own /
Department / All**. It only ever widens access on top of the app's
built-in rules, never narrows it — see
[How the Access Control Matrix works](17-access-control-matrix.md) for the
full mechanics (role vs. user rows, how conflicting grants resolve, and a
worked before/after example) and screenshots for the screens below.

The index lists every role and lets you jump to any individual user's
override:

![Access control matrix index](images/masters-admin/23-access-control-matrix-index.png)

A **role's** matrix is the default for everyone holding that role:

![Role access matrix](images/masters-admin/24-access-control-role-matrix.png)

A **user's** matrix overrides their role's defaults one cell at a time —
anything left on "Inherit from role" falls back to the role setting, and
clearing every action for a resource removes the override row entirely
rather than leaving a dead blank rule behind:

![User access override](images/masters-admin/25-access-control-user-override.png)

### Non-model rows: feature-level toggles, not just whole records

Most matrix rows govern a real record type (Audits, Findings, Assets...) —
View/Edit/Delete on the whole thing. A few rows instead govern a single
*feature* that doesn't fit that shape, and only ever use the **View**
column (Edit/Delete show **Not applicable**, since there's nothing to
edit or delete). Both are on [Documents](07-documents.md#version-control-and-other-tab-visibility)
today:

- **Documents — Version Control tab** — shows or hides the Version
  Control tab. Ships with one seeded default — the **Document
  Controller** role is granted **View: All** — but from here it's just
  another matrix row.
- **Documents — Approval/Activity/Download Log tabs** — widens who sees
  those three tabs *beyond* whoever Admin Department/Admin All already
  shows them to (that base grant isn't matrix-driven — `document_access_level`
  is a per-user tier, not a Role, so there's no single rule that could
  express "everyone at that tier"). Ships with no default row, since the
  tier-based grant already covers the normal case — use this one to add a
  specific role or a specific person without changing their tier.

Either way, once a row exists you can widen it to another role, grant it
to one specific user, or take it away, exactly like any other matrix
cell:

![Documents — Version Control tab row](images/masters-admin/26-access-control-matrix-synthetic-row.png)

---
Previous: [Getting Started](00-getting-started.md) · Next: [Audits](02-audits.md)
