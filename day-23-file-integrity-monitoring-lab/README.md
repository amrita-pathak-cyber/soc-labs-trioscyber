# Day 23: File Integrity Monitoring Lab

## Task
Create a dedicated harmless test file in a monitored lab location, modify it, and observe the resulting Wazuh file-integrity event if the selected path is monitored. Do not alter critical system files.

**Tools:** Wazuh FIM (syscheck), Linux/Windows endpoint

> **Status:** Theoretical walkthrough. No Wazuh manager is deployed in my lab yet (see Days 17 to 20), so the test was not executed and no alerts were observed. The report will be re-submitted with real command output, alerts, and screenshots once a manager and agent are available.

## What I planned
- **Use a canary directory, not a real system path.** A dedicated disposable folder (`/home/cyberamy/fim_test` on Linux, `C:\FIM_Test` on Windows) keeps the test separate from real system files and from normal OS activity.
- **Add the folder to syscheck** with one `<directories check_all="yes" realtime="yes">` entry in the agent's `ossec.conf`. `check_all` enables the full set of integrity checks, and `realtime` replaces the default 12-hour scheduled scan with near-immediate detection.
- **Restart the agent** so the new configuration is applied.
- **Create a harmless test file**, then **append a line to it** to generate a second, different event.

## Expected Wazuh alerts (from the Wazuh ruleset, not observed)

| Action | Rule | Level | Description |
|---|---|---|---|
| Create `test1.txt` | 554 | 5 | File added to the system |
| Append a line to `test1.txt` | 550 | 7 | Integrity checksum changed (MITRE T1565.001) |

## Key findings
- A canary directory proves the detection pipeline works end to end before FIM is pointed at sensitive paths.
- Real-time monitoring is what makes a FIM test observable in one sitting. The default scan interval is 43,200 seconds (12 hours).
- Creating and modifying a file produce two different alerts. Wazuh rates a change to an existing file (level 7) above a new file appearing (level 5).
- FIM shows that a file changed, not who changed it. An analyst still needs to find out which process or user made the change before closing the alert.

## Key takeaway
FIM is only trustworthy once it has been tested somewhere safe. Starting with a disposable canary folder lets me confirm the configuration, the alert flow, and the rule behavior without risking the system, and gives me a baseline for what a clean detection looks like before I monitor anything that matters.

## Evidence
📄 [Full report (theoretical walkthrough)](./Day-23-File-Integrity-Monitoring-Lab.pdf)
