# Day 29: SOC Incident Report

## Task
Create a concise SOC incident report based on one alert investigated during the month: summary, affected asset, timeline, evidence, severity, analysis, actions taken, and escalation/recommendation.

**Tools:** Windows event logs, Wazuh rule logic, MITRE ATT&CK, document

> **Status:** Real investigation, consolidated. The alert is the controlled failed-logon test from Day 25, evidence-collected and hash-sealed as case INC-20260929-001 in Day 26, and mapped to MITRE ATT&CK in Day 27. No Wazuh manager is deployed in my lab yet (see Days 17 to 20), so the detection is assessed against Wazuh rule 60204's definition and not a live alert. The report below is a one-page summary. The full 22-page version is linked at the bottom.

## Incident summary

| Field | Value |
|---|---|
| Incident ID | INC-20260929-001 |
| Title | Repeated failed interactive logons against local administrator account CyberAmy |
| Detection | 8 x Event ID 4625 in 226 seconds; meets the criteria of Wazuh rule 60204 (level 10) |
| MITRE ATT&CK | Credential Access (TA0006) / Brute Force: Password Guessing (T1110.001) |
| Initial severity | Medium |
| Final severity | Low (benign true positive) |
| Status | Closed, not escalated |

On 29 September 2026, between 13:07:33 and 13:11:19 IST, eight failed logons were recorded against CyberAmy on DESKTOP-AUHDFVM, followed by a successful logon at 13:12:10. Investigation showed the attempts came from the machine's own console and the successful logon was the account owner. The activity was an authorized self-test.

## Affected assets
- **Host:** DESKTOP-AUHDFVM, a Windows lab VM holding no business data.
- **Account:** CyberAmy, a local administrator. The account is the more important asset, because a successful unauthorized guess would give full control of the workstation.

## Timeline (IST, 29 September 2026)

| Time | Event |
|---|---|
| Before 13:07 | Lockout policy checked (threshold 10) |
| 13:07:33 | First failed logon (4625, Logon Type 2) |
| 13:08:08 to 13:10:34 | Failures 2 to 7, 23 to 40 seconds apart |
| 13:11:19 | Eighth failure: 8 in 226 seconds, inside rule 60204's 240-second window |
| 13:12:10 | Successful logon (two linked 4624 events, a UAC split token) |
| Same day | Failures counted in Event Viewer; log exported; case folder built, hashed, and sealed |

## Evidence
Sealed case folder with exported Security logs (incident window and full), a parsed CSV, eight endpoint-state captures, six screenshots, case notes, a chain of custody, and SHA-256 hashes of all 18 evidence files. Key fields: Logon Type 2, source IP 127.0.0.1, Status 0xC000006D, SubStatus 0xC000006A.

## Severity
- **Initial: Medium.** Wazuh rated the rule level 10, the target was an administrator account, and a successful logon followed the failures. The alert alone could not separate an attack from a user forgetting a password.
- **Final: Low (benign true positive).** The source was the local console, the successful logon was one ordinary logon by the account owner, and the activity was a controlled test. The rule fired correctly, but the activity was not malicious.

## Analysis
- **Pattern:** a repetitive password-guessing pattern against one account (T1110.001). Spraying and credential stuffing were ruled out.
- **Source:** Logon Type 2 from 127.0.0.1 means typed at the console, not over the network.
- **Success:** the two 4624 events at 13:12:10 list each other as linked logons, one elevated and one not. That is one administrator logon, not two.
- **Defenses:** the lockout policy (threshold 10) was in place. The endpoint enforces no minimum password length and keeps no password history.

## Actions taken
Triaged the failures, attributed the successful logon, exported and parsed the logs, captured endpoint state, wrote the case notes and chain of custody, hashed the evidence, mapped to ATT&CK, and closed the case. No containment was needed. In a genuine case with an unexplained successful logon, I would disable the account, reset its password, isolate the host, and review activity after 13:12:10.

## Escalation and recommendations
**Not escalated:** the source, account owner, and authorization were all confirmed. Escalation would be required if the success could not be attributed, the source were remote, other accounts or hosts showed the pattern, or later activity looked suspicious.

| # | Recommendation | Priority |
|---|---|---|
| 1 | Enforce a minimum password length | High |
| 2 | Enable password history | Medium |
| 3 | Deploy the Wazuh manager and enroll this endpoint | High |
| 4 | Consider MFA for administrator logons | Medium |
| 5 | Use a separate analyst account for evidence collection | Medium |
| 6 | Forward Security logs centrally or increase log size | Medium |
| 7 | Record planned security tests before they start | Low |
| 8 | Keep the detection threshold below the lockout threshold, and review both | Review |

## Key takeaway
An incident report should let a manager understand the case in one page and let an auditor trace every statement to evidence. Severity is assessed twice: cautiously on arrival, then revised only when the evidence earns it.

## Evidence
📄 [Full report (22 pages, with screenshots)](./Day-29-SOC-Incident-Report.pdf)
