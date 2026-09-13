# Day 1: Isolated Lab Setup

## Task
Build an isolated lab using VMware with at least one Linux VM and one Windows VM. Configure networking so both systems can communicate, record each VM's IP address, test connectivity with ping, and capture the final topology.

**Tools:** VMware Workstation Pro, Kali Linux, Windows 11, `ip a` / `ipconfig`, `ping`

## What I did
- Installed VMware Workstation Pro and set up two VMs: Kali Linux and Windows 11, both on the same NAT network
- Recorded each VM's IP address using `ip a` (Kali) and `ipconfig` (Windows): Kali at 192.168.84.128, Windows at 192.168.84.129
- Tested connectivity in both directions with `ping -c 4 <ip>` from Kali and `ping <ip>` from Windows — confirmed 0% packet loss both ways
- Hit a Windows Defender Firewall block on inbound ICMP; resolved it by disabling the firewall profile via an elevated PowerShell command (`netsh advfirewall set allprofiles state off`)
- Captured the final topology showing both VMs connected on the shared network

## Key takeaway
The connectivity failure occurred not due to a network misconfiguration; it was Windows Defender Firewall silently dropping inbound ICMP by default. In a SOC context, this is the same instinct behind alert triage: a system not responding doesn't automatically mean it's down or compromised. It might just mean a control is doing exactly what it's supposed to do. Checking the control layer before escalating saves time and avoids false alarms.

## Evidence
📄 [Full report with screenshots](./Day 1-Lab-Setup.pdf)
