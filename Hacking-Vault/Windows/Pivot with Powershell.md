
PowerShell cmdlets that support credential objects can also be used to pivot into the environment:

```
PS C:\Users\Attacker> $cred = Get-Credential
PS C:\Users\Attacker> Get-ADComputer -Server 10.10.122.10 -Filter * -Credential $cred | select DNSHostName

DNSHostName
-----------
dc-2.dev.cyberbotic.io
fs.dev.cyberbotic.io
wkstn-2.dev.cyberbotic.io
web.dev.cyberbotic.io
sql-2.dev.cyberbotic.io
wkstn-1.dev.cyberbotic.io
```



### ⚠ Opsec




### Properties
---
📆 created   {{17-02-2024}} 18:25
🏷️ tags: #redteam #crto 

---

