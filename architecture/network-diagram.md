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
    Host["Host Machine\nVMware Workstation"]
    NAT["VMware NAT Network\nVMnet8"]
    Kali["Kali Linux VM\nXFCE + Ollama"]
    AI["Local AI Engine\nOllama - Mistral"]
    Lab["Lab Targets\nDVWA | OWASP BWA | Metasploitable"]

    Host --> NAT
    NAT --> Kali
    Kali --> AI
    Kali --> Lab
