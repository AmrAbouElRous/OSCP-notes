# Metasploitable2

Metasploitable2 is an **intentionally vulnerable Ubuntu Linux VM** provided for safe penetration‑testing practice in isolated labs.

> **Goal:** add a downloaded Metasploitable2 `.vmdk` to Oracle VirtualBox, create a NAT network named `hacking` using the `192.168.1.0/24` subnet, attach both Kali (attacker) and Metasploitable2 (target) to that NAT network so they can reach each other, and verify SSH access using the default credentials. If needed, adjust SSH client options to accept older `ssh-rsa` host key algorithms.

---

## 1. Create a VM from the Metasploitable2 VMDK

1. Open **Oracle VM VirtualBox**.
2. Click **New** → give a name (e.g., `Metasploitable2`).
3. **Type:** Linux. **Version:** choose a 32‑bit Linux that matches the image (e.g., `Ubuntu (32-bit)` or `Other Linux (32-bit)`).
4. Set RAM (e.g., `512`–`1024` MB).
5. When asked for a hard disk choose **Use an existing virtual hard disk file** → click the folder icon → **Add** → browse to the downloaded `.vmdk` → select it → **Create**.

> If you have an `.ova`/`.ovf`, use **File → Import Appliance...** instead.
>
> Default username and password is msfadmin

---

## 2. Create or use an existing Kali VM

- If you already have Kali, skip this step.
- To create: **New** → `Kali` → Type `Linux` → Version `Debian (64-bit)` (or matching ISO) → assign `2048` MB RAM (recommended) → create disk and install from ISO.

---

## 3. Create a NAT Network named `hacking`

VirtualBox provides "NAT Network" so multiple VMs attached to it can communicate while staying isolated from your physical network.

1. In VirtualBox go to **File → Preferences → Network** (or **Tools → Network** depending on version).
2. Open the **NAT Networks** tab and click the **+** (create) button.
3. Rename the new NAT network to: `hacking`.
4. Edit its CIDR/IPv4: set the network to `192.168.1.0/24`.
5. (Optional) Enable DHCP so VirtualBox assigns addresses automatically. Example DHCP pool: `192.168.1.2`–`192.168.1.254`.
6. Save/close the dialog.

> Menu names vary by VirtualBox version. Look for **Preferences → Network** or **Tools → Network (NAT Networks)**.

---

## 4. Attach both VMs to the `hacking` NAT network

1. Power off both VMs (editing network while running may be blocked).
2. Right‑click the VM → **Settings** → **Network**.
3. For **Adapter 1**:
   - Check **Enable Network Adapter**.
   - **Attached to:** select **NAT Network**.
   - **Name:** choose `hacking` from the dropdown.
4. Repeat for the other VM.
5. Start both VMs.

---

## 5. Check IP addresses inside each VM

Open a terminal in each VM and run:

```bash
# preferred
ip addr show
# or (if net-tools installed)
ifconfig -a
```

Typical example (DHCP-dependent):

- Metasploitable2: `192.168.1.4`
- Kali (attacker): `192.168.1.5`

---

## 6. Basic connectivity & enumeration from Kali

From Kali run:

```bash
ping -c 3 192.168.1.4
nmap -sS -Pn 192.168.1.4
# or a faster/less noisy scan
nmap -F 192.168.1.4
```

You should see common open services; `22/tcp` (SSH) is usually open.

---

## 7. SSH into Metasploitable2

Try the default account:

```bash
ssh msfadmin@192.168.1.4
# password: msfadmin
```

### If OpenSSH refuses the host key (old `ssh-rsa` / SHA-1)

Modern OpenSSH may reject older algorithms. To allow the client to accept that older key **for this host only**, add a host entry to your SSH config:

```bash
nano ~/.ssh/config
```

Add:

```
Host 192.168.1.4
    HostKeyAlgorithms +ssh-rsa
    PubkeyAcceptedAlgorithms +ssh-rsa
    SetEnv TERM=xterm
    RequestTTY yes
```
> Error opening terminal: xterm-256color means that Metasploitable2 does not have the necessary terminal information files (terminfo) to understand how to display the graphical interface of nano using the specific terminal type your client is advertising (xterm-256color). 
> This usually happens when you are connecting via SSH from a modern terminal emulator (like the one in Kali Linux) to an older system like Metasploitable2 so we SetEnvironment for terminal to be xterm.

Save and retry `ssh msfadmin@192.168.1.4`.

> Alternatively, use inline options without editing the config:
>
> ```bash
> ssh -o HostKeyAlgorithms=+ssh-rsa -o PubkeyAcceptedAlgorithms=+ssh-rsa msfadmin@192.168.1.4
> ```

**Security note:** only relax these options for trusted lab hosts.

---

## 8. Starting the SSH server on Metasploitable2 (if needed)

If `ssh` is not running on the target, start it inside Metasploitable2:

```bash
# as root or with sudo
sudo /etc/init.d/ssh start
# other actions
sudo /etc/init.d/ssh stop
sudo /etc/init.d/ssh restart
```

After starting, re-scan with `nmap` and retry the SSH connection.

---

## 9. Troubleshooting

- **NAT network ****************************************************************************hacking**************************************************************************** not visible in VM settings**: ensure the NAT network exists in VirtualBox Preferences and the VM is powered off.
- **VMs cannot reach each other**: verify both adapters are attached to the same NAT Network name `hacking` and both VMs show `192.168.1.x` addresses.
- **DHCP not assigning addresses**: either enable DHCP on the NAT network or set static IPs inside the VMs in the `192.168.1.0/24` range (avoid `.1` if VirtualBox uses it).
- **SSH still refused after config change**: confirm the `Host` line matches the IP you connect to, or use the inline `-o` options.
- **Permission denied on SSH with default creds**: double-check username/password (`msfadmin`/`msfadmin`) and that the target's SSH service is running.

##
