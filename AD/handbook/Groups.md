# Group Management

## Create Vertrieb Group 
```powershell
New-ADGroup -Name "Vertrieb" `
  -SamAccountName "Vertrieb" `
  -GroupScope Global -GroupCategory Distribution `
  -Description "Alle Mitarbeiter im Vertrieb"
```