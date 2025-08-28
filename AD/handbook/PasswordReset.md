# Password Reset Function

```powershell
function ResetPwd {
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
Export-ModuleMember -Function ResetPwd
```

[← Back to Index](../AD.md)