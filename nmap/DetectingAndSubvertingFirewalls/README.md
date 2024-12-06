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

## Bypassing Firewall Rules

To bypass the firewall rules in order to try to scan the systems behind, `nmap` make uses of certain flags

### Exotic Scans

Those scans uses some exotic or combination of flags:
- `-sF` - FIN Scan
- `-sM` - Maimon Scan
- `-sW` - Window Scan
- `-sN` - Null Scan
- `-sF --scanflags SYN` - Set the flags to the scans

### Source port manipulation

Changing the source port, with `-g 53` or `--source-port 53` could give you more results, because some ports are allowed by default in some firewalls (like DNS port, 53).

```bash
nmap -sS -vv -Pn -g 53 192.168.1.1
```

### IPv6

Most of the time IPv6 isn't filtered on the firewall, so it can be used to bypass it

**Note**: it's necessary that you and the target had an IPv6

```bash
nmap -sS -6 www.kame.net
```

### IP Idle Scan

It allows you so scan other machines or networks using an idle machine, with the flag `-sI`

### Fragmentation

Sometimes you could bypass some firewall rules fragmenting the packets, with the flag `-f` or `--mtu`
- `-f` - Adds 8 to the maximum of packet fragmentation (`-f -f` adds 16)
- `--mtu` - Allows you to specify the maximum data bytes of each fragment as multiple of 8 (it can't be combined with `-f`)

If the fragmented packets reach the destiny, you could use another tool: [fragoroute](https://www.monkey.org/~dugsong/fragroute/)

### Proxies

Those are machines that redirect traffic to another networks or machines, it could be used to bypass firewalls and IDS

### MAC Address Spoofing

Nmap support the option to spoof the mac address, for example to bypass wifi filters, with the option `--spoof-mac`
- `0` - Set a random mac
- `AA:BB:CC:DD:EE:FF` - Use this mac
- `Apple` - Use a random mac of this vendor
- `deadbeefcafe` - Use a random mac of this vendor
- `0020F2` - Use this vendor and randomize the rest
- `Cisco` - Use a cisco mac

**Note**: this implies the option `--send-eth` to send packets to the layer eth

### Source Routing

Nmap allows you to find an alternative route for a network:
- `--ip-options "L 192.168.0.7 192.168.30.9"` - Find an alternative route for this hosts, without being strict
- `--ip-options "S 192.168.0.7 192.168.30.9"` - Find an alternative route for this hosts, being strict, you have to specify every jump

**Note**: if it finds an alternative route, you could try to connect with `netcat` with the option `-g`

### FTP Bounce Scan

Nmap allows you to use the machines that have the ftp port open to scan other machines, using the bounce scan `-b IP`

```bash
nmap -p 22,25,135 -Pn -v -b 192.168.1.1 scanme.nmap.org
```

### Real Example

1. `nmap -n -sn -PE -T4 10.10.10.0/24` - Ping scan with ICPM Echo request (all hosts are dead)
1. `nmap -vv -n -sn -PE -T4 --packet-trace 10.10.10.7` - Use some hosts on the network (one by one) to verify what is happening (all are filtered)
1. `nmap -vv -n -sS -T4 -sn --reason 10.10.10.0/24` - SYN scan with `--reason` to know the reason behind all are dead, if `non responsive` probably it's the sending rate
  1. Scan again to see if the `non responsive` ports are the same, if this is not the case
    1. Probably it's the rate limit, so you have to use a slower scan `-T3` or `-T2`
    1. If it's filtered, you could try another type of scan like `-sA`
1. `nmap -vv -n -Pn -sI 10.10.6.30:445 -p 25 10.10.6.60` - Idle scan to access the target network (you can use the `--packet-trace` to have a better look of what's going on)
1. `nmap -p -sn -PE --ip-options "L 10.10.6.60" --reason 10.10.6.30` - Use the same ping scan but with an alternative route
1. `nmap -p -sn -PE --ip-options "L 10.10.6.60" --reason 10.10.10.7` - The same as above but to the network we cannot reach
