# 🧑‍💻 1. Types of Users

| **Type**           | **Description**                                             |
| ------------------- | ----------------------------------------------------------- |
| **Root user**       | Superuser with full control (username: `root`)              |
| **Regular users**   | Created for individuals; limited permissions                |
| **System users**    | Used by system services (e.g., `www-data` for web servers)  |

---

# 🧩 2. Important Files

| **File**             | **Purpose**                                  |
| -------------------- | -------------------------------------------- |
| `/etc/passwd`        | Basic user account information               |
| `/etc/shadow`        | Encrypted passwords & password aging         |
| `/etc/group`         | Group information                            |
| `/etc/sudoers`       | Global sudo permissions configuration        |
| `/etc/sudoers.d/`    | Custom user- or service-specific sudo rules (safe & modular) |

> ⚠️ **Never edit `/etc/sudoers` directly using `nano` or `cat`!**  
> Always use:
> ```bash
> sudo visudo
> ```
> **Why:**  
> - It validates syntax before saving (prevents lockouts).  
> - Much safer than manual editing.

Example of passwordless sudo:
```bash
amro ALL=(ALL:ALL) NOPASSWD: ALL
```

---

# 🧠 3. Useful User Commands

| **Command** | **Description** |
| ------------ | ---------------- |
| `who`        | Show who is logged in |
| `last`       | Display login history |
| `w`          | Show active users and their activities |
| `users`      | List logged-in usernames |

---

# 👤 4. User Management Commands

## ➕ Create a User
```bash
sudo useradd -m -s /bin/bash username
```
Example:
```bash
sudo useradd -m -s /bin/bash msylhy
```

View available options:
```bash
useradd --help
```

---

## 🔑 Set or Change Password
```bash
sudo passwd username
```
Example:
```bash
sudo passwd msylhy
```

---

## 🗑️ Delete a User
```bash
sudo userdel -r username
```
Example:
```bash
sudo userdel -r msylhy
```

For help:
```bash
userdel --help
```

---

## 🔄 Switch Between Users
To switch to another user:
```bash
su username
```
Example:
```bash
su amro
```
To return to your previous user:
```bash
exit
```

> 💡 **Tip:** Use `su - username` to start a full login shell (loads environment variables and home directory).

---

# 🏠 5. User Home & Shell

Each user has:
- **Home directory:** `/home/username`  
- **Default shell:** `/bin/bash`

Modify them with `usermod`:
```bash
sudo usermod -d /home/new_home username     # Change home directory
sudo usermod -s /bin/zsh username           # Change shell
sudo usermod -l newname oldname             # Rename user
```

---

# 🧩 6. Group Management

Groups organize users with similar permissions.

| **Command** | **Description** |
| ------------ | ---------------- |
| `groups`                | Show groups for current user |
| `groups username`       | Show groups for a specific user |
| `sudo groupadd groupname` | Create a new group |
| `sudo usermod -aG groupname username` | Add a user to a group |
| `sudo gpasswd -d username groupname` | Remove a user from a group |
| `cat /etc/group` | View all groups |

Example:
```bash
sudo groupadd developers
sudo usermod -aG developers amro
```

> 🧠 **Tip:** The `-aG` flag appends the user to the group **without removing existing groups**.

---

# 🔐 7. Locking & Unlocking Accounts

Lock a user account:
```bash
sudo usermod -L username
```

Unlock a user account:
```bash
sudo usermod -U username
```

Check if locked:
```bash
sudo passwd -S username
```

---

# 👁️ 8. View Detailed User Info

Display all info about a user:
```bash
id username
```

Example:
```bash
id amro
```
Output shows UID, GID, and group memberships.

Check last password change date:
```bash
chage -l username
```

---

# 🧰 9. Temporary Privilege Escalation (sudo)

Run a command as another user:
```bash
sudo -u username command
```

Example:
```bash
sudo -u www-data whoami
```

List your allowed sudo commands:
```bash
sudo -l
```

---

# 🧩 10. Sudo Configuration Deep Dive

## `/etc/sudoers`
The **main sudo configuration file**.  
Example:
```bash
root ALL=(ALL:ALL) ALL
```
Meaning:  
> The `root` user can run any command as any user on any host.

⚠️ **Never edit directly!**  
Use:
```bash
sudo visudo
```

---

## `/etc/sudoers.d/`
A directory for **modular sudo rules** per user/group.

✅ **Advantages:**
- Keeps main config clean.  
- Easy to manage user-specific permissions.  
- Safer: one small file per user/service.

---

## `visudo`
- Safely edits sudo configuration.  
- Checks syntax before saving.  
- Prevents broken sudo setup.

---

## 🔒 Safe Way to Give Passwordless Sudo

**Step 1 — Create a custom sudo file**
```bash
sudo visudo -f /etc/sudoers.d/amro_custom
```

**Step 2 — Add this line**
```bash
amro ALL=(ALL:ALL) NOPASSWD: ALL
```

**Step 3 — Save and exit**
(Ctrl + O → Enter → Ctrl + X)

**Step 4 — Set secure permissions**
```bash
sudo chown root:root /etc/sudoers.d/amro_custom
sudo chmod 440 /etc/sudoers.d/amro_custom
```

**Step 5 — Test**
```bash
sudo -l
sudo whoami
```

