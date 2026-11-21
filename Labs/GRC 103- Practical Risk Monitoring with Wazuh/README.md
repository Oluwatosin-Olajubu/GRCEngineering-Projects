# Practical Risk Monitoring with Wazuh- File Integrity Monitoring

A hands-on Governance, Risk and Compliance (GRC) laboratory
demonstrating how **Wazuh File Integrity Monitoring (FIM)** can detect
the creation, modification and deletion of a sensitive file on a Windows
endpoint.

The project documents the deployment and validation of a Wazuh
monitoring environment, the investigation of FIM alerts, troubleshooting
encountered during setup, and the GRC implications of the results.

## Project overview

Organisations rely on the integrity and availability of financial and
other sensitive business information. Unauthorised changes to these
files can lead to fraud, inaccurate reporting, poor management
decisions, regulatory exposure and reputational damage.

In this lab, I configured a Wazuh manager on an Ubuntu virtual machine
and connected a Windows 11 endpoint running the Wazuh agent. I enabled
real-time FIM monitoring for a dedicated folder on the Windows endpoint
and carried out controlled file-activity tests.

### Objectives

-   Deploy and access a Wazuh manager.
-   Install and register a Windows endpoint agent.
-   Configure real-time FIM monitoring for a designated sensitive-files
    folder.
-   Generate and review file-creation, modification and deletion alerts.
-   Interpret alert severity and relevant event metadata.
-   Evaluate the control from a GRC and risk-monitoring perspective.
-   Identify complementary preventive and corrective controls.

## Lab architecture

  Component                    Configuration
  ---------------------------- -----------------------------------------
  SIEM / monitoring platform   Wazuh 4.12.0
  Manager operating system     Ubuntu 20.04.6 LTS
  Manager environment          VMware Workstation Pro virtual machine
  Endpoint operating system    Windows 11 Pro
  Endpoint monitoring          Wazuh Windows agent 4.12.0
  Monitoring capability        File Integrity Monitoring (FIM)
  Monitored directory          `C:\Users\<WindowsUser>\SensitiveFiles`
  Test file                    `Q4_Financial_Forecast.docx`

**Architecture:** Windows 11 endpoint → Wazuh agent → Wazuh manager on
Ubuntu → Wazuh dashboard and alert logs.

> The assignment brief referred to VirtualBox. I used VMware Workstation
> Pro to run the supplied Wazuh server appliance. The monitoring design
> and test objectives remained the same.

## Environment setup

### 1. Deploy the Wazuh manager

I imported the supplied Wazuh Server OVA into VMware Workstation Pro and
started the Ubuntu-based appliance. The appliance ran Wazuh 4.12.0.

During initial startup, the network interface was not obtaining an IP
address. I brought the interface up and requested a DHCP lease, then
verified the assigned address before opening the Wazuh dashboard over
HTTPS.

The dashboard presented a browser certificate warning because the lab
used a self-signed certificate. I proceeded to the dashboard in the
controlled lab environment.

### 2. Install and connect the Windows agent

I installed the Wazuh 4.12.0 Windows agent on my Windows 11 Pro laptop
and registered the endpoint with the manager.

I verified the endpoint's connection status from the manager and
confirmed that the Windows agent was active. Following a manager IP
address change after a restart, I updated the manager address in the
Windows agent configuration and restarted the agent service to restore
connectivity.

### 3. Configure the monitored folder

I created a dedicated folder under my Windows user profile:

``` powershell
New-Item -ItemType Directory -Path "$env:USERPROFILE\SensitiveFiles" -Force
```

I then edited the Windows agent's `ossec.conf` configuration and added
the folder to the existing `<syscheck>` section:

``` xml
<directories realtime="yes">C:\Users\<WindowsUser>\SensitiveFiles</directories>
```

Replace `<WindowsUser>` with the relevant Windows profile folder name
when applying this configuration on another machine. I retained the
existing `syscheck` configuration, saved the file and restarted the
Wazuh Agent service.

I checked the agent log to confirm that the folder was being monitored
with real-time options enabled.

## FIM validation and results

I used the required test filename, `Q4_Financial_Forecast.docx`, and
performed three controlled actions in the monitored folder.

  ---------------------------------------------------------------------------
  Test           Action             Wazuh rule Alert                 Severity
                                               description   
  -------------- ------------ ---------------- ------------- ----------------
  File creation  Created the               554 File added to                5
                 test file                     the system    

  File           Added text                550 Integrity                    7
  modification   to the file                   checksum      
                                               changed       

  File deletion  Deleted the               553 File deleted                 7
                 test file                                   
  ---------------------------------------------------------------------------

### 1. Creation detection

I created the test file using PowerShell:

``` powershell
New-Item -Path "$env:USERPROFILE\SensitiveFiles\Q4_Financial_Forecast.docx" -ItemType File
```

Wazuh generated a **Rule 554 -- File added to the system** alert for the
Windows endpoint.

### 2. Modification detection

I added content to the test file:

``` powershell
Set-Content "$env:USERPROFILE\SensitiveFiles\Q4_Financial_Forecast.docx" "Updated Q4 financial forecast - Version 2"
```

Wazuh generated a **Rule 550 -- Integrity checksum changed** alert. The
event included changes to file metadata and integrity hashes,
demonstrating that FIM detected a change to the monitored file.

### 3. Deletion detection

I deleted the test file:

``` powershell
Remove-Item "$env:USERPROFILE\SensitiveFiles\Q4_Financial_Forecast.docx"
```

