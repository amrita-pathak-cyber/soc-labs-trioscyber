# SOC Internship Labs: TriosCyber

Daily hands-on labs from a 30-day SOC analyst internship at TriosCyber. Each day is a separate task: set up, scan, capture, hunt through logs, triage, and write it up as a report. The series ends with a final end-to-end investigation on Day 30.

Every folder holds a short summary of the task and what I found, plus the full report as a PDF with screenshots. Where a task could not be run live (for example, Wazuh work, because I could not deploy a manager), the report says so plainly.

## Progress
| Day | Task | Skill shown |
|---|---|---|
| 1 | [Isolated Lab Setup](./day-01-lab-setup) | Virtual lab build, networking, connectivity testing |
| 2 | [Nmap Scanning and Service Discovery](./day-02-nmap-scanning) | Host and port discovery, service baseline, exposure rating |
| 3 | [Packet Filter and Protocol Identification](./day-03-packet-filter-protocol-identification) | Wireshark capture, display filters, DNS, HTTP and TLS |
| 4 | [TCP Connection Analysis](./day-04-tcp-connection-analysis) | TCP handshake and teardown, sequence numbers |
| 5 | [Endpoint Security Baseline Collection](./day-05-endpoint-security-baseline-collection) | Identity, process, network and service baseline on Linux |
| 6 | [Controlled Failed Login Generation](./day-06-controlled-failed-login-generation) | Windows Event IDs 4625 and 4624 |
| 7 | [SOC Threat Simulation and IOC Collection](./day-07-soc-threat-simulation-ioc-collection) | Indicators of compromise, hashes, server-side logs |
| 8 | [Basic Alert Triage Exercise](./day-08-basic-alert-triage-exercise) | Severity rating and disposition with justification |
| 9 | [Windows Log Hunt](./day-09-windows-log-hunt) | Security, System and Application logs, MITRE ATT&CK mapping |
| 10 | [Linux Authentication Log Hunt](./day-10-linux-authentication-log-hunt) | journalctl, grep, PAM and sudo behavior |
| 11 | [Log Filtering and Pattern Detection](./day-11-log-filtering-pattern-detection) | grep, awk and sort frequency analysis |
| 12 | [SIEM Data Exploration](./day-12-siem-data-exploration) | Searched, filtered, and counted events in SIEM data with SPL; separated duplicate events from real activity (synthetic data) |
| 13 | [Correlation Rule Design Exercise](./day-13-correlation-rule-design-exercise) | Designed correlation rules using thresholds, time windows, and conditions |
| 14 | [Web HTTP Log Inspection](./day-14-web-http-log-inspection) | Inspected web server access logs for request patterns and anomalies |
| 15 | [Alert Prioritization Lab](./day-15-alert-prioritization-lab) | Built a scoring-based alert prioritization sheet to rank alerts by risk |
| 16 | [Wazuh Lab Deployment Preparation](./day-16-wazuh-lab-deployment-preparation) | Planned the Wazuh lab: components, resources, and network setup |
| 17 | [Install Wazuh All-in-One Lab](./day-17-install-wazuh-all-in-one-lab) | Attempted all-in-one install; failed at the indexer start; documented as a theoretical report |
| 18 | [Wazuh Component Health Check](./day-18-wazuh-component-health-check) | Health-check procedure for Wazuh components; interim status report, not executed (no manager) |
| 19 | [Linux Wazuh Agent Deployment](./day-19-linux-wazuh-agent-deployment) | Agent enrollment procedure on Linux; walkthrough, not executed (no manager) |
| 20 | [Windows Wazuh Agent Deployment](./day-20-windows-wazuh-agent-deployment) | Agent enrollment procedure in CMD and PowerShell; not executed, no manager available |
| 21 | [Endpoint Event Collection Test](./day-21-endpoint-event-collection-test) | Generated and analyzed real 4625 and 7040 events; mapped to Wazuh rules (theoretical, no manager yet) |
| 22 | [Wazuh Dashboard Investigation](./day-22-wazuh-dashboard-investigation) | Planned agent, time, and rule filters plus Saved Search in Wazuh Discover; theoretical, no dashboard yet |
| 23 | [File Integrity Monitoring Lab](./day-23-file-integrity-monitoring-lab) | Canary-directory FIM design with syscheck config and expected rules 554 and 550; theoretical, no manager yet |
| 24 | [Wazuh Rule and Alert Analysis](./day-24-wazuh-rule-and-alert-analysis) | Three-alert triage across authentication, FIM, and SSH brute force; theoretical, based on Day 21 event plus default rules |
| 25 | [Brute-Force Style Detection Simulation](./day-25-brute-force-style-detection-simulation) | Generated and counted 8 real failed logons, applied Wazuh rule 60204 logic, and made a documented escalation decision |
| 26 | [Incident Evidence Collection](./day-26-incident-evidence-collection) | Built a sealed incident case folder with UTC-normalized log exports, chain of custody, and SHA-256 hashes |
| 27 | [MITRE ATT&CK Mapping Lab](./day-27-mitre-attck-mapping-lab) | Mapped real failed-logon evidence to T1110.001, rejected alternatives, and checked Wazuh's own ATT&CK tags |
| 28 | [Threat Intelligence / IOC Enrichment](./day-28-threat-intelligence-ioc-enrichment) | Enriched the EICAR hash with VirusTotal and MISP; recorded the context that changes analyst priority |
| 29 | [SOC Incident Report](./day-29-soc-incident-report) | Consolidated incident report for INC-20260929-001: severity, analysis, actions, escalation decision, recommendations |
| 30 | [Final Practical SOC Investigation](./day-30-final-practical-soc-investigation) | End-to-end investigation of a simulated backdoor admin account: triage, evidence, ATT&CK mapping, containment, closure |

## Lab environment
- VMware Workstation Pro on a Windows host
- Kali Linux, Windows 11 and Metasploitable 2 virtual machines on one isolated NAT network
- Ubuntu Server VM (used for the Wazuh installation attempt)

## Tools covered

**Network analysis**
- Nmap (scanning and service discovery)
- Wireshark (packet filtering, protocol identification, TCP connection analysis)

**Windows and Linux logs**
- Windows Event Viewer (Event IDs 4624, 4625, 4672, 4720, 4732, 7040 and others)
- PowerShell and Command Prompt (`wevtutil`, `Get-WinEvent`, `auditpol`, `Get-FileHash`, `net` commands)
- Linux command line (`journalctl`, `grep`, `awk`, `sort`) for authentication log analysis
- Python `http.server` logs for web request inspection

**SIEM and detection**
- Splunk (SPL searches, stats, correlation logic)
- Wazuh (architecture, ruleset, FIM, dashboard filtering, agent enrollment, VirusTotal integration). Studied and documented; the manager was not deployed in my lab, so Wazuh work is theoretical and is labeled that way.
- Correlation rule design and a scoring-based alert prioritization sheet (spreadsheet)

**Threat intelligence and frameworks**
- MITRE ATT&CK (T1110.001, T1136.001, T1098.007)
- VirusTotal and MISP warninglists (IOC enrichment)

**Investigation and reporting**
- Evidence collection with chain of custody and SHA-256 hashing
- Incident reports, severity rating, and escalation decisions

## About
I'm Amrita Pathak, a technical content writer building hands-on SOC and GRC experience. More about me is on my [GitHub profile](https://github.com/amrita-pathak-cyber).

All labs run in an isolated personal lab, and any synthetic data is labeled as such in the report.

This repository is shared for portfolio and demonstration purposes. All content is original work and not licensed for reuse or redistribution.
