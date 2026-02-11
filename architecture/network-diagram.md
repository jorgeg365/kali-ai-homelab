# Network Architecture – Kali AI Home Lab

This document describes the **network layout** used in the Kali Linux
AI-assisted home lab.

The design prioritizes:
- Safety
- Isolation
- Reproducibility
- Ethical testing

---

## 🧠 High-Level Overview

- Kali Linux runs inside a **VMware virtual machine**
- Networking mode is **NAT**
- The lab is isolated from the physical LAN by default
- All testing is performed against:
  - The Kali VM itself
  - Other lab VMs
  - Intentionally vulnerable applications

No unauthorized scanning of the real network is performed.

---

## 🌐 Network Diagram (Logical View)

```mermaid
flowchart LR
    Host["Host\nVMware"]
    NAT["NAT\nVMnet8"]
    Kali["Kali VM\nXFCE"]
    AI["AI\nMistral"]
    Lab["Lab VMs"]

    Host --> NAT
    NAT --> Kali
    Kali --> AI
    Kali --> Lab
**Legend**
- **Host**: Physical machine running VMware Workstation  
- **NAT (VMnet8)**: Isolated virtual network with internet access  
- **Kali VM**: Kali Linux XFCE environment  
- **AI**: Local Ollama engine running Mistral  
- **Lab VMs**: DVWA, OWASP BWA, Metasploitable


