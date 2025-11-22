# 🌐 DNS and PTR Record Lookup Guide

## 🔹 1. Using `dig` (Powerful & Detailed)

### 🎯 Specific Record Types
```bash
dig eelu.edu.eg A       # IPv4 address
dig eelu.edu.eg AAAA    # IPv6 address
dig eelu.edu.eg MX      # Mail servers
dig eelu.edu.eg NS      # Name servers
dig eelu.edu.eg TXT     # TXT records (SPF, verification, etc.)
dig eelu.edu.eg CNAME   # Canonical name (alias)
dig -x 102.223.94.154   # PTR (Reverse DNS → IP to Domain)
```

### 📦 Get All Record Types
```bash
dig eelu.edu.eg ANY
```

### 🌐 Use a Specific DNS Server (e.g., Cloudflare,Google)
```bash
# Cloudflare DNS resolver
dig @1.1.1.1 eelu.edu.eg ANY
# Google DNS resolver
dig @8.8.8.8 megacorpone.com A +noall +answer
```

### 🧹 Clean Output (Answers Only)
```bash
dig eelu.edu.eg ANY +noall +answer
```

### 🧩 Automatically Get IP → PTR
```bash
IP=$(dig +short eelu.edu.eg)
dig -x $IP +short
```

---

## 🔹 2. Using `host` (Simple & Fast)
```bash
host -t A eelu.edu.eg        # IPv4 address
host -t AAAA eelu.edu.eg     # IPv6 address
host -t MX eelu.edu.eg       # Mail servers
host -t NS eelu.edu.eg       # Name servers
host -t TXT eelu.edu.eg      # TXT records (SPF, verification, etc.)
host -t CNAME eelu.edu.eg    # Canonical name (alias)
host 102.223.94.154          # PTR (Reverse DNS → IP to Domain)
```

---

## 🔹 3. Using `nslookup` (Classic Tool)
```bash
nslookup eelu.edu.eg          # Default lookup (A record)
nslookup -query=A eelu.edu.eg # IPv4 address
nslookup -query=AAAA eelu.edu.eg # IPv6 address
nslookup -query=MX eelu.edu.eg   # Mail servers
nslookup -query=NS eelu.edu.eg   # Name servers
nslookup -query=TXT eelu.edu.eg  # TXT records
nslookup -query=CNAME eelu.edu.eg # Canonical name (alias)
nslookup 102.223.94.154          # PTR (Reverse DNS → IP to Domain)
```

### 🧠 Interactive Mode
```bash
nslookup
> set type=MX
> eelu.edu.eg
> set type=PTR
> 102.223.94.154
> exit
```
---
### Additional
8.8.8.8 is not the IP address for Google’s website — it’s the IP address of Google Public DNS, a DNS resolver service provided by Google.
```
┌──(amro㉿amro)-[~]
└─$ dig google.com A +noall +answer
google.com.             267     IN      A       142.250.200.206

┌──(amro㉿amro)-[~]
└─$ dig @8.8.8.8 google.com A +noall +answer     
google.com.             300     IN      A       142.250.200.206
                                                                       
┌──(amro㉿amro)-[~]
└─$ dig @1.1.1.1 google.com A +noall +answer
google.com.             205     IN      A       142.251.37.238

┌──(amro㉿amro)-[~]
└─$ whois 142.250.200.206 | grep -E 'OrgName|Organization'
whois 142.251.37.238 | grep -E 'OrgName|Organization'

Organization:   Google LLC (GOGL)
OrgName:        Google LLC
Organization:   Google LLC (GOGL)
OrgName:        Google LLC
```
-**Different IPs** for the same domain (like google.com) are usually because of load balancing, specifically DNS-based load balancing (also called GeoDNS or Anycast routing).
🌍 GeoDNS (Geographical Load Balancing)
Each DNS resolver (Google, Cloudflare, etc.) may give you an IP of the nearest data center based on your location or their server’s location.

⚡ Load Distribution
Large companies like Google use multiple servers worldwide. DNS returns different IPs so that not all users hit the same server.

🔁 Redundancy / Failover
If one data center or IP is down, DNS automatically returns another available one — ensuring reliability.

🧠 Anycast Routing (used by big providers)
A single IP (like 8.8.8.8) is actually used by many servers globally. Your request automatically goes to the nearest node.


✅ **Tip:** Use `+short` with `dig` for simplified outputs.

