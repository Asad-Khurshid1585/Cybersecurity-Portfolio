# pfSense Network Security Lab

This project documents the installation and configuration of a network security lab using pfSense and Suricata, with three virtual machines:

- **pfSense** (Firewall & IDS/IPS)
- **Ubuntu Client**
- **Ubuntu Server**

## Project Overview

The goal of this project is to set up a secure network environment for testing and learning purposes. The lab simulates a real-world scenario with a firewall and intrusion detection/prevention system (IDS/IPS) using Suricata on pfSense.

## Lab Topology

```
[Ubuntu Client] <--> [pfSense] <--> [Ubuntu Server]
```

- **pfSense** acts as the gateway/firewall between the client and server.
- **Suricata** is installed on pfSense for network intrusion detection and prevention.

## Steps Performed

### 1. Virtual Machine Setup
- Created three VMs:
  - **pfSense** (latest ISO)
  - **Ubuntu Client** (Desktop or Server)
  - **Ubuntu Server**
- Configured network interfaces:
  - pfSense with two NICs (WAN and LAN)
  - Client and Server connected to LAN and WAN sides respectively

### 2. pfSense Installation & Configuration
- Installed pfSense on the dedicated VM
- Configured basic settings (interfaces, IP addresses, DHCP, DNS)
- Verified connectivity between client, pfSense, and server

### 3. Suricata Installation on pfSense
- Installed Suricata package via pfSense package manager
- Configured Suricata interfaces and rules
- Enabled IDS/IPS mode
- Tested detection and alerting functionality

## Key Features
- Network segmentation and firewalling with pfSense
- Real-time network monitoring and intrusion detection with Suricata
- Hands-on experience with open-source security tools

## Usage
1. Start all three VMs
2. Ensure pfSense is running and both Ubuntu machines can communicate through it
3. Use the Ubuntu Client to generate traffic and test Suricata alerts

## References
- [pfSense Documentation](https://docs.netgate.com/pfsense/en/latest/)
- [Suricata Documentation](https://suricata.io/docs/)