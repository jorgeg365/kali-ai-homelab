# Step-by-Step Setup – Kali AI Home Lab

This guide documents the complete setup of a **Kali Linux AI-assisted home lab**
using VMware, NAT networking, and local AI (Ollama + Mistral).

The goal is to create a **safe, ethical, and reproducible** environment for
cybersecurity learning and analysis.

---

## 1. Create the Kali Linux Virtual Machine

### 1.1 Download Kali Linux
- Download the **Kali Linux VMware image** (recommended) or ISO
- Desktop environment: **XFCE**

### 1.2 Import or Install
- VMware image: Open directly in VMware Workstation
- ISO: Create a new VM and complete the installer

### 1.3 VM Hardware Configuration (Recommended)
- **RAM:** 12–16 GB (8 GB minimum)
- **CPU:** 4 cores minimum (6–8 preferred)
- **Disk:** 80 GB recommended
- **Network Adapter:** NAT
- Enable:
  - Connected
  - Connect at power on

### 1.4 Take Initial Snapshot
After first successful boot:
- Snapshot name: `clean-kali-base`

---

## 2. Update Kali Linux

```bash
sudo apt update
sudo apt full-upgrade -y
sudo reboot
```
# If repository signing errors appear, ensure /etc/apt/sources.list contains:

deb [signed-by=/usr/share/keyrings/kali-archive-keyring.gpg] http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware

Fix DNS Issues (If Large Downloads Fail)

# If downloads time out or stall:
