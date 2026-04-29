**✅ WORK COMPLETED TODAY**
**1. Virtual Machine Networking Setup**
Configured dual network adapters:
Adapter 1: NAT (internet access)
Adapter 2: Host-Only (internal lab communication)
Ensured Kali and Windows VMs were in the same host-only subnet (192.168.56.0/24)
**2. Internet Connectivity Fix (Kali VM)**
Problem:
Kali VM initially had no internet access
eth0 interface was not receiving IP address
DNS resolution failed (http.kali.org unreachable)
Root Cause:
VirtualBox NAT DHCP misconfiguration
Missing or failed DHCP assignment on eth0
Solution:
Restarted network services (NetworkManager)
Reset network interfaces (ip link set eth0 down/up)
Verified NAT adapter settings in VirtualBox
Successfully obtained IP: 10.0.2.15
**3. Host-Only Network Setup (Kali ↔ Windows)**
Problem:
Windows and Kali initially unable to communicate properly
Packet loss and “host unreachable” errors observed
Solution:
Verified both VMs on same Host-Only network adapter
Confirmed IP addressing:
Kali: 192.168.56.5
Windows: 192.168.56.10
Enabled ICMP response in Windows firewall using command line rule
**4. Clipboard Sharing Setup**
Installed VirtualBox Guest Additions in Kali VM
Enabled:
Shared Clipboard: Bidirectional
Improved mouse integration and usability
**5. Network Troubleshooting & Validation**
Verified connectivity:
Kali → Windows: SUCCESS ✔
Windows → Kali: OPTIONAL (not required for lab)
Confirmed stable dual-interface configuration
**6. First Security Reconnaissance (Nmap)**

Performed initial port scan using:

Nmap

Result:

Open ports discovered on Windows VM:

135/tcp → MSRPC
139/tcp → NetBIOS
445/tcp → SMB (Microsoft-DS)
Interpretation:
Windows host is exposing key internal services
SMB service identified as primary attack surface
Firewall is filtering most additional ports (1000 filtered states)
**7. Service Enumeration**

Further validation using Windows netstat:

Confirmed active listening services:
SMB (445)
RPC (135)
NetBIOS (139)
Multiple dynamic ports also active (Windows system services)
**⚠️ CHALLENGES FACED**
**1. Kali VM No Internet Access**
Caused by NAT DHCP failure
Resolved by network reset and adapter reconfiguration
**2. Nmap Returning Help Menu Instead of Scan**
Cause: Incorrect command syntax (missing target or flags)
Fixed by using correct format: nmap -sV <target_ip>
**3. Host-Only Network Misunderstanding**
Initial confusion between NAT and NAT Network
Resolved by standardizing NAT + Host-Only architecture
**4. Windows Firewall Blocking Ping / Scan Visibility**
ICMP and service visibility initially restricted
Partial resolution using firewall rule adjustments
**5. “Host Unreachable” / Packet Loss Issues**
Caused by inconsistent routing and firewall behavior
Fixed by verifying IP subnet alignment and adapter consistency
**📚 KEY LEARNINGS (IMPORTANT)******
**🔹 1. Virtual networking is the foundation of cybersecurity labs**

Without correct VM networking:

No scanning
No exploitation
No tool functionality
🔹 **2. NAT vs Host-Only roles are critical**
NAT → internet access only
Host-Only → attacker-target communication

Misconfiguration leads to complete lab failure.

🔹** 3. Nmap is only as good as network visibility**

Nmap cannot detect:

Filtered ports behind firewall
Misconfigured routing
Broken interfaces
🔹 **4. Windows security model hides services by default**
Open ports may exist but remain “filtered”
Firewall plays major role in scan visibility
🔹 **5. Troubleshooting is a core cybersecurity skill**

Most time was spent on:

network debugging
interface configuration
connectivity validation

Not actual scanning — this is realistic in real-world environments.

🎯 FINAL STATUS (DAY 3)

✔ Dual VM lab successfully established
✔ Internet working on Kali
✔ Internal network working (Kali ↔ Windows)
✔ Clipboard sharing enabled
✔ First reconnaissance scan completed
✔ Windows attack surface identified (SMB, RPC, NetBIOS)
