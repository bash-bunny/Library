# Combining Strategies

Is it possible to combine the options for Host Discovery to try to discover all hosts that are UP

## Advanced Options

Some advanced options for discovering hosts with `nmap`

- `-v` - Verbose
- `--source-port (-g)` - It allows you to choose the source port and bypass some firewalls
- `-n` - Disable inverse DNS resolution
- `-R` - Enable inverse DNS resolution for all hosts (included those that are down)
- `--dns-servers` - Servers for DNS resolution
- `--data-length <length>` - Adds bytes of random data to each packet, it works with TCP, UDP and ICMP to bypass some IDS and firewalls (some of them mark as suspicious packets with length 0)
  - Put ICMP with 32 bytes to emulate Windows
  - Put ICMP with 56 bytes to emulate Linux
- `--ttl <val>` - It's possible to change the TTL of the packets
- Timing Options - To modify how fast should go nmap
  - `-T0` - `-T5` - From 0 to 5, 0 slowest and 5 fastest, between `T3` and `T4` it's OK to discover hosts
- `--max-parallelism`, `--min-parallelism` - How many packets are launch at the same time
- `--min-rtt-timeout`, `--max-rtt-timeout`, `--initial-rtt-timeout` - How much time `nmap` waits for a response
- `--randomize-hosts` - Scans the hosts in random order
- `--reason` - `nmap` shows the reason why that particular host is UP
- `--packet-trace` - Shows every packet send and received by `nmap`
- `-D <decoy1,decoy2,..>` - Sends scans from spoofed IP Address
- `-6` - Discovery by IPv6
- `-S <source IP address>`, `-e <sending device name>` - Change the source IP and name of the device

## Combining Options

Combination of all the techniques to discover hosts

Ports with more probability of access (being open or closed), in order of probability:
- 80/http
- 25/smtp
- 22/ssh
- 443/https
- 21/ftp
- 113/auth
- 23/telnet
- 53/domain
- 554/rtsp
- 3389/ms-term-server
- 1723/pptp
- 389/ldap
- 636/ldapssl
- 256/FW1-securemote

And it's usually recommended to choose a high port (above 1024 privilege ports), because some firewall only filter privilege ports. For UDP use the 53 port and a high unused port like 37542

### Discover

Ideal combination for discovering hosts `-PE -PP -PS21,22,23,25,90,113,31339 -PA80,113,443,10042 --source-port 53`

### Example

```bash
# Generate random 50000 hosts
nmap -n -sL -iR 50000 -oN - | grep -i "scan report" | awk '{print $5}' | sort -n > 50K_IPs

# Basic Discovery
nmap -sn -T4 -iL 50K_IPs

# Advanced Discovery
nmap -sn -PE -PP -PS21,22,23,25,80,113,31339 -PA 80,113,443,10042 -T4 --source-port 53 -iL 50K_IPs -oA 50KHosts_ExtendedPing
```
