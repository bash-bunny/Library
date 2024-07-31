# Overview

Different techniques to scan ports and how do it the best way

## Port States

- Open - The port accept connections
- Closed - The host is alive but that particular port closed
- Filtered - `nmap` don't know if the port is open or closed
- Unfiltered - the port is accessible but `nmap` don't know if its open or closed, it only happens with the ACK scanner (you can solve it with Windows Scan, SYN Scan or Fin Scan)
- Open | Filtered - `nmap` don't know if the port is open or filtered because there is no response, it happens with the UDP, IP protocol, FIN, NULL and XMAS scan
- Closed | Filtered - `nmap` don't know if the port is closed or filtered, it only happens with the Idle Scan

## Quick Scan

Basic scan for a domain or IP address, it does:
- Make an inverse DNS resolution if it's an IP address to get the hostname or get the IP via DNS if it's a hostname
- Make a ping to the target to see if it's alive
  - ICMP echo request and ICMP timestamp
  - TCP ACK to the ports 80 and 443
- Launch a TCP scan to the 1000 more common ports (listed in nmap-services)
- Print the results

```bash
# nmap <target>
nmap example.com
nmap 8.8.8.8
```

More complex scan:
- `-p0-` - Scan ports from 0 to 65535 (all ports)
- `-v` - Verbose
- `-A`- Aggressive Mode (Operating System, Port version, traceroute and default scripts)

```bash
nmap -p0- -v -A -T4 scanme.nmap.org
```

## Port Scan Flags

All flags to scan ports and what is their use. All of them starts with `-s` except the *bounce* scan

- `-sS` - Syn Scan (TCP scan with the SYN flag active). It's "more" stealthy than the TCP Scan
- `-sT` - TCP Connect. It makes a full connection with the target completing the three way handshake
- `-sU` - UDP Ports Scan
- `-sF`, `-sX`, `-sN` - TCP FIN, Xmas and Null Scan. They are useful to bypass firewalls and explore the systems behind them
- `-sA` - TCP ACK. It's useful to map firewall rules and in particular if the firewall are stateful or not
- `-sW` - TCP Window. It's like the ACK scan but it can detect open ports in certain machines
- `-sM` - TCP Maimon. To avoid firewalls, it's similar to the FIN scan but also includes the ACK flag. It allows you to bypass certain firewalls
- `-sI` - TCP Idle. The stealthiest scan that exploits trust relationships, but is really slow and complex
- `-sO` - IP protocol. It doesn't scan ports, it scan protocols that are allowed in the target (ICPM, TCP, IGMP, etc)
- `-b` - TCP FTP bounce. It tries to trick the FTP servers to perform scans with proxies

## IPv6 Scanning

Nmap allows you to scan IPv6 with the flag `-6`. It works the same as the rest of the scans, but the source and target must be configured to use IPv6

```bash
nmap -6 -sV www.eurov6.org
```
