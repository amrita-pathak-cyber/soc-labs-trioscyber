# Day 7: SOC Threat Simulation and IOC Collection

## Task
Perform a harmless, controlled simulation in an isolated lab and collect the observable indicators it produces: username, hostname, timestamp, source IP, and process or file details.

**Tools:** Kali Linux, Metasploitable 2, `ip a`, `ifconfig`, `ping`, Python `http.server`, `curl`, `sha256sum`, `ls -la`, `stat`

## What I did
- Confirmed both machines were on the same network: Kali at 192.168.84.128 and Metasploitable at 192.168.84.130, with 0% packet loss on a ping
- Created a harmless text file on Kali and recorded its SHA-256 hash before moving it
- Served the file over HTTP from Kali with `python3 -m http.server 8080`
- Downloaded it onto Metasploitable into `/tmp` with `curl`, so the file crossed the network and was not simply copied
- Read the Kali server log line that recorded the request
- Recalculated the hash on Metasploitable and read the file's `ls -la` and `stat` metadata
- Put every indicator into one IOC table

## Key findings
- The Kali server log recorded the source IP (192.168.84.130), the time (09/Sep/2026 02:34:32) and the request (`GET /connection_test.txt`, status 200) on its own, with no input from the downloading machine
- The SHA-256 hash matched on both machines, so the file arrived unchanged
- The file's Modify and Change times were identical (02:34:40), which is normal for a file just created by `curl`. The Access time was later because the hash check read the file
- The IOC table holds eleven items: source IP, server IP, hostname, username, two timestamps, path, size, hash, permissions, and the tools used

## Key takeaway
Evidence written by a server is stronger than evidence reported by the machine under investigation. The Kali log recorded who asked for the file and when, and the downloading machine could not alter it. A successful download only shows that data arrived, and the matching hash shows the data was correct. Doing the transfer over the network was also what produced a real source IP. Creating the file directly on Metasploitable would have left file details but no source address and no independent log.

## Evidence
📄 [Full report with screenshots](./Day-7-SOC-Threat-Simulation-IOC-Collection.pdf)
