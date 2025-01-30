# Automated Password Change: PowerShell
```
# Import Active Directory module
Import-Module ActiveDirectory

# Define admin account to exclude
$excludedAccount = "[YourAccountName]"

# Function to generate a random secure password
function Generate-SecurePassword {
    $length = 24
    $complexity = 5
    Add-Type -AssemblyName System.Web
    $password = [System.Web.Security.Membership]::GeneratePassword($length, $complexity)

    # Ensure the password includes at least one uppercase letter, one lowercase letter, one digit, and one special character
    $upper = [char[]]"ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    $lower = [char[]]"abcdefghijklmnopqrstuvwxyz"
    $digits = [char[]]"0123456789"
    $special = [char[]]"!@#$%^&*()_-+={}[]|"

    # Add one character from each set to ensure complexity requirements
    $password += $upper | Get-Random
    $password += $lower | Get-Random
    $password += $digits | Get-Random
    $password += $special | Get-Random

    # Shuffle the password to avoid predictable patterns
    $password = -join ($password.ToCharArray() | Sort-Object {Get-Random})

    return $password
}

# Get all users in Active Directory
$users = Get-ADUser -Filter *

foreach ($user in $users) {
    # Skip admin account
    if ($user.SamAccountName -eq $excludedAccount) {
        Write-Output "Skipping password change for user : $($user.SamAccountName)"
        continue
    }

    # Generate password
    $newPassword = Generate-SecurePassword

    # Change the user's password
    Set-ADAccountPassword -Identity $user.SamAccountName -NewPassword (ConvertTo-SecureString -AsPlainText $newPassword -Force)

    # Force user to change password at next logon
    Set-ADUser -Identity $user.SamAccountName -ChangePasswordAtLogon $true

    Write-Output "Changed password for user: $($user.SamAccountName)"
}
```
