# Day 17: Install Wazuh All-in-One Lab

## Task
Install Wazuh in an all-in-one configuration (indexer, manager and dashboard on one node) on the `wazuh-server` VM prepared on Day 16, using the official installation assistant.

**Tools:** Wazuh 4.14.7 installation assistant, Ubuntu Server 24.04.5 LTS, `journalctl`, `systemctl`, `apt`, SSH

> **Outcome:** The installation was **not completed**. This page documents the attempts, the evidence I collected and my diagnosis. The screenshots are from the real failed attempts. They are not simulated.

## What I did
- Ran the official installer (`wazuh-install.sh -a`) several times. Each run generated certificates and installed the indexer package, but the indexer service then failed to start, and the installer's cleanup removed the indexer each time
- Checked `max_map_count` and found the standard default of 262144, which ruled out a common OpenSearch prerequisite as the cause
- Read the service logs with `journalctl`. systemd stopped the indexer with "start operation timed out" after about 3 minutes, and the service had used 5 minutes 11 seconds of CPU time in that window
- Captured kernel soft-lockup warnings in which a CPU core went unresponsive for 21 to 29 seconds
- Checked the installer's help output to confirm the right flags for a single-node setup
- Tried a manual route to control the service start myself, and hit an apt package-not-found error and a GPG keyring error while re-registering the Wazuh repository

## Key findings
- The failure was the same on every attempt, regardless of how long I waited, so it wasn't a timing problem
- The indexer wasn't hung. It was working hard and never became ready, which points to resource exhaustion on a 6 GB VM, not a stalled process
- My most likely explanation is that the indexer's Java heap is too large for this VM, which pushes the system into swap thrashing. This is a diagnosis. I have not yet tested the fix, so it isn't confirmed
- The planned fix is to cap the heap (for example `-Xms1g -Xmx1g` in `jvm.options`) before the service first starts
- No data was lost, and the certificate bundle from the installer can be reused

## Key takeaway
A failed install is still evidence. Repeating the same command and hoping for a different result wasn't working, so the useful step was to stop and read what the system was recording. CPU time used, kernel warnings and service logs all pointed the same way. A diagnosis should also state how sure it is. Mine fits the evidence, but it stays a hypothesis until the fix is applied and the indexer stays up.

## Evidence
📄 [Full report with screenshots](./Day-17-Install-Wazuh-All-in-One-Lab-Attempt.pdf)
