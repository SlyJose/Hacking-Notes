
You can read the policy from:

- GPO
- Local registry of a machine

#### 🖊️ Reading from GPO

1) Similar process as [[Enumerating LAPS]], get GPO, get registry.pol, parse
```
beacon> powershell Get-DomainGPO -Domain dev-studio.com | ? { $_.DisplayName -like "*AppLocker*" } | select displayname, gpcfilesyspath

displayname gpcfilesyspath                                                                        
----------- --------------                                                                        
AppLocker   \\dev-studio.com\SysVol\dev-studio.com\Policies\{7E1E1636-1A59-4C35-895B-3AEB1CA8CFC2}

beacon> download \\dev-studio.com\SysVol\dev-studio.com\Policies\{7E1E1636-1A59-4C35-895B-3AEB1CA8CFC2}\Machine\Registry.pol
[*] started download of \\dev-studio.com\SysVol\dev-studio.com\Policies\{7E1E1636-1A59-4C35-895B-3AEB1CA8CFC2}\Machine\Registry.pol (7616 bytes)
[*] download of Registry.pol is complete
```

2) Once you can read the policy, values will look something like this:

```
KeyName     : Software\Policies\Microsoft\Windows\SrpV2\Exe\a61c8b2c-a319-4cd0-9690-d2177cad7b51
ValueName   : Value
ValueType   : REG_SZ
ValueLength : 700
ValueData   : <FilePathRule Id="a61c8b2c-a319-4cd0-9690-d2177cad7b51" Name="(Default Rule) All files located in the
              Windows folder" Description="Allows members of the Everyone group to run applications that are located
              in the Windows folder." UserOrGroupSid="S-1-1-0" Action="Allow"><Conditions><FilePathCondition
              Path="%WINDIR%\*"/></Conditions></FilePathRule>
```

#### 📔 Reading from local registry

1) Query the registry at `HKLM:Software\Policies\Microsoft\Windows\SrpV2`

```
PS C:\Users\Administrator> Get-ChildItem "HKLM:Software\Policies\Microsoft\Windows\SrpV2"

    Hive: HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SrpV2

Name                           Property
----                           --------
Appx                           EnforcementMode : 1
                               AllowWindows    : 0
Dll                            AllowWindows : 0
Exe                            EnforcementMode : 1
                               AllowWindows    : 0
Msi                            EnforcementMode : 1
                               AllowWindows    : 0
Script                         EnforcementMode : 1
                               AllowWindows    : 0
```

2) Read the ruleset/property you want:

```
PS C:\Users\Administrator> Get-ChildItem "HKLM:Software\Policies\Microsoft\Windows\SrpV2\Exe"

    Hive: HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SrpV2\Exe

Name                           Property
----                           --------
921cc481-6e17-4653-8f75-050b80 Value : <FilePathRule Id="921cc481-6e17-4653-8f75-050b80acca20" Name="(Default Rule) All files located in the Program Files folder" Description="Allows
acca20                         members of the Everyone group to
                                       run applications that are located in the Program Files folder." UserOrGroupSid="S-1-1-0" Action="Allow"><Conditions><FilePathCondition
                                       Path="%PROGRAMFILES%\*"/></Conditions></FilePathRule>
a61c8b2c-a319-4cd0-9690-d2177c Value : <FilePathRule Id="a61c8b2c-a319-4cd0-9690-d2177cad7b51" Name="(Default Rule) All files located in the Windows folder" Description="Allows members
ad7b51                         of the Everyone group to run
                                       applications that are located in the Windows folder." UserOrGroupSid="S-1-1-0" Action="Allow"><Conditions><FilePathCondition
                               Path="%WINDIR%\*"/></Conditions></FilePathRule>
```

#### ⚠ Opsec


### Properties
---
📆 created   {{29-02-2024}} 14:50
🏷️ tags: #redteam #crto 

---

