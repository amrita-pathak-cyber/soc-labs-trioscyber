# Day 18: Wazuh Component Health Check

## Task
Verify that the Wazuh server components are running and reachable. Record the service status, dashboard access and any troubleshooting done, using `systemctl` and the Wazuh Dashboard.

**Tools:** Windows Disk Management, `systemctl`, `journalctl`, Wazuh Dashboard

> **Status:** This is an interim report. The health check itself has **not been performed**, because it depends on a working Wazuh installation, which is still in progress (see Day 17). The report documents what was completed and the exact procedure planned for when Wazuh is running. No `systemctl` output or dashboard screenshot is included, because none exists yet.

## What I did
- Decided to move Wazuh off the resource-constrained VM and onto a native dual-boot Ubuntu Server install on the desktop, after the Day 17 failures
- Backed up the desktop's D: drive before touching any partition
- Used Windows Disk Management (Shrink Volume) to shrink D: by 100 GB, and checked that a 100 GB unallocated region now sits next to D: with no data lost
- Wrote the remaining plan in phases: download Ubuntu Server and build a USB installer, install Ubuntu into the free space (reusing the existing Windows boot partition), install Wazuh with the Java heap set explicitly before the first start, then run the health check
- Wrote out the verification procedure: `systemctl status` for the indexer, manager, dashboard and Filebeat, a `journalctl` check for any service that isn't running, and a dashboard login check with a screenshot

## Planned health check
| Component | What it does | Check |
|---|---|---|
| `wazuh-indexer` | Stores and indexes the security data | `systemctl status`, expect active (running) |
| `wazuh-manager` | Receives and analyzes agent and log data | `systemctl status`, expect active (running) |
| `wazuh-dashboard` | Web interface for alerts and search | `systemctl status`, then log in through the browser |
| `filebeat` | Ships data from the manager to the indexer | `systemctl status`, expect active (running) |

For any service that isn't active, the plan is to capture its log with `journalctl` straight away, so a failed check comes with an explanation and not just a pass or fail.

## Key findings
- The 100 GB of unallocated space is verified, and it is the only part of this task that has been completed
- The remaining steps (USB installer, Ubuntu install, Wazuh install, health check) are planned and not executed
- Wazuh's indexer is the component that failed to start on Day 17, and the manager and dashboard both depend on it, so it comes first in the check

## Key takeaway
I would rather hand in an honest status report than invent output I never saw. Showing exactly what is done, what is planned and what is missing keeps the evidence trustworthy. Backing up first and shrinking the existing partition, so nothing is deleted, was the safe way to prepare a machine that still holds my other data. A verified report will follow once Wazuh is running.

## Evidence
📄 [Interim status report (PDF)](./Day-18-Wazuh-Component-Health-Check.pdf)
