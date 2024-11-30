There are a few methods to hunt for the presence of LAPS.

#### 🖊️ via Cobalt Strike

1)   If it's applied to a machine that you have access to, **AdmPwd.dll** will be on disk.

```
beacon> run hostname
wkstn-2

beacon> ls C:\Program Files\LAPS\CSE

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
 179kb    fil     05/05/2021 07:04:14   AdmPwd.dll
```

2) We could also search for GPOs that have "LAPS" or some other descriptive term in the name.

```
beacon> powershell Get-DomainGPO | ? { $_.DisplayName -like "*laps*" } | select DisplayName, Name, GPCFileSysPath | fl

usncreated               : 25966
displayname              : LAPS
gpcmachineextensionnames : [{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{D02B1F72-3407-48AE-BA88-E8213C6761F1}][{C6DC5466-785
                           A-11D2-84D0-00C04FB169F7}{942A8E4F-A261-11D1-A760-00C04FB9603F}][{D76B9641-3288-4F75-942D-08
                           7DE603E3EA}{D02B1F72-3407-48AE-BA88-E8213C6761F1}]
whenchanged              : 8/16/2022 12:39:45 PM
objectclass              : {top, container, groupPolicyContainer}
gpcfunctionalityversion  : 2
showinadvancedviewonly   : True
usnchanged               : 26068
dscorepropagationdata    : {9/7/2022 1:05:58 PM, 1/1/1601 12:00:00 AM}
name                     : {2BE4337D-D231-4D23-A029-7B999885E659}
flags                    : 1
cn                       : {2BE4337D-D231-4D23-A029-7B999885E659}
gpcfilesyspath           : \\dev.cyberbotic.io\SysVol\dev.cyberbotic.io\Policies\{2BE4337D-D231-4D23-A029-7B999885E659}
distinguishedname        : CN={2BE4337D-D231-4D23-A029-7B999885E659},CN=Policies,CN=System,DC=dev,DC=cyberbotic,DC=io
whencreated              : 8/16/2022 12:20:45 PM
versionnumber            : 17
instancetype             : 4
objectguid               : bbe274cc-6fcc-4e17-8804-bdc0ae952515
objectcategory           : CN=Group-Policy-Container,CN=Schema,CN=Configuration,DC=cyberbotic,DC=io
```

3) As well as computer objects where the **ms-Mcs-AdmPwdExpirationTime** property is not null (any Domain User can read this property).

```
beacon> powershell Get-DomainComputer | ? { $_."ms-Mcs-AdmPwdExpirationTime" -ne $null } | select dnsHostName

dnshostname              
-----------              
wkstn-2.dev.cyberbotic.io
web.dev.cyberbotic.io    
sql-2.dev.cyberbotic.io  
wkstn-1.dev.cyberbotic.io
```

4) If we locate the correct GPO, we can download the LAPS configuration from the gpcfilesyspath.

```
beacon> ls \\dev.cyberbotic.io\SysVol\dev.cyberbotic.io\Policies\{2BE4337D-D231-4D23-A029-7B999885E659}\Machine

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     08/16/2022 12:39:19   Applications
          dir     09/13/2022 15:38:58   Microsoft
          dir     08/16/2022 12:23:37   Preferences
          dir     08/16/2022 12:21:04   Scripts
 575b     fil     08/16/2022 12:22:23   comment.cmtx
 920b     fil     08/16/2022 12:22:23   Registry.pol

beacon> download \\dev.cyberbotic.io\SysVol\dev.cyberbotic.io\Policies\{2BE4337D-D231-4D23-A029-7B999885E659}\Machine\Registry.pol
[*] started download of \\dev.cyberbotic.io\SysVol\dev.cyberbotic.io\Policies\{2BE4337D-D231-4D23-A029-7B999885E659}\Machine\Registry.pol (920 bytes)
[*] download of Registry.pol is complete
```

5) The `Parse-PolFile` cmdlet from the [GPRegistryPolicyParser](https://github.com/PowerShell/GPRegistryPolicyParser) package can be used to convert this file into human-readable format.

```
PS C:\Users\Attacker> Parse-PolFile .\Desktop\Registry.pol

KeyName     : Software\Policies\Microsoft Services\AdmPwd
ValueName   : PasswordComplexity
ValueType   : REG_DWORD
ValueLength : 4
ValueData   : 3

KeyName     : Software\Policies\Microsoft Services\AdmPwd
ValueName   : PasswordLength
ValueType   : REG_DWORD
ValueLength : 4
ValueData   : 14

KeyName     : Software\Policies\Microsoft Services\AdmPwd
ValueName   : PasswordAgeDays
ValueType   : REG_DWORD
ValueLength : 4
ValueData   : 30

KeyName     : Software\Policies\Microsoft Services\AdmPwd
ValueName   : AdminAccountName
ValueType   : REG_SZ
ValueLength : 20
ValueData   : LapsAdmin

KeyName     : Software\Policies\Microsoft Services\AdmPwd
ValueName   : AdmPwdEnabled
ValueType   : REG_DWORD
ValueLength : 4
ValueData   : 1

KeyName     : Software\Policies\Microsoft Services\AdmPwd
ValueName   : PwdExpirationProtectionEnabled
ValueType   : REG_DWORD
ValueLength : 4
ValueData   : 0
```

In this example, we can infer:

```
- Password complexity is upper, lower and numbers.
- Password length is 14.
- Passwords are changed every 30 days.
- The LAPS managed account name is LapsAdmin.
- Password expiration protection is disabled.
```


❗Once ready, enumerate the `ms-Mcs-AdmPwd` property: [[Enumerate ms-Mcs-AdmPwd]]


#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 08:43
🏷️ tags: #redteam #crto 

---

