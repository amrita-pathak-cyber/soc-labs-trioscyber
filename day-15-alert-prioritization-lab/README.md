# Day 15: Alert Prioritization Lab

## Task
Score and prioritize five alerts, each with a priority level and a written justification. The alerts are simulated from this internship's own lab evidence.

**Tools:** Excel (scoring formulas and lookup tables), evidence from Days 6, 10, 11, 13 and 14

## What I did
- Picked five alerts from my own lab evidence: failed Windows logons, a failed Linux sudo attempt, 404 reconnaissance probes, a request with a faked sqlmap user-agent, and a brute-force pattern
- Rated each alert Low, Medium or High on four factors: asset criticality, repetition, user impact and evidence quality
- Converted each rating to a score from 1 to 3, summed them to a total from 4 to 12, and mapped the total to a score-based priority with formulas
- Compared the score-based priority with my own call, and marked any difference as an override with the reason written down
- Built a scoring reference tab and an evidence index tab, so every alert traces back to its source with no outside context

## Alert priority
| Alert | Scenario | Score | Final priority |
|---|---|---|---|
| 01 | 4 failed logons then a success, same account, local console | 6 | Low |
| 02 | 1 failed sudo, then success and a cached session | 6 | Low |
| 03 | 3 reconnaissance probes returning 404 | 8 | Medium |
| 04 | One request with a sqlmap user-agent returning 200 | 6 | **High (override)** |
| 05 | 11 failed logins from one source, aimed at privileged usernames | 10 | High |

## Key findings
- Alerts 1 and 2 are fully explained by ordinary human error and rate Low
- Alert 3 is a recognizable scan, but every request failed, so it stops at Medium
- Alert 4 scored Low (6) because it is a single request, but it is a request that names a real exploitation tool and reached live content. I escalated it to High and documented the override in the sheet
- Alert 5 scored 10, with high volume against privileged account names, and was confirmed two independent ways
- The model is an additive scorecard, not the standard Risk = Likelihood x Impact formula. The sheet says so openly and explains why a fourth factor, evidence quality, was added

## Key takeaway
A score is where triage starts, and it doesn't settle the matter. The scoring model put the faked sqlmap request at Low, and the right call was to override it, because one high-confidence indicator should not wait for repetition. What makes an override acceptable is that it is deliberate and written down, not a silent departure from the model. Evidence quality belongs in the score too, because how far you trust a piece of evidence decides how much weight the rest of the score deserves.

## Evidence
📄 [Full scoring sheet (Excel)](./Day-15-Alert-Prioritization-Lab.xlsx)
