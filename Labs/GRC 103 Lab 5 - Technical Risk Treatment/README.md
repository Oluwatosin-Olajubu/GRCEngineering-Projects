# GRC103 Module 3: Technical Risk Treatment Lab

| | |
|---|---|
| **Student** | [full name] |
| **Scenario** | GRC analyst and technical control implementer, Northbridge Services (fictional) |
| **Lab dates** | 5–7 November 2026 |

This package contains my evidence for the Module 3 technical risk treatment lab. It shows the original and treated configurations, the control tests, the logs, the backup and recovery test, and my treatment decisions. Only fictional data was used. No passwords, private keys or VM disk images are included.

## 1. Package contents

| Item | Purpose |
|---|---|
| `GRC103_Technical_Risk_Treatment_Lab_Report.docx` | Full report: management summary, baseline, risk register, treatments, tests, logs, recovery, decisions, costs, residual risk |
| `GRC103_Cost_and_Risk_Calculations.xlsx` | Risk scores, option costs and illustrative ALE, with formulas |
| `screenshots/figure-01.png … figure-57.png` | Lab screenshots, numbered as in Appendix A of the report |
| `baseline/` | Baseline output files from Kali |
| `avoidance/` | Post-avoidance output files from Kali |
| `preventive/` | Output files for archive and firewall tests, if any |
| `logs/` | Excerpts of the Nginx access log and UFW log |
| `configs/` | Nginx site file and the legacy service unit file |

## 2. Laboratory

| System | Detail |
|---|---|
| Hypervisor | VMware Workstation, host-only network 192.168.164.0/24 |
| Ubuntu Server | Ubuntu 26.04.1 LTS, 4 GB RAM, 2 vCPUs, 20 GB disk, 192.168.164.131 (ens33). Nginx 1.28.3, UFW 0.36.2, Python 3.14.4 |
| Kali Linux | Kali 2025.4. Host-only address 192.168.164.128 at the first connectivity check and 192.168.164.132 from the baseline onward (DHCP). Nmap 7.95 |
| Windows host | 192.168.164.1, used once for an SSH session |
| Temporary NAT | Used on Ubuntu only to install packages. The final tests ran on the host-only network |

Timestamps: Ubuntu and Nginx logs are UTC. Kali shell and Nmap times are EST (UTC−5).

## 3. Evidence map: practical step to evidence

| Step | What was done | Evidence | Report section |
|---|---|---|---|
| 1. Prepare servers | Installed Nginx, UFW, Python, curl; checked versions and connectivity | Figures 1–8 | 2, 3.1 |
| 2. Fictional content and legacy service | Created portal, archive and legacy content; created and fixed `grc-legacy.service` | Figures 9–19, `configs/grc-legacy.service` | 3.1 |
| 3. Lab web service | Wrote the Nginx site (listing on). Keyboard fault led to a one-off SSH session | Figures 20–27, `configs/grc-lab` | 3.1, 13 |
| 4. Baseline | Nmap, curl and `ss -lntp` from Kali and Ubuntu | Figures 28–34, `baseline/*.txt` | 3.2 |
| 5. Baseline risk register | R1–R4 scored and control gaps recorded | Report Section 4, `.xlsx` Risk register sheet | 4 |
| 6. Avoidance | Disabled the legacy service and verified | Figures 35–38, `avoidance/*.txt` | 5 |
| 7. Archive hardening | Moved archive to `/srv/grc-private`, autoindex off, deny rule | Figures 39–44, `configs/grc-lab` | 6.1 |
| 8. Firewall | UFW default deny, one allow rule for Kali on port 80 | Figures 45–47 | 6.2 |
| 9. Log correlation | Nginx access log and UFW BLOCK entries | Figures 48–49, `logs/` | 7 |
| 10. Backup and recovery | Backup, SHA-256, deletion, restore, verification | Figures 50–57 | 8 |
| 11. Treatment decisions | Avoidance, mitigation, transfer, acceptance records | Report Section 10 | 10 |
| 12. Costs and financial analysis | Option costs, combinations, illustrative ALE | Report Section 11, `.xlsx` Costs and ALE sheets | 11 |
| Residual risk, monitoring | Re-scored risks; monitoring plan | Report Sections 9 and 12 | 9, 12 |

## 4. Key results

| Test | Original | Treated |
|---|---|---|
| Port 8080 (legacy service) | open, HTTP 200 | closed after disabling (T06, T07); filtered once UFW was on (T15) |
| `/archive/` | HTTP 200 listing | HTTP 403 (T09) |
| `/archive/dummy-records.txt` | HTTP 200 | HTTP 403 (T10) |
| `/public/` (approved portal) | HTTP 200 | HTTP 200 throughout, including after UFW (T08, T14) |
| Firewall | none observed | default deny; allow 80/tcp from 192.168.164.132 only (T13) |
| Backup and restore | none | file restored, `sha256sum -c` OK, identical ETag and size (T21–T22) |

Residual risk (Section 12): R1 Low (4), R2 Medium (5), R3 Medium (5), R4 Medium (9).

## 5. Measured, simulated and assumed

- **Measured:** results produced by commands in my lab, shown in the screenshots and evidence files.
- **Simulated:** approvals and sign-offs, such as change record CR-001 and the risk acceptance. No real approvals took place.
- **Assumption:** all prices, the annual rate of occurrence and loss amounts. These are fictional inputs from the brief. Likelihood and impact scores are classroom judgements.

## 6. Known limitations

- Source-based blocking is shown for ports 22 and 8080 (Nmap filtered results and UFW BLOCK log entries for Kali and the Windows host). The portal on port 80 was tested from the approved source only.
- The rollback for CR-001 is a documented procedure. It was not executed, and the legacy service was left disabled.
- Recovery time (about 11 minutes) comes from manual steps and visible timestamps. It is not a production recovery time objective.
- SSH was left enabled on the server. UFW filters it, but disabling it is recommended hardening.
- The only backup is on the same host as the data.
- Kali's host-only address changed from .128 to .132 by DHCP. The firewall rule and all later tests use .132.

## 7. Secrets and redaction

No passwords, private keys or VM disk images are included. Screenshots show hostnames, internal lab addresses and a public SSH host-key fingerprint only.
