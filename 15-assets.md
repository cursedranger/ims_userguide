# Assets

Sidebar → **Operations → Assets** (routes live under `/physical_assets`).
Equipment requiring calibration and/or maintenance tracking.

## List and create

![Index](images/assets/01-index.png)

**New** sets a unique code, name, department, location, owner,
calibration/maintenance frequency (months), and status
(active/under maintenance/retired):

![New form](images/assets/02-new-form.png)
![Show](images/assets/03-show.png)

**Calibration overdue** / **Maintenance overdue** badges appear
automatically on the header once a due date has passed.

## Calibration and maintenance records

Add a calibration record — due date, completed date, provider, pass/fail
result, and next due date:

![Calibration form filled in](images/assets/04-calibration-form-filled.png)
![Calibration recorded](images/assets/05-calibration-tab-recorded.png)

Maintenance records follow the identical pattern on their own tab, with a
free-text **work performed** field instead of a pass/fail result:

![Maintenance recorded](images/assets/06-maintenance-tab-recorded.png)

Assets with an approaching or passed due date roll up into the
**Calibration Due** report and the dashboard's Operations Alerts — see
[Dashboards & Reports](16-dashboards-and-reports.md).

## Filtering

![Filtered to active](images/assets/07-index-filtered.png)

---
Previous: [Suppliers](14-suppliers.md) · Next: [Dashboards & Reports](16-dashboards-and-reports.md)
