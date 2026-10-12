# Day 25: Brute-Force Style Detection Simulation

## Task
Generate repeated safe authentication failures against my own lab account at a controlled pace. Investigate the resulting telemetry, count the attempts, and determine whether the pattern should be escalated.

**Tools:** Windows Event Viewer, Wazuh (rule logic only)

> **Status:** The Windows side was executed for real: eight failed logons and one successful logon on my lab VM, all located in Event Viewer. The Wazuh side is theoretical. No manager is deployed yet, so I applied Wazuh's correlation rule 60204 to my real timestamps by hand. The result will be confirmed against a live alert once a manager is available.

## What I did
- Checked the account lockout policy first with `net accounts` (threshold 10, lockout duration 10 minutes, observation window 10 minutes), so the test could not lock the account.
- Made **8 deliberate failed logons** against my lab account at the Windows login screen, about 23 to 45 seconds apart, then one correct logon to confirm the account was not locked.
- Filtered the Security log to Event ID 4625, excluded unrelated earlier events by timestamp, and counted the 8 test events.
- Located the successful logon (Event ID 4624) and confirmed the account from the **New Logon** section of the event.
- Applied Wazuh rule 60204 to the real timestamps and made an escalation decision.

## Results

| Measure | Value |
|---|---|
| Failed attempts (Event 4625, Logon Type 2) | 8 |
| First to last failure | 13:07:33 to 13:11:19 (226 seconds) |
| Interval between attempts | 23 to 45 seconds |
| Successful logon (Event 4624) | 13:12:10 (51 seconds after the last failure) |
| Wazuh rule 60204 window | 240 seconds, so 226 seconds is inside it |

## Key findings
- The 8 failures fit inside 60204's 240-second window with 14 seconds to spare, so this pattern meets the rule's criteria on paper. Because my count equals the threshold exactly, the live alert still needs to be confirmed.
- A Windows 4624 event has two account fields: **Subject** (who performed the logon operation, often the machine account or SYSTEM) and **New Logon** (who actually logged in). Reading only Subject gives the wrong answer.
- A rule firing and an attack happening are separate questions. The rule's threshold is met, but the source was my own lab console, I was the account owner, and a correct logon followed. That makes it a benign true positive.

## Key takeaway
Good triage separates "did the detection fire correctly" from "is this actually malicious". The alert is valid, the investigation explains it, and the ticket is closed with the reasoning written down.

## Evidence
📄 [Full report with screenshots](./Day-25-Brute-Force-Style-Detection-Simulation.pdf)
