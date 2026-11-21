# Northbridge Services: Risk Monitoring and Reporting Lab

GRC103 Module 4, Lab 2. All scenario data is fictional.

## What's in this package

| File | What it is |
|---|---|
| `Northbridge_Risk_Monitoring_Workbook.xlsx` | Calculation workbook: source data, visible formulas, Green/Amber/Red classifications, dashboard and charts |
| `Northbridge_Management_Report_Risk_Register_Action_Plan.docx.pdf` | Three-page Word report (A4 portrait): management report, risk register, action plan |

## Workbook guide

| Sheet | Contents |
|---|---|
| Dashboard | March status grid with colour-coded results, three labelled charts, March coverage gap |
| Source Data | The brief's dataset and the Green/Amber/Red thresholds (blue cells are inputs) |
| Calculations | Indicator formulas, status formulas, supporting figures such as unmonitored endpoints and late alerts |
| Notes | How I calculated things, how zero denominators are handled, observed results versus assumptions |

Everything on the Dashboard and Calculations sheets is a live formula. Change a blue cell on Source Data and the results, colours and charts update.

## Formulas used

- Monitoring coverage = active endpoints / eligible endpoints × 100
- Timely alert review = alerts reviewed within 24h / critical alerts × 100
- Restore success = successful tests / tests performed × 100
- Overdue action rate = overdue actions / actions due × 100
- Unauthorised changes = used directly as a count

If a denominator is 0, the cell shows N/A rather than 0% or 100%.
Statuses use unrounded values. `ROUND(x, 9)` only removes floating-point noise.

## Results at a glance

| Indicator | Jan | Feb | Mar |
|---|---|---|---|
| Monitoring coverage | 98.0% Green | 95.0% Amber | 90.0% Red |
| Timely alert review | 95.0% Green | 80.0% Red | 70.0% Red |
| Restore success | 100.0% Green | 90.0% Amber | 80.0% Red |
| Overdue actions | 10.0% Green | 25.0% Red | 40.0% Red |
| Unauthorised changes | 1 Amber | 3 Red | 5 Red |

- March coverage gap: 12 endpoints (120 eligible minus 108 monitored). Green needs 118 monitored, so 10 more.
- Priority concerns: late alert review, unmonitored endpoints, failed restores. Overdue actions are also Red and are covered in the requests to management.
- Risk register: R1 = 16, R2 = 16, R3 = 15, R4 = 12. All High, none Critical.


## Assumptions and limits

- Observed results come from the brief's data. Causes such as analyst capacity or new endpoints not being enrolled are possibilities, not confirmed findings.
- The rise in unauthorised changes alongside falling coverage is a correlation across three data points, not proof of cause.
- Alerts are not treated as incidents.
- Risk scores, owners and deadlines are proposals. No action has been started.
- Thresholds are classroom thresholds. Three months of data shows direction but cannot support a reliable long-term forecast.
