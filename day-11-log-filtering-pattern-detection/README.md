# Day 11: Log Filtering and Pattern Detection

## Task
Create a small sample authentication log, then use `grep`, `awk` and `sort` to identify repeated usernames, repeated source IPs and repeated failure messages, and note which patterns a SOC analyst would care about.

**Tools:** Kali Linux, `grep`, `awk`, `sort`, bash heredoc

> **Data note:** The log analyzed here (`sample_auth.log`) is synthetic. I wrote it by hand in real sshd log format so the correct answer was known in advance. It is not captured traffic and not a real attack.

## What I did
- Wrote a 16-line SSH authentication log directly in the terminal with a bash heredoc, built with repeated usernames, two source IPs, and both failed and successful logins
- Printed the file back with `cat` to confirm it saved correctly
- Counted the failure message with `grep -c "Failed password"`, which gave 14 of 16 lines
- Counted usernames with `grep` piped into `awk '{count[$9]++}'` and ranked them with `sort -rn`
- Counted source IPs the same way using field 11
- Filtered the two successful logins separately and compared them with the failures
- Stayed within the three tools the task names, with no `uniq`

## Pattern summary
| Pattern | Top result | Count |
|---|---|---|
| Failure message | "Failed password" | 14 of 16 lines |
| Username | root (admin next) | 7 (admin 4) |
| Source IP | 203.0.113.5 | 11 (second IP 3) |
| Successful login | kali from 192.168.84.1 | 2 |

## Key findings
- One IP, 203.0.113.5, produced 11 of the 14 failures, including 5 of the 7 `root` attempts and 3 of the 4 `admin` attempts
- Rankings show a dictionary-attack shape: two heavily targeted usernames and several one-off guesses (MITRE ATT&CK T1110)
- The second IP targeted the same two usernames at about a quarter of the volume. The log can't show whether that is a second actor or the same one on a different path
- The only account that ever logged in successfully, `kali`, never appears among the failures and came from a different, internal address

## Key takeaway
A repeated IP alone could be a misconfigured script, and a repeated username alone could be one person forgetting a password. The same IP hitting the same short list of high-value usernames is much stronger evidence than either one. Testing the commands on data where the answer is already known is also how detection logic should be validated before it is pointed at real logs. The counts here come from `awk`'s associative arrays, which showed that `awk` can tally and aggregate on its own and does not only reformat fields.

## Evidence
📄 [Full report with screenshots](./Day-11-Log-Filtering-Pattern-Detection.pdf)
