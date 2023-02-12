# Basic-Profile-Mover
Basic Profile Mover script coded in PowerShell.

## The Script

Here is an example of a PowerShell script that will copy all files, folders, and settings of the user "malopez" profile from a PC named "malopez-01" on a domain to a new PC named "malopez-02" on the same domain:

```PowerShell
$src = "\\malopez-01\C$\Users\malopez\"
$dst = "\\malopez-02\C$\Users\malopez\"

# Copy all files and folders
Get-ChildItem -Path $src -Recurse | Copy-Item -Destination $dst -Force

# Copy the user's registry settings
$Registry = "HKEY_CURRENT_USER"
$Key = "Software\Microsoft\Windows\CurrentVersion\Explorer"
$RegKey = $Registry + "\" + $Key

Export-RegistryKey -Path $RegKey -File "C:\RegistryExport.reg"

# Copy the registry export to the new PC
Copy-Item "C:\RegistryExport.reg" "\\malopez-02\C$\RegistryExport.reg"

# Import the registry export on the new PC
Invoke-Command -ComputerName "malopez-02" -ScriptBlock {Import-RegistryKey -File "C:\RegistryExport.reg"}
```

This script uses `Invoke-Command` to remotely access the source PC "malopez-01" and copy all files and folders from the source location to the destination location. It then uses `Export-RegistryKey` to export the current user "malopez" registry settings and copy them to the new PC. It then uses `Invoke-Command` and `Import-RegistryKey` to import the registry export on the new PC named malopez-02.

Please note that this script will only work if you have the correct permissions to access both PCs. Make sure that you are running the script on a computer that has administrative access to both source and destination PCs, and that the source and destination paths are correct for your network. Also, this script is a basic example and it might not cover all the settings and configurations you would like to transfer.
