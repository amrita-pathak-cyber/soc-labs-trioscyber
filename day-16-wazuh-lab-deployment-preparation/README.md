# Day 16: Wazuh Lab Deployment Preparation

## Task
Prepare a dedicated Wazuh server virtual machine for installation. Verify that its operating system is supported, and confirm its hostname, network connectivity, disk space, RAM and administrative access. Record the final network plan before installing Wazuh.

**Tools:** VMware Workstation Pro, Ubuntu Server 24.04.5 LTS, `cat`, `hostname`, `ip`, `ping`, `df`, `free`, `lvextend`, `resize2fs`

> **Data note:** Everything in this report was captured from the live `wazuh-server` VM. Nothing is simulated or synthetic.

## What I did
- Moved the task to a second, roomier Dell PC after finding that my lab laptop had only about 21 GB of free disk space after cleanup, not enough for a Wazuh server alongside its other VMs
- Created a new VM called `wazuh-server` with 2 CPU cores, 6 GB of RAM and a 40 GB disk, and set its network adapter to Bridged instead of NAT
- Installed Ubuntu Server 24.04.5 LTS in the text-mode installer, including the OpenSSH server
- Ran a verification pass: OS version, hostname, IP address, internet connectivity, disk, RAM and `sudo` access
- Found that the guided installer had given only 19 GB of the 40 GB disk to the root filesystem, and fixed it online with `lvextend` and `resize2fs`, with no reboot and no data loss
- Recorded the network plan and the next step

## Verification summary
| Check | Result |
|---|---|
| Operating system | Ubuntu 24.04.5 LTS, supported by Wazuh |
| Hostname | `wazuh-server` |
| Network | Real address on the home network via Bridged mode, 0% packet loss to the internet |
| RAM | 5.7 GiB visible of 6 GB allocated (below Wazuh's 8 GB recommendation) |
| Disk | 38 GB usable, 29 GB free after the LVM fix (below the 50 GB recommendation) |
| Admin access | `sudo whoami` returns `root` |

## Key findings
- The 40 GB disk showed up as 19 GB. Ubuntu's guided LVM setup doesn't always give a logical volume all the free space in its volume group, and `lvextend` followed by `resize2fs` corrected it
- Bridged mode was chosen over NAT so the Wazuh server can later reach the lab VMs, which will be enrolled as agents
- Two compromises against Wazuh's small-deployment recommendations remain (6 GB RAM against 8 GB, 38 GB disk against 50 GB). They are driven by the host hardware and are documented, not hidden

## Key takeaway
Check the machine before you install onto it. The disk problem showed up in the first `df -h`, and it would have been far harder to diagnose halfway through a Wazuh install. Hardware limits are part of the job, so state them plainly: what the official minimum is, what I could provide, and why. Moving the task to a different machine was a better decision than forcing a cramped setup onto the original laptop.

## Evidence
📄 [Full report with screenshots](./Day-16-Wazuh-Lab-Deployment-Preparation.pdf)
