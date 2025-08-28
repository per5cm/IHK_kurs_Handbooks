# User Management

## Create New User
```powershell
New-ADUser -Name "Jonas Help" `
  -SamAccountName "jhelp" `
  -UserPrincipalName "Jonas.Help@domain.com" `
  -Path "OU=Helpdesk,OU=IT-Abteilung,OU=Tier1,DC=domain,DC=com" `
  -AccountPasword (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force)
  -Enabled $true
```

