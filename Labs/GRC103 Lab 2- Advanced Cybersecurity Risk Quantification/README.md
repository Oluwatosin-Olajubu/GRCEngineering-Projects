# Lab 3b: Advanced Cybersecurity Risk Quantification

**SecureBank Financial: SecureMobile Banking Platform**

| | |
|---|---|
| **Author** | Oluwatosin Olajubu |
| **Registration no.** | `...........` |
| **Date** | 27 October 2026 |
| **Role** | Lead GRC Analyst (reviewed by CISO and CFO) |

A quantitative risk assessment of a mobile banking platform. It turns three critical penetration-test findings into annual financial exposure (SLE, ARO, ALE), evaluates three security controls (ROI, payback, residual risk), and produces an executive briefing with a treatment plan.

This is a local calculation exercise. No virtual machines, live systems or scanning were used. All inputs come from the lab brief.

---

## Headline results

| Finding | SLE | ARO | ALE / year | Priority |
|---|---|---|---|---|
| F2 Database injection | $125,000,000 | 25% | $31,250,000 | Critical |
| F3 Session hijacking | $7,500,000 | 40% | $3,000,000 | High |
| F1 API authentication bypass | $5,000,000 | 15% | $750,000 | Medium |
| **Total** | | | **$35,000,000** | |

| Control | Initial | Maint./yr | Risk reduction/yr | Year-1 ROI | Payback |
|---|---|---|---|---|---|
| Web Application Firewall | $150,000 | $25,000 | $23,437,500 | 13,292.9% | ~2 days |
| MFA Enhancement | $200,000 | $30,000 | $2,850,000 | 1,139.1% | ~26 days |
| API Security Gateway | $350,000 | $50,000 | $675,000 | 68.8% | ~204 days |

**Recommendation:** implement all three, in the order WAF (live by day 30), then MFA and API Gateway (live by day 90).
Total ALE falls from **$35.0M to $8.04M a year (77%)** for **$700,000** up front plus **$105,000** a year.

The API Gateway is the marginal control and is the most sensitive to the assumptions (see the `Sensitivity` sheet).

---

## Method

```
SLE = driver A × driver B                (e.g. records × cost per record)
ARO = supplied exploit probability       (read as incidents per year)
ALE = SLE × ARO

Risk reduction     = baseline ALE × control effectiveness
Residual ALE       = baseline ALE − risk reduction
Year-1 cost        = initial + annual maintenance
ROI                = (risk reduction − cost) ÷ cost
Payback (years)    = initial ÷ (risk reduction − annual maintenance)
```

Control mapping: API Gateway → Finding 1, WAF → Finding 2, MFA → Finding 3.

---

## Repository contents

| File | What it is |
|---|---|
| `GRC_Lab3b_Risk_Quantification_Workbook.xlsx` | **Main deliverable.** Editable workbook with live formulas and native Excel charts |
| `GRC_Lab3b_Risk_Quantification_Report.docx.pdf` | Report with step-by-step worked calculations |
| `GRC_Lab3b_Executive_Dashboard.pdf` | One-page executive dashboard (static copy) |
| `README.md` | This file |

### Workbook map

| Sheet | Purpose |
|---|---|
| `Cover` | Name, registration number, colour code, formula summary |
| `Dashboard` | One-page executive dashboard |
| `Inputs` | Every supplied figure and every assumption, in one place |
| `Assumptions` | Assumption register (what, why, effect if wrong) |
| `Risk_Calc` | SLE, ARO, ALE with the working written out |
| `Risk_Matrix` | Priority levels and probability-impact heat map |
| `Control_Analysis` | 16-step cost-benefit table, ROI, payback, recommendation |
| `Timeline` | Month-by-month risk reduction (line graph data) |
| `Gantt` | Treatment plan: 30 days, 90 days, one year |
| `Budget_Optimiser` | Best control combination for each capital budget |
| `Sensitivity` | Break-even ALE and what-if tables |
| `Chart_Data` / `Charts` | Chart source tables and native charts |

**Colour code:** blue text = supplied figure; blue on yellow = my assumption; black = same-sheet formula; green = cross-sheet link.

---

## How to use

1. Open the workbook in Excel (or LibreOffice).
2. Change any blue or yellow cell on `Inputs` (for example a probability or a control's effectiveness).
3. Everything recalculates: ALE, ROI, recommendations, heat map, charts and dashboard.
4. In the Word report, the workbook link is at the first page

The workbook has about 660 formulas and no formula errors. Key figures were checked against an independent calculation.

---

## Supplied figures vs assumptions

Figures **supplied by the brief**: exploit probabilities, loss drivers (maximum loss, records, cost per record, accounts, loss per account), and control cost, effectiveness and maintenance.

**My assumptions** (full register on the `Assumptions` sheet, with the effect if each is wrong):

- ARO equals the exploit probability, read as per year.
- The full at-risk population is compromised in one event.
- Each control reduces only its mapped finding; effectiveness reduces ALE proportionally.
- No discounting; 3-year ROI horizon.
- Decision hurdle: payback ≤ 1 year and 3-year ROI ≥ 100%.
- Risk bands (Critical ≥ $10M ALE, High ≥ $1M, Medium ≥ $100k; probability High ≥ 30%).
- Gantt dates, durations and owners.

The risk bands and the decision hurdle are **illustrative judgments, not taken from a standard**. If you have an organisational risk appetite, replace them on the `Inputs` sheet.

---

## Background reading

- Gordon and Loeb model of optimal security investment (a general ceiling of about 37% of expected loss; it depends on the model assumptions): <https://en.wikipedia.org/wiki/Gordon%E2%80%93Loeb_model>
- ROSI formula as used here, `(ALE × mitigation − cost) ÷ cost`, as defined in Section II.B of: <https://arxiv.org/pdf/2304.07909>


---

## Limitations

- ALE is a long-run average, not a prediction for any one year.
- Results depend on the supplied probabilities and loss figures; ROI figures are very large because the Finding 2 exposure is very large.
- Controls are assumed to work independently, at constant effectiveness, from their go-live date.

- ## AI Declaration
- ClaudeAI and Chatgpt helped me with some of the calculations and with grammar checks, structural arrangements.
