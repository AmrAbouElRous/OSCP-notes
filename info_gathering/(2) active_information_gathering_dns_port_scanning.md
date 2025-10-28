# Active Information Gathering — DNS & Port Scanning

---

## Quick overview

Active information gathering collects data by interacting with target systems (DNS queries, port scans, banner grabs, etc.). This document focuses on DNS enumeration and port-scanning basics you noted — with practical examples, tips, and learning exercises.

---

## 1) DNS enumeration

DNS enumeration discovers DNS records, subdomains, and related infrastructure. Common manual tools:

- `dig` — powerful, scriptable DNS lookup tool.

  - Examples:
    - `dig example.com A`  # IPv4
    - `dig example.com MX` # mail servers
    - `dig example.com ANY` # all available records (use carefully)
    - Reverse lookup: `dig -x 1.2.3.4` or `dig -x 4.3.2.1.in-addr.arpa PTR`

- `host` — simple lookup utility.

  - `host -t mx example.com`

- `nslookup` — interactive or single-shot lookups (legacy but still useful).

  - `nslookup -type=ns example.com`

**Notes:**

- `dig` is preferred for scripting and parsing because of its flexible flags (e.g., `+short`, `+noall`, `+answer`).
- DNS can reveal mail servers, name servers, TXT records (SPF, DKIM), CNAMEs, and potential subdomain targets.

---

## 2) Automated DNS enumeration tools

**Quick one-liners:**

```bash
# Reverse lookups from list of IPs
for ip in $(cat ips.txt); do dig -x "$ip" +short; done

# Subdomain A record lookups
for sub in $(cat subs.txt); do dig "$sub.example.com" A +short; done
```

**Example commands:**

```bash
# Full DNS reconnaissance with dnsrecon
dnsrecon -d example.com -a   # Performs multiple DNS checks (A, MX, NS, zone transfer, etc.)

dnsrecon -d example.com -t axfr   # Attempts zone transfer (if allowed)

dnsrecon -d example.com -D /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t brt  # Brute-force subdomains

# Enumerate with dnsenum
dnsenum example.com  # Basic enumeration

dnsenum -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt example.com  # Brute-force mode
```

Automated DNS tools speed up repetitive enumeration tasks (zone transfers, brute-force subdomain discovery, reverse lookups, WHOIS, etc.) so you can cover more ground reliably. Use them to automate lookups from wordlists and to combine passive OSINT with active queries.

- `dnsrecon` — performs zone transfers, brute-force subdomain discovery, and various DNS record checks.
- `dnsenum` — automates subdomain brute-force (wordlists), reverse lookups, and WHOIS lookups.
-

---

## 3) Port scanning tools

- `nc` (netcat) — quick TCP/UDP connectivity checks, banner grabbing, and simple listeners.
  - Example: `nc -vn 10.0.0.5 80`  # connect to port 80 verbosely
- `nmap` — the industry-standard network scanner (discovery, port/service detection, OS detection, scripts).

---

## 4) Nmap scan types (cheat sheet)

- `-sS` — TCP SYN (half-open) scan (stealthy). Sends SYN, waits for SYN/ACK or RST; sends RST instead of completing handshake.
- `-sT` — TCP Connect scan (completes full TCP connection using OS connect()). Less stealthy, but works without raw sockets.
- `-sU` — UDP scan. Slower and trickier; many UDP services drop packets silently.
- `-sn` — Ping scan (host discovery only, no port scan). By default uses ICMP/ARP/other probes depending on privileges.
- `-O` — Enable OS detection.
- `--osscan-guess` — Make aggressive guesses when OS detection is inconclusive.

### Example commands

- Quick stealth scan: `nmap -sS -p 1-65535 10.0.0.0/24`
- UDP plus TCP: `nmap -sS -sU -p U:53,161,T:22,80 10.0.0.1`
- Ping only (no ports): `nmap -sn 10.0.0.0/24`
- OS detection: `nmap -O --osscan-guess 10.0.0.1`

