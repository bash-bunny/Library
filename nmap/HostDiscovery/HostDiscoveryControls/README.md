# Overview

DNS and Host Discovery with `nmap`

## DNS Resolution

Flags for DNS

- `-n` - Don't make DNS resolution
- `-R` - Do DNS resolution for every host (including those who seem down)
- `--system-dns` - Use the system DNS instead of the name server configured
- `--dns-servers` - Use this DNS servers, it could be useful to scan private networks using their own DNS and the `-sL` option

## Control flags

- `sL` - List Scan. Read the list of host and make a reverse DNS query for each of them
  - `nmap -sL www.stanford.edu/28`
  - `nmap -iL recon.txt -sL`
- `sn` - Ping Scan. Make a ping discovery but don't scan any port. By default it send a ICMP request and ACK packet to the 80 port. In local network it send ARP requests unless `--send-ip` is specified
  - `nmap -sn -T4 www.lwn.net/24`
- `Pn` - Disable Ping Scan. Scan every host without making a Ping discovery before
