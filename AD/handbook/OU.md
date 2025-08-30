# OU Management

---

## 1. Create an OU
**Why:** OUs organize users, groups, and computers.

**PowerShell:**
```powershell
New-ADOrganizationalUnit -Name "TestOU" -Description "OU for Training purpose to FLEX" -Path "DC=ralys,DC=domain,DC=com" -ProtectedFromAccidentalDeletion $true
```

**GUI:**
1. Open **Active Directory Users and Computers**.
2. Right‑click the domain -> **New -> Organizational Unit**.
3. Name it and check *Protect from accidental deletion*.

---

## 2. Find an OU & GUID
**Why:** To identify your OU.

**PowerShell:**
```powershell
Get-ADOrganizationalUnit -Filter "Name -eq 'TestOU'" -Properties ObjectGUID
```

**Why:** To get OU list

```powershell
Get-ADOrganizationalUnit -Filter *
```

**GUI:**
1. Right‑click OU -> **Properties**.
2. Open **Attribute Editor** -> copy **objectGUID**.

---

## 3. Rename an OU
**PowerShell:**
```powershell
Rename-ADObject -Identity "OU=TestOU,DC=ralys,DC=domain,DC=com" -NewName "TestOU-Remastered"
```

**GUI:** Right‑click OU -> **Rename**.

---

## 4. Move an OU
**PowerShell:**
```powershell
Move-ADObject -Identity "OU=TestOU-Remastered,DC=ralys,DC=domain,DC=com" -TargetPath "OU=HQ,DC=ralys,DC=domain,DC=com"
```

**GUI:** Good old fashion drag and drop the OU into the new container.

---

## 5. Delete an OU
**Important:** Must disable protection first.

**PowerShell:**
```powershell
# Step 1: disable protection
Set-ADOrganizationalUnit -Identity "OU=TestOU,DC=ralys,DC=domain,DC=com" -ProtectedFromAccidentalDeletion $false
# or
Set-ADOrganizationalUnit -Identity $guid -ProtectedFromAccidentalDeletion $false
# Step 2: delete
Remove-ADOrganizationalUnit -Identity "OU=TestOU,DC=ralys,DC=domain,DC=com" -Recursive -Confirm:$false
# or
Remove-ADOrganizationalUnit -Identity $guid
```

**GUI:**
1. Right‑click OU -> **Properties -> Object tab** -> uncheck *Protect from accidental deletion*.
2. Delete the OU.

---

## 6. Restore Deleted OU (Optional)
**PowerShell:**
```powershell
# Find deleted OUs
Get-ADObject -IncludeDeletedObjects -Filter 'isDeleted -eq $true -and objectClass -eq "organizationalUnit"'

# Restore
Restore-ADObject -Identity [Guid]"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

**GUI:** Use the **Active Directory Administrative Center (ADAC)** -> **Deleted Objects** container → restore.

---

## Quick Menu
```powershell
# Create OU
New-ADOrganizationalUnit -Name "OU1" -Path "DC=ralys,DC=domain,DC=com"

# Find OU + GUID
Get-ADOrganizationalUnit -Filter "Name -eq 'OU1'" -Properties ObjectGUID

# Rename
Rename-ADObject -Identity "OU=OU1,DC=ralys,DC=domain,DC=com" -NewName "OU2"

# Move
Move-ADObject -Identity "OU=OU2,DC=ralys,DC=domain,DC=com" -TargetPath "OU=HQ,DC=ralys,DC=domain,DC=com"

# Delete
Set-ADOrganizationalUnit -Identity "OU=OU2,DC=ralys,DC=domain,DC=com" -ProtectedFromAccidentalDeletion $false
Remove-ADOrganizationalUnit -Identity "OU=OU2,DC=ralys,DC=domain,DC=com" -Recursive -Confirm:$false

# Restore
Restore-ADObject -Identity [Guid]"<deletedGUID>"
```

---


[← Back to Index](../AD.md)