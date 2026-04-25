**Day 2 – Cybersecurity Lab Setup (VirtualBox Networking & VM Configuration)**
**Objective**

The objective of Day 2 was to build and troubleshoot a basic cybersecurity lab environment using Virtual Machines. The lab includes a Kali Linux attacker machine and a Windows target machine connected through a Host-Only network in VirtualBox.

**The goal was to achieve**:

Internal network communication between VMs
Proper IP configuration in the 192.168.56.0/24 subnet
Enable foundation for penetration testing and SOC lab setup
**Environment Setup**

The lab was created using:

Oracle VM VirtualBox
Kali Linux (Attacker machine)
Windows 10 VM (Target machine)

    Network Configuration:

NAT network used for Kali (internet access)
Host-Only Adapter used for internal communication
VirtualBox Host-Only Network: 192.168.56.0/24
    **⚙️ Network Configuration Summary**
**Windows VM:**
Static IP: 192.168.56.10
Subnet: 255.255.255.0
Network Profile: Private (attempted configuration)
Adapter: Host-Only
**Kali VM:**
eth0: NAT (10.0.2.x)
eth1: Host-Only (192.168.56.5)
**🔍 Testing Performed**
Verified IP configuration using:
ipconfig (Windows)
ip a (Kali Linux)
Performed connectivity test:
ping 192.168.56.10

**Result:**

❌ 100% packet loss observed
No ICMP response from Windows VM
**🚨 Issues Faced**
1. Network Isolation Issue
Windows VM showing “Unidentified Network”
No response to ICMP requests
2. VirtualBox Host-Only Configuration Confusion
Adapter binding inconsistencies between VMs
Difficulty ensuring both VMs used same Host-Only interface
3. Windows Network Profile Restrictions
Limited visibility of “Private/Public network” option
Firewall and network isolation affected connectivity
**🛠️ Troubleshooting Steps Attempted**
Verified VirtualBox network adapter settings
Restarted both VMs in correct order
Disabled Windows Firewall (Private profile)
Attempted network profile change to Private
Verified IP addressing consistency across subnet
**The goal was to achieve:**
Kali Linux VM successfully configured with dual network interfaces
Windows VM successfully assigned static IP in Host-Only network
However, inter-VM connectivity (ping) not yet functional
Further debugging required on Windows network binding and VirtualBox host-only configuration
**🎯 Learning Outcomes**
Understood VirtualBox networking modes (NAT vs Host-Only)
Learned VM-to-VM communication setup
Understood importance of subnet alignment (192.168.56.0/24)
Learned real-world troubleshooting approach for lab environments
Identified common Windows VM networking issues
