# OSCP Notes

This repository contains my personal notes while studying and practicing for the **OSCP certification**. Each topic, attack technique, and lab exercise is documented in its own dedicated `.md` file for easy reference.

## 📌 Purpose
These notes help me:
- Track everything I learn during my OSCP preparation.
- Document all enumeration, exploitation, privilege escalation steps.
- Practice structured note‑taking for the exam.
- Build a reusable playbook for future pentesting.

## 🧪 Lab Environment
I am practicing on a VirtualBox lab with the following machines:

| Role | OS | IP | Notes |
|------|------|------|------|
| Attacker | Kali Linux | 192.168.1.5 | Main attack box |
| Target 1 | Metasploitable 2 | 192.168.1.4 | Vulnerable Linux machine |
| Target 2 | Microsoft Server 2012 | 192.168.1.6 | Windows target for enumeration & exploitation |

NAT Network : 192.168.1.0/24 , DHCP : enabled

> Each target has its own folder with separate markdown files for enumeration, exploitation, and privilege escalation.

## 🔍 What I Document in Each `.md` File
Every service or attack path gets its own file including:
- **Nmap results** (full commands + output)
- **Manual enumeration** steps
- **Vulnerabilities discovered**
- **Exploitation steps**
- **Post‑exploitation actions**
- **Privilege escalation** notes
- **Fixes or troubleshooting** findings

## 🛠️ Tools Frequently Used
- Nmap
- Netcat
- Hydra
- Metasploit (only when allowed)
- Enum4linux
- SNMP tools (snmpwalk, snmp-check)
- Windows & Linux privesc scripts

## 📚 Study Focus
- Enumeration methodology
- Manual exploitation
- Privilege escalation on Linux & Windows
- Report-quality note writing
- Maintaining clean structure for fast review

## 🚀 Goal
Build strong habits and efficient note-taking to prepare for OSCP exam conditions.

---
**These notes are for learning purposes only. All testing is done in an isolated, legal lab.**

