# pfSense Firewall Configuration

---

## 0. Setup Overview

**Why:**  
To implement a secure perimeter firewall for a small company network following BSI principles (Default Deny, least exposure, clear segmentation).

**System:**  
- **pfSense version:** 2.7.0-RELEASE (amd64)  
- **Hardware (virtual):** 1 CPU, 1024 MB RAM, 20 GB Disk, 2 NICs  
- **WAN:** vtnet0 → 10.10.10.215 / 24  
- **LAN:** em0 → 192.178.99.215 / 24  
- **Gateway:** 10.10.10.1  
- **Access:** HTTPS → `https://192.178.99.215`  
- **IPv6:** Disabled  
- **DHCP:** Disabled (handled by Windows Server)

---

## 1. Interface Assignment

**Why:**  
To map virtual network interfaces correctly before IP configuration.

**Console:**  
```bash
Option 1) Assign Interfaces
Do VLANs need to be set up first? [y/n]: n
Enter the WAN interface name: vtnet0
Enter the LAN interface name: em0
Do you want to proceed? [y/n]: y
```

**Result:**  
```
WAN -> vtnet0
LAN -> em0
```

---

## 2. WAN Configuration

**Why:**  
Defines the external (upstream) network connection.

**Console:**  
```bash
Option 2) Set interface(s) IP address
Enter number of interface: 1  (WAN)
Configure IPv4 via DHCP? (y/n): n
Enter IPv4 address: 10.10.10.215
Enter subnet bit count: 24
Enter upstream gateway: 10.10.10.1
Set as default gateway? (y/n): y
Configure IPv6? (y/n): n
Enable DHCP server on WAN? (y/n): n
Revert to HTTP for webConfigurator? (y/n): n
```

**Result:**  
WAN = **10.10.10.215/24**, Gateway = 10.10.10.1, IPv6 disabled.

---

## 3. LAN Configuration

**Why:**  
Defines the internal network segment for local devices and servers.

**Console:**  
```bash
Option 2) Set interface(s) IP address
Enter number of interface: 2  (LAN)
Configure IPv4 via DHCP? (y/n): n
Enter IPv4 address: 192.178.99.215
Enter subnet bit count: 24
Enter gateway: [Press ENTER for none]
Configure IPv6? (y/n): n
Enable DHCP server on LAN? (y/n): n
Revert to HTTP for webConfigurator? (y/n): n
```

**Result:**  
LAN = **192.178.99.215/24**, no gateway, IPv6 disabled, static setup.

---

## 4. WebConfigurator Access

**Why:**  
To manage pfSense through a secure browser connection.

**Steps:**
1. From a client inside LAN, open browser →  
   `https://192.178.99.215/`
2. Accept the SSL certificate warning.
3. Default credentials:  
   - **Username:** `admin`  
   - **Password:** `pfsense`

**(Optional):** Change port from **443** → **8443** for BSI compliance.

---

## 5. Connectivity Test

**Why:**  
To confirm that routing and gateway configuration are operational.

**Console:**
```bash
Option 7) Ping host
Enter host: 8.8.8.8
```

**Result:**
```
3 packets transmitted, 3 packets received
0% packet loss
```

Connectivity confirmed — WAN is online.

---

## 6. Security Hardening

**Why:**  
To align firewall with BSI NET.3.2 recommendations.

| Baustein | Maßnahme | Begründung |
|-----------|-----------|------------|
| A2/A3/A4 | Default-Deny Firewall-Policy | Minimiert Angriffsfläche |
| A6 | Admin access via port 8443 | 443 is a well-known target |
| A7 | Anti-Lockout Rule active | Prevents admin lockouts |
| A8 | Routing services disabled | Only firewalling active |
| A9 | Logging enabled by default | Provides traceability |
| A10 | Fragmentation protection | Built-in pf engine defense |
| A17 | IPv6 disabled | Smaller attack surface |

---

## 7. Summary of Key Settings

| Parameter | Value |
|------------|--------|
| WAN IP | 10.10.10.215/24 |
| WAN Gateway | 10.10.10.1 |
| LAN IP | 192.178.99.215/24 |
| IPv6 | Disabled |
| DHCP | Disabled |
| Admin Access | HTTPS → 192.178.99.215 |
| pfSense Version | 2.7.0-RELEASE |
| Security Policy | Default Deny / Allowlist only |

---

## 8. Verification Commands

**Show Interface Status:**
```bash
ifconfig
```

**Check Gateway Connectivity:**
```bash
ping 10.10.10.1
```

**Firewall Logs:**
```
Status → System Logs → Firewall
```

---

## 9. Maintenance

- **Backup Config:** Diagnostics → Backup/Restore.  
- **Update System:** Console → Option 13 (Update from console).  
- **Restart Services:** Console → Option 16 (Restart PHP-FPM).  
- **Reboot:** Console → Option 5.

---

## 10. Access Recap

| Access Type | Address | Protocol |
|--------------|----------|-----------|
| WebConfigurator | https://192.178.99.215 | HTTPS |
| Console | Proxmox → pfSense Console | Direct |
| Ping Test | 8.8.8.8 | ICMP |

---

[← Back to Index](../index.md)