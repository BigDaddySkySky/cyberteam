# Initial User and Firewall Configurations
## List Admins
```
Get-LocalGroupMember administrators
```

## Remove Unnecessary Members
```
$members = "<userName>", "<Domain\<email@outlook.com>"
Remove-LocalGroupMember -Group "Administrators" -Member $members
```

## Check Firewall Status
```
Get-NetFirewallProfile | Format-Table Name, Enabled
```

## Block Critical Ports
```
New-NetFirewallRule -DisplayName "<name displayed>" -Direction <Inbound/Outbound> -LocalPort <Port#> -Protocol <protocol abbreviation> -Action <Block/Allow>
```

# SSH Setup and Confiuration
## Start SSH Service
```
Start-Service sshd
```

## Configure SSH
Edit the config file at C:\Program Files\OpenSSH-Win64\sshd_config_default

## Add Necessary configurations
- AllowGroups <Domain>\<Group>
- AllowGroups <groupName>
- DenyGroups Adminsitrators
- PubkeyAuthentication yes
- PasswordAuthentication yes

## Restart SSH Service
```
Restart-Service sshd
```

# Auditing and Logging Setup
## Configure Audit Policies
- Path Win + R gpedit.msc
- Navigate to: Computer Configuration > Windows Settings > Security Settings > Local Policies > Audit Policies
- Enable Auditing for:
	- Account logon events
	- Account management
	- Logon events
	- Policy change
	- Privilege use
	- Process tracking

# Disk and Password Management
## Set Drive as Read-Only
```
diskpart
list disk
select disk <diskNum>
attributes disk add read-only
exit
```

## Change Passwords **or** Disable Accounts
- Use Active Directory Users and Computers for user management

## Update Password Policy
- Path: Win + R > gpedit.msc
- Navigate to: Computer Configuration > Security Settings > Account Policies > Password Policy
- Set minimum password length to 12 characters and enable complexity requirements

# System Updates and Processes
## Update Windows
- Go to Settings > Updates
- Manual Updates [Here](https://www.catalog.update.microsoft.com/home.aspx)

## Monitor Processes with [Process Explorer](https://download.sysinternals.com/files/ProcessExplorer.zip)
- Enable VirusTotal under Options
- Sort by Working Set to view RAM usage

# SMB Hardening
## Check if SMBv1 is Enabled
```
Get-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```

## Disable SMBv1 if Enabled
```
Set-SmbServerConfiguration -EnableSMB1Protocol $false
```

## Harden SMBv2/3
- Windows Firewall > New Rule (both inbound and outbound)
- Block TCP ports 139, 445 and UDP ports 137, 138

# Additional Services and Tasks Management
## View and manage Services
- List Services
```
Get-Service
```
- Stop Services
```
Stop-Service -Name "<serviceName>"
```
- Disable unnecessary services
	- Print Spooler
	```
	Stop-Service -Name Spooler -Force
	Set-Service -Name Spooler -StartupType Disabled
	```
	- Fax Services
	```
	Get-Service -Name MapsBroker | Set-Service -StartupType Disabled
	Stop-Service MapsBroker
	```
- Scheduled Tasks
	- List all tasks
	```
	Get-ScheduledTask
	```
	- View Task Info
	```
	Get-ScheduledTask -TaskName "TaskName" | Format-List *
	```
	- Delete a Task
	```
	Unregister-ScheduledTask -TaskName "<taskName>" -Confirm:$false
	```

# Final Monitoring and Maintenance
## Download and install [Sysmon](https://download.sysinternals.com/files/Sysmon.zip)
- Download Config FIle
```
Invoke-WebRequest -Uri https://raw.githubusercontent.com/olafhartong/sysmon-modular/master/sysmonconfig.xml -OutFile C:\Windows\config.xml
```
- Install Sysmon
```
C:\Path\to\sysmon64.exe -accepteula -i C:\Windows\Config.xml
```

## Monitor Events in Event Viewer
- Path: Applications and Services Logs > Microsoft > Windows > Sysmon
- Key events to monitor
	- 1: Process Creation
	- 2: File Creation Time
	- 3: Network Connection
	- 5: Process Termination
	- 6: Driver Loaded
	- 7: Image Loaded
	- 8: CreateRemote Thread
	- 11: File Creation
	- 12: Registry Event (Object Create/Delete)
	- 13: Registry Event (Value Set)

# Additional Tools
- [Autoruns](https://live.sysinternals.com/autoruns.exe)
- Check network connections and ports
```
netstat -ab | more
```
