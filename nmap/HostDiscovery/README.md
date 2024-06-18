# Host Discovery

Techniques to discover hosts with `nmap`

- [Finding Organizations IP](FindingOrgIp/README.md)
- [Host Discovery Controls](HostDiscoveryControls/README.md)

## Hosts Options

- `-iL` - Read the hosts from a file
- `-iR` - Select random IPs and scan it (you can find random webpages with this)
  - `0` - Continue forever
  - `Number` - Scans that number of IPs
- `--exclude`, `--excludefile` - Exclude some IPs by stdin or by file
- `-sL` - Only list the hosts and make a reverse DNS discovery (useful to just list what's behind some IPs)
