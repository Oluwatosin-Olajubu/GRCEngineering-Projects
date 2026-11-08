# Phishing Campaign Simulation Lab (GoPhish)

A controlled phishing-awareness simulation completed as a GRC practical. It covers how to configure a campaign, record the events, calculate metrics with clear denominators, and turn the results into a risk assessment and management recommendations.

> **Safety notice:** This lab used only mailboxes I control, fictional identities and a fictional training login page. No real users were targeted and no real credentials were collected. Do not reuse this material against people or systems you do not own or have written authorisation to test.

## Author

| | |
|---|---|
| **Name** | Oluwatosin Olajubu |

## Scenario

As a GRC analyst, run a simulated VPN password-reset phishing campaign in a local laboratory, observe the campaign events, calculate risk metrics and prepare recommendations for management.

## Laboratory setup

| Component | Purpose |
|---|---|
| Kali Linux (virtual machine) | Hosts GoPhish and the landing-page server |
| GoPhish v0.12.1 | Campaign management, tracking and results dashboard |
| Mailtrap | Email sandbox / SMTP capture, so no message leaves the lab |
| Firefox | Access to the GoPhish admin interface, Mailtrap and the landing page |

Everything ran on a local computer. GoPhish's admin interface was accessed locally on port 3333.

## What was done

1. **Installed and started GoPhish** on Kali Linux.
2. **Created a sending profile** pointing at the Mailtrap SMTP sandbox, then verified delivery with a test email.
3. **Built a fictional landing page** ("Fake VPN Portal", SecureConnect VPN theme) that records a dummy form-submission event.
4. **Wrote the email template** ("VPN Password Reset Required").
5. **Created a target group** ("Lab Test Group") of test addresses I control.
6. **Launched the campaign** ("Q3 GRC Phishing Simulation") and checked the message in Mailtrap.
7. **Reviewed the results** on the GoPhish dashboard and campaign list.
8. **Assessed the risk** and wrote an executive summary with prioritised recommendations.

## Results

All rates use **5 emails sent (campaign entries)** as the denominator.

| Metric | Count | Rate |
|---|---|---|
| Emails sent | 5 | 5/5 = 100% |
| Emails opened | 3 | 3/5 = 60% |
| Links clicked | 3 | 3/5 = 60% |
| Dummy data submitted | 0 | 0/5 = 0% |
| Emails reported | 0 | 0/5 = 0% |

## Limitations

- The sample is tiny and all activity came from a single tester, so these figures validate the simulation workflow. They do not measure the behaviour of real users.
- The tracking link was opened by me from the Mailtrap view.
- Open tracking is unreliable (image blocking, mail previews).
- The five campaign entries may not be five different people.
- The results do not show that an organisation's training or email controls are effective or ineffective.

## Recommendations (summary)

1. Provide an easy phishing-reporting mechanism and track the reporting rate.
2. Use MFA, preferably phishing-resistant, for privileged and high-risk accounts.
3. Continue proportionate, realistic awareness training.
4. Maintain SPF, DKIM, DMARC, and URL and attachment protection.
5. Repeat simulations periodically with an authorised, broader population and compare over time.

## Repository contents

```
.
├── README.md
├── report/
│   └── GoPhish_Phishing_Simulation_Lab_Report_....docx.pdf
└── brief/             # original laboratory brief
```

Adjust the folder names to match what you actually commit.

## Tools

- [GoPhish](https://getgophish.com/)
- [Mailtrap](https://mailtrap.io/)
- [Kali Linux](https://www.kali.org/)
