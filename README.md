# Kali AI Home Lab

This repository documents my **Kali Linux AI-assisted home lab**, built to practice and demonstrate cybersecurity skills across multiple domains using **ethical, authorized, and isolated environments**.

The lab combines traditional security tooling with **local AI assistance** to improve analysis, learning, and reporting — without autonomous scanning or exploitation.

---

## 🎯 Goals

- Build a reproducible Kali Linux home lab
- Practice ethical penetration testing and defensive analysis
- Use AI locally to assist with **reasoning and interpretation**
- Maintain safe defaults and clear authorization boundaries
- Create documentation suitable for learning, teaching, and portfolio use

---

## 🖥️ Environment Overview

### Host
- VMware Workstation
- Internet access provided via NAT
- No direct exposure of Kali to the physical LAN by default

### Kali Linux VM
- Desktop: XFCE
- Networking: NAT (isolated)
- Storage: Expanded virtual disk
- Clipboard: Enabled via VMware Tools
- Time format: 12-hour clock (GUI configured)
- Login screen customized via LightDM

---

## 🤖 AI Integration

This lab uses **Ollama** to run AI models **locally inside Kali Linux**.

### Model
- **Mistral** (CPU-only)

### Why Local AI?
- No data leaves the system
- No API keys or cloud dependency
- Works offline
- Safer for handling security tool output

### AI Use Cases
- Interpreting scan results
- Explaining tool output
- Identifying misconfigurations
- Suggesting safe next steps
- Supporting learning and documentation

> AI is used for **analysis and reasoning only**, not for autonomous scanning or exploitation.

---

## 🔍 Example Workflow

```bash
nmap -sC -sV 127.0.0.1 | scanai
