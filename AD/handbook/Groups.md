# Group Management

---

## 1. Create a New Group
**PowerShell:**
```powershell
New-ADGroup -Name "Vertrieb" `
  -SamAccountName "Vertrieb" `
  -GroupScope Global `
  -GroupCategory Security `
  -Description "Alle Mitarbeiter im Vertrieb"
```

**Notes:**
- `GroupScope`: Global, DomainLocal, or Universal.
- `GroupCategory`: Security (permissions) or Distribution (mail lists).
- [Group scope table](tabels/table_gs.md)

**GUI:**
1. Open **Active Directory Users and Computers (Benutzer und Computer)**.
2. Right-click the target OU -> **New (neu) -> Group (Gruppe)**.
3. Enter name, choose **Group scope (Gruppenbereich)** and **Group type (Gruppentyp)**.

---

## 2. Find a Group
**PowerShell:**
```powershell
Get-ADGroup -Identity "Vertrieb" -Properties * | Format-List Name,SamAccountName,GroupScope,GroupCategory,DistinguishedName
```

**GUI:**
- In ADUC, search for the group in the domain or OU.

---

## 3. Add and Remove Members
**PowerShell:**
```powershell
# Add user to group
Add-ADGroupMember -Identity "Vertrieb" -Members "jhelp"

# Remove user from group
Remove-ADGroupMember -Identity "Vertrieb" -Members "jhelp" -Confirm:$false
```

**GUI:**
1. Open group **Properties (Eigenschaften)**.
2. Go to **Members (Mitglieder)** tab.
3. Use **Add (Hinzufügen)** or **Remove (Entfernen)**.

---

## 4. List Group Members
**PowerShell:**
```powershell
Get-ADGroupMember -Identity "Vertrieb"
```

**GUI:**
- Open group **Properties (Eigenschaften) -> Members (Mitglieder)** tab.

---

## 5. Rename a Group
**PowerShell:**
```powershell
Rename-ADObject -Identity "CN=Vertrieb,OU=Groups,DC=domain,DC=com" -NewName "Sales"
```

**GUI:**
- Right-click group -> **Rename (Umbenennen)**.

---

## 6. Move a Group
**PowerShell:**
```powershell
Move-ADObject -Identity "CN=Vertrieb,OU=Groups,DC=domain,DC=com" -TargetPath "OU=HQ,DC=domain,DC=com"
```

**GUI:**
- Good old fashioned drag and drop the group into the new OU.

---

## 7. Delete a Group
**PowerShell:**
```powershell
Remove-ADGroup -Identity "Vertrieb" -Confirm:$false
```

**GUI:**
- Right-click group -> **Delete (Löchen)**.

---

## Quick Cheatsheet
```powershell
# Create Group
New-ADGroup -Name "Sales" -SamAccountName "Sales" -GroupScope Global -GroupCategory Security

# Add/Remove Members
Add-ADGroupMember -Identity "Sales" -Members "jdoe"
Remove-ADGroupMember -Identity "Sales" -Members "jdoe" -Confirm:$false

# List Members
Get-ADGroupMember -Identity "Sales"

# Rename
Rename-ADObject -Identity "CN=Sales,OU=Groups,DC=domain,DC=com" -NewName "Sales-EMEA"

# Move
Move-ADObject -Identity "CN=Sales,OU=Groups,DC=domain,DC=com" -TargetPath "OU=HQ,DC=domain,DC=com"

# Delete
Remove-ADGroup -Identity "Sales" -Confirm:$false
```

---

[← Back to Index](../AD.md)