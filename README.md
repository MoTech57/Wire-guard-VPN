## Project Overview

This project demonstrates the implementation of a secure remote access VPN using WireGuard on an OPNsense firewall. The goal is to provide encrypted access to internal network resources (NAS, Plex server) without exposing services to the public internet.

Key skills demonstrated:
- VPN configuration (WireGuard)
- Firewall rule management
- NAT configuration
- Network segmentation
- Secure remote access design



# Network Topology

## Logical Diagram

<p align="center">
  <img src="https://i.imgur.com/tRV1xtU.png" alt="WireGuard VPN Network Diagram" width="850">
</p>

### Network Flow

```text
Remote Device → Internet → OPNsense Firewall → Internal LAN → NAS Resources
```

The WireGuard VPN tunnel securely connects remote devices to the internal network without exposing NAS services directly to the public internet.

---

# Environment

| Device | IP Address | Purpose |
|---|---|---|
| OPNsense Firewall | 192.168.x.x | Gateway / VPN |
| UGREEN NAS | 192.168.x.x | File Storage |
| Raspberry Pi | 192.168.x.x | Plex Media Server |
| WireGuard VPN Network | 10.x.x.x/24 | VPN Tunnel Network |

---

# Configuration Steps

# 1. Install WireGuard Plugin

Navigate to:

```text
System → Firmware → Plugins
```

Install:

```text
os-wireguard
```

> Note: In newer OPNsense releases, WireGuard may already be integrated and available under the VPN section.

---

# 2. Enable WireGuard

Navigate to:

```text
VPN → WireGuard → General
```

- Enable WireGuard
- Save and Apply changes

---

# 3. Create a Local Instance

Navigate to:

```text
VPN → WireGuard → Local
```

Click:

```text
+ Add
```

Configure:

| Setting | Example Value |
|---|---|
| Name | wg0 |
| Listen Port | 51820 |
| Tunnel Address | 10.10.10.1/24 |

Generate the following:

- Private Key
- Public Key

Save and Apply changes.

---

# 4. Configure Peer Device

Navigate to:

```text
VPN → WireGuard → Endpoints
```

Add a peer device (Laptop / Phone).

Example:

| Setting | Example Value |
|---|---|
| Allowed IPs | 10.10.10.2/32 |
| Endpoint Address | Dynamic / Remote Client |

Save configuration.

---

# 5. Assign WireGuard Interface

Navigate to:

```text
Interfaces → Assignments
```

- Add the WireGuard interface
- Enable the interface
- Save and Apply

---

# 6. Configure Firewall Rules

## WAN Rule

Navigate to:

```text
Firewall → Rules → WAN
```

Allow:

| Setting | Value |
|---|---|
| Protocol | UDP |
| Destination Port | 51820 |

---

## WireGuard Interface Rule

Navigate to:

```text
Firewall → Rules → WireGuard
```

Allow traffic from:

```text
WireGuard Net → LAN Net
```

---

# 7. Configure Outbound NAT

Navigate to:

```text
Firewall → NAT → Outbound
```

Switch mode to:

```text
Hybrid Outbound NAT
```

Create NAT rule for:

```text
10.10.10.0/24
```

---

# 8. Configure WireGuard Client

Install the WireGuard client on:

- Windows
- macOS
- Linux
- Android
- iPhone

Example client configuration:

```ini
[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.10.10.2/24
DNS = 192.168.x.x

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = yourdomain.ddns.net:51820
AllowedIPs = 192.168.x.x/24
PersistentKeepalive = 25
```

---

# 9. Testing Connectivity

After connecting to the VPN:

## Verify Gateway Access

```bash
ping 192.168.x.x
```

## Access NAS Resources

### SMB Share

```text
\\192.168.x.x
```

### Web Interface

```text
http://192.168.x.x
```

---

# Security Considerations

- NAS services are not publicly exposed
- VPN traffic is fully encrypted
- Firewall policies restrict unauthorized access
- WireGuard uses modern cryptography
- Internal resources remain segmented and protected
