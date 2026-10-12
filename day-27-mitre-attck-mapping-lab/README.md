# Day 27: MITRE ATT&CK Mapping Lab

## Task
Choose one simulated behavior from the lab, map it to the most relevant MITRE ATT&CK tactic and technique, and record the supporting evidence.

**Tools:** MITRE ATT&CK, Windows event logs, Wazuh ruleset (rule definitions only)

> **Status:** Mapping exercise based on real evidence: the 8 failed logons from Day 25, collected and hash-sealed as case INC-20260929-001 in Day 26. No Wazuh manager is deployed yet (see Days 17 to 20), so the Wazuh section compares the real rule definitions against my real evidence. It is not a live alert.

## What I did
- Chose the repeated failed-logon behavior because it is the one behavior in my lab with complete, verified evidence, rather than the one that sounded most interesting.
- Re-extracted the key fields (account, source, logon type, failure reason, timestamps) from the sealed evidence file instead of the live log.
- Mapped the behavior down the ATT&CK levels: tactic, technique, sub-technique.
- Tested every alternative sub-technique and related technique against the evidence and recorded why each was rejected.
- Compared my mapping with the ATT&CK tags Wazuh attaches to its own rules.
- Checked MITRE's detection guidance and mitigations against what my lab actually had in place.

## The mapping

| Level | Result |
|---|---|
| Tactic | Credential Access (TA0006) |
| Technique | Brute Force (T1110) |
| Sub-technique | Password Guessing (T1110.001) |
| Evidence | 8 x Event ID 4625, one account, valid user name (SubStatus 0xC000006A), one source (127.0.0.1, Logon Type 2), 226 seconds |
| Rejected | T1110.002 Cracking, T1110.003 Spraying, T1110.004 Credential Stuffing, T1078 Valid Accounts, T1531 Account Access Removal |
| Case verdict | Benign true positive: authorized self-test |

## Key findings
- A mapping describes behavior, not intent. 8 password guesses against one account is T1110.001 whether an attacker or an intern typed them. The benign verdict is a separate step.
- Map to the most specific level the evidence supports and stop there. One account with many guesses rules out spraying, and live logon failures rule out offline cracking.
- Wazuh's tags need checking, not copying. Rule 60204 agrees with the evidence at the technique level (T1110), but the single-event rules carry broader or different tags (T1078, T1531) that do not fit this incident.
- Mapping exposed a real hardening gap in the lab: the VM has a minimum password length of 0. The lockout threshold of 10 is in place, which matches MITRE's mitigation M1036.

## Key takeaway
ATT&CK gives every SOC the same words for the same behavior, but it only helps if the mapping is driven by evidence. Showing why the other options were rejected is what makes a mapping defensible.

ATT&CK® is a registered trademark of The MITRE Corporation.

## Evidence
📄 [Full report with screenshots](./Day-27-MITRE-ATTCK-Mapping-Lab.pdf)
