# Overview

Different techniques to scan ports and how do it the best way

**Note**: `nmap` store statistics about lose packets to make more or less broadcasts

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

## Tweaking Speed and parallelization

Nmap has some options to manage the network conditioning and parallelization, to speed things up.

**Speed and retransmissions**:
- `--min-rate 100` - Minimum rate at which nmap sends packets is 100
- `--max-retries 0` - Nmap is not going to try to re-send packets that fails

**Host and Port Parallelization**:
- `--min-hostgroup` - Define the minimum number of hosts to scan in parallel
- `--min-parallelism` - Defines the minimum number of ports to scan at the same time
- `-T4` - Define a series of options to speed up, it goes from 0 (slowest) to 5 (fastest)
- `--max-rtt-timeout` - Maximum RTT


## Port Scanning Techniques

Different techniques to scan ports with `nmap`

### TCP SYN Scan (-sS)

Default scan in `nmap`, it uses the flag `-sS` and needs a privilege user (usually `root`)

```bash
nmap -p22,113,119 scanme.nmap.org
```

- Port Open
  - Sends a SYN packet
  - Receive a SYN/ACK
  - The OS sends a RST because it receive a SYN/ACK that doesn't expect
- Port Close
  - Send SYN Packet
  - Receive RST packet
- Filtered Port
  - Send SYN Packet
  - Doesn't receive any answer, sends another SYN packet
  - If `nmap` receive ICMP errors like
    - Type 3
    - Codes: 1,2,3,9,10 or 13

### TCP Connect Scan (-sT)

It stablish a full TCP connection with the target, it can be used by non-privileged users. It requires more requests and more time than the SYN Scan, to retrieve the same information, and the machines usually logs this information.

It's better to use the SYN Scan instead of this one, because `nmap` has more control of it.

- Open port
  - Send SYN request
  - Receive SYN/ACK
  - Send ACK
  - Receive the banner of the application
  - Send RST

The rest is the same as the SYN Scan, but it requires twice as much as requests as SYN Scan

### UDP Scan (-sU)

It can be combined with the TCP scan (-sS), and do it at the same time

It sends an empty UDP request to the target ports

- Status
  - Open - Any kind of answer
  - Open | Filtered - No answer after the broadcast
  - Closed - ICMP Port unreachable (type 3, code 3)

#### Open vs Filtered

To distinguish open ports and filtered ports use the Version scan with UDP

```bash
nmap -sUV -F scanme.nmap.org
```

It's possible to use `hping3` with `traceroute`. Launch a `traceroute` to an open port and a closed one, and the difference in hop numbers gives you a hint on getting if it's open or not

```bash
hping3 --udp --traceroute -t 8 -p 53 scanme.nmap.org
hping3 --udp --traceroute -t 8 -p 54 scanme.nmap.org
```

#### Speeding UPD Scans

UPD scans are pretty slow by default but it's possible to speed it up

- Increase host parallelism
  - `--min-hostgroup 100` - Scan 100 hosts at the same time
- Scan popular ports first
  - `-F` - Scans the 100 most popular ports
- Specify the intensity of Version detection
  - `--version-intensity 0` - It makes `nmap` to only try those requests that are more probably to work for a particular port
- Use the timeout to avoid slow hosts
  - `--host-timeout 900000` - 15 minutes of timeout, `nmap` stops scanning that host if it didn't end in that time
- Use the verbose mode to get an approximation of how long is going to take
  - `-v` - Verbose mode

```bash
nmap -sUV -T4 -F --version-intensity 0 scanme.nmap.org -v
```

### TCP FIN, NULL and XMAS

These kind of scans can bypass certain non-stateful firewalls and some routing filters. It works better with Linux machines

**Answers Codes**:
- `filtered` - `ICMP` unreachable error (type 3, code 1,2,3,9,10 or 13)
- `closed` - Answer with `TCP RST` packet
- `open | filtered` - There is no response

#### Null Scan
- `-sN` - TCP Header to 0

#### FIN Scan
- `-sF` - FIN bit to 1

#### Xmas Scan
- `-sX` - Put 1 into the bits of FIN, PSH, URG


### Custom Scan Types

It's possible to customize what flags `nmap` uses with `--scanflags`, for example `--scanflags URGACKPSHRSTSYNFIN` uses all the flags (despite isn't useful)

#### Examples

```bash
# Custom SYN/FIN Scan that can bypass some firewalls
nmap -sS --scanflags SYNFIN -T4 www.google.com

# The same but with URG, PSH or RST
nmap -sS --scanflags SYNURG scanme.nmap.org
nmap -sS --scanflags SYNURGPSHFIN scanme.nmap.org
nmap -sS --scanflags SYNRST scanme.nmap.org

# PSH flag with the FIN, NULL or XMAS scan
nmap -sF --scanflags PSH 192.168.1.1
```

### TCP ACK Scan (-sA)

It's used to map firewall rules and determine if they are stateful or not, and what ports are filtered

**Port States**:
- `unfiltered` - `TCP RST` response
- `filtered` - There is no response or `ICMP unreachable` error (type 3, code 1,2,3,9,10 or 13)

### TCP Window Scan (-sW)

It acts like the `ACK` Scan but to find what ports are open and what are closed, given that open ports answers with a window size more than 0. This scan only works on some machines

**Answers codes**:
- `open` - `TCP RST` with the window size
- `closed` - `TCP RST` with window size 0
- `filtered` - There is no answer or `ICMP unreachable` error (type 3, code 1,2,3,9,10 or 13)

```bash
nmap -sW -T4 scanme.nmap.org
```

### TCP Maimon Scan (-sM)

Sends packets with the flags FIN/ACK, it was useful before, now most of the machines answers with all ports closed

```bash
nmap -sM -T4 scanme.nmap.org
```

### TCP Idle Scan (-sI)

It allows you to scan a machine without sending any packet to that particular machine, it makes use of a "zombie" machine, spoofing their packets. To find a good candidate for a zombie we can use the `-O` and `-v` flags, which gives us the ID (`Incremental | Broken little-endian incremental` are good candidates). By default it uses the port 80, it's possible to change it with `:port`

```bash
# Scan www.ria.com using kiosk.adobe.com without ping to avoid leak our IP
nmap -Pn -p- -sI kiosk.adobe.com www.ria.com
# Change the default port and use the port 113
nmap -Pn -p- -sI kiosk.adobe.com:113 www.ria.com
```

### Ip Protocol Scan (-sO)

It's used to find what kind of protocols (TCP, ICMP, IGMP, etc) supports the target machine. It's usually used to find out what kind of machines are behind, typically uses TCP, UDP and ICMP (maybe IGMP), but *routers* uses GRE and EGP, *firewalls* and *VPNs* uses IPSec and SWIPE

**Answers**:
- `open` - Any response of any protocol
- `closed` - ICMP protocol unreachable error
- `filtered` - Other ICMP errors
- `open|filtered` - There is no response

```bash
nmap -sO 192.168.1.1
```
### TCP FTP Bounce Scan (-b)

It makes use of a machine with ftp to make the scans, the machine with the ftp service sends files to other machines to determine if the ports are open or not. It's useful to bypass firewalls, but it's usually patched, so don't usually works.

```bash
# It tries to connect to the ftp server with the credentials anonymous:-wwwwuser@
nmap -Pn -b ftp.microsoft.com google.com
# It uses the credentials user:password on the ftp.microsoft.com on port 121
nmap -Pn -b user:password@ftp.microsoft.com:121 google.com
```
