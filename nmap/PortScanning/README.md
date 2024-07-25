# Overview

Explanation of the results of nmap port scanning, and use of multiple techniques for it

## Port States

- Open - The port accept connections
- Closed - The host is alive but that particular port closed
- Filtered - `nmap` don't know if the port is open or closed
- Unfiltered - the port is accessible but `nmap` don't know if its open or closed, it only happens with the ACK scanner (you can solve it with Windows Scan, SYN Scan or Fin Scan)
- Open | Filtered - `nmap` don't know if the port is open or filtered because there is no response, it happens with the UDP, IP protocol, FIN, NULL and XMAS scan
- Closed | Filtered - `nmap` don't know if the port is closed or filtered, it only happens with the Idle Scan
