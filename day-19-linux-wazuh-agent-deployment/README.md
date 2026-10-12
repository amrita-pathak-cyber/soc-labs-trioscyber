# Day 19: Linux Wazuh Agent Deployment

## Task
Install and enroll a Wazuh agent on a Linux endpoint, and confirm it appears in agent management with an active status.

**Tools:** Wazuh agent, Linux VM, `apt`, `systemctl`, `journalctl`, Wazuh Dashboard

> **Status:** This is a **walkthrough of the planned procedure**. It was **not executed**. Enrolling an agent needs a running Wazuh manager, and I could not bring one online (see Days 17 and 18). No terminal output or dashboard screenshot is included, because none exists yet. I will update this page with real evidence once a manager is available.

## What I did
- Worked for four days to get a Wazuh manager running: repeated all-in-one installs in a VM, a diagnosis of why the indexer would not start, and the start of a move to a native dual-boot install
- Stopped the dual-boot migration before the install step. The only machine with enough disk space is a shared household computer, and I could not make a complete, verified backup of its drive, so I would not apply an irreversible disk change there without the other user's sign-off
- Wrote the full agent deployment procedure with `<MANAGER_IP>` as a placeholder
- Wrote out the checks I will run and the evidence I will capture once a manager exists

## Planned procedure
| Step | Action |
|---|---|
| 1 | Add Wazuh's signing key and package repository to the Linux endpoint, then `apt-get update` |
| 2 | Install the agent with the `WAZUH_MANAGER` variable set, so the manager address is written into `ossec.conf` automatically |
| 3 | `daemon-reload`, `enable` and `start` the `wazuh-agent` service |
| 4 | Check the service locally with `systemctl status wazuh-agent`, expecting active (running) |
| 5 | Confirm in the Dashboard's agent list that the endpoint shows Active, with a Last Keep Alive time that keeps updating |

If the agent fails, the plan is to read its log with `journalctl -u wazuh-agent` and check the likely causes: an unreachable manager IP, a blocked enrollment port (1515) or a stale `client.keys` file from an earlier attempt.

## Key findings
- Enrollment is a client-server handshake, so the agent has nothing to register with until a manager is running
- A local `active (running)` status is necessary but not enough. The real proof is the agent showing as Active on the manager side with a live keep-alive time
- Ports 1514 (agent traffic) and 1515 (enrollment) are the common failure points when a firewall blocks them

## Key takeaway
A procedure you haven't run is a plan and not a result, and this page says so. I stopped before an
