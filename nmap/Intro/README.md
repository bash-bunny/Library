# Introduccion

Introduction to nmap with little commands to start playing with it

## Reverse DNS Lookup

```bash
nmap -sL 1.1.1.1
```

## Full scan

```bash
nmap -sS -p- -PS22,80,113,33334 -PA80,113,21000 -PU19000 -PE -A -T4 -oA full-scan-tcp 192.168.1.0/24
```

### Flags

- `-sS` SYN Scan
- `-p-` All ports scan
- `-PS` Ping Scan with SYN TCP
- `-PA` Ping Scan with ACK
- `-PU` Ping UDP
- `-PE` ICMP Echo Request
- `-A` Aggressive scan
- `-T4` Timming 4
- `-oA` Store the output of nmap in all the formats
