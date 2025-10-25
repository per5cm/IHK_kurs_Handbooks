# pfSense Post-Setup Hardening & Correction Guide

---

## 11. Network Correction – Move LAN to RFC1918 Range

**Why:**  
`192.178.99.0/24` isn’t a private range. It can conflict with real internet routes.  
We’ll migrate to **192.168.99.0/24** safely without breaking DNS, AD, or DHCP.

---

### Step 1 – Change LAN IP

**Console (Option 2):**
```bash
Option 2) Set interface(s) IP address
Select interface: 2  (LAN)
Configure IPv4 via DHCP? (y/n): n
Enter IPv4 address: 192.168.99.1
Enter subnet bit count: 24
Gateway: [Press Enter]
Configure IPv6? (y/n): n
Enable DHCP server on LAN? (y/n): n
Revert to HTTP for webConfigurator? (y/n): n
```

Reconnect via `https://192.168.99.1`.

---

### Step 2 – Adjust Windows Server & Clients

**Windows Server (Domain Controller):**
```
IP Address: 192.168.99.147
Subnet Mask: 255.255.255.0
Gateway: 192.168.99.1
Preferred DNS: 127.0.0.1 (or itself)
```

**Clients (if static):**
```
IP Address: 192.168.99.x (range 145-146)
Gateway: 192.168.99.1
DNS: 192.168.99.147
```

**DHCP Server Scope (if used):**
```
Range: 192.168.99.145 – 192.168.99.146
DNS: 192.168.99.147
Lease Time: 8 days
```

---

### Step 3 – Verify NAT

**pfSense GUI:**
1. Navigate -> *Firewall -> NAT -> Outbound*  
2. Set **Mode: Automatic outbound NAT**  
3. Ensure it includes:
   ```
   Interface: WAN
   Source: 192.168.99.0/24
   Translation: Interface Address
   ```
4. Save → Apply Changes.

---

### Step 4 – Add Allowlist Rules (LAN -> WAN)

**pfSense GUI:**
1. Go to *Firewall -> Rules -> LAN*.  
2. Delete “Allow all” rule.  
3. Add the following **Allow Rules** (top to bottom):

| # | Protocol | Destination | Port | Description |
|---|-----------|--------------|------|--------------|
| 1 | ICMP | any | any | Allow Ping / Traceroute |
| 2 | TCP | any | 80 | HTTP outbound |
| 3 | TCP | any | 443 | HTTPS outbound |
| 4 | TCP/UDP | 192.168.99.147 | 53 | DNS to DC |
| 5 | UDP | 192.168.99.147 | 123 | NTP to DC |
| 6 | TCP/UDP | any | 88,389,445,464,3268,3269 | AD services |
| 7 | Block | any | any | Default Deny rule |

4. Apply changes.

---

### Step 5 – Secure Web Access

**pfSense GUI:**
1. *System -> Advanced -> Admin Access*  
2. Change **WebConfigurator port** → `8443`.  
3. Turn OFF “Redirect HTTP to HTTPS”.  
4. Keep **Anti-Lockout Rule** active.  
5. Apply → reconnect via `https://192.168.99.1:8443`.

---

### Step 6 – Block IPv6

**pfSense GUI:**
1. *Firewall -> Rules -> Floating -> Add Rule*  
2. Action: **Block**  
3. Interface: **LAN + WAN**  
4. Protocol: **IPv6**  
5. Description: *Block all IPv6 traffic*  
6. Save → Apply.

---

### Step 7 – Verify Everything

```bash
ping 192.168.99.1       # from client → pfSense
ping 8.8.8.8            # internet reach
nslookup google.com     # via DNS on DC
tracert 8.8.8.8         # ensure route passes pfSense
```

**Expected results:**
- Clients resolve DNS via DC.
- Internet reachable.
- Logs show only allowed traffic.
- IPv6 attempts blocked.

---

### Step 8 – Backup New Configuration

**pfSense GUI:**
```
Diagnostics -> Backup/Restore -> Download configuration
```
Filename example:  
`pfsense_config_postfix_192.168.99.1.xml`

---

### Final BSI Alignment Summary

| Section | Measure | Status |
|----------|----------|--------|
| NET.3.2 A2-A4 | Default Deny / Allowlist | 
| NET.3.2 A6 | Port 8443 Admin Access | 
| NET.3.2 A17 | IPv6 Disabled & Blocked | 
| NET.3.2 A9 | Logging Active | 
| SYS.1.2.3 | DNS/AD Roles Segregated | 
| Audit | Config Backups + Documentation | 

---

[← Back to Index](../index.md)