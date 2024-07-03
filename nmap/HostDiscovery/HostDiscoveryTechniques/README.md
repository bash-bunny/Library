# Host Discovery Techniques

Techniques to discover multiple hosts with `nmap` like

`nmap -sn -PE -R -v microsoft.com ebay.com citibank.com google.com slashdot.org yahoo.com`

Where
- `-sn` -> Ping Discovery
- `-PE` -> ICMP only ping scan
- `-R` -> Do the inverse DNS resolution although the host isn't alive
- `-v` -> Verbose output

## TCP SYN Ping

It uses the flag `-PS`
It sends a TCP packet with the SYN Flag activated to the port you specified, by default is the port 80

It's possible to modify the port with something like: `-PS22-25,80,113,1050,35000`

## TCP ACK Ping

It uses the flag `-PA`
It sends a TCP packet with the flag ACK active, if the host answers with a RST packet, the host is alive. By default it send the packet to the port 80

It's possible to modify the port with something like: `-PA22-25,80,113,1050,35000`

## UDP Ping

It uses the flag `-PU`
It sends an empty UDP packet to the port you specify, by default is the port 31338

It's possible to modify the port with something like: `-PU22-25,80,113,1050,35000`

## ICMP Ping Types

Nmap can send different types of ICMP packets, to do that there are multiple flags
- `-PE` -> ICMP Echo request (a lot of machines do not answer to this now)
- `-PP` -> Timestamp reply
- `-PM` -> Address Mask reply

## IP Protocol Ping

It uses the flag `-PO`
It sends IP packets with the protocol in the header. The accepted protocols are ICMP, IGMP and IP-in-IP

## ARP Scan

It uses the flag `-PR`
It sends ARP packets to discover hosts in the same network

Examples to compare the speed of both

`nmap -n -sn -PR --packet-trace --send-eth 192.168.30.37`
- `-n` -> Without DNS resolution
- `-sn` -> Without port scanning
- `--packet-trace` -> To see what are the packets sent
- `--send-eth` -> To send raw packets vi ethernet

`nmap -n -sn --send-ip --packet-trace 192.168.30.37`
- `--send-ip` -> Avoid send ARP packets
