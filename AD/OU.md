# OU Management

## Create OU
```powershell
New-ADOrganizationalUnit `
  -Name "TestOU" `
  -Description "Training OU" `
  -Path "DC=ralys,DC=domain,DC=com" `
  -ProtectedFromAccidentalDeletion $true
```

## Find User GUID
```powershell
Get-ADOrganizationalUnit -Filter * | -Filter "SamAccountname -eq 'Peter'"
```

## Remove OU by GUID
```powershell
Set-ADOrganizationalUnit -Identity $guid -ProtectedFromAccidentalDeletion $false
Remove-ADOrganizationalUnit -Identity $guid
```
## Loop Delete Multiple OUs
```powershell
$guids = "..."
foreach ($guid in $guids) {
    Set-ADOrganizationalUnit -Identity $guid -ProtectedFromAccidentalDeletion $false
    Remove-ADOrganizationalUnit -Identity $guid
}
```