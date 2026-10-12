# Day 24: Wazuh Rule and Alert Analysis

## Task
Select three different Wazuh alerts/events. For each, record the rule/alert level, description, agent, timestamp, and recommended first triage action.

**Tools:** Wazuh Dashboard

> **Status:** Theoretical walkthrough. No Wazuh manager or dashboard is deployed in my lab yet (see Days 17 to 20), so no alert was pulled from a live dashboard. Alert 1 is based on a real Windows event I generated in Day 21. Alerts 2 and 3 are illustrative examples built from Wazuh's default rules. All three will be replaced with observed alerts once a dashboard is available.

## What I did
- Chose three alerts from three different categories and severity levels, so the exercise shows triage range and not three near-duplicates.
- Recorded rule ID, level, description, agent, timestamp, and rule group for each.
- Wrote a specific first triage action for each one, based on what the rule means and what it can lead to.
- Compared the three side by side to decide which would be worked first.

## The three alerts

| Alert | Rule / Level | Category | Basis | First triage action |
|---|---|---|---|---|
| 1. Windows logon failure | 60122 / 5 | Authentication | Real Windows event from Day 21 | Confirm the attempt was self-initiated; check for a repeat pattern before escalating |
| 2. Hosts file integrity change | 550 / 7 | File integrity (FIM) | Illustrative | Review what changed, find the modifying process or user, escalate if domains are redirected |
| 3. SSH brute force | 5712 / 10 | Remote access | Illustrative | Identify the source IP, block it if external, then check for a successful login (rule 5715) from the same source |

## Key findings
- Alert level sets a starting priority, but context decides the action. The level 5 logon failure needed almost no action once its source was explained, while the level 10 SSH alert needed immediate containment.
- Rule 5712 is already a frequency-based correlation rule, so it represents a pattern. The single Windows 4625 only escalates under rule 60204 after 8 failures in 240 seconds.
- A brute-force alert followed by a successful login from the same source is a different incident class and should be escalated, not closed at first line.
- A change to a hosts file deserves more attention than its level suggests because of what the file controls, but legitimate software can also change it, so intent should not be assumed.

## Key takeaway
First-line triage is judgment, not field-filling. Reading the rule ID, level, agent, and time is the starting point. The analyst's value is in asking what this alert could lead to, what to check first, and when to escalate.

## Evidence
📄 [Full report (theoretical walkthrough)](./Day-24-Wazuh-Rule-and-Alert-Analysis.pdf)
