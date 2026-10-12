# SOC Internship Labs: TriosCyber

Daily hands-on labs from a month-long SOC analyst internship at TriosCyber. Each day is a separate task: set up, scan, capture, hunt through logs, triage, and write it up as a report. A final project follows Day 30.

Every folder holds a short summary of the task and what I found, plus the full report as a PDF with screenshots.

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

More days are added as they are completed.

## Lab environment
- VMware Workstation Pro on a Windows host
- Kali Linux, Windows 11 and Metasploitable 2 virtual machines on one isolated NAT network
- Tools so far: Nmap, Wireshark, Windows Event Viewer, journalctl, grep, awk, sort

## About
I'm Amrita Pathak, a technical content writer building hands-on SOC and GRC experience. More about me is on my [GitHub profile](https://github.com/amrita-pathak-cyber).

All labs run in an isolated personal lab, and any synthetic data is labeled as such in the report.

This repository is shared for portfolio and demonstration purposes. All content is original work and not licensed for reuse or redistribution.
