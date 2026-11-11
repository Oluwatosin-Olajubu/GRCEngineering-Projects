# GRC103 Lab 2: Ransomware Risk Treatment

Case analysis and management decisions for **Northbridge Community Health**, a fictional three-clinic healthcare provider with a $100,000 first-year cyber budget. The exercise draws on the documented 2021 ransomware attack on Ireland's Health Service Executive (HSE).

> **Course:** GRC103, Risk Assessment and Management Techniques (ICDFA)
> **Author:** Oluwatosin Olajubu | **Registration no.:** C11/26/CGRCE/17602
> **Official source:** Health Service Executive (2025) *Cyber-attack and HSE response*. Available at: https://www2.hse.ie/services/cyber-attack/what-happened/ (page last reviewed 1 December 2025; accessed 09-11 November 2026).

Real HSE incident facts are kept separate from the fictional Northbridge assumptions throughout. Northbridge, its budget, option prices, the insurance terms and the mid-exercise change are classroom inputs, not real data.

## Repository contents

| File | Description |
|---|---|
| `GRC103_Lab2_Ransomware_Risk_Treatment_Report.pdf` | Submission report (4 pages) |
| `GRC103_Lab2_Risk_Register_Budget_Insurance.xlsx` | Workbook with live formulas, dropdowns and colour coding |

## The five tasks

1. **Risk register:** four prioritised business risks with asset, threat, weakness, impact, owner, likelihood and impact scores, and a written explanation for each rating.
2. **Initial treatment portfolio:** options chosen within the $100,000 budget, covering avoidance, mitigation, transfer and a time-limited acceptance.
3. **Insurance calculations:** insurer payment, retained loss and first-year outlay under illustrative policy terms.
4. **Revised decision:** the portfolio rebuilt after the restore test fails, paper bookings are found to last only four hours, and option B rises to $35,000.
5. **30-day plan and board memo:** actions with owners, costs, dates and effectiveness tests, proposed residual risk, and a board memo of 300 words or fewer.

## Key results

### Risks (initial scores, likelihood × impact)

| ID | Risk | Score | Rating | Owner |
|---|---|---|---|---|
| R1 | Service disruption | 4 × 5 = 20 | Critical | Chief Operating Officer |
| R2 | Sensitive-data loss | 3 × 5 = 15 | High | Data Protection Officer |
| R3 | Unnecessary supplier exposure | 4 × 3 = 12 | High | Head of IT |
| R4 | Delayed detection | 4 × 4 = 16 | High | Head of IT |

Scale: Low 1-4, Medium 5-9, High 10-16, Critical 17-25. Critical risks cannot be accepted. High risks go to the board.

### Treatment options

| ID | Option | Strategy | Cost |
|---|---|---|---|
| A | Retire the unused supplier workflow | Avoidance | $5,000 |
| B | MFA rollout and access review | Mitigation | $25,000 (later $35,000) |
| C | Protected backups, restoration tests, recovery plan | Mitigation | $30,000 |
| D | Managed detection and incident escalation | Mitigation | $35,000 |
| E | Targeted awareness training and reporting process | Mitigation | $10,000 |
| F | Network segmentation and legacy dependency review | Mitigation | $40,000 |
| G | Illustrative cyber insurance | Transfer | $20,000 |

### Portfolios

| | Selected | Committed | Remaining or reserve |
|---|---|---|---|
| Initial | A, B, C, E, G | $90,000 | $10,000 remaining |
| Revised | A, B ($35,000), C, E | $80,000 | $20,000 ring-fenced recovery reserve |

In the revised portfolio, G (insurance), D and F are deferred. Insurance pays money rather than restoring clinic bookings, so its $20,000 became a recovery reserve.

### Insurance maths (illustrative terms)

Deductible $25,000, cap $250,000, premium $20,000.

```
insurer payment = MIN(MAX(loss - deductible, 0), cap)
retained loss   = loss - insurer payment
total outlay    = retained loss + premium
```

| Covered loss | Insurer pays | Retained loss | Total first-year outlay |
|---|---|---|---|
| $180,000 | $155,000 | $25,000 | $45,000 |
| $400,000 | $250,000 | $150,000 | $170,000 |

If a claim is excluded, Northbridge bears the entire loss and the premium is also spent.

### Proposed residual risk (controls count only after their test passes)

| ID | Initial | Residual |
|---|---|---|
| R1 | 20 Critical | 15 High |
| R2 | 15 High | 10 High |
| R3 | 12 High | 3 Low |
| R4 | 16 High | 12 High |

R4 is accepted for 90 days, to 14 February 2027, with the Head of IT as owner and the Board Chair as simulated approver. R1 is never accepted. Three decisions go to the board: running without insurance while recovery is unproven, the 90-day acceptance, and R1 staying High after treatment.

## Workbook guide

| Sheet | Contents |
|---|---|
| Cover | Details, citation, real-versus-fictional note, sheet guide, colour legend |
| Risk Register | Risks, 1-5 dropdowns, score and rating formulas, 5×5 heat map, evidence table |
| Budget | Options, initial and revised selections, totals and budget checks |
| Insurance | Covered, excluded and uninsured scenarios |
| Residual Risk | Initial and residual scores, credited and uncredited controls |
| 30-Day Plan | Actions, owners, costs, target dates, effectiveness tests, cross-checks |
| Decisions | Initial and revised decision records, acceptance record |

**Colour coding:** Critical is deep red with white text, High is light red, Medium is yellow and Low is green. Single scores 1 to 5 run from green to deep red. Selected flags are green (1) or red (0), and YES/NO checks are green or red.

**Dropdowns:** likelihood and impact (1-5), selected (1 or 0), strategy (Avoidance, Mitigation, Transfer, Acceptance), and plan type (Committed or Reserve).

**Editing:**
- Change the band limits in cells `C13:C15` of the Risk Register sheet. Ratings, colours and the heat map follow.
- Change colours via Home > Conditional Formatting > Manage Rules. A plain cell fill will not override the rules.
- In the workbook, blue text is typed input, black is a formula, green is a link from another sheet, and yellow marks cells to fill in.

## Assumptions and limits

- Everything about Northbridge, the prices, the insurance terms and the later change is fictional.
- Owners are role titles assigned for the exercise. The Board Chair is a simulated approver.
- The 8-hour paper-booking drill in the plan is a chosen target. The brief only states that paper lasts four hours.
- Residual scores are proposals. Restoration (option C) has already failed once, so it earns no credit until a retest passes.
- Dropping insurance in the revised portfolio raises retained exposure ($155,000 more on a $180,000 loss, $250,000 more on a $400,000 loss). Keeping insurance and dropping the reserve is the main alternative.

## Academic note

This repository holds coursework. If you are a student on the same course, use it only as a reference and follow your institution's academic-integrity rules.
