# Day 4 — Network Stabilization and Git CLI Workflow

## Objective
Fix persistent internet connectivity issues in Kali Linux and transition from GitHub web uploads to professional Git CLI workflow.

---

## Lab Environment

### Hypervisor
- VirtualBox

### Attacker Machine
- Kali Linux

### Target Machine
- Windows VM

---

## Network Architecture

### Adapter 1
- NAT
- Provides internet access for Kali Linux

### Adapter 2
- Host-Only Adapter
- Enables communication between Kali and Windows VM

---

## Issues Encountered

### 1. No Internet Access in Kali
Symptoms:
- Unable to ping google.com
- Unable to clone GitHub repositories
- DNS resolution failures

---

### 2. NAT Adapter Failed to Receive IP Address
Observed:
- eth0 had no IP address
- NetworkManager stuck at:
  "connecting (getting IP configuration)"

---

### 3. DHCP Failure
Error:
"IP configuration could not be reserved"

---

## Troubleshooting Process

### Verified Network Adapters
Checked:
- NAT configuration
- Host-Only Adapter configuration
- Cable Connected option

---

### Network Diagnostics Used

Commands:
```bash
ip a
nmcli device status
ping google.com

