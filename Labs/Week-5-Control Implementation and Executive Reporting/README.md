# Security Governance Lab: Equifax Breach Simulation

**Course:** GRC102, Information Security Governance
**Lab dates:** 5 to 8 October 2026
**Environment:** Kali Linux VM with Docker
**Author:** Oluwatosin Olajubu

A hands-on lab that recreates the conditions behind the 2017 Equifax breach (an unpatched Apache Struts flaw, CVE-2017-5638), analyses the governance failures, adds controls and builds board-level reporting. All customer data is made up. The attack and patching are simulated.

## Environment

Three containers on one Docker network (`secgov_network`):

| Container | Role | Address | Image |
| --- | --- | --- | --- |
| s2-045-struts2-1 | Vulnerable web server | 172.19.0.2 | vulhub/struts2:2.3.30 |
| monitoring_server | Monitoring | 172.19.0.3 | ubuntu:20.04 |
| database_server | Customer records | 172.19.0.4 | mysql:5.7 |

Notes:
- The lab sheet names the web container `web_server` and the image `vulhub/struts2:s2-045`. My running container uses the vulhub `s2-045` folder with image `2.3.30`, so scripts use that name and address.
- From Kali, the containers are reliably reachable only via the published port (`localhost:8080`).

## Quick start

```bash
cd security_governance_lab
docker compose up -d database monitoring        # database + monitoring
# web server: start from the vulhub s2-045 folder
python3 vulnerability_scanner.py                # scan
python3 simulate_attack.py                      # simulated attack
python3 analyze_governance_failures.py          # failure analysis
python3 implement_governance_controls.py --all  # apply controls
python3 governance_metrics.py                   # charts, dashboard, board report
xdg-open executive_dashboard.html
```

Check `docker ps` before every test. Containers stopped during my lab and distorted some results.

## Results by part

| Part | What was done | Outcome |
| --- | --- | --- |
| 1. Environment | Docker setup, 3 containers, scanner, patch tool, governance tracker | Scanner found: Struts (Critical, potentially vulnerable), MySQL default password (High), no network segmentation (Medium) |
| 2. Failure analysis | Simulated attack, analysis, Equifax comparison | 6 governance failures: patch management, incident detection, policy implementation, vulnerability management, metrics, access control |
| 3. Controls | Script adding 5 control groups, verification, manual DB password test | Controls recorded. DB password fix proved by direct tests. Struts and network fixes not proved |
| 4. Metrics | 5 trend charts, executive dashboard, board report, metrics framework, user guide | All files produced, but figures are sample data |

## Evidence strength for controls

| Control | Evidence | Strength |
| --- | --- | --- |
| Database password (CTL-006) | Default rejected, new password accepted | Strong |
| Struts patch (CTL-004) | Simulated status only; final scan could not connect | Weak |
| Network segmentation (CTL-005) | Simulated status only; scanner still flags it | Weak |
| Security monitoring (CTL-007) | Script and schedule created, never shown running | Partial |
| Governance oversight (CTL-008, POL-004) | Document and policy exist | Partial |

## Problems fixed along the way

- **Docker/Compose missing:** installed Docker engine and Compose v2.
- **Scanner could not resolve container name:** switched to container IP.
- **Scanner decoding error on MySQL binary reply:** read response as raw bytes.
- **Attack script could not reach 172.19.0.2:** pointed it at `localhost:8080`.
- **Analysis script expected `vulnerabilities`:** changed to read `results`.
- **Patch tool overwrote its own status file:** documented; fix is to update one key at a time.
- **Scanner falsely reported DB password as safe** (likely container was stopped): tested manually, changed root password with `ALTER USER`, confirmed default is rejected.

## Limitations

- Struts flaw is "potentially vulnerable" only; not confirmed by exploit.
- Attack and patching are simulations; the Struts container was never actually upgraded.
- DB fix covers only the local root account. The app_user password and the compose file still hold weak credentials.
- Part 4 figures are random sample data with the final month set to ideal, so the dashboard disagrees with the tracker.
- Tracker marks "lower is better" metrics Green incorrectly (remediation time, incidents).

## Recommendations

1. Upgrade Struts to 2.3.32+ (or 2.5.10.1+) and re-test.
2. Finish the DB fix: other admin accounts, app_user, compose file, secrets tool, re-scan.
3. Make the scanner distinguish "could not connect" from "login refused".
4. Make the patch tool keep a full status record.
5. Segment web and database networks.
6. Fix dashboard logic (higher/lower is better) and feed it real scan data.
7. Add metrics for weak/default passwords and direct web-to-DB paths.
8. Make monitoring real: scheduled, alerting a named owner, tested.
9. Set containers to auto-restart.
10. Assign a named owner to every policy, control and metric.

## Files

| File | Purpose |
| --- | --- |
| docker-compose.yml, database_init/init.sql | Containers and sample data |
| vulnerability_scanner.py | Three-check scanner |
| patch_management.py | Simulated patching |
| governance_tracker.py, initialize_governance.py | Policy/control/risk tracker |
| simulate_attack.py, attack_log.json | Simulated attack |
| analyze_governance_failures.py | Failure analysis |
| implement_governance_controls.py | Applies controls |
| governance_metrics.py | Charts, dashboard, board report |
| equifax_comparison.md, governance_controls_report.md | Reports |
| executive_dashboard.html, board_report.md, metrics_framework.md, dashboard_user_guide.md | Part 4 outputs |
| metrics_visualizations/ | Five trend charts |

The full write-up with all 128 screenshots is in the lab report.
