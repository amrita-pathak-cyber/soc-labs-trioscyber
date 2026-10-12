# Day 13: Correlation Rule Design Exercise

## Task
Define a simple detection rule based on repeated failed authentication events, using a threshold and a time window. Test the rule manually against a known dataset, then validate the result in Splunk.

**Tools:** Splunk Enterprise, SPL (`rex`, `sort`, `streamstats`), Kali Linux

> **Data note:** The data is `sample_auth.log`, the same synthetic SSH log I wrote by hand for Days 11 and 12. It is not captured traffic and not a real attack. Reusing it meant the correct answer was already known, so the manual and automated results could be checked against each other.

## What I did
- Defined the rule: flag any source IP with 3 or more failed logins inside a rolling 5-minute window
- Worked through every failed event by hand in time order, keeping a running count per source IP and marking when each one crossed the threshold
- Wrote the rule in SPL with `streamstats` and ran it against the same file over All Time, because the log's fixed timestamps fall outside Splunk's default range
- Compared the 10 returned events one by one against the hand-worked table
- Saved the query as a Report called "Correlation Rule - 3 Failures in 5 Minutes"
- Stopped Splunk and shut the VM down cleanly, then confirmed the saved Report was still there afterwards

## Results
| Item | Result |
|---|---|
| Manual walkthrough | 10 alerting events (9 from 203.0.113.5, 1 from 198.51.100.9) |
| Splunk `streamstats` | 10 alerting events, same breakdown |
| False positives | 0 (the legitimate `kali` account never triggered) |
| False negatives | 0 (both attacking sources triggered) |
| Threshold rejected | 5 failures in 5 minutes, which would have missed 198.51.100.9 entirely |

## Key findings
- The hand-worked table and the Splunk query agreed exactly, which is strong evidence that the rule's logic is right and not just one implementation of it
- I checked a stricter threshold of 5 against the actual data first. It would have missed the secondary source, which only ever produced 3 failures
- The rule counts each source IP on its own, so the quieter source wasn't hidden underneath the louder one
- The whole log spans only about 96 seconds, so this test never made events age out of the 5-minute window. Window expiry is untested and needs a longer dataset

## Key takeaway
A threshold should be checked against real data before it is trusted. A higher bar looks safer, but here it would have meant a real false negative. A rule is only trustworthy once it has been confirmed both ways: it fires on the attack pattern and stays silent on the legitimate account. Checking the logic by hand and then in a query gives stronger evidence than either alone. This test also had a clear gap, and saying so plainly is part of the result.

## Evidence
📄 [Full report with screenshots](./Day-13-Correlation-Rule-Design-Exercise.pdf)
