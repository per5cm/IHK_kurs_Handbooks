# User Management

---

## 1. Create a New User
**PowerShell:**
```powershell
New-ADUser -Name "Jonas Help" `
  -SamAccountName "jhelp" `
  -UserPrincipalName "Jonas.Help@domain.com" `
  -Path "OU=Helpdesk,OU=IT-Abteilung,OU=Tier1,DC=domain,DC=com" `
  -AccountPassword (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force) `
  -Enabled $true
```

**GUI:**
1. Open **Active Directory Users and Computers**.
2. Right‑click the target OU -> **New -> User**.
3. Enter **First name, Last name, User logon name**.
4. Set password and choose options (user must change password, never expires, etc.).

---

## 2. Find a User
**PowerShell:**
```powershell
Get-ADUser -Identity "jhelp" -Properties * | Format-List Name,SamAccountName,DistinguishedName,Enabled
```

**GUI:**
- In Active Directory Users and Computers (ADUC), click the **OU** or domain root, then use the **Find/Search** bar.

---

## 3. Reset User Password
**PowerShell:**
```powershell
Set-ADAccountPassword -Identity "jhelp" -Reset -NewPassword (ConvertTo-SecureString "NewP@ssw0rd123" -AsPlainText -Force)
Enable-ADAccount -Identity "jhelp"
```

**GUI:**
1. Right‑click user -> **Reset Password**.
2. Enter new password and options.

---

## 4. Enable or Disable a User
**PowerShell:**
```powershell
# Disable
Disable-ADAccount -Identity "jhelp"

# Enable
Enable-ADAccount -Identity "jhelp"
```

**GUI:**
- Right‑click user -> **Disable Account** or **Enable Account**.

---

## 5. Move User to Another OU
**PowerShell:**
```powershell
Move-ADObject -Identity "CN=Jonas Help,OU=Helpdesk,OU=IT-Abteilung,OU=Tier1,DC=domain,DC=com" `
  -TargetPath "OU=Users,OU=Tier1,DC=domain,DC=com"
```

**GUI:**
- Good old fashioned drag and drop the user object into the new OU.

---

## 6. Remove (Delete) a User
**PowerShell:**
```powershell
Remove-ADUser -Identity "jhelp" -Confirm:$false
```

**GUI:**
- Right‑click user -> **Delete**.

---

## Quick Cheatsheet
```powershell
# Create User
New-ADUser -Name "John Doe" -SamAccountName "jdoe" -Path "OU=Users,DC=domain,DC=com" -AccountPassword (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force) -Enabled $true

# Find User
Get-ADUser -Identity "jdoe"

# Reset Password
Set-ADAccountPassword -Identity "jdoe" -Reset -NewPassword (ConvertTo-SecureString "NewP@ssw0rd" -AsPlainText -Force)

# Disable/Enable
Disable-ADAccount -Identity "jdoe"
Enable-ADAccount -Identity "jdoe"

# Move User
Move-ADObject -Identity "CN=John Doe,OU=Users,DC=domain,DC=com" -TargetPath "OU=Helpdesk,DC=domain,DC=com"

# Delete User
Remove-ADUser -Identity "jdoe" -Confirm:$false
```

---

[← Back to Index](../AD.md)

