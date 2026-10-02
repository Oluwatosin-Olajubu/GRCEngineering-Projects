# Linux Security Monitoring and Auditing: From Technical Evidence to Governance Assurance

**GRC102 – Information Security Governance · Module 4 – Monitoring and Auditing Security Controls**
**Week 4 Practical Laboratory**

A lab report written from the point of view of a **Security Control Assurance Analyst**. It documents hands-on security monitoring on an instructor-authorised Kali Linux VM and, more importantly, turns the raw technical output into governance findings: who owns each issue, how serious it is, what a SIEM would do with it, and what evidence would prove it is fixed.

| | |
|---|---|
| **Author** | Oluwatosin Olajubu |
| **Environment** | Kali Linux (rolling) |
| **Evidence window** | Mostly 28 Sep – 1 Oct 2026, plus older journal history already on the VM |
| **Report date** | 30 September 2026 |
| **Format** | PDF document (`.docx.Pdf`) with 28 annotated screenshots |

---

## What this project covers

The lab is split into three modules, each followed by a governance interpretation of what was found.

### Module 1 – auditd
- Found that `auditd` was **not installed by default**, then installed, enabled, and verified it (`systemctl status auditd` shows `active (running)`, Figure 2a).
- Wrote five custom rules in `/etc/audit/rules.d/custom.rules`:

  ```
  -w /etc/passwd -p rwxa -k passwd_changes
  -w /etc/shadow -p rwxa -k shadow_changes
  -a always,exit -F arch=b64 -S execve -k program_execution
  -a always,exit -F arch=b32 -S execve -k program_execution
  -w /var/log/auth.log -p wa -k auth_failures
  ```
- Generated a harmless test event (opened `/etc/passwd` in `nano` without saving) and traced it with `ausearch` and `aureport`.

### Module 2 – Log analysis
- Used `journalctl` by unit, time window, and priority, and live-followed the journal.
- Used `grep` to look for failed passwords, `sudo` activity, errors, and warnings.

### Module 3 – Lynis
- Ran a full `lynis audit system` scan (Lynis 3.1.6): hardening index **61/100** across 272 tests, with 1 warning and 48 suggestions.
- Fixed one low-risk suggestion (`apt-listchanges`, DEB-0811) and re-ran the scan to prove it disappeared. This is the same close-the-loop approach used for the findings below.

---

## Key findings

| ID | Finding | Priority | Owner |
|---|---|---|---|
| **W4-F01** | The `auth_failures` audit rule has never fired on a real failed login. Every record under that key is the rule being created, so the control is untested. | High | System/Linux Administrator |
| **W4-F02** | No firewall, intrusion detection, or malware scanner installed (confirmed by Lynis). | High | System/Linux Administrator |
| **W4-F03** | Multiple kernel/network settings differ from Lynis's hardened baseline, and no GRUB password is set. | Moderate | System/Linux Administrator |
| **W4-F04** | `systemd-sslh-generator` crashed with a segfault on at least two separate boots, unnoticed until the error-priority logs were reviewed. | Moderate | System/Linux Administrator |

Evidence gaps found in the first draft have been closed. For example, the direct `active (running)` confirmation for auditd was captured and added as Figure 2a. The remediation plan tracks each item with an owner, target, and a "how I'd know it's fixed" retest.

---

## Report structure

| Section | Content |
|---|---|
| 1 | Executive Summary |
| 2 | Scope and Authorisation |
| 3 | Methodology |
| 4 | Module 1 Findings – Setting Up and Using auditd |
| 5 | Module 2 Findings – Looking Through the Logs |
| 6 | Module 3 Findings – Running Lynis |
| 7 | Control-Monitoring Table |
| 8 | How This Would Feed a SIEM |
| 9 | Answering the Governance Questions |
| 10 | Remediation and Retest Plan |
| 11 | Conclusion |
| Appendix A | Finding Records (W4-F01 to W4-F04) |
| Appendix B | Screenshot Index (Figures 1–27, plus 2a) |

---

## Repository layout

```
.
├── README.md
└── GRC102_W4_Lab_OluwatosinOlajubu_RegNo.docx.pdf   # the full lab report
```
---

## Tools used

`auditd` · `auditctl` · `ausearch` · `aureport` · `journalctl` · `grep` · `Lynis 3.1.6` · `nano` · `apt`

---

## Scope, ethics, and redaction

- All work was done **only on the single VM assigned for this lab**. No other machine, network, or account was scanned or accessed.
- No real authentication attack was attempted, and `/etc/passwd` and `/etc/shadow` were not modified.
- Screenshots were reviewed before inclusion. The interactive `sudo` password prompt line was blacked out as a precaution (Figures 1, 9, and 25), and the full contents of `/etc/passwd` were deliberately left out.

---

## Academic integrity

This repository is a record of my own coursework, published for portfolio purposes. If you are a student on the same course, please use it for reference only and do your own lab work and analysis.

---

## Contact

Oluwatosin Olajubu · [GitHub profile](https://github.com/your-username)
