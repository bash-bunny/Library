# Detecting and Subverting Firewalls and IDS

Nmap is capable of detecting and subverting firewalls and IDS rules, to scan the systems behind

## Determining Firewall Rules

The default scanner (`-sS`) in `nmap` detects between open, closed and filtered ports

```bash
nmap -sS -T4 scanme.nmap.org
```

### ACK Scan

It says what ports are filtered and those who are not

```bash
nmap -sA -T4 scanme.nmap.org
```

We can differentiate between stateless and stateful firewalls:
- Stateless - They don't store the state of the connections
- Stateful - They store the state of the connections

To differentiate we can combine the `-sS` scan with the `-sA`:
- If both of them show the same open/closed and unfiltered ports, it's probably stateful
- If one of them shows more unfiltered ports, it's probably stateless

### IP ID Tricks

We could use the information contained in the ID of the IP address, it can be used for:
- Idle Scanning
- Detect when the firewall is forging RST packets
- Detect which IP addresses could go through the firewall

To test it we need
- `nmap`
- `hping3`

#### Steps

- We need a machine in the internal network with at least one port accessible from outside (open or closed)
  - Routers, Printers or Windows Machines (Linux doesn't work so well)
  - The machine has to have little traffic
- Verify with `hping3` that the machine has an ID predictable (for example it increments one by one)
  - `hping3 -c 5 -i 1 -p 80 -S 192.168.1.30`
    - `-c 5` - Send 5 packets
    - `-i 1` - Waits 1 second between packet and packet
    - `-p 80` - Port 80 to send the packets
    - `-S` - Send SYN Packets
- Flood the target machine with packets
  - `hping3 --spoof 192.168.1.222 --fast -p 80 -c 10000 -S 192.168.1.1`
    - `--spoof 192.168.1.222` - Spoof the IP address `192.168.1.222`
    - `--fast` - To send packets quickly
    - `-p 80` - Port 80 to send the packets
    - `-c 10000` - Send 100000 packets
    - `-S` - Send SYN Packets
- The objective is to increase the PID of the machine, it's necessary to use an IP address close to the target
- Repeat the test before sending the 5 packets to the machine to see if the PID increases
  - If it increases more than 1 per second, that PID isn't unique (there are some machines that does that) and that works too
- Repeat the last step with IP addresses that could go through the firewall, some private directions like `10.0.0.0/8`, `192.168.0.0/16`, `172.16.0.0/12`, and test local host and other local IPs `127.0.0.1`, `127.0.0.0/8` (this one to detect if the IP `172.0.0.1` it's hardcoded)

With all of that you can get a list of IP addresses which are permitted through the firewall, that could be useful to exploit trusted relationships or as a decoys (`-D`) that could go through the firewall

### UDP Version Scanning

UDP cannot differentiate between open and filtered ports, so to avoid that you can use the version flag (`-V`)

```bash
nmap -sV -sU -p50-59 scanme.nmap.org
```
