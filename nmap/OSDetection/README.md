# Remote OS Detection

Nmap can detect the remote Operating System of a device with the flag `-O` or with `-sV -v` which can give you an idea of the OS

```bash
# Basic
nmap -O -v scanme.nmap.org

# Version
nmap -sV -v scanme.nmap.org

# Combined
nmap -sV -O -v scanme.nmap.org
```

## Example

- Detect Rogue Wireless Access Points

```bash
# -p 1-85,113,443,8080,8100 typicall ports of WAPs
nmap -A -oA wapscan -p 1-85,113,443,8080,8100 -T4 --min-hostgroup 50 --max-rtt-timeout 1000 --initial-rtt-timeout 300 --max-retries 3 --host-timeout 20m --max-scan-delay 1000 192.168.1.0/24
```
