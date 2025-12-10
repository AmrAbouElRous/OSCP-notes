# Linux System Hirachy
![Screenshot](img/00.png)
# 🧩 Most Important Linux Directories

```
/
├── bin/
├── boot/
├── dev/
├── etc/
├── home/
├── root/
├── tmp/
├── usr/
└── var/
```

---

### 📁 /bin → **Essential user commands**
Contains the most basic programs needed for all users — even when no other partitions are mounted.

**Examples:**
`ls`, `cat`, `cp`, `mv`, `bash`, `echo`, `grep`

---

### 📁 /boot → **Boot loader and kernel files**
Holds everything the system needs to start up (boot).

**Examples:**
`vmlinuz` (Linux kernel), `initrd.img`, `grub/`

---

### 📁 /dev → **Device files**
Special files that represent hardware or virtual devices.

You can interact with hardware as if it’s a file.

**Examples:**
`/dev/sda` (disk), `/dev/tty` (terminal), `/dev/null` (data sink)

---

### 📁 /etc → **System configuration files**
Contains configuration files for the system and installed software.

**Examples:**
`/etc/passwd` (user accounts), `/etc/ssh/sshd_config` (SSH server config), `/etc/fstab` (filesystem mounts)

---

### 📁 /home → **User home directories**
Each regular user has a folder here for personal files and settings.

**Examples:**
`/home/amro/Downloads`, `/home/amro/.bashrc`

---

### 📁 /root → **Root user’s home directory**
Like `/home/root`, but for the system administrator (root).

**Example:** `/root/.bashrc`

---

### 📁 /tmp → **Temporary files**
Used for temporary data by programs and users.  
Cleared automatically on reboot.

**Example:** `/tmp/test.log`

---

### 📁 /usr → **User programs and data**
Contains most user-level programs, libraries, and documentation.  
Think of it like “Program Files” on Windows.

#### Inside `/usr`

| Directory | Purpose | Examples |
|------------|----------|-----------|
| `/usr/bin` | Main user executables | `python3`, `gcc`, `vim`, `firefox` |
| `/usr/sbin` | System admin tools | `apache2`, `sshd`, `fdisk` |
| `/usr/lib` | Libraries for `/usr/bin` programs | `libc.so`, `libssl.so` |
| `/usr/local/bin` | Manually installed programs (by you) | Custom scripts or tools |
| `/usr/share` | Shared read-only data | Man pages, docs, icons |

---

### 📁 /var → **Variable data**
Contains files that change often — logs, mail, caches, web data, etc.

**Examples:**
- `/var/log/` → system logs  
- `/var/www/` → website files  
- `/var/spool/` → print/mail queues
---
### More Knowledge
```bash
┌──(root㉿amro)-[/]
└─# print $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/games:/usr/games	
```
