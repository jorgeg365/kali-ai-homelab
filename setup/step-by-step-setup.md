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
## If repository signing errors appear, ensure /etc/apt/sources.list contains:

deb [signed-by=/usr/share/keyrings/kali-archive-keyring.gpg] http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware

## 3. Fix DNS Issues (If Large Downloads Fail)

If downloads time out or stall:
```
sudo rm -f /etc/resolv.conf
echo -e "nameserver 1.1.1.1\nnameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```
Verify:
```
nslookup ollama.com
```
(Optional) Make DNS persistent:
```
sudo chattr +i /etc/resolv.conf
```
4. Install Ollama (Local AI Engine)
4.1 Install Ollama
```
curl -fsSL https://ollama.com/install.sh | sh
```
4.2 Download AI Model
```
ollama pull mistral
```
4.3 Test Ollama
```
ollama run mistral
```
CPU-only warning is normal in virtual machines.
5. Configure AI Aliases (Zsh)

Kali uses zsh by default.

Edit the shell configuration:
```
nano ~/.zshrc
```
Add:
```
alias ai='ollama run mistral'
alias explain='ollama run mistral "Explain the following output:"'
alias scanai='ollama run mistral "Analyze this scan output, identify real risks, and suggest safe next steps:"'
```
Reload:
```
source ~/.zshrc
```
Test:
```
ai "Explain what Nmap does."
```
6. Create AI-Assisted Nmap Command (nmapai)

Create a wrapper that runs Nmap and sends output to AI:
```
sudo tee /usr/local/bin/nmapai >/dev/null <<'EOF'
#!/usr/bin/env bash
set -euo pipefail

if [[ $# -lt 1 ]]; then
  echo "Usage: nmapai <target-ip-or-hostname> [extra nmap args]"
  exit 1
fi

TARGET="$1"
shift || true

TMP="$(sudo mktemp)"
trap 'sudo rm -f "$TMP"' EXIT

echo "[*] Running nmap against: $TARGET"
sudo nmap -sC -sV --top-ports 200 -T4 --max-retries 1 --host-timeout 120s -n \
  --reason --open "$TARGET" "$@" -oN "$TMP"

echo
echo "[*] AI analysis (local):"
sudo cat "$TMP" | ollama run mistral \
"Analyze this nmap output. Summarize open ports and services, identify notable risks or misconfigurations, and suggest safe validation steps. Do not provide exploitation instructions."
EOF
```
Make it executable:
```
sudo chmod +x /usr/local/bin/nmapai
```
Test safely:
```
nmapai 127.0.0.1
```
7. Networking Guidelines

Default mode: NAT

Only scan:

Localhost (127.0.0.1)

Lab VMs on the same NAT network

Avoid scanning real LAN devices unless explicitly authorized

Use Bridged networking only when intentionally required


