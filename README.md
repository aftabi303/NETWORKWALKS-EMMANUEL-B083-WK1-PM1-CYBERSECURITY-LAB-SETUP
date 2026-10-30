# NETWORKWALKS-EMMANUEL-B083-WK1-PM1-CYBERSECURITY-LAB-SETUP
Cybersecurity lab setup: VirtualBox + Kali Linux on an isolated NAT Network (10.0.0.0/24) for ethical hacking practice. Networkwalks Week 1 project.

# 🔐 Cybersecurity Lab Environment Setup

Building an isolated virtual lab for penetration testing and ethical hacking practice.

![Skill](https://img.shields.io/badge/Skill-Cybersecurity-red) ![Kali](https://img.shields.io/badge/Kali%20Linux-2026.2-blue) ![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-green) ![NetworkWalks](https://img.shields.io/badge/NetworkWalks-WK1--PM1-black)

---

## 📌 Project Overview

This project sets up a virtual cybersecurity and penetration-testing laboratory using VirtualBox and Kali Linux.

The lab is a controlled environment where security tools, network scanning and other ethical hacking practice can be done safely and repeatedly, without touching any real systems.

It runs on a private virtual network, so more machines (Windows, Android, etc.) can be added later and used as targets for authorized testing.

---

## 🧰 Lab Requirements

| Item | Details |
|---|---|
| Virtualization | Oracle VirtualBox <<< version, e.g. 7.2.x >>> |
| Attacker machine | Kali Linux 2026.2 (pre-built VirtualBox image) |
| Network type | NAT Network (`NatNetwork`) |
| Subnet | 10.0.0.0/24 |
| Kali IP address | 10.0.0.2/24 |
| Gateway / DNS | 10.0.0.1 / 8.8.8.8 |
| Host PC | <<< e.g. Windows 10, 8 GB RAM, 256 GB SSD, Core i5 >>> |

---

## ✅ Steps Completed

### Step 1: Installed 7-Zip and VirtualBox
I installed 7-Zip (to extract the compressed Kali download) and the latest recommended version of VirtualBox on my Windows PC.

### Step 2: Created the NAT Network
In VirtualBox, I opened **File > Tools > Network > NAT Networks** and created a network named `NatNetwork` with the prefix `10.0.0.0/24` and DHCP enabled.

![NAT Network settings](01-natnetwork.png)

### Step 3: Imported Kali Linux
I downloaded the Kali Linux virtual machine image for VirtualBox, extracted it with 7-Zip and imported it into VirtualBox. The VM uses 4096 MB of memory.

### Step 4: Configured the VM settings
- **Network:** Adapter 1 changed from Bridged Adapter to **NAT Network** (`NatNetwork`), with Promiscuous Mode set to Allow All.

  ![Kali network adapter](02-kali-adapter.png)

- **Shared folder:** my host Downloads folder is shared with Kali with auto-mount enabled. It appears as `sf_Downloads` on the Kali desktop.

  ![Shared folder settings](03-shared-folder.png)

- **Clipboard and drag & drop:** Shared Clipboard and Drag'n'Drop set to **Bidirectional**.

  ![Clipboard and drag and drop](04-clipboard-dragdrop.png)

### Step 5: Set the static IP address
In Kali, under **Edit Connections > Wired connection 1 > IPv4 Settings**, I set the method to Manual with:
- Address: `10.0.0.2`
- Netmask: `24`
- Gateway: `10.0.0.1`
- DNS: `8.8.8.8`

The `ip a` command confirms that `eth0` has `10.0.0.2/24`.

![IP configuration](05-ip-a.png)

### Step 6: Tested connectivity
I pinged the gateway (`10.0.0.1`), `8.8.8.8` and `google.com`. All replies were received with 0% packet loss, confirming full Internet access and working DNS.

![Ping tests](06-ping-tests.png)

### Step 7: Took a snapshot
I took a VirtualBox snapshot of the working Kali VM, so I can restore a clean state if something breaks later.

![Snapshot](07-snapshot.png)

---

## 🛠️ Troubleshooting

**Problem:** After I set the static IP, the network icon in Kali kept loading and the connection never came up.

**Cause:** This is a known issue with VirtualBox 7 and newer Kali versions, where NetworkManager's duplicate address check delays activation of the static IP.

**Solution:** I disabled the duplicate address detection timeout and restarted the connection:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

After this, the IP was applied and the Internet worked.

---

## 📚 What I Learned

- The difference between Bridged and NAT Network modes, and why a NAT Network is safer for an isolated lab.
- How to assign a static IP with NetworkManager, and how to test connectivity with `ip a` and `ping`.
- How shared folders, a shared clipboard and drag and drop make working between host and VM easier.
- Why snapshots matter: they give a clean restore point before testing.
- How to troubleshoot a network problem step by step and fix it with the command line.

---

## ⚠️ Disclaimer

This lab is for education and learning only. All testing is done inside my own virtual lab environment, on systems I own.

---

*Project: Week 1 (WK1-PM1) | Networkwalks Cybersecurity Program*
