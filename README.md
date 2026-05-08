# Wire-guard-VPN
For this project, I will demonstrate how to set up a WireGuard VPN to securely access LAN resources—such as a UGREEN NAS—remotely, without exposing any services directly to the internet.

# Secure Remote NAS Access with OPNsense & WireGuard VPN

The environment was designed with security, remote accessibility, and network segmentation in mind while maintaining low power consumption for a home lab setup.

---

# Objectives

- Securely access NAS files remotely
- Avoid exposing SMB or NAS management ports publicly
- Configure encrypted VPN connectivity
- Implement firewall rules and controlled remote access
- Gain hands-on experience with VPN deployment and network security

---

# Technologies Used

| Technology | Purpose |
|---|---|
| OPNsense | Router / Firewall |
| WireGuard | Secure VPN Tunnel |
| UGREEN NAS | File Storage |
| Switch | LAN Connectivity |
| VLANs | Network Segmentation |
| Dynamic DNS | Remote Connectivity |

---

# Network Topology

## Logical Diagram

![Network Diagram]([https://imgur.com/a/F6TP7Tb](https://imgur.com/tRV1xtU)))

---

# Environment

| Device | IP Address | Purpose |
|---|---|---|
| OPNsense Firewall | 192.168.1.1 | Gateway / VPN |
| NAS | 192.168.1.50 | File Storage |
| Raspberry Pi | 192.168.1.60 | Plex Server |
| WireGuard VPN Network | 10.0.0.0/24 | VPN Clients |

---

# Why WireGuard?

WireGuard was selected because it is:

- Lightweight
- Faster than traditional VPN protocols
- Easier to configure and maintain
- Modern cryptography
- Excellent mobile support

---

# VPN Architecture

Remote devices connect securely to the OPNsense firewall through WireGuard.

Traffic is encrypted and routed into the internal LAN, allowing secure access to NAS resources as if connected locally.

---

# Configuration Steps

# 1. Install WireGuard Plugin

Navigate to:

```text
System → Firmware → Plugins
