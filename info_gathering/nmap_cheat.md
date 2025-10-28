# Nmap Cheat Sheet — theory + commands

> An editable, practical Nmap cheat‑sheet: quick theory, most useful flags, and ready-to-run examples. Use it for recon, pentesting labs, and notes. ⚡

---

## Table of contents
1. What is Nmap
2. Installation
3. Basic theory (scans & concepts)
4. Scan types (with short explanation)
5. Host discovery
6. Port specification / ranges
7. Timing & performance
8. Service/version/OS detection
9. NSE (Nmap Scripting Engine)
10. Output options
11. Evasion & stealth (notes)
12. Useful command examples (grouped)
13. Quick reference table
14. Troubleshooting & tips

---

## 1. What is Nmap
Nmap (Network Mapper) is an open-source utility for network discovery and security auditing. Common uses:
- Host discovery (what's online)
- Port scanning (what services/ports are open)
- Service & version detection
- OS fingerprinting
- Scriptable interaction with services (NSE)

---

## 2. Installation
- Debian/Ubuntu: `sudo apt update && sudo apt install nmap`
- Arch: `sudo pacman -S nmap`
- macOS (Homebrew): `brew install nmap`
- Windows: download from nmap.org installer

---

## 3. Basic theory (scans & concepts)
- Open port = service listening on that TCP/UDP port.
- TCP handshake: `SYN -> SYN/ACK -> ACK` (used by connect scans).
- SYN scan (`-sS`) sends SYN and inspects response (stealthy).
- TCP connect scan (`-sT`) completes full handshake (less stealthy).
- UDP scan (`-sU`) sends UDP payloads or empty packets; slow and noisy.
- Version detection (`-sV`) probes services to get exact versions.
- OS detection (`-O`) attempts to fingerprint remote OS.

---

## 4. Primary scan types
- `-sS` — SYN scan (fast, stealthy, requires privileges).
- `-sT` — TCP connect scan (no raw sockets; slower, noisy).
- `-sU` — UDP scan.
- `-sA` — ACK scan (used for firewall/ACL rule discovery).
- `-sN/-sF/-sX` — Null/FIN/Xmas scans (evade some filters; unreliable on modern stacks).
- `-sP` / `-sn` — Ping/host discovery only (do not scan ports).
- `-sV` — Service/version detection.
- `-O` — OS detection.
- `-sC` — Run default scripts (equivalent to `--script=default`).
- `-Pn` — Treat host as up (skip host discovery)

---

## 5. Host discovery (who's up)
- `-sn 192.168.1.0/24` — ping sweep (no ports).
- `-PS80,443` — TCP SYN ping to ports 80 and 443.
- `-PA22,3389` — TCP ACK ping.
- `-PU12345` — UDP ping.
- `-PR` — ARP ping (on local LAN; most reliable for local networks).

Notes: On local networks, ARP is fastest & most reliable. Remote networks often block ICMP & TCP pings.

---

## 6. Port specification / ranges
- Single port: `-p 80`
- Range: `-p 1-1000`
- List: `-p 22,80,443`
- Exclude ports: `-p 1-65535 --exclude-ports 53,123`
- Top ports: `--top-ports 100` (scans most common ports)
- All ports: `-p-` (equivalent to `-p 1-65535`)

---

## 7. Timing & performance
Timing templates `-T0` (paranoid) → `-T5` (insane). Typical choices:
- `-T3` — default conservative.
- `-T4` — faster, OK on reliable networks.
- `-T5` — very fast, noisy; may trigger IDS/IPS.

Useful flags:
- `--min-rate <n>` / `--max-rate <n>` — packets per second rate control.
- `--host-timeout` — stop slow hosts.
- `--scan-delay` / `--max-scan-delay` — delay between probes.

---

## 8. Service/version/OS detection
- `-sV` — probes services to identify application and version.
- `--version-intensity <0-9>` — how many probes to send; default ~7.
- `-O` — enable OS detection (requires multiple probes and privileges).
- `--osscan-guess` — guess OS when fingerprint is not exact.

Combine: `nmap -sS -sV -O target`.

---

## 9. NSE — Nmap Scripting Engine
- Scripts live in `/usr/share/nmap/scripts/` or similar.
- Categories: `auth`, `default`, `discovery`, `intrusive`, `safe`, `vuln`, `exploit`, etc.
- Run default set: `-sC` (equivalent to `--script=default`).
- Run specific script: `--script http-enum` or `--script smb-enum-shares`.
- Use wildcards: `--script 'vuln or auth'` or `--script 'http-*'`.
- Output script arguments: `--script-args=arg1=val1,arg2=val2`

Examples of useful scripts:
- `vuln` category: `smb-vuln-ms17-010`, `http-shellshock`, `ftp-vsftpd-backdoor`.
- `discovery`: `http-enum`, `smb-enum-shares`.
- `ssl`: `ssl-enum-ciphers`, `ssl-cert`.

**Be careful** with `intrusive` or `exploit` scripts — they may crash services.

---

## 10. Output options
- Normal: `-oN file.txt`
- XML: `-oX file.xml`
- Grepable (deprecated but sometimes useful): `-oG file.gnmap`
- All formats: `-oA basename` (produces `basename.nmap`, `.xml`, `.gnmap`)

Tip: Save XML when using tools that ingest Nmap data (e.g., `xsltproc`, Dradis, or parsing scripts).

---

## 11. Evasion & stealth (notes)
- `-sS` + `-T2/3` for stealth.
- `-D RND:10` — decoys (appear as scans from other IPs). Example: `-D 192.168.1.5,ME,10.10.10.10` (`ME` uses your IP).
- `-S <src-ip>` — spoof source IP (requires network routing privileges; replies will go to spoofed IP; usually only works on same LAN and raw sockets).
- `--source-port <port>` — set source port (may bypass simple ACLs).
- `--data-length <n>` — pad probes to change signatures.
- `--fragment` (`-f`) — fragment IP packets (may bypass simple ACLs/IDS). Less effective today.

**Ethics & Legality:** Only use evasion techniques on targets you own or have permission to test.

---

## 12. Useful command examples
### Basic discovery
- Ping sweep LAN:
```bash
nmap -sn 192.168.1.0/24
```

- Quick TCP top ports scan:
```bash
nmap --top-ports 100 -T4 10.0.0.5
```

### Stealthy port scan
```bash
sudo nmap -sS -p 1-1000 -T2 10.10.10.10
```

### Full connect scan (no raw sockets)
```bash
nmap -sT -p- 198.51.100.23
```

### Version + default scripts (service enumeration)
```bash
sudo nmap -sS -sV -sC -p 22,80,443 target.example.com
```

### Aggressive scan (OS, versions, scripts)
```bash
sudo nmap -A -p- 203.0.113.5
# -A == -sV -sC -O --traceroute
```

### UDP scan (slow; run with patience)
```bash
sudo nmap -sU -p 53,69,123 10.0.0.0/24
```

### Scan a list of targets from file
```bash
nmap -iL targets.txt -p 22,80,443 -T4
```

### Output to all formats
```bash
sudo nmap -sS -sV -oA myscan 10.11.1.0/24
```

### Use NSE for vulnerability detection
```bash
sudo nmap --script vuln -p 80,443,445 10.11.1.0/24
```

### Specific NSE script with arguments
```bash
nmap --script http-vuln-cve2014-3704 --script-args="unsafe=1" -p80 target
```

### Use decoys
```bash
sudo nmap -sS -D 192.0.2.1,ME,198.51.100.2 -p 22 target
```

### Fragment packets (old trick)
```bash
sudo nmap -sS -f target
```

---

## 13. Quick reference table

| Action | Flag(s) | Short expl. |
|---|---:|---|
| SYN scan | `-sS` | stealth, needs root |
| TCP connect | `-sT` | no raw sockets |
| UDP scan | `-sU` | slow |
| Ping only | `-sn` | host discovery |
| Version detect | `-sV` | service probe |
| OS detect | `-O` | fingerprinting |
| Default scripts | `-sC` | common NSE scripts |
| Specific scripts | `--script` | run NSE scripts |
| All output | `-oA <name>` | nmap, xml, gnmap |
| Aggressive | `-A` | sV + O + script + traceroute |
| Skip discovery | `-Pn` | assume host up |
| Top ports | `--top-ports N` | fastest common ports |
| All ports | `-p-` | scan 1-65535 |
| Timing template | `-T0..-T5` | slower -> faster |

---

## 14. Troubleshooting & tips
- If many hosts show as down, try `-Pn` to skip host discovery — might be ICMP/TCP filtered.
- UDP scans are slow — start with `--top-ports 50` or targeted UDP ports (53,67,68,123,161,500,4500).
- If `-O` or `-sS` fails due to privileges, run with `sudo` or as root.
- Use `--packet-trace` to debug the exact packets Nmap is sending/receiving.
- Update Nmap scripts and signatures: `sudo nmap --script-updatedb` (if available) or update package.
- When parsing output in scripts, prefer XML (`-oX`) or grepable (`-oG`) for automation.

---

## 15. Further reading
- `man nmap` — comprehensive reference.
- https://nmap.org/book/man.html — official manual & book.
- `/usr/share/nmap/scripts/` — read NSE scripts to learn usage and script-args.

---

## Editable notes
Add your lab-specific commands, favorite script lists, or personal flags below.

- Example: my common scan (`alias`):
```bash
alias nmapfast='nmap -sS --top-ports 200 -T4'
```


<!-- EOF -->

