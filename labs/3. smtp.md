# SMTP Exploitation — OSCP Lab Notes (Metasploitable2)

> Compact, actionable checklist and commands for SMTP enumeration and exploitation against a Metasploitable2 target. Update IPs if your lab uses different addresses.

---

## 0. Lab context (added)
- **Target:** Metasploitable2 VM
- **SMTP target IP (lab):** `192.168.1.4` (**Read the previous md file to know how to get that ip**)
- **SSH access to VM (lab example):** `ssh msfadmin@192.168.1.4` — use the IP you actually SSH to in your environment. I included both IPs because your lab network may expose services on different addresses; replace with the correct one if needed.
---

## Ports to check
- **Submission/SMTP:** `25`, `587`, `465` (465 is SMTPS)
```
┌──(amro㉿amro)-[~]
└─$ nmap 192.168.1.4
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
and so on ...
```
---
## User enumeration (Metasploitable2 examples)
There are many tools for enumetaring users of SMTP
- smtp-user-enum -M VRFY -U users.txt -t 192.168.1.4
- nmap -v -p 25 --script smtp-enum-users --script-args smtp-enum-users.userdb=users.txt 192.168.1.4
> /usr/share/nmap/scripts 
> /usr/local/share/seclists



```bash
┌──(amro㉿amro)-[~]
└─$ smtp-user-enum -M VRFY -U /usr/local/share/seclists/Usernames/top-usernames-shortlist.txt -t 192.168.1.4 
Starting smtp-user-enum v1.2 ( http://pentestmonkey.net/tools/smtp-user-enum )

 ----------------------------------------------------------
|                   Scan Information                       |
 ----------------------------------------------------------

Mode ..................... VRFY
Worker Processes ......... 5
Usernames file ........... /usr/local/share/seclists/Usernames/top-usernames-shortlist.txt
Target count ............. 1
Username count ........... 17
Target TCP port .......... 25
Query timeout ............ 5 secs
Target domain ............ 

######## Scan started at Wed Nov  5 21:21:54 2025 #########
192.168.1.4: root exists
192.168.1.4: user exists
192.168.1.4: mysql exists
192.168.1.4: ftp exists
######## Scan completed at Wed Nov  5 21:21:57 2025 #########
4 results.

17 queries in 3 seconds (5.7 queries / sec)

```

Manual checks using `nc`/`telnet` (example session):
```
nc 192.168.1.4 25
HELO attacker
VRFY root
EXPN postmaster
RCPT TO:<root@metasploitable.localdomain>
```

Record any valid users discovered (Metasploitable2 often responds positively to some VRFY/RCPT tests).

---

## 5. Sending mail manually & verifying delivery (use SSH to confirm)

```bash
┌──(amro㉿amro)-[~]
└─$ telnet 192.168.1.4 25
Trying 192.168.1.4...
Connected to 192.168.1.4.
Escape character is '^]'.
220 metasploitable.localdomain ESMTP Postfix (Ubuntu)
HELO attacker
250 metasploitable.localdomain
MAIL FROM:<Attacker@gmail.com>
250 2.1.0 Ok
RCPT TO:<root@metasploitable.localdomain>
250 2.1.5 Ok
DATA
354 End data with <CR><LF>.<CR><LF>
Subject: test from attacker
i am attcker
.
250 2.0.0 Ok: queued as 10A2ECBB9
Quit
```

# On your machine, SSH into the Metasploitable2 VM to confirm delivery
ssh msfadmin@192.168.1.4
```bash
ssh msfadmin@192.168.1.4
msfadmin@metasploitable:~$ cd /var/mail && ls
msfadmin  root  user
msfadmin@metasploitable:/var/mail$ sudo cat root
From Attacker@gmail.com  Wed Nov  5 14:33:58 2025
Return-Path: <Attacker@gmail.com>
X-Original-To: root@metasploitable.localdomain
Delivered-To: root@metasploitable.localdomain
Received: from attacker (unknown [192.168.1.5])
        by metasploitable.localdomain (Postfix) with SMTP id 10A2ECBB9
        for <root@metasploitable.localdomain>; Wed,  5 Nov 2025 14:32:21 -0500 (EST)
Subject: test from attacker
Message-Id: <20251105193259.10A2ECBB9@metasploitable.localdomain>
Date: Wed,  5 Nov 2025 14:32:21 -0500 (EST)
From: Attacker@gmail.com
To: undisclosed-recipients:;

i am attcker
```
If your delivered message appears in `/var/mail/root` (or other user), the mail pipeline worked and you can inspect content for evidence of command execution or trigger behavior.
---
## Other useful tools
- swaks
`swaks --to root@metasploitable.localdomain --from Attacker@gmail.com --server 192.168.1.4 --helo attacker --header "Subject: test from attacker" --body "i am attcker"`
- `hydra`— brute force SMTP AUTH (if present):
`hydra -L users.txt -P passwords.txt 192.168.1.4 smtp`
---
## Forensics & reporting checklist (include VM evidence)
- Save banner outputs, nmap results, `smtp-user-enum` outputs, and the exact commands used to send mails.
- Copy `/var/log/mail.log` and `/var/mail/*` after exploitation to your attack host for reporting.
- Timestamp everything and do not alter evidence after capture (copy files, do not move).



