# Day 8: Basic Alert Triage Exercise

## Task
Take a security event from earlier lab work and triage it as a SOC analyst would. Identify the affected asset and user, the timeframe, the likely cause, a severity rating, and a disposition (close, monitor or escalate), with a justification for each.

**Tools:** Windows Event Viewer, Day 6 evidence (4625 and 4624 .evtx exports), Excel

## What I did
- Used the failed-login activity from Day 6 as the alert to triage
- Re-read the exported 4625 and 4624 events and pulled out the asset, user, source, logon type and failure code
- Built a chronological timeline of all five authentication events
- Worked out the timeframe: first failure to successful logon took 10 minutes 42 seconds
- Rated the likely cause, severity and disposition, and wrote a justification for each
- Added an analyst note on how future failed-login clusters should be handled

## Triage result
| Field | Value |
|---|---|
| Affected asset | DESKTOP-AUHDFVM (Windows 11 lab endpoint) |
| Affected user | CyberAmy (local account) |
| Timeframe | 19:20:05 to 19:30:47, 10 minutes 42 seconds |
| Activity | 4 failed logons (Event 4625), then 1 successful logon (Event 4624) |
| Source | 127.0.0.1, local interactive console (Logon Type 2) |
| Likely cause | Legitimate user error, self-resolved |
| Severity | Low |
| Disposition | Close |

## Key findings
- All four failures carried SubStatus 0xc000006a, a wrong password on a valid account. A guessed or nonexistent username would have returned 0xc0000064
- Every event shared the same account, logon type and origin, and the success came about 3 minutes 44 seconds after the last failure
- Four failures is well under the 10-attempt lockout threshold, and the source was the physical console, not the network
- The severity is Low and not Informational, because failed authentication should always stay visible in the record

## Key takeaway
Triage is a decision, not a description. Each rating needs a reason that someone else could check against the evidence. This one closed because every field pointed to the same benign explanation. The same four failures from a network address, against several usernames, or continuing past a normal user's patience would have changed the result. Closing this ticket also does not mean closing the pattern. Future clusters should be reviewed on their own evidence, not matched against this ticket.

## Evidence
📄 [Full triage report (Excel)](./Day-8-Basic-Alert-Triage-Exercise.xlsx)
