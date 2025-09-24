# Mailserver (Weretis) + Thunderbird Setup

## 0. Before You Start
**Why:** Ensure your Windows Server is ready.


## 1. Install DNS Role

**GUI:**
1. Server Manager -> Add Roles -> DNS Server.

2. Create **Forward Lookup Zone**: `yourdomain.local`.

3. Create **Reverse Lookup Zone**: match your network (`192.178.99.213/24`).


## 2. Open Firewall Ports
**Why:** Allow SMTP + IMAP traffic.

---

**PowerShell:**
```powershell
New-NetFirewallRule -DisplayName "SMTP 25"  -Direction Inbound -Protocol TCP -LocalPort 25  -Action Allow
New-NetFirewallRule -DisplayName "SMTP 465" -Direction Inbound -Protocol TCP -LocalPort 465 -Action Allow
New-NetFirewallRule -DisplayName "SMTP 587" -Direction Inbound -Protocol TCP -LocalPort 587 -Action Allow
New-NetFirewallRule -DisplayName "IMAP 143" -Direction Inbound -Protocol TCP -LocalPort 143 -Action Allow
New-NetFirewallRule -DisplayName "IMAPS 993"-Direction Inbound -Protocol TCP -LocalPort 993 -Action Allow
```

---


## 3. Install Weretis Mailserver
**Why:** Provides SMTP (send) and IMAP (receive).

1. Install Weretis (setup.exe).
2. In Weretis Admin:
   - Enable **SMTP** (25 + 465 + 587).
   - Enable **IMAP** (143 + 993).
3. Create Domain - E-Mail `test1@yourdomain.local` 


## 4. Thunderbird Setup

**Steps:**
1. Open Thunderbird -> Add Mail Account.
2. Enter E-Mail + Password (`test1@yourdomain.local`).


## 5. Test Mail Flow