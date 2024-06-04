# Introduccion

Introduccion a nmap con pequeños comandos para su manejo

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
- `-p-` Escaneo de todos los puertos
- `-PS` Ping Scan con SYN TCP
- `-PA` Ping Scan con ACK
- `-PU` Ping UDP
- `-PE` ICMP Echo Request
- `-A` Aggressive scan
- `-T4` Timming 4
- `-oA` Guardar todos los tipos de ficheros de la salida de nmap
