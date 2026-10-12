# Day 28: Threat Intelligence / IOC Enrichment

## Task
Take a safe, non-sensitive IOC from sample data or a known training indicator. Check available reputation and context sources, and record what additional context would affect analyst prioritization.

**Tools:** VirusTotal (hash search), MISP warninglists, Wazuh (integration described only)

> **Status:** The VirusTotal and MISP lookups were done live on 30 September 2026, using hash search only: no file was downloaded, created, or uploaded. The Wazuh VirusTotal integration is described from its documentation and was not run, because no Wazuh manager is deployed in my lab yet (see Days 17 to 20).

## What I did
- Chose the SHA-256 of the **EICAR antivirus test file**, an industry-standard harmless training indicator.
- Searched the hash on VirusTotal using the SEARCH tab, so nothing was uploaded or shared.
- Read the detection ratio, the individual vendor labels, file identification, file history, and community content.
- Checked the same hash against the **MISP EICAR warninglist** as an independent second source.
- Listed the context that would raise or lower the priority of this indicator.
- Described how Wazuh's VirusTotal integration would automate the lookup, and linked the exercise to the Pyramid of Pain.

## What the enrichment showed

| Source | Finding | Effect on priority |
|---|---|---|
| VirusTotal detection ratio | 66 / 68 engines (on 30 Sept 2026) | Raises it, if viewed alone |
| Vendor labels | Mostly "EICAR test" / "not a virus" | Lowers it |
| File identification (Magic, TrID) | EICAR antivirus test file, 68 bytes | Lowers it |
| History | First submitted 2006, first seen in the wild 2012 | Lowers it: long known, not emerging |
| MISP warninglist | SHA-256, SHA-1, and MD5 all listed as EICAR test virus | Lowers it strongly |
| Automated tags (powershell, via-tor, and others) | Do not fit a 68-byte text string | Unreliable without corroboration |

**Default assessment:** low priority, known benign test file.

## Context that would change the priority
- **Where it appeared:** a test VM is low, a production server or finance laptop is higher.
- **How it arrived:** downloaded by the security team is low, an email attachment or USB is higher.
- **How many hosts:** one host is low, many hosts with no scheduled test is higher.
- **Was it blocked:** if antivirus did not stop it, that is a real finding even though the file is harmless.
- **Is it embedded:** inside an archive or document deserves investigation.

## Key findings
- A detection ratio is a count of opinions, not a verdict. 66 detections did not make it dangerous, and 2 non-detections would not have made it clean.
- Two independent sources, vendor labels plus a curated warninglist, together turn an alarming-looking score into a low-priority result.
- Generic machine-learning verdicts, automated tags, and community content are leads to corroborate, not conclusions.
- Search by hash. Files uploaded to VirusTotal are shared with vendors and premium users, so sensitive files should never be uploaded.
- Hashes sit at the bottom of the Pyramid of Pain, because an attacker can change a hash by altering one byte. Hash enrichment is quick for triage but easy to evade.

## Key takeaway
Reputation data is only the starting point. Priority comes from context: where the indicator appeared, how it arrived, how many hosts, and whether defenses stopped it. The unblocked case matters most, because it says something about the endpoint and not about the file.

## Evidence
📄 [Full report with screenshots](./Day-28-Threat-Intelligence-IOC-Enrichment.pdf)
