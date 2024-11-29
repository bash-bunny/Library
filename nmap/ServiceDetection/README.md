# Service Detection

Nmap allows you to detect which services are running on the ports scanned with `-sV` and `-A`

## Intensity

You can tell nmap which intensity use to get the version of the ports, by default is 7
- `--version-intensity <0-9>`
  - `nmap -sV --version-intensity 3 scanme.nmap.org`
  - `nmap -sV --version-light scanme.nmap.org` - Version intensity 2
  - `nmap -sV --version-all scanme.nmap.org` - Version intensity 9

It's possible to see how it works with `--version-trace -d`: `nmap -sSV -T4 -F -d --version-trace insecure.org`

## Configuration

It's possible to edit the file `nmap-services-probes` to change how the services are scanned

```text
# It't a comment

# Exclude directive
Exclude 53,T:9100,,U-30000-40000 # Nmap by default avoid ports 9100 and 9107 (printers)

# Probe directive
## Probe <protocol> <probename> <probesendstring>
Probe TCP GetRequest q|GET / HTTP/1.0\r\n\r\n
```
