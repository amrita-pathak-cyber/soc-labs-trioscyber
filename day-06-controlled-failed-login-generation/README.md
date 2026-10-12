# Day 6: Controlled Failed Login Generation

## Task
Generate several safe, intentional failed authentication attempts on a lab account without locking it out, then locate and examine the resulting events in the operating system's own logs.

**Tools:** Windows 11 VM, Windows Event Viewer, Event IDs 4625 and 4624

## What I did
- Chose Windows 11 because a failed login there needs no service setup, only a wrong password at the sign-in screen
- Entered a wrong password for the local account CyberAmy four times, well under the default lockout threshold of 10
- Signed in with the correct password afterwards to prove the account was never locked
- Opened the Security log in Event Viewer and filtered 2,400+ events down to Event ID 4625, which returned exactly four failed logons
- Read the event fields on the Details tab, including the Status, SubStatus and Logon Type
- Filtered for Event ID 4624 to find the successful logon, then compared it with the failed ones
- Exported both event sets as .evtx files so the evidence could be reopened and re-filtered later

## Key findings
- SubStatus 0xc000006a means a wrong password. A nonexistent username would have returned a different code (0xc0000064)
- All four failures were Logon Type 2 (interactive) from 127.0.0.1, so they were local attempts and not network logins
- The failures ran from 19:20:05 to 19:27:03 and the successful logon came at 19:30:47, with the same account and the same logon type
- The first 4624 result was not the right one. It was an automatic service logon by the machine account, so I had to check each candidate's account name and logon type

## Key takeaway
The Status code only says a logon failed. The SubStatus code says why, and that is what separates someone mistyping a real account's password from someone guessing usernames. Context decided how serious this was. One account, one local method, a short run of failures and then a success is a typical forgotten-password pattern. The same failures from a network address, or against many different accounts, would need escalation. The 4624 search also showed that a filter picks the event type but not the right event.

## Evidence
📄 [Full report with screenshots](./Day-6-Controlled-Failed-Login-Generation.pdf)
