# Finding Organizations IP

Multiple techniques to find the IP address of an organization

## DNS

Using the command `host`

```bash
host -t ns target.com
host -t a target.com
host -t aaaa target.com
host -t mx target.com
host -t soa target.com
```

Trying DNS zone transfer to discover more information about the target

```bash
dig @name-server -t axfr target.com
dig @ns2-auth.sprintlink.net -t AXFR target.com
```

Using `nmap` to make a reverse DNS on the target without scanning the ports and `traceroute`

```bash
nmap -Pn -T4 --traceroute www.target.com
```

## Whois

Obtain information about the target with `whois`

```bash
whois IP # Information about any IP
whois -h whois.arin.net\? # Syntax of the whois queries to ARIN database
whois -h whois.arin.net @target.com # Contacts of ARIN with emails in target.com
whois -h whois.arin.net "n target*" # Get all the network blocks that start with target
whois -h whois.arin.net "o target*" # Get all the organizations that start with target
```

## Webpages

- [SearchDNS Netcraft](https://searchdns.netcraft.com/?host)
- [Google](https://www.google.com)
  - `site:target.com`

## BGP Routers

- [ASN Cymru](https://asn.cymru.com/)
  - To query the ASN (Autonomous System)
- [Robtex](https://www.robtex.com/)
  - To resolve the IP address from an ASN

### Telnet

```bash
telnet route-views.routeviews.org
```
