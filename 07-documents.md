# Documents

Sidebar → **Documents → Documents**. Controlled documents with sequential
review/publish approvals, immutable published revisions, distribution and
acknowledgement tracking, and generated Control/Master copy PDFs with a
cover sheet and approval signatures baked in. Who can create, review, and
publish for each department is configured once in the
[Document Workflow Designer](01-masters-and-admin.md#document-workflow-designer).

## List and create

![Documents index](images/documents/01-index.png)

**New Document** picks a category (which sets the `DOC-<category>-NNN`
numbering and default review frequency), title, department (only
departments you're a configured creator for are offered, unless you can
manage documents globally), owner, confidentiality, review frequency, QMS
level, and related standards/clauses:

![New document form](images/documents/02-new-form.png)

A document with no revisions yet has nothing to view or download:

![Show, no version yet](images/documents/03-show-no-version.png)

## First revision → review → publish

**Start new revision** (Current Version tab) takes a label, change
summary, and a `.docx` file:

![Start revision form](images/documents/04-start-revision-form.png)

It saves as an editable **Draft** revision — edit it freely, replace the
file, or re-save until it's ready:

![Draft revision, editable](images/documents/05-current-tab-draft-revision.png)

**Submit for approval** shows exactly who the configured reviewers and
publishers are for this department before you submit (and warns if none
are configured), plus lets you add extra approvers for this submission
only:

![In review](images/documents/06-current-tab-in-review.png)

The Approval History tab shows both stages — **every** reviewer must
approve before it moves to the publish stage, and **every** publisher must
approve before it goes effective:

![Approval history, review stage pending, publish stage queued](images/documents/07-approvals-tab-review-stage.png)

Reviewers and publishers approve from **My Approval Requests**, same as
everywhere else. Once the last publisher approves, the revision becomes
the document's **effective, current version** — immutable from here on:

![Current version, effective](images/documents/09-current-tab-effective.png)

## The generated Control/Master Copy PDFs

Publishing kicks off a background job that renders a **Control Copy** and
a **Master Copy** PDF, each with an auto-generated cover sheet — document
number, title, department, revision, issue date, and a full approval
signature table with real timestamps — watermarked accordingly:

![Preview tab, control copy with signatures](images/documents/11-preview-tab.png)

**Download Control Copy** / **Download Master Copy** on the document's
header stay disabled/show a "still generating" message until that job
finishes; every download (not just view) is logged.

## Version Control and other tab visibility

Every past revision stays visible on the **Version Control** tab, with its
status, author, submission and effective dates, and what it superseded:

![Version Control tab](images/documents/10-history-tab.png)

**Record periodic review** (Current Version tab) logs a review outcome
("No change" or otherwise) with an optional note against the effective
version, independent of starting a new revision:

![Review recorded](images/documents/16-show-review-recorded.png)

Not everyone sees every tab on a document's page. Two rules apply, and
they're independent of each other:

**View Department / View All** (`document_access_level`) only ever see
**Current Version**, **Distribution & Acknowledgements**, and, where
applicable, **Preview** — nothing else:

![Tabs for a View Department user](images/documents/19-tabs-view-department.png)

**Admin Department / Admin All** see every tab *except* Version
Control — Approval History, Comments & Activity, and Download Log join
the set above:

![Tabs for an Admin Department user](images/documents/20-tabs-admin-department.png)

**Version Control** is a tab that isn't governed by `document_access_level`
at all — it's its own row in the
[Access Control Matrix](01-masters-and-admin.md#non-model-rows-feature-level-toggles-not-just-whole-records)
("Documents — Version Control tab"), seeded by default so the **Document
Controller** role sees it and no one else does, but from there it's a
normal matrix cell — an admin can grant it to another role, or to one
specific user, from Masters & admin → Access Control Matrix without any
code change:

![Tabs for a Document Controller (Version Control included)](images/documents/21-tabs-document-controller.png)

Approval History/Comments & Activity/Download Log have a second matrix row
of their own too — **"Documents — Approval/Activity/Download Log
tabs"** — but unlike Version Control it ships with no default rule, since
Admin Department/Admin All already show these three tabs on their own.
Use this row when you need to widen access *without* promoting someone to
an admin tier — e.g. granting one particular auditor visibility into
approval history on documents, while everyone else with their role stays
on the narrow View-tier tab set.

## Distribution and acknowledgement

Distribute the effective version to a department, location, role, or
individual user, optionally **requiring acknowledgement**:

![Distribution form filled in](images/documents/12-distribution-form-filled.png)
![Distribution added](images/documents/13-distribution-tab-added.png)

A distributed user sees the version in their own **My Work → Documents
Awaiting Acknowledgement** (see [Getting Started](00-getting-started.md))
and can acknowledge it directly from the Distribution tab; acknowledgement
timestamps accumulate below the distribution list:

![Acknowledged](images/documents/15-distribution-tab-acknowledged.png)

## Download log

Every control/master copy download (who, when, which copy) is logged per
document — and in full across the whole app from the sidebar's own
**Download Log**:

![Download log tab](images/documents/14-download-log-tab.png)

## Archiving

**Archive** removes a document from the active list (a soft state, not a
delete — its history and downloads stay intact) with a confirmation
prompt:

![Document archived](images/documents/17-show-archived.png)
![Index filtered to the demo documents](images/documents/18-index-filtered.png)

## Master Document Register

A Master Document Register is a controlled document that lists other
controlled documents — same version control, approvals, and publishing as
any document above, plus its own linked-document bookkeeping. Pick
**Master Register** as the Type when creating it (Type can't be changed
after creation, same as Category/Department):

![Master Register created](images/documents/22-register-created.png)

### Linking documents

The **Linked Documents** tab (only shown on a register) lets you add any
published controlled document — adding one automatically starts a draft
revision of the register if none is in progress, the same
**Start new revision** mechanism used above. This stays available even
after the register has already published — adding a document once there's
no revision in progress starts a fresh draft that carries every
already-published entry forward first, so nothing is ever lost:

![Document linked to the register](images/documents/23-linked-document-added.png)

Document number, title, current revision, effective date, owner, status,
and next review date are all shown live from the linked document — never
typed in by hand, and never a draft/in-review/rejected version, since only
a document's currently effective revision can ever be linked. **Remove**
only appears on a revision still in progress — once a revision has
published, its entries are read-only history, same as any other document.
Adding straight after a publish, with no revision in progress, looks like
this — the **Add** panel is there, and both the carried-forward and the
newly linked document already show as entries on the new Rev 3 draft:

![Adding a document once the register has already published, with previously published entries carried forward](images/documents/31-add-after-publish.png)

**Download Index (PDF)**, in the same tab's card header, generates a
standalone PDF listing every currently linked document with those same
columns — the exact same content, built fresh from the live entries each
time it's downloaded (no waiting, nothing to regenerate).

### No file to upload

Unlike an ordinary document, a register's **Start new revision** and
**Save draft** forms never ask for a `.docx` file — a register has nothing
to upload:

![A register's revision-in-progress panel, with no file field](images/documents/32-register-no-file-needed.png)

The Linked Documents Index *is* its content: once MR approves, it's
rendered straight into the register's own Control/Master Copy PDFs, cover
sheet and watermark included, exactly like every other document's:

![Linked documents index PDF](images/documents/30-linked-documents-index-pdf.png)

### MR-only approval

Submitting a register's revision skips the usual reviewer/publisher
roster entirely — it goes to whoever holds the **Management
Representative** role only:

![Register submitted for MR approval](images/documents/24-register-submitted-for-mr.png)

MR approval works exactly like every other approval queue in this
app — from **My Approval Requests**:

![MR's approval queue](images/documents/25-mr-approval-queue.png)
![Approving the register](images/documents/26-mr-approval-detail.png)

Once approved, the register publishes like any document — a new revision
of the **register itself**, with its own Control/Master Copy PDFs:

![Register published](images/documents/27-register-published.png)

### Automatic Pending Update

Whenever a document linked into a register publishes a *new* revision,
the register detects it automatically: a fresh draft revision is created
(or an already in-progress one is refreshed) with the linked document's
latest revision/effective date already filled in, and the register is
flagged **Pending Update** until the MR reviews and approves that draft:

![Register flagged Pending Update after a linked document republished](images/documents/28-register-pending-update.png)

The Linked Documents tab on that draft already shows the refreshed
revision — nothing to re-enter by hand:

![Linked document entry refreshed to the new revision](images/documents/29-linked-documents-refreshed.png)

Publishing the register's own next revision (same MR approval flow above)
clears **Pending Update** and, as always, never touches the linked
document's own revision history — that stays governed entirely by its own
review/publish workflow, completely unchanged by any of this.

### Requiring every document to declare its register

A super admin can turn on the **Require Master Register link** setting
(RailsAdmin → Settings — see
[Masters & Admin](01-masters-and-admin.md#settings)). Once on:

- Creating or editing a controlled document shows a required **Master
  Register** dropdown (never shown on a register itself).
- The first time that document publishes a revision, it's added to its
  declared register automatically — no manual "Add" step needed — flagging
  the register **Pending Update** exactly as if someone had linked it by
  hand. Every later revision keeps auto-refreshing that entry the same way.

This is an addition, not a replacement: manually linking documents into a
register (above) still works regardless of this setting, and a document can
still be linked into other registers beyond its own declared one.

---
Previous: [Management Reviews](06-management-reviews.md) · Next: [Context & Interested Parties](08-context-and-interested-parties.md)
