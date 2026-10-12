# Day 3: Packet Capture and Protocol Identification with Wireshark

## Task
Capture live lab traffic in Wireshark while performing a DNS lookup and visiting an HTTP page and an HTTPS page. Filter the capture down to DNS, TCP and HTTP/HTTPS traffic, then identify source and destination addresses, ports and protocol behavior for each.

**Tools:** Wireshark 4.6.6, Kali Linux, `nslookup`, `dig`, Firefox

## What I did
- Started a Wireshark capture on the Kali eth0 interface and kept it running for the whole exercise, ending at 8,785 packets saved as a .pcapng file
- Generated DNS traffic with `nslookup` and `dig` against example.com
- Generated web traffic by visiting http://neverssl.com (plain HTTP) and https://example.com (HTTPS)
- Applied four display filters and measured each: DNS 126 packets (1.4%), TCP 8,365 (95.2%), HTTP 8 (0.1%), TLS 1,646 (18.7%)
- Used Follow HTTP Stream to read the full neverssl.com request and response in plain text, then compared it with the unreadable bytes of a TLS Application Data packet
- Recorded each protocol's transport, ports and behavior in a summary table

## Key findings
- HTTP exposed everything: the request line, headers and full page HTML. TLS exposed only the handshake before switching to unreadable data
- The TLS Client Hello still named the destination hostname in the clear (SNI)
- Much of the traffic was Firefox background activity: a connectivity check, OCSP certificate checks over plain HTTP, and several background HTTPS connections
- Every TCP connection started with a complete three-way handshake, and DNS returned several valid addresses for one name

## Key takeaway
Encryption protects what was said, not who was spoken to. Even with TLS, an observer on the network can still see which hostnames a device connected to, so the destination is visible even when the content is not. The capture also showed how little of the traffic was deliberate. Most packets were background noise, and separating that noise from the traffic under investigation is the first job in any real packet analysis.

## Evidence
📄 [Full report with screenshots](./Day-3-Packet-Filter-and-Protocol-Identification.pdf)
