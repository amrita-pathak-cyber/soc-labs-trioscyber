# Day 4: TCP Connection Analysis

## Task
Capture a TCP connection to an authorized local service in Wireshark and locate the SYN, SYN-ACK and ACK packets that open it. Record the sequence of events, the source and destination ports involved, and the packets that tear the connection down at the end.

**Tools:** Wireshark 4.6.6, Kali Linux, Python `http.server`, `curl`

## What I did
- Started a local web server with `python3 -m http.server 8000` so the connection target was fully under my control
- Captured on the loopback interface (lo) instead of eth0, because traffic between a machine and itself never reaches the physical adapter
- Sent one request with `curl http://127.0.0.1:8000/` and confirmed the server logged a 200 response
- Saved a capture of exactly 12 packets, all belonging to a single connection
- Opened the TCP header of the handshake packets (SYN, SYN-ACK, ACK) and the teardown packets (FIN, ACK) to check ports, flags, and sequence and acknowledgment numbers
- Checked the raw sequence and acknowledgment numbers against each other, not just Wireshark's relative values

## Key findings
- The handshake used client port 36580 and server port 8000, and every raw acknowledgment number was exactly one higher than the sequence number it answered
- The server closed the connection first, and the teardown took three packets instead of four, because the client combined its ACK and FIN into one packet
- Wireshark showed a starting sequence number of 0, but the raw value on the wire was 1569237085. Real TCP stacks pick random starting numbers on purpose
- No retransmissions, gaps or out-of-order packets appeared anywhere in the capture

## Key takeaway
A healthy TCP connection leaves a clear trail: a handshake, data, then a clean close, with sequence numbers that line up at every step. Knowing what normal looks like is what makes abnormal stand out. A reset before the handshake finishes, retransmissions, or sequence jumps would each point to something wrong. Capturing on the right interface also mattered. Loopback gave 12 clean packets, where a live interface would have buried them in noise.

## Evidence
📄 [Full report with screenshots](./Day-4-TCP-Connection-Analysis.pdf)
