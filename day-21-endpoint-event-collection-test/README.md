# Day 21: Endpoint Event Collection Test

## Task
Generate real Windows security events (a failed logon and a service state change), analyze them in Event Viewer, and map each one to Wazuh's default detection rule structure, including the correlation logic.

**Tools:** Windows Event Viewer, Services (services.msc), Wazuh ruleset reference

> **Status:** Part A (event generation and analysis) was executed on my Windows 11 VM with real screenshots. Part B (Wazuh rule mapping) is theoretical: no Wazuh manager is deployed yet, so no rule was triggered or tested. The mappings will be confirmed once a manager is running.

## What I did

**Part A: Practical (executed)**
- Generated a failed logon for the local user CyberAmy and found it in the Security log as **Event 4625** (Logon Type 2, Status 0xc000006d, logged 29-09-2026 09:56:06).
- Restarted the Print Spooler service and searched for **Event 7036** (service entered running/stopped state). The search returned zero results. I could not verify why, and I have recorded it as an open gap rather than guessing.
- As a substitute, changed the Print Spooler startup type (automatic to manual, then back) and captured the resulting **Event 7040** pair at 10:24:47 and 10:25:16, attributed to DESKTOP-AUHDFVM\CyberAmy.

**Part B: Theoretical (not executed)**
- Mapped Event 4625 to Wazuh rule **60122** (level 5, group authentication_failed).
- Studied correlation rule **60204** (level 10, 8 failures within 240 seconds from the same source, MITRE T1110) and worked out that a single test event triggers only the base rule, not the correlation rule.
- Found that Event 7040 has no default alerting rule (base rules 60000 and 60002 are level 0).
- Drafted a custom rule, **100050**, to alert on service startup-type changes. It is a proposal and has not been tested.

## Evidence summary

| Event | Source | Observed in my lab | Wazuh coverage |
|---|---|---|---|
| 4625 failed logon | Security log | Yes | Rule 60122 (level 5); correlation 60204 needs 8 in 240 s |
| 7036 service state | System log | No (zero results, cause unverified) | Not tested |
| 7040 startup type change | System log | Yes | No default alert; custom rule 100050 proposed |

## Key findings
- A single failed logon is visible in Event Viewer but would raise only a level 5 base alert in Wazuh. Escalation to level 10 depends on repetition within a time window.
- Not every meaningful endpoint change has a default detection rule. Service startup-type changes (7040) are logged by Windows but silent in Wazuh until a custom rule is written.
- A custom rule for 7040 would be noisy if it fires on every service. Production use should filter on the service name (win.eventdata.param1).
- When an expected event (7036) does not appear, the honest step is to document it and test an alternative rather than assume the cause.

## Key takeaway
Detection depends on two things: the endpoint logging the event, and the SIEM having a rule that cares about it. Generating the event is the easy half. Knowing which events have default coverage, which need custom rules, and how thresholds turn single events into alerts is what lets an analyst tune detection instead of just receiving it.

## Evidence
📄 [Full report with screenshots](./Day-21-Endpoint-Event-Collection-Test.pdf)
