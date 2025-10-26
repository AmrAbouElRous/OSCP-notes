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

### 🌐 Use a Specific DNS Server (e.g., Cloudflare)
```bash
dig @1.1.1.1 eelu.edu.eg ANY
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

✅ **Tip:** Use `+short` with `dig` for simplified outputs.

