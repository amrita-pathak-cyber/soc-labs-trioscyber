# Day 10: Linux Authentication Log Hunt

## Task
Generate or identify authentication activity on a Linux VM, then use `journalctl` and `grep` to extract the successful and failed authentication events. Build a timeline of at least three timestamped events.

**Tools:** Kali Linux, `sudo`, `journalctl`, `grep`, `tail`

## What I did
- Cleared sudo's cached credentials with `sudo -k`, so the next sudo command would have to prompt for a password
- Ran `sudo whoami`, entered a wrong password so sudo rejected it, then entered the correct one in the same invocation
- Tried `tail` on `/var/log/auth.log` and found the file doesn't exist on this Kali install, so I switched to the systemd journal
- Ran `journalctl -t sudo --no-pager | tail -20` to confirm the attempt was recorded
- Ran `journalctl -t sudo --no-pager | grep -i "authentication failure\|session opened"` to isolate the failure and session lines by pattern
- Matched the log lines to their process IDs and built a three-event timeline

## Authentication timeline
| Timestamp | Event | Process | Meaning |
|---|---|---|---|
| 01:17:33 | Authentication failure | sudo[2854] | Wrong password rejected |
| 01:19:08 | Session opened | sudo[2854] | Correct password accepted |
| 01:21:04 | Session opened | sudo[4648] | New sudo command using a cached credential, no password prompt |

## Key findings
- The missing `auth.log` was a finding in itself. This Kali install logs authentication to the systemd journal only, so `grep` and `tail` had to work on `journalctl` output
- Several wrong passwords in one sudo invocation produce a single failure line, because PAM logs one failure per invocation. Windows logs a separate 4625 event for each wrong password
- Sudo never locked the account. The next prompt in the same invocation accepted the right password at once, unlike a typical Windows lockout policy
- The 01:21:04 session had no password prompt because sudo caches a successful login for a short window. It is a real logged event, but it traces back to the earlier authentication
- `tail` and `grep` returned the same log entries, which gives more confidence that nothing was misread

## Key takeaway
The same question, whether a login attempt failed or succeeded and when, has different answers in different places. On Linux, counting failure lines undercounts the passwords tried. A string of sudo failures doesn't lock anyone out. A session with no password prompt is not an unexplained escalation, because it traces back to a login that happened minutes earlier. An analyst who carries Windows assumptions onto a Linux host will misjudge the severity of what they find. Learn the concept first, then check how each platform records it.

## Evidence
📄 [Full report with screenshots (PDF, use the download icon if the preview doesn't load)](./Day-10-Linux-Authentication-Log-Hunt.pdf)
