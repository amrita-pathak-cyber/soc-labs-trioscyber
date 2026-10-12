# Day 2: Nmap Scanning and Service Discovery

## Task
Run an Nmap scan only against your own lab VM. Identify live hosts, open ports and detected services. Compare the discovered services with the services intentionally enabled and note any unexpected exposure.

**Tools:** Nmap 7.99, Kali Linux, Metasploitable 2, VMware Workstation Pro

## What I did
- Imported Metasploitable 2 into VMware from its pre-built .vmx file and read its IP address (192.168.84.130) with `ifconfig`. It sits on the same NAT network as Kali (192.168.84.0/24)
- Confirmed Kali could reach the target with ping before any scanning began
- Ran a ping sweep with `nmap -sn` and found 5 live hosts. Only .130 was a real target, and the other four were Kali and VMware's own network services
- Ran a full 65,535-port TCP sweep with `nmap -p- -T4` and found 30 open ports, far more than a default top-1000 scan would show
- Ran `nmap -sV -sC` on those ports to identify service versions, and saved the output to a file
- Built a baseline table of every discovered service, marked each as expected for the lab, and rated how risky it would be on a production host

## Key findings
- **Critical:** the vsftpd 2.3.4 trojaned backdoor (CVE-2011-2523), an unauthenticated root shell on port 1524, the UnrealIRCd 3.2.8.1 trojaned backdoor (CVE-2010-2075), and distccd remote code execution (CVE-2004-2687)
- **High:** anonymous FTP login, plaintext Telnet and r-services, SMTP with SSLv2 and export-grade ciphers, and SMB with signing disabled

## Key takeaway
Every service on this host was there by design, so nothing counted as unauthorized. The real work was judging severity. Two of the critical findings, vsftpd and UnrealIRCd, were backdoors planted in the downloadable software itself. The version banner was the only warning sign, and no configuration change could fix them. On a real host, a root shell like port 1524 means isolate the machine first and open a ticket after.

## Evidence
📄 [Full report with screenshots](./Day-2-Nmap-Scanning-and-Service-Discovery.pdf)
