# Day 30: Final Practical SOC Investigation

## Task
Perform an end-to-end investigation using a safe, self-generated lab scenario: collect telemetry, identify the alert or pattern, triage it, investigate logs, collect evidence, map ATT&CK behavior, and produce a closure or escalation recommendation.

**Tools:** Windows Security logs, PowerShell, MITRE ATT&CK, Wazuh (rule logic only)

> **Status:** Fully executed on my Windows lab VM on 2 October 2026. Detection was done from the Windows Security log, because no Wazuh manager is deployed in my lab yet (see Days 17 to 20). The Wazuh section is based on published Wazuh material and was not run live.

## Scenario
A backdoor-style change that real attackers use for persistence: I created a local account named `svc_update` and added it to the Administrators group on the lab VM. The account was removed at the end. This is a different tactic from the brute-force case in Days 25 to 29, to show the same method works on new activity.

## Workflow

| Phase | What I did | Result |
|---|---|---|
| 1. Preparation | Created case folder INC-20261002-001; confirmed account-management auditing was on with `auditpol` before generating anything | Telemetry confirmed |
| 2. Simulation | Created the account and added it to Administrators, bracketed by timestamps | Activity window known |
| 3. Detection and triage | Queried the Security log for account events; rated initial severity | **High** |
| 4. Investigation | Exported the logs and analyzed the export: what changed, who, from where, was the account used | One actor, one console session, account never used |
| 5. Evidence | Captured account and system state before touching anything | 11 evidence files |
| 6. ATT&CK mapping | Mapped the behavior | T1136.001, T1098.007 |
| 7. Containment | Disabled, then deleted the account; verified removal in the log | Contained, closed |

## Key results

| Item | Finding |
|---|---|
| Account created | 12:01:54 IST (Event 4720) |
| Added to Administrators | 12:02:50 (Event 4732), 56 seconds later |
| Actor | CyberAmy, one interactive console logon at 11:34:36 (127.0.0.1, elevated) |
| Used? | No. `svc_update` never logged on (Last logon: Never) |
| Disabled / deleted | 12:59:41 / 12:59:55 (58 minutes after creation) |
| MITRE ATT&CK | T1136.001 Create Account: Local Account; T1098.007 Additional Local or Domain Groups (Persistence, Privilege Escalation) |
| Final severity | Low, benign true positive; closed, not escalated |

## Key findings
- Triage asks "could this be bad?" and investigation answers it. I rated it High at triage because a new administrator account within a minute of creation is a known backdoor pattern, then lowered it to Low only after the evidence showed one known actor, a local console session, and an unused account.
- My first query filtered on the event IDs I expected and missed an extra event (4728). Exporting every event in the window and then analyzing it found what the filter missed.
- **Visibility gap:** no process creation events (4688) were recorded, so the logs show that the account was created but not which command created it. This is a real finding about the endpoint.
- **Detection gap:** nothing alerted. The activity was found only because I looked, and Wazuh's coverage of additions to Administrators should be tested, not assumed.
- Evidence was captured before containment, because once an account is deleted its properties can no longer be queried.

## Top recommendations
1. Alert on new local accounts and additions to Administrators (deploy Wazuh and test the rules).
2. Enable process creation auditing, with command-line logging.
3. Limit day-to-day use of administrator accounts.
4. Review local accounts and administrators against a baseline regularly.

## Key takeaway
A complete investigation runs from confirming the logs exist, through triage, evidence preservation, ATT&CK mapping, and containment, to a closure decision that I can defend. The most useful output was not the verdict but the two gaps it exposed: no alert fired, and the command that made the change was not recorded.

## Evidence
📄 [Full report with screenshots (39 pages)](./Day-30-Final-Practical-SOC-Investigation.pdf)
