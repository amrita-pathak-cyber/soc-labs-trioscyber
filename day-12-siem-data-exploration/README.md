# Day 12: SIEM Data Exploration

## Task
Install and configure a SIEM (Splunk), index authentication-related log data, search for authentication events, apply time-based filtering, and produce one saved visualization or saved search from the results.

**Tools:** Splunk Enterprise 10.4.3 (free trial license), Kali Linux, SPL (Search Processing Language)

> **Data note:** The data indexed here (`sample_auth.log`) is the same synthetic SSH log I wrote by hand for Day 11. It is not captured traffic and not a real attack. I reused it on purpose so Splunk's results could be checked against a count I already knew.

## What I did
- Downloaded the 64-bit `.deb` package directly inside the Kali VM and installed it with `dpkg`
- Started Splunk for the first time and hit a refusal to run as root, then restarted with the `--run-as-root` flag that Splunk itself offers for single-user lab setups
- Accepted the license, created the admin account, and opened the web interface at `127.0.0.1:8000`
- Uploaded `sample_auth.log` with the Upload input, checked Splunk's event preview line by line, and saved a custom source type called `ssh_auth_sample`
- Recovered from a VM freeze under memory pressure by giving the VM more RAM
- Found that the interruption had indexed the file twice, with 28 events instead of the expected 14, and fixed the search with `| dedup _raw` instead of deleting indexed data
- Applied a custom 30-second time range, extracted usernames with `rex`, counted them with `stats`, and saved the result as a column chart in a Report called "Failed Login Attempts by Username"

## Key findings
- After deduplication, Splunk returned exactly 14 failed events, the same number `grep -c` gave on Day 11. Two different tools agreed on the same data
- Inside the 30-second window, the counts were root 3, admin 2 and test 1
- I knew the right answer was 14, so the doubled count of 28 was caught immediately and not by an after-the-fact audit
- `dedup` fixed the problem without touching the stored data, while deleting would have needed an elevated permission that is off by default

## Key takeaway
SPL and the shell tools from Day 11 are the same skill in different forms. `grep` becomes a search term, and `awk` and `sort` become `rex` and `stats`. Testing a new tool against data whose answer you already know is what made the duplicate upload obvious. When something goes wrong, the smaller, reversible fix is usually the right one. Setup, memory limits and recovery are part of operating a SIEM, not a detour from it.

## Evidence
📄 [Full report with screenshots](./Day-12-SIEM-Data-Exploration.pdf)
