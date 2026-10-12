# Day 20: Windows Wazuh Agent Deployment

## Task
Install and enroll the Wazuh agent on a Windows lab VM. Verify the service status locally and confirm the endpoint is visible and active in Wazuh.

**Tools:** Wazuh agent (MSI installer), Windows, PowerShell and CMD, Wazuh Dashboard

> **Status:** This is a **walkthrough of the planned procedure**. It was **not executed**. As with Day 19, enrollment needs a running Wazuh manager, and I could not bring one online. No terminal output or dashboard screenshot is included, because none exists yet. I will update this page with real evidence once a manager is available.

## What I did
- Documented why no manager was available: the Day 17 installation failures, the Day 18 disk preparation, and the decision to pause the dual-boot migration rather than risk an irreversible change on a shared machine I couldn't fully back up
- Wrote the Windows agent procedure with `<MANAGER_IP>` as a placeholder, in both CMD and PowerShell
- Listed the checks to run and the evidence to capture once a manager exists

## Planned procedure
| Step | CMD | PowerShell |
|---|---|---|
| Install and enroll (silent, manager address set at install time) | `msiexec.exe /i wazuh-agent-4.14.7-1.msi /q WAZUH_MANAGER="<MANAGER_IP>"` | same command, run from the installer's folder |
| Start the service | `NET START WazuhSvc` | `Start-Service wazuhsvc` |
| Check the service locally | `sc query WazuhSvc` | `Get-Service wazuhsvc` |
| Read the agent log if it fails | | `Get-Content '...\ossec-agent\ossec.log' -Tail 50` |

The last step is to open the Dashboard's Agents view and confirm the Windows endpoint is listed as Active with a recent Last Keep Alive time.

## Key findings
- On Windows there is no repository setup step as on Linux, because the single MSI file is self-contained
- The `WAZUH_MANAGER` parameter writes the manager address into `ossec.conf` at install time, so no manual editing is needed
- Running locally is necessary but not enough. The proof is the agent showing as Active on the manager's side
- Likely failure causes are a wrong manager address, a firewall blocking ports 1514 and 1515, or the two machines sitting on different network segments

## Key takeaway
A procedure is not a result, and this page says so. What it does show is that I know what each parameter does, how to confirm success on both sides of the connection, and where to look first when it fails. The plan is ready to run the moment a manager exists, and I will replace it with real output when it does.

## Evidence
📄 [Walkthrough report (PDF)](./Day-20-Windows-Wazuh-Agent-Deployment.pdf)
