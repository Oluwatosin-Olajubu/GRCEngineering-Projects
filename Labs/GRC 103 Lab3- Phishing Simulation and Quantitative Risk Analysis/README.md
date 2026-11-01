# Phishing Risk Quantification: GlobalTech Inc. (Lab 3)

A GRC lab project that turns three phishing simulation campaigns into financial risk figures, checks the maths in the lab brief, and recommends security controls with a cost-benefit case.

**Author:** Oluwatosin Olajubu  | **Date:** 01 November 2026

> This is my GRC Engineering coursework. The scenario (GlobalTech Inc.), the campaign data and the breach-cost and control-effectiveness figures come from the lab brief and are scenario assumptions, not verified industry benchmarks.

## About this Assignnent

As the team lead, I was given results from three phishing simulations at a fictional 2,000-person company and asked to work out how exposed it is and what to spend on. I:

1. Calculated open, click-through, credential-submission and data-entry rates, plus conversion rates with the correct denominators.
2. Estimated annualised financial loss using an explicit annual-occurrence assumption.
3. Calculated 95% confidence intervals and tested whether the campaigns really differ.
4. Built a risk matrix and a control recommendation table with ROI and break-even.
5. Checked the brief's worked examples and logged every error and unsupported assumption I found.

## What's in this repository

| File | What it is |
| --- | --- |
| `Lab3_Calculation_Workbook.xlsx` | The calculation workbook. 9 sheets, 547 live formulas, no formula errors. |
| `Lab3_Report.docx.pdf` | The written report (executive briefing, method, results, errors found, limitations, references). Export it from the report doc and add it here. |
| `Lab 3A` | The lab instruction itself.
| `README.md` | This file. |

## Workbook guide

Open the workbook in Excel, LibreOffice Calc or an equivalent. Everything recalculates from the **Inputs** sheet.

| Sheet | Contents |
| --- | --- |
| `README` | Colour legend, sheet index, name and registration fields |
| `Inputs` | Supplied data, analyst assumptions, control parameters |
| `Phase1_Rates` | Basic rates and conversion metrics, with worked calculations shown |
| `Phase2_Exposure` | The brief's method with corrected arithmetic, and the event-based ALE model |
| `Stats_CI` | Wald and Wilson confidence intervals, finite-population check, two-proportion z-tests |
| `Control_Table` | Costs, effectiveness, benefit, ROI, payback, combined portfolios, break-even |
| `Risk_Matrix` | Likelihood x impact scores, ratings, financial estimates, priorities, actions |
| `Sensitivity` | Effects of changing occurrence rate, breach cost, breach probability and effectiveness |
| `Error_Log` | Each problem found in the brief, with the corrected figure |

**Colour code:** blue text is a hard-coded input, yellow fill is an assumption you can change, black is a formula, green is a link to another sheet, and red-tinted rows in `Error_Log` are errors or unsupported figures.

## Key results

| Result | Value |
| --- | --- |
| Pooled credential submission rate | 24.4% (105 of 430) |
| Highest credential rate | Campaign B (IT password reset), 30.0%, 95% CI 24.3% to 35.7% |
| Campaign B vs A / vs C | Significantly higher (p = 0.022 / p = 0.008) |
| Highest click rate | Campaign C (executives), 75.0% |
| Base-case annualised loss (event model) | about $3.8 million |
| Brief's headline figure | $652.5 million (3.8x annual payroll, not supportable) |
| Core controls (MFA, email gateway, training) | $342,163 including staff time; modelled loss reduction about 97% |
| Break-even | 0.113 of a breach per year ($490,000 / $4,350,000) |

## Main findings about the brief

- Campaign A's exposure should be $391.5M (brief: $156.6M) and Campaign C's $326.25M (brief: $104.4M) under the brief's own method.
- Treating each compromised account as an independent 25% breach is unsupported. I applied the 25% once per credential-theft event.
- The annual occurrence rate was never stated. I made it an explicit, adjustable assumption.
- The "new click-through rate" in the training example is actually the credential submission rate.
- Overall credential theft rate is 24.4%, not 22.1%. The 93.75% breach probability and the "industry average" figures have no source.
- Controls overlap, so effectiveness figures cannot be added (99.9% + 60% + 20% = 179.9%).

The full list with corrected values is on the `Error_Log` sheet.

## Key assumptions

- 1 successful real-world campaign per lure type per year (tested from 0.25 to 4)
- Impact multiplier of 1.5 for the executive campaign
- MFA planning effectiveness of 90% (supplied 99.9% kept as an upper bound)
- 20% effectiveness for the email gateway (implied by the brief)
- 0% credit for monitoring, analytics and the simulation programme
- Staff time costed at $40.87 per hour ($85,000 / 2,080)

## Limitations

- Breach cost, breach probability and control effectiveness are scenario assumptions, not benchmarks.
- The test groups were not random samples and each used a different lure, so extending rates to all 2,000 employees is indicative only.
- Controls are assumed to combine independently, which real controls may not.


## Sources

- IBM Security and Ponemon Institute, *Cost of a Data Breach Report* (2025, 2026)
- Verizon, *2025 Data Breach Investigations Report*
- Meyer et al. (2023), "How effective is multifactor authentication at deterring cyberattacks?", arXiv:2305.00945
- CISA (2022), *Implementing Phishing-Resistant MFA*
- Ho et al. (2025), "Understanding the efficacy of phishing training in practice", IEEE S&P
- NIST SP 800-30 Rev. 1, *Guide for Conducting Risk Assessments*
- Wilson (1927); Brown, Cai and DasGupta (2001) for confidence intervals

The report has the full reference list.

## Academic integrity

If you are on the same course, use this as a reference for method and structure, and complete your own analysis and write-up.
AI assisted in some of the calculations, structural arrangement, grammatical struture, tautology elimination and also step guides to navigating some of the challenges encountered. However, all of these inputs were independently verified 
