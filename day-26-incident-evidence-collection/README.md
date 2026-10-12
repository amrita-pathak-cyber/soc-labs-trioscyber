# Day 26: Incident Evidence Collection

## Task
Take one suspicious pattern from my lab and collect supporting evidence: affected endpoint, user, timestamps, source details, relevant logs and screenshots. Organize the evidence in a clear incident folder structure.

**Tools:** Windows PowerShell, wevtutil, Event Viewer, Wazuh (rule logic only)

> **Status:** Executed on my Windows lab VM. The pattern is the controlled sequence of 8 failed logons from Day 25, treated as if it were a real alert. The only missing item is a Wazuh alert, because no manager is deployed yet (see Days 17 to 20). A status note in the case folder documents that gap. The evidence package itself stays on the lab VM, and this repo holds the report.

## What I did
- Built a case folder, `INC-20260929-001`, with six numbered subfolders: case notes, screenshots, raw logs, parsed logs, endpoint info, and Wazuh.
- Confirmed the VM time zone (IST, UTC+05:30) and converted the incident window to UTC, because the log export filter works on UTC timestamps.
- Exported the Security log with `wevtutil`, once filtered to the incident window (Event IDs 4624 and 4625) and once in full. Raw exports were collected first, as the most volatile evidence.
- Parsed a readable CSV from the exported file, not from the live log.
- Captured eight endpoint state files (system info, network config, account details, local admins, lockout policy, and so on).
- Wrote a case summary and a chain of custody record, then hashed all 18 evidence files with SHA-256 as the final step, so nothing changes after sealing.

## What the evidence showed

| Item | Finding |
|---|---|
| Endpoint | DESKTOP-AUHDFVM (Windows VM) |
| Account | CyberAmy (local administrator) |
| Failed logons | 8, from 13:07:33 to 13:11:19 IST (226 seconds), Logon Type 2 |
| Source | 127.0.0.1, the machine's own console |
| Failure reason | Status 0xC000006D, SubStatus 0xC000006A (valid user name, wrong password) |
| Success | 13:12:10, recorded as two linked 4624 events |
| Case status | Closed: benign true positive (authorized lab test) |

## Key findings
- Re-reading the exported log confirmed the Day 25 timestamps exactly and added three things I had not established: the SubStatus, the local-console source, and the double logon.
- The two successful 4624 events at 13:12:10 are one logon recorded twice: a UAC split token. I stated it as a prediction first, then proved it from the Linked Logon IDs and Elevated Token values.
- Event Viewer shows local time, but exports are filtered on UTC. Mixing them is a common evidence error.
- Handling problems (a hidden paste character, garbled pasted notes, stray files) were caught, fixed before hashing, and recorded in the chain of custody.

## Key takeaway
Evidence is only useful if someone who wasn't there can verify it. That means raw data before parsed copies, normalized time, written notes, a chain of custody, and hashes recorded at the end. Rebuilding the findings from the exported evidence, not from memory, made the analysis stronger than my first pass in Day 25.

## Evidence
📄 [Full report with screenshots](./Day-26-Incident-Evidence-Collection.pdf)