---

## 5) How the SYN scan works — step-by-step

1. **Initiation:** Nmap sends a TCP packet with the **SYN** flag set to a target port — mimicking the first step of a normal TCP handshake.
2. **Open port:** If a service is listening, the target responds with **SYN/ACK**. Nmap records the port as **open** and immediately sends a **RST** (reset) to tear down the connection — this avoids completing the three-way handshake. This is why it’s called a “half-open” scan.
3. **Closed port:** If nothing is listening, the target responds with **RST**. Nmap records the port as **closed**.
4. **Filtered port:** If a firewall or filtering device drops the packet (no response) or sends an ICMP unreachable, Nmap marks the port as **filtered** after retries.

**Why use SYN scan?** Faster and stealthier than a full connect scan, often bypasses simple logging, and minimizes side effects on the target.

---

## 6) ICMP / ping scan

- `-sn` triggers host discovery. One method is an ICMP echo request (ping sweep).
- If a host replies with ICMP echo reply, it’s up. Firewalls or host-based filtering often block ICMP, causing false negatives.

**Notes:**

- Use multiple discovery methods (`-Pn`, ARP discovery on local networks) if ICMP is blocked.
- Example: `nmap -sn -PE 10.0.0.0/24`  (`-PE` forces ICMP echo requests)

---

## 7) Nmap NSE scripts

Nmap scripting engine (NSE) extends nmap with scripts for service discovery, vulnerability checks, enumeration, and more.

- Script location (typical): `/usr/share/nmap/scripts`
- Run a single script: `nmap -v --script http-headers 149.56.244.87`
- Run a category: `nmap --script vuln 10.0.0.1`

**Useful categories:**

- `default` — general useful checks
- `discovery` — additional discovery probes
- `vuln` — vulnerability checks (use carefully)

---

## 8) Practical tips & common flags

- Use `-v` or `-vv` for verbose output when debugging scans.
- Add `-oA <basename>` to save outputs in all formats (`.nmap`, `.gnmap`, `.xml`).
- Limit speed with `-T2`/`-T3` for stealth, speed up with `-T4`/`-T5` (risky/noisy).
- Use `--reason` to get reasons for port states (useful for learning).
- For parsing in scripts, use `-oG -` (grepable) or `-oX` (XML).
- Respect the scope: only scan systems you own or have explicit permission to test.

---

## 9) Learning exercises (practice tasks)

1. Use `dig` to enumerate records for a domain you control; find A, MX, NS, TXT, and SOA records.
2. Run `dnsrecon` with a small wordlist and compare passive vs active discovery results.
3. On your lab network, run `nmap -sn` vs `nmap -Pn -sn` and observe differences.
4. Perform `-sS` and `-sT` scans against an intentionally vulnerable VM and note differences in logs and outputs.
5. Explore a few NSE `discovery` scripts and document what extra info they provide.

---

## 10) Resources & wordlists

- SecLists (subdomains): `/usr/share/seclists/Discovery/DNS/` or clone from GitHub.
- Nmap official docs & NSE script reference: `https://nmap.org` (read the docs regularly).
- `dnsrecon` and `dnsenum` man pages and GitHub repos.

---

## 11) Short cheat-sheet (copyable)

```
# DNS
dig example.com A
dig -x 1.2.3.4
host -t mx example.com
nslookup -type=ns example.com

# DNS tools
dnsrecon -d example.com -a
dnsenum -f /path/to/subs.txt example.com

# nmap
nmap -sS -p 1-65535 10.0.0.1
nmap -sU -p 53,161 10.0.0.1
nmap -sn 10.0.0.0/24
nmap -O --osscan-guess 10.0.0.1
nmap -v --script http-headers 149.56.244.87

# netcat
nc -vn 10.0.0.1 80

# file locations (examples)
/usr/share/nmap/scripts
/usr/share/seclists/Discovery/DNS
```

