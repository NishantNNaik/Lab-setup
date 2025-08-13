# Cyber Lab Setup (Day 1) — VirtualBox, Kali, Windows

This repository documents the exact steps I followed to create a safe, isolated cybersecurity lab using VirtualBox with **Kali Linux (attacker)** and **Windows 10 (victim)**. The goal is to keep Windows isolated from the internet while Kali can access both the internet (for updates/tools) and the private lab network.

---

## Topology

```
Internet
   |
 [Host PC]
    |
  NAT ----> Kali (Adapter 1 - NAT for internet)
            Kali (Adapter 2 - Host-Only) <----> Windows (Adapter 1 - Host-Only)
```

- **Kali:** 2 adapters → `NAT` (internet) + `Host-Only` (private lab)
- **Windows:** 1 adapter → `Host-Only` only (no internet)

---

## VirtualBox Settings

### Kali Linux (Attacker VM)
- **Adapter 1**  
  - Attached to: `NAT`  
  - Cable connected: ✅  
  - Promiscuous mode: `Deny` (default)

- **Adapter 2**  
  - Attached to: `Host-Only Adapter` (`vboxnet0`)  
  - Cable connected: ✅  
  - Promiscuous mode: `Allow VMs` (for sniffing in the lab)

### Windows 10 (Victim VM)
- **Adapter 1**  
  - Attached to: `Host-Only Adapter` (`vboxnet0`)  
  - Cable connected: ✅  
  - Promiscuous mode: `Deny` (default)

> Create the Host-Only network via **VirtualBox → (File) Tools → Network Manager → Host-only Networks → Create**. Defaults are typically `192.168.56.0/24` with a DHCP range.

---

## Verifying IP Addresses

Start both VMs.

### On Windows (CMD)
```bat
ipconfig
```
Look for the **VirtualBox Host-Only** interface with an IP like `192.168.56.101`.

### On Kali (Terminal)
```bash
ip a
```
Expect **two** IPs:  
- NAT: usually `10.x.x.x` or `192.168.x.x`  
- Host-Only: usually `192.168.56.x`

---

## Connectivity Tests (must pass)

### Ping from Kali → Windows
```bash
ping <Windows_HostOnly_IP>
```

### Ping from Windows → Kali (Host-Only IP)
```bat
ping <Kali_HostOnly_IP>
```

### Internet from Kali (should work)
```bash
ping -c 2 google.com
```

### Internet from Windows (should fail — that’s good)
```bat
ping google.com
```

If ping fails **only** from Windows to Kali, temporarily disable the Windows firewall for lab testing:
- Control Panel → Windows Defender Firewall → Turn off (domain/private/public) → *Remember to re-enable after tests.*

---

## First Scan (Nmap)

From **Kali**, discover hosts on the host-only subnet (default `192.168.56.0/24`):
```bash
sudo nmap -sn 192.168.56.0/24
```

Scan the Windows target (replace with actual IP):
```bash
sudo nmap -sS -sV -O -Pn <Windows_HostOnly_IP> -oN scan-results.txt
```

- `-sS`: SYN scan (stealth)  
- `-sV`: service/version detection  
- `-O`: OS detection  
- `-Pn`: skip host discovery (treat host as up)  
- `-oN`: save results to a text file

> Commit `scan-results.txt` and any screenshots to this repo as proof-of-work.

---

## (Optional) Wireshark Capture on Kali

Start Wireshark and capture on the **Host-Only** interface (e.g., `enp0s8`/`eth1`).  
Look for:
- ARP requests/replies
- TCP 3-way handshake (SYN → SYN/ACK → ACK)
- HTTP/DNS if you run local services later

---

## Repository Structure Suggestion

```
/Cyber-Lab-Setup
├── README.md                 # This file
├── lab-setup.md              # Detailed step-by-step with screenshots
├── screenshots/              # Put PNG/JPG here
│   └── (nmap.png, wireshark.png, settings.png)
└── scan-results.txt          # Output from first nmap scan
```

> On GitHub web, you can create folders by typing a path like `screenshots/nmap.png` when adding a file.

---

## Troubleshooting Notes

- Ensure both VMs are attached to the **same** Host-Only network name (e.g., `vboxnet0`).  
- Check **Cable connected** is ticked for each adapter.  
- If you can’t enable an adapter: fully power off the VM, then edit settings; run VirtualBox as Administrator on Windows hosts.  
- Windows firewall can block ICMP ping — temporarily disable or add an inbound rule for ICMPv4.  
- Confirm DHCP is enabled for Host-Only, or set static IPs in the same `/24` (e.g., Kali `192.168.56.102`, Windows `192.168.56.101`).

---

## Security Notes

- Keep **Windows VM offline** (Host-Only only).  
- Re-enable Windows firewall after testing.  
- Don’t expose intentionally vulnerable services to the public internet.  
- Snapshot your VMs before risky tests so you can revert quickly.

---

## Metadata

- **Author:** Nishant Naik
- **Date:** 2025-08-13
- **Tags:** cybersecurity, virtualbox, kali, windows, lab, nmap, wireshark