Wazuh generated a **Rule 553 -- File deleted** alert.

The test file used a `.docx` extension to match the assignment filename.
It was created as a file and populated with text for the controlled
test; it was **not** a genuine Microsoft Word document.

### Results at a glance

-   File creation was detected.
-   File modification was detected, with integrity checksum changes
    recorded.
-   File deletion was detected.
-   The events were associated with the monitored Windows endpoint.
-   The tests demonstrated real-time FIM alerting in the configured lab
    environment.

All three actions were deliberately performed as test activities. They
do not represent evidence of an actual security incident or unauthorised
user activity.

## GRC and risk analysis

### Risk addressed

**Unauthorised modification or deletion of sensitive financial
information.**

If a financial forecast or other controlled business record is changed
without approval, the organisation may rely on inaccurate information,
make poor decisions, process incorrect payments or suffer financial
loss. Unauthorised deletion may also disrupt operations and make it
difficult to reconstruct records.

Potential consequences include:

-   Financial loss, fraud or inaccurate financial reporting.
-   Poor business decisions based on unreliable information.
-   Operational disruption and recovery costs.
-   Audit findings, regulatory scrutiny or legal disputes.
-   Reputational damage and reduced stakeholder confidence.

### Control assessment

FIM is primarily a **detective control**. It provides visibility of
changes to monitored files, but it does not, by itself, prevent an
authorised or unauthorised user from changing or deleting those files.

It should therefore be supported by preventive and corrective controls.

  -----------------------------------------------------------------------
  Control type                        Example controls
  ----------------------------------- -----------------------------------
  Preventive                          Role-based access control, least
                                      privilege, restrictive NTFS
                                      permissions, read-only access where
                                      appropriate, MFA and segregation of
                                      duties

  Detective                           Real-time FIM, centralised alert
                                      monitoring, alert review and
                                      escalation procedures

  Corrective                          Incident investigation, access
                                      revocation, credential reset,
                                      restoration from a verified backup
                                      and documented remediation
  -----------------------------------------------------------------------

### Suggested monitoring metrics

1.  **FIM alerts by event type and severity** -- monitor file creation,
    modification and deletion trends across endpoints and reporting
    periods.
2.  **Endpoint coverage and agent health** -- measure the proportion of
    in-scope endpoints with correctly configured FIM and an active
    agent.
3.  **Detection and response times** -- track the time between a file
    event, alert generation, triage and resolution.

The lab demonstrated real-time alert generation, but did not formally
measure detection latency or incident response time.

### Audit and compliance relevance

Wazuh alerts can provide useful evidence for internal control reviews
and audits. Depending on the system configuration and the organisation's
control framework, event records can help demonstrate monitoring of
sensitive files by providing timestamps, endpoint information, file
paths, event descriptions, rule identifiers, severity and integrity
metadata.

Wazuh can support control activities relevant to frameworks and
requirements such as SOX, PCI DSS and other information-security or
data-protection obligations. However, **deploying Wazuh does not
automatically make an organisation compliant**. Compliance depends on
the wider control environment, scope, configuration, operational
procedures, evidence retention and governance.

An absence of FIM alerts should not automatically be treated as proof
that no file changes occurred. The organisation should also verify agent
availability, monitoring configuration, log retention and endpoint
coverage for the period under review.

## Troubleshooting and lessons learned

The lab involved several practical configuration and connectivity
issues:

-   **Network interface unavailable on the manager:** The Ubuntu
    appliance initially did not have an active network address. I
    brought the interface up, requested a DHCP lease and verified the
    resulting address.
-   **Manager address changed after restart:** The manager received a
    different DHCP address. I updated the manager address in the Windows
    agent configuration and restarted the service to re-establish
    communication.
-   **Agent registration verification:** I checked the agent identity
    and connection status on both the Windows endpoint and the manager
    rather than relying only on the dashboard.
-   **Windows log access permissions:** Reading the protected agent log
    initially returned an access-denied message. I opened PowerShell
    with Administrator privileges and repeated the check.
-   **FIM configuration:** I added the target folder to the existing
    `syscheck` configuration, restarted the agent and checked the agent
    log to confirm that real-time monitoring had started.
-   **Virtual machine and storage considerations:** I used the supplied
    appliance rather than unnecessarily removing other lab virtual
    machines. This helped preserve the existing learning environment
    while completing the assignment.

These issues reinforced the importance of validating network
connectivity, agent status, configuration and log evidence before
interpreting missing alerts as a monitoring failure.

## Key takeaways

This practical exercise demonstrated how Wazuh FIM can strengthen
security monitoring by identifying changes to sensitive files and
generating centralised alerts for investigation.

From a GRC perspective, the value of the technology lies not only in
detecting events, but also in translating those events into actionable
risk information, assigning responsibility for review, measuring control
effectiveness and retaining reliable audit evidence.

**The main lesson:** file integrity monitoring is most effective when it
is part of a wider control framework that combines prevention,
detection, response, governance and continuous oversight.

## Disclaimer

This repository documents a controlled educational laboratory. It is
provided for learning and demonstration purposes only. The tests were
performed on systems under my control. The results should not be
interpreted as a penetration test, independent security assessment or
certification of compliance.

## References

-   [Wazuh
    documentation](https://documentation.wazuh.com/current/index.html)
-   [Wazuh -- File Integrity
    Monitoring](https://documentation.wazuh.com/current/user-manual/capabilities/file-integrity/index.html)
-   [Wazuh -- Windows agent
    installation](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html)
