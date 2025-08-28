# Active Directory + PowerShell

Add Organisation in Powershell

    New-ADOrganizationalUnit `
    -Name TestOU `
    -Description "Hello this is Description!" `
    -DisplayName TestOU `
    -Path "DC=ralys,DC=domain,DC=com"

Remove OU by GUID

    Get-ADOrganizationalUnit -Filter *
    # Example GUID 
    $guid = "aead1715-73d0-4a1b-b240-560132c73c34"

    # Disable protection
    Set-ADOrganizationalUnit -Identity $guid -ProtectedFromAccidentalDeletion $false
    Remove-ADOrganizationalUnit -Identity $guid

Loop for multiple OUs

    $guids = "aead1715-73d0-4a1b-b240-560132c73c34"

    foreach ($guid in $guids) {
        Set-ADOrganizationalUnit -Identity $guid -ProtectedFromAccidentalDeletion $false
        Remove-ADOrganizationalUnit -Identity $guid
    }

Create a user and groups

# New user in Helpdesk OU
    New-ADUser -Name "Jonas Help" `
    -SamAccountName "jhelp" `
    -UserPrincipalName "Jonas.Help@domain.com" `
    -Path "OU=Helpdesk,OU=IT-Abteilung,OU=Tier1,DC=domain,DC=com"

# Vertrieb group (distribution)
    New-ADGroup -Name "Vertrieb" `
    -SamAccountName "Vertrieb" `
    -GroupScope Global -GroupCategory Distribution `
    -Description "Alle Mitarbeiter im Vertrieb"

# Backup-Operatoren group (security)
    New-ADGroup -Name "Backup-Operatoren" `
    -SamAccountName "Backup-Operatoren" `
    -GroupScope Global -GroupCategory Security `
    -Description "Für Backup-Systeme"

# Password Reset Function

    function ResetPwd2 {
        [CmdletBinding()]
        param(
            [Parameter(Mandatory)][string]$Name,
            [string]$NewPlainTextPassword = "P@ssw0rd!"
        )

        $filter = "UserPrincipalName -like '$Name*'"
        $user = Get-ADUser -Filter $filter -ErrorAction Stop

        $sec = ConvertTo-SecureString $NewPlainTextPassword -AsPlainText -Force
        Set-ADAccountPassword -Identity $user -NewPassword $sec
        Set-ADUser -Identity $user -ChangePasswordAtLogon $true

        Write-Host "Pwd Set | Muss ändern!"
        $user.UserPrincipalName
    }
    Export-ModuleMember -Function ResetPwd2
