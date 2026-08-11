# Designing a Statutory Form

**Who this is for:** whoever is responsible for the registers and returns your
factory inspector asks to see — usually the Safety Officer, the HR/statutory
compliance lead, or the IMS administrator.

**What you need in front of you before you start:** your state's **published
Factory Rules**, or the actual blank forms your inspectorate issues. This
guide will not tell you what Form 21 looks like in Maharashtra. Nothing in
this application will, and any tool that claims to should be treated with
suspicion — the numbers and columns differ by state and are revised by
notification.

---

## Contents

1. [The idea: the format is data](#1-the-idea-the-format-is-data)
2. [Why no Indian state ships pre-filled](#2-why-no-indian-state-ships-pre-filled)
3. [What is already there](#3-what-is-already-there)
4. [Before you design: three decisions](#4-before-you-design-three-decisions)
5. [Create the format](#5-create-the-format)
6. [Design the columns](#6-design-the-columns)
7. [Print it and read it](#7-print-it-and-read-it)
8. [Mark it verified](#8-mark-it-verified)
9. [Adding the rest of your state's forms](#9-adding-the-rest-of-your-states-forms)
10. [Bulk-loading many jurisdictions](#10-bulk-loading-many-jurisdictions)
11. [What the registers cannot yet produce](#11-what-the-registers-cannot-yet-produce)

---

## 1. The idea: the format is data

The Factories Act 1948 prescribes almost none of its own forms. Section 112
gives that power to the **State Government**, and every state has used it
differently: the annual return is a different form number in Maharashtra than
in Tamil Nadu, and the columns differ with it.

So in this application a statutory form is split in two:

| Half | What it is | Where it lives |
|---|---|---|
| **The register** | How the rows are gathered — which incidents, which people, over what period | Code. There are four: accident register, young persons register, OSHA 300, OSHA 300A. |
| **The format** | Your state's form number, title, citation, and which columns in which order with what labels | **Data you edit in the app.** |

Adding a state is filling in a format. It is never a code change, and you
never need a developer for it.

---

## 2. Why no Indian state ships pre-filled

Only two jurisdictions ship configured:

- **United States (OSHA)** — Forms 300 and 300A, marked **verified**. These
  are federal, fixed, and specified column by column in 29 CFR 1904 itself,
  so they can be shipped correct.
- **India — Model Rules baseline** — marked **unverified**. Most state Rules
  derive from the Model Factories Rules, so this is a sensible starting
  shape, but the Model Rules are not what governs you. Your state's Rules are.

No individual Indian state is pre-filled, deliberately. A wrong form number
that *looks* authoritative is worse than an empty register: somebody prints
it, believes they are compliant, and finds out otherwise in front of an
inspector. Filling in your state from its actual Rules takes a few minutes per
form and is the only way the result can be trusted.

---

## 3. What is already there

**Statutory Forms** (sidebar) lists every format configured and lets you print
any of them for a period. **Statutory Formats** (the "Manage formats" button)
is where you add and design them.

Four registers are available to draw from:

| Register | What it gathers | Basis |
|---|---|---|
| Register of accidents and dangerous occurrences | One row per **injured person**, not per incident — the Act counts people | §§88, 88A |
| Register of young persons | Anyone under 18, derived from date of birth | §§67–71 |
| OSHA Form 300 | Recordable injuries and illnesses, first aid excluded | 29 CFR 1904.29 |
| OSHA Form 300A | Annual totals over the same recordable set | 29 CFR 1904.32 |

---

## 4. Before you design: three decisions

**Which register does this form draw from?** Look at your blank form and ask
what each row *is*. A row per injured person is the accident register. A row
per young worker is the young persons register. If your form's rows are
neither, this application cannot fill it yet — see §11.

**What is the form's number and citation?** Copy them exactly from the Rules.
"Form 21" and "Rule 119, Maharashtra Factories Rules 1963" are what an
inspector matches against.

**Which columns does it have, in what order, with what wording?** Write them
down in the order they appear across the page. You will type them in §6.

---

## 5. Create the format

**Statutory Forms → Manage formats → New format.**

| Field | What to put |
|---|---|
| Jurisdiction code | ISO-3166-2 style: `IN-MH`, `IN-GJ`, `IN-TN`. This keeps two states' Form 21 apart. |
| Jurisdiction | The state's name as you want it to read: `Maharashtra`. |
| Form number | Exactly as the Rules give it: `Form 21`. |
| Title | The form's own title. |
| Citation | The rule that prescribes it. |
| Draws from | One of the four registers above. |
| Frequency | Continuous, monthly, half-yearly, annual, or on event. This is a label — it does not schedule anything. |

Save. The format is created carrying **every column its register can
produce**, so it prints something useful immediately, and you narrow it next.

> **Shortcut:** on any existing format, **Copy for another state** pre-fills
> everything and clears the jurisdiction and verification. For your second and
> subsequent states this is much faster than starting blank.

---

## 6. Design the columns

You land on the edit page after saving. Scroll to **Design the form —
columns**.

There is one row per column the register can produce:

- **Print** — tick the ones your form has. Anything unticked is simply not
  printed; the underlying data still exists and other formats can still use it.
- **Order** — the number it appears in, left to right. They are sorted by this,
  so you can renumber freely.
- **Label on the form** — type the wording *your form* uses. If your Form 21
  says "Name of injured person", type that. This is what prints, and it is
  what makes the output recognisable to an inspector.

Leave a label blank and it falls back to a readable version of the data key,
which is fine for a draft and poor on a printed return.

Save. The **Columns printed** panel on the format's page now shows exactly
what will appear, in order.

> Changing **Draws from** changes which columns are available. Save first,
> then reopen the page to design against the new register.

---

## 7. Print it and read it

**Statutory Forms → Open** on your format.

Set the period — it defaults to the current calendar year, which is what
annual returns and the OSHA 300A are filed for — and **Print**.

Read the printed page against your blank form, column by column. This is the
step that catches a mislabelled or out-of-order column, and it takes a minute.

Two things worth knowing about the output:

- **A nil return still prints as the form**, headers and all, with a `NIL`
  row. That is deliberate: a nil return is evidence the register was kept, and
  a blank page is not.
- **Nothing prints that the viewer could not see on screen.** The registers
  respect the same permissions and site boundary as every other page, so
  printing never widens access.

**Export CSV** gives the same columns and labels for anyone who wants the data
in a spreadsheet.

---

## 8. Mark it verified

Every format ships and is created **unverified**, and says so — on the list,
on the register, and in a warning above the printed page.

Once you have checked the form number, the citation and every column against
the published Rules, edit the format and set:

- **Checked against the published Rules** — tick it
- **Checked by** — your name
- **Checked on** — the date

The warning disappears and the format shows as **Verified**. This is not
decoration: it is how the next person knows whether to trust the layout, and
how you know which of your states still need doing.

---

## 9. Adding the rest of your state's forms

Repeat §5–§8 per form. A typical Indian factory needs, at minimum:

- the accident register (§88)
- the register of young persons, if any are employed (§§67–71)
- the annual return
- the half-yearly return, where the state requires one

The first two are supported now. The returns need headcount and man-day
figures — see §11.

---

## 10. Bulk-loading many jurisdictions

If you run factories in several states, do not type each one. Formats are
importable from a spreadsheet:

**Admin (`/admin`) → Statutory Form Definitions → Import.**

Importable columns: jurisdiction code, jurisdiction name, form code, title,
statutory reference, register kind, frequency, notes, verified, verified by,
verified on, active.

**The column layout is deliberately not importable.** It is a nested structure,
and a spreadsheet cell full of JSON is exactly how a form ends up printing a
blank strip down the page. Import the identity and citation, then design the
columns in the app — it is a minute per form and you get to see the result.

Only an IMS administrator can import.

---

## 11. What the registers cannot yet produce

Stated plainly, so you do not go looking:

**Anything needing hours worked, man-days or periods of work.** That includes
the **register of adult workers (§62)** and the headcount and man-day figures
on the **annual return**. These need a shift roster — people assigned to
shifts by date — which this application does not yet have. The engine will
carry these forms the moment that data exists; nothing about the format design
changes.

**Total days away and total days on restricted work** on the OSHA 300A. The
system records that a case *was* lost-time; it does not record how many days
were lost. The summary prints "Not recorded" rather than a false zero.

**The OSHA 300A executive certification.** 29 CFR 1904.32(b)(4) requires a
company executive to certify the summary. That is a signature with a stated
meaning, which the approval engine cannot yet produce, so the summary prints a
signature block to be signed by hand.

**Leave with wages registers.** Deliberately out of scope — that is payroll,
and this application should integrate with an HR system rather than grow one.

---

## Related

- [Designing a Permit Form](34-designing-a-permit-form.md) — the same
  form-is-data idea applied to permits to work
- [Factories Act & OSHA mapping](standards/factories-act-osha-mapping.md) —
  what the application covers against each section, and what it does not
