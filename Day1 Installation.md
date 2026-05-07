# Day 1 — Cybersecurity Lab Initial Setup

## Objective
Set up the foundational cybersecurity lab environment using VirtualBox and Kali Linux.

---

## Environment Setup

### Installed Software
- VirtualBox
- Kali Linux VM
- Windows VM

---

## Virtual Machine Configuration

### Kali Linux
Purpose:
- Attacker machine
- Penetration testing environment

### Windows VM
Purpose:
- Target machine
- Internal lab testing

---

## Initial Networking Setup

Configured dual-adapter architecture:

### Adapter 1
- NAT
- Internet access for Kali

### Adapter 2
- Host-Only Adapter
- Communication between Kali and Windows

---

## Initial Challenges

### Mouse integration issue
Problem:
- Mouse movement captured incorrectly inside VM

Solution:
- Restarted VM
- VirtualBox mouse integration stabilized

---

## Networking Verification

Commands used:
```bash
ip a
ping
