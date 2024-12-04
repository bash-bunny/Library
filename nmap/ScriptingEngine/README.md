# Nmap Scripting Engine

Nmap comes with it's own scripting engine, and also a lot of scripts that anyone could use to enhance their scans
- `-sC` - Launch the default scripts
- `--script` - To launch a particular script or set of scripts, you can define arguments for the script with `--script-args`

## Categories

The scripts are separated within the following categories
- `aut` - For trying credentials
- `default` - Those are the scripts that run with `-sC` or `-A`
  - `identd-owner`
  - `http-auth`
  - `ftp-anon`
- `discovery` - To discover more about the network
  - `html-title`
  - `smb-enum-shares`
  - `snmp-sysdescr`
- `external` - Send data to an external entity, like `whois`
- `intrusive` - Those are scripts that could crash a system
  - `http-open-proxy`
  - `snmp-brute`
- `malware` - Test if the system is infected with malware or backdoors
  - `smtp-strangeport`
  - `auth-spoof`
- `safe` - Those are the group of scripts considered "safe", that is really weird to crash a system with them
  - `ssh-hostkey`
  - `html-title`
- `version` - Scripts to try to figure out the version of the services, it only runs when it's used `-sV`
  - `skypev2-version`
  - `pptp-version`
  - `iax2-version`
- `vuln` - To check typical vulnerabilities on a system
  - `realvnc-auth-bypass`
  - `xampp-default-auth`

## Command Line Arguments

- `-sC` - It's equivalent to `--scripts=default`
- `--script <script-categories> | <directory> | <filename> | all` - To run the scripts we want
- `--script-args` - Arguments for the scripts
- `--script-trace` - It's similar to `--packet-trace`, but it works at application level, not at packet level, it prints all communications
- `--script-updatedb` - To update the nmap database of scripts

## Examples

```bash
# Launch the default scripts with the arguments
## user=foo password=bar and whois with the database of ripe
nmap -sC --script-args user=foo,pass=bar,whois={whodb=nofollow+ripe}

# Launch the script for nmap in the current directory with --script-trace
nmap --script=./showSSHVersion.nse --script-trace example.com

# Launch custom scripts from the category and the scripts that are safe
nmap --script=mycustomscript,safe example.com
```