✅ If it shows `root` without a password prompt — success!

---

# 💡 11. Summary Table

| **Item**           | **Description**                        | **Edit Method**                                 | **Safe?** |
| ------------------- | -------------------------------------- | ------------------------------------------------ | ---------- |
| `/etc/sudoers`      | Main sudo configuration                | `sudo visudo`                                   | ⚠️ Risky   |
| `/etc/sudoers.d/`   | User-specific sudo rules               | `sudo visudo -f /etc/sudoers.d/filename`        | ✅ Safe    |
| `visudo`            | Validates syntax before saving         | Used for both                                   | ✅ Always  |
| `NOPASSWD`          | Allows sudo without password           | In `/etc/sudoers.d/customfile`                  | ✅ Safe if intentional |

---

# 🖥️ TTY vs PTS in Linux

## 🔹 TTY (Teletype Terminal)

- Refers to **real text-based consoles** provided by Linux.  
- Switch between them using:

Ctrl + Alt + F1 … F6


- Each **tty1–tty6** is an independent **text-only login screen** (no GUI).  
- **tty7** is usually reserved for the **graphical desktop** (X11/Wayland).  
- You can log in directly using your **username and password** here.

**Example:**  
`/dev/tty1` → User logged in on a physical console.

---

## 🔹 PTS (Pseudo-Terminal Slave)

- Refers to **virtual terminals** created inside your **graphical environment** or via **SSH**.  
- Every time you open a **terminal window**, **new tab**, or **SSH session**, Linux creates a new `/dev/pts/X`.  
- Each PTS is linked to a “master” process that manages input/output in the GUI or over the network.

**Examples:**  

/dev/pts/0 → First terminal window
/dev/pts/1 → Second tab
/dev/pts/2 → SSH session

---

# 🧭 Visual Overview: TTY vs PTS in Linux

Understanding how **real terminals (TTYs)** and **virtual terminals (PTS)** relate inside a Linux system:

```
 ┌──────────────────────────────────────────────────────────────┐
 │                     Linux System                             │
 │                                                              │
 │  ┌───────────────┬───────────────┬───────────────┐           │
 │  │ tty1 (text)   │ tty2 (text)   │ tty3 (text)   │  ← Switch using Ctrl+Alt+F1..F6
 │  │ Login screen  │ Empty console │ Other console │           │
 │  └───────────────┴───────────────┴───────────────┘           │
 │                                                              │
 │  ┌────────────────────────────────────────────────────────┐  │
 │  │ tty7 (graphical session)                               │  │
 │  │  ┌────────────────────────────┐                        │  │
 │  │  │ GUI Terminal → /dev/pts/0  │  ← 1st terminal tab     │  │
 │  │  │ GUI Terminal → /dev/pts/1  │  ← 2nd terminal tab     │  │
 │  │  │ SSH session  → /dev/pts/2  │  ← remote shell         │  │
 │  │  └────────────────────────────┘                        │  │
 │  └────────────────────────────────────────────────────────┘  │
 │                                                              │
 └──────────────────────────────────────────────────────────────┘
```

### 🧠 Key Points

- **`tty1`–`tty6`** → Real text consoles (accessible with `Ctrl + Alt + F1..F6`)
- **`tty7`** → Usually where your graphical desktop (X11/Wayland) runs
- **`/dev/pts/*`** → Virtual terminals inside your GUI (each terminal tab or SSH session)

### 🔍 Quick Checks

```bash
tty         # Show which terminal you’re using
who         # See all logged-in users and their terminals
last        # View historical logins (tty vs pts)
w           # shows who is logged in and what they are doing in real time
```
---


### 🔐 Scenario: User tempest Cannot Access Root While amro Can

Only amro is able to elevate to root using sudo, while tempest cannot.
```bash
┌──(amro㉿amro)-[/home]
└─$ ls                 
amro  tempest
                                                                                                                                                                                                              
┌──(amro㉿amro)-[/home]
└─$ su tempest
Password: 
┌──(tempest㉿amro)-[/home]
└─$ sudo su
[sudo] password for tempest: 
tempest is not in the sudoers file.
                                                                                                                                                                                                            
┌──(tempest㉿amro)-[/home]
└─$ su amro
Password: 
┌──(amro㉿amro)-[/home]
└─$ sudo su
┌──(root㉿amro)-[/home]
└─# 
```
🔎 Checking Group Membership: tempest is not in the sudo group:
```bash
┌──(root㉿amro)-[/home]
└─# groups tempest
tempest : tempest
                                                                                                                                                                                                              
┌──(root㉿amro)-[/home]
└─# groups amro   
amro : amro adm dialout cdrom floppy sudo audio dip video plugdev users netdev bluetooth lpadmin wireshark scanner vboxsf kaboxer

```
🛠️ Adding tempest to the Sudo Group
```bash
┌──(root㉿amro)-[/home]
└─# usermod -aG sudo tempest     

┌──(root㉿amro)-[/home]
└─# groups tempest
tempest : tempest sudo
```
✅ Testing Access
```bash
┌──(root㉿amro)-[/home]
└─# su tempest 
┌──(tempest㉿amro)-[/home]
└─$ sudo su
[sudo] password for tempest: 
┌──(root㉿amro)-[/home]
└─# 

```
