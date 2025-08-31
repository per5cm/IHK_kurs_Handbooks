# Password Reset Function

---

## 1. Reset Password for a User
**PowerShell:**
```powershell
Set-ADAccountPassword -Identity "jhelp" -Reset -NewPassword (ConvertTo-SecureString "NewP@ssw0rd!" -AsPlainText -Force)
Set-ADUser -Identity "jhelp" -ChangePasswordAtLogon $true
```

**GUI:**
1. In **Active Directory Users and Computers (ADUC)**, right-click the user.
2. Select **Reset Password**.
3. Enter the new password.
4. Optionally check **User must change password at next logon**.

---

## 2. Bulk Password Resets
**PowerShell:**
```powershell
$users = "jhelp","jdoe","asmith"
foreach ($user in $users) {
    Set-ADAccountPassword -Identity $user -Reset -NewPassword (ConvertTo-SecureString "Welcome2025!" -AsPlainText -Force)
    Set-ADUser -Identity $user -ChangePasswordAtLogon $true
}
```

---

## 3. Password Reset Function (Reusable)
Add this function to your admin scripts:
```powershell
function ResetPwd {
    param(
        [Parameter(Mandatory)][string]$UserName,
        [string]$NewPlainTextPassword = "P@ssw0rd!"
    )

    $user = Get-ADUser -Filter "SamAccountName -eq '$UserName'"
    $sec = ConvertTo-SecureString $NewPlainTextPassword -AsPlainText -Force

    Set-ADAccountPassword -Identity $user -NewPassword $sec
    Set-ADUser -Identity $user -ChangePasswordAtLogon $true

    Write-Host "Password reset for $UserName. User must change at next logon."
}
```

### 3a) Make it a **script** and use it (dot-sourcing)
**When to use:** Quick, local use in a single session.

1) **Save the script** as `ResetPwd.ps1` (e.g., `C:\Admin\Scripts`).
```powershell
# C:\Admin\Scripts\ResetPwd.ps1
function ResetPwd { <function body from above> }
```
2) **Allow local scripts (Current User only):**
```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
```
3) **Unblock if downloaded:**
```powershell
Unblock-File C:\Admin\Scripts\ResetPwd.ps1
```
4) **Load it into the current session (dot-source):**
```powershell
. C:\Admin\Scripts\ResetPwd.ps1   # note the leading dot + space
```
5) **Run it:**
```powershell
ResetPwd -UserName "jhelp" -NewPlainTextPassword "Temp2025!"
```
> **Tip:** Dot-sourcing must be repeated in new sessions unless you add it to your profile.

---

## 4. Unlock Accounts (Optional)
Sometimes a user is locked after multiple failed attempts.

**PowerShell:**
```powershell
Unlock-ADAccount -Identity "jhelp"
```

**GUI:**
- In ADUC, right-click user -> **Properties -> Account tab** -> check/uncheck *Unlock account*.

---

## Quick Cheatsheet
```powershell
# Reset + force password change
Set-ADAccountPassword -Identity "jdoe" -Reset -NewPassword (ConvertTo-SecureString "TempP@ss1" -AsPlainText -Force)
Set-ADUser -Identity "jdoe" -ChangePasswordAtLogon $true

# Bulk reset
$users = "jdoe","asmith"; foreach ($u in $users) { Set-ADAccountPassword -Identity $u -Reset -NewPassword (ConvertTo-SecureString "Welcome2025!" -AsPlainText -Force); Set-ADUser -Identity $u -ChangePasswordAtLogon $true }

# Unlock account
Unlock-ADAccount -Identity "jdoe"
```

---

[← Back to Index](../AD.md)