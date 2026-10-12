# Day 9: Windows Log Hunt

## Task
Inspect the Windows Security, System and Application logs on a lab endpoint. Select at least five events across different categories, filter by time or event source instead of browsing unfiltered, and judge whether each event would be useful to a SOC analyst in a real investigation.

**Tools:** Windows 11 VM, Windows Event Viewer, Filter Current Log

## What I did
- Filtered the Security log by a custom time range covering the recent lab activity, which returned 439 events
- Filtered the System log by event source (Service Control Manager), which returned 98 events
- Filtered the Application log by severity (Critical, Error and Warning), which cut the log down to 49 events
- Picked five events from the three logs and read each one in full on both the General and Details tabs, checking the Subject, source and message before describing it
- Rated each event's SOC usefulness and mapped two of them to MITRE ATT&CK techniques
- Built a consolidated table of all five events

## Events examined
| Event ID | Log | What it was | SOC usefulness |
|---|---|---|---|
| 4672 | Security | Special privileges assigned to a new logon, by the SYSTEM account | Situational, not useful here |
| 4798 | Security | A user's local group membership was enumerated (T1069) | Situational, benign here |
| 7045 | System | Microsoft Defender Core Service installed (T1543.003) | High value as a type, benign here |
| 1000 | Application | widgetservice.exe crash | Low priority alone, useful for timelines |
| 86 | Application | Certificate enrollment failure, repeated five times | Low priority, explainable lab noise |

## Key findings
- The first 4672 looked like a privileged human logon, but its Subject was SYSTEM (SID S-1-5-18), so it was routine Windows activity
- I had assumed Event 86 was a domain certificate enrollment failure. The full message showed it was the VM's virtual TPM trying to reach an Azure attestation endpoint that doesn't exist
- Event types that map to real attack techniques, like 7045 for persistence and 4798 for discovery, can still be harmless in a given instance
- Each log needed a different filter: time range for Security, event source for System, severity level for Application

## Key takeaway
An Event ID and a source name are only a starting point. Two of the five events would have been described wrongly if I had stopped there. Reading the Subject and the full message is what separates a false alarm from a real finding. The opposite mistake is just as costly. 7045 and 4798 were benign here, but dismissing them by habit is how a real service-installation or discovery event gets missed. The habit to carry into a real queue is to verify every time and not assume either way.

## Evidence
📄 [Full report with screenshots](./Day-9-Windows-Log-Hunt.pdf)
