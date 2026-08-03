# Sites (Corporate & Plants)

Most companies running this app are not a single building. There is a
central **corporate office** and some number of **sites** — plants, units,
depots. Each site runs its own management system day to day and should not
be reading another site's incidents, findings or documents. Corporate needs
to see everything, and to look at one plant at a time when it wants to.

That is what a **Site** is: the boundary around a set of records.

> Nothing else changed. Every module, form and workflow in this guide works
> exactly as described — sites are one extra layer underneath all of them.
> If your organization only has one site, the app looks and behaves exactly
> as it did before sites existed, and you can skip this chapter.

## Site, location, department — which is which

These three are easy to confuse, so:

| | What it is | Example |
|---|---|---|
| **Site** | The tenancy boundary. Who can see the record at all. | Plant 1, Plant 2, Corporate Office |
| **Location** | An area or building **inside** a site. Descriptive. | Shop Floor 2, Warehouse, Gate 3 |
| **Department** | The function that owns the work. | Production, QA, Maintenance |

A Location now belongs to exactly one Site. A record tagged "Shop Floor 2"
is implicitly at whichever site that floor is in.

Departments are the interesting one, because they work **two ways**:

- **A corporate department** (no site set) exists once and is selectable
  from every site. Use this for functions that genuinely are one team —
  Legal, Finance, Corporate HSE.
- **A site department** belongs to one plant. Use this for functions each
  plant runs separately. Plant 1 and Plant 2 can each have their own
  `PROD` / Production, each with its own department head, and neither sees
  the other's records.

This is why department codes only have to be unique *within* a site now.

## What a site user sees

Someone posted at Plant 1 sees Plant 1's records. Not "sees Plant 2 greyed
out" — Plant 2's records simply are not there: not in lists, not in search,
not in dashboard counts, not in reports, not in CSV exports, and not by
typing a URL directly.

This holds no matter what else they have been granted. If an administrator
gives a Plant 1 user an "All" row in the [Access Control
Matrix](17-access-control-matrix.md), that widens what they can do **within
their own sites** — it does not reach across sites. The site boundary is
applied after every other permission and can only narrow.

## Corporate access and the site switcher

Cross-site access is a deliberate per-person setting, not something you get
automatically by holding a senior-sounding role. On a user record (Masters
& admin → Users) there is **Site access level**:

- **Own sites** — sees only the sites listed on that user, under **Sites**.
- **All sites** — the corporate view: every site.

Between the two, a **regional manager** is just an "Own sites" user who has
three sites listed rather than one. There is no separate concept for it.

A user who can reach more than one site gets a **site picker in the
topbar**, next to the notification bell. It reads **All sites** by default:

- Leave it on **All sites** to see everything together. Lists gain a
  **Site** column and a **Site** filter so you can tell rows apart.
- Pick a site to work as though you were at that plant. Every list,
  dashboard card, chart, report and export re-scopes at once — you do not
  have to set a filter per page.

The picker only ever narrows. Choosing a site you have not been given
access to does nothing.

The picker also decides **where new records land**. If you are switched to
Plant 2 and raise a finding, it is Plant 2's finding. On **All sites**, new
records go to your own primary site.

> If you only have one site the picker is hidden — there would be nothing
> to switch between.

## Records corporate publishes to everyone

Two things deliberately cross the boundary, because a group-wide management
system needs them to:

**Corporate documents.** A document can be marked **shared with all sites**.
It is then visible, downloadable and acknowledgeable at every plant, while
still being owned and revised centrally — the right shape for a group HSE
policy or a company-wide procedure. Ordinary documents stay at their own
site. On the Documents list a shared document carries an **All sites**
badge. Distribution can also target a whole **Site**, alongside the
existing department/location/role/user targets. See
[Documents](07-documents.md).

**Corporate audits.** An audit can be marked **spans all sites**, for a
corporate-led audit that covers more than the site that owns it. See
[Audits](02-audits.md).

Everything else — incidents, findings, objectives, risks, CAPA, MOC,
training, assets, health records, visitors — belongs to exactly one site
with no exceptions.

## Masters are shared

Reference data is defined once for the whole organization and used by every
site: standards and clauses, document categories, audit checklist
templates, roles, risk matrix levels, suppliers, customers, chemicals, and
email templates. There is no per-site copy to keep in step.

## Setting sites up

In **Masters & admin → Sites**:

1. Create one site per plant, with a short code (`PLANT1`) and name.
2. Mark exactly one site as **Corporate** — the head office. Only one row
   may carry this flag.
3. Under **Locations**, put each existing location under the right site.
4. Under **Departments**, leave genuinely shared functions with no site,
   and assign plant-specific ones to their plant.
5. Under **Users**, give each person their **Sites** and their **Site
   access level**.

Step 5 matters: a user with **Own sites** and no sites listed sees no
operational records at all. That is deliberate — the system fails closed
rather than guessing — but it does mean a new account is not usable until
someone gives it a posting.

## If you are upgrading an existing installation

Nothing disappears and nothing needs re-entering. Upgrading:

- creates a single site named after your organization profile, marked
  corporate;
- files every existing record, location and user under it;
- leaves every existing department as **corporate** (no site), so they stay
  selectable everywhere;
- gives super admins and anyone holding `ims_admin` or `top_management`
  the **All sites** view.

The result behaves exactly as it did before — one site, everyone in it. The
boundary only starts doing anything once you create a second site and start
assigning people to it.

Reference numbers stay sequential across the whole organization
(`FND-2026-0001`), not per site, so numbers already issued remain valid and
unique.

---

See also: [Masters & Admin](01-masters-and-admin.md) ·
[How the Access Control Matrix works](17-access-control-matrix.md) ·
[Documents](07-documents.md) · [Dashboards & Reports](16-dashboards-and-reports.md)
