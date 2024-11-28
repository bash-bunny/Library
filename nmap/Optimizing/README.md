# Overview

Different techniques to optimize `nmap` performance

Example: `nmap` as stateless scanner:
- `--min-rate 1000 --max-retries 0`
  - `--min-rate 1000` - Sends 1000 packets per second
  - `--max-retries 0` - Disable the retransmissions
- `--max-rtt-timeout` - Maximum round trip time of 200 milliseconds

## Scan Time Reduction

Techniques to reduce the scan time

- Discover hosts alive: `-sn`
- Limit the number of ports
  - `-F` - scans the top 100 ports
  - `--top-ports 56` - Scans the top 56 more common ports
  - `-p` - Ports number
- `-n` - Disable inverse DNS resolution
- Parallelization - Nmap has it's own parallelization, so most of the time it's better to leave it on it's own

Avoid using advanced scanners
- `-sC`, `-sV`, `-A` - Those take too much time
- `-O` - Avoid OS detection or combine it with other options
  - `--osscan-limit` - Limit the scan of OS to the hosts that has at least one TCP open port and one TCP closed port
  - `--max-os-retries 1` - If it fails one time, it stop trying to guess the host

**Note**: It's better to separate UDP and TCP scans to give them different options and different optimizations (UDP is super slow)

## Low Level Timing Options

- `--min-hostgroup`, `--max-hostgroup` - Minimum and maximum hosts to scan in parallel
- `--min-parallelism`, `--max-parallelism` - Minimum and maximum requests to send in parallel
- `--min-rtt-timeout`, `--max-rtt-timeout`, `--initial-rtt-timeout` - Minimum, maximum and initial response time that nmap waits an answer
- `--max-retries` - Maximum number of retries that nmap tries
- `--host-timeout` - Maximum time to say that a host is dead
- `--scan-delay`, `--max-scan-delay` - Delay between requests send to the same host
- `--min-rate`, `--max-rate` - Minimum and maximum rate of requests send
- `--defeat-rst-ratelimit` - Defeat RST limit that some hosts put on

## Low Timing Templates

These templates modify the Low Level Timing Options to tweak nmap

- `-T0` - Paranoid, used for evade IDS
- `-T1` - Sneaky, used for evade IDS
- `-T2` - Polite, it uses less resources and bandwidth
- `-T3` - Normal, default working mode
- `-T4` - Aggressive, for networks with a big bandwidth or in local networks
- `-T5` - Insane, it sacrifices precision for speed
