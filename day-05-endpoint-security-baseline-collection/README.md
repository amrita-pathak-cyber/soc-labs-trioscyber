# Day 5: Endpoint Security Baseline Collection

## Task
Collect a basic security baseline from a lab endpoint: hostname, current user, running processes, active network connections, and startup/services. Save the actual command output, not just screenshots, so it can be compared against future output during a real alert investigation.

**Tools:** Kali Linux, `hostname`, `whoami`, `id`, `ps`, `ss`, `systemctl`

## What I did
- Chose Kali Linux as the endpoint and saved every command's output to text files in a `day5_baseline` folder
- Recorded identity with `hostname`, `whoami` and `id`. The account is `kali` (UID 1000) and belongs to the `sudo` group
- Captured every running process with `ps aux`
- Captured network state in two passes with `ss`: first listening ports only, then all sockets including established connections
- Captured services in two passes with `systemctl`: what is running now, and what is enabled to start on boot
- Checked the saved files with `ls -la` to confirm each one had real content

## Key findings
- Zero listening ports, and exactly one established connection: the DHCP lease between Kali's NetworkManager and the VMware DHCP service (port 68 to 192.168.84.254 port 67)
- 20 running services and 18 enabled services, all standard Kali infrastructure
- Most of the process list is kernel worker threads, which show up in square brackets and are owned by root
- My first attempt at the hostname and user commands failed because I chained them on one line, and `hostname` treated `whoami` as a new name to set. It left two empty files, which I caught and re-ran

## Key takeaway
A baseline is only useful if it is checked. A file that exists but is empty is not evidence, and the empty files from my first attempt proved it. The finished baseline covers four stages of a real compromise: identity, execution, outbound communication and persistence. A clean network baseline helps most. With zero listening ports, any new open port or unfamiliar connection in a future investigation would stand out immediately.

## Evidence
📄 [Full report with screenshots](./Day-5-Endpoint-Security-Baseline-Collection.pdf)
