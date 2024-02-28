
**Antimalware Scan Interface AMSI**

#### 🖊️ what is it

AMSI is a component of Windows which allows applications to integrate themselves with an antivirus engine by providing a consumable, language agnostic interface.  

It was designed to tackle "fileless" malware that was so heavily popularized by tools like the [EmpireProject](https://github.com/EmpireProject/Empire), which leveraged PowerShell for complete in-memory C2.

#### 📔 environment

Any 3rd party application can use AMSI to scan user input for malicious content.  Many Windows components now use AMSI including PowerShell, the Windows Script Host, JavaScript, VBScript and VBA.

####  📗 alerts

If we try to execute a well known PowerShell payload on a machine with Defender on, it will get blocked.

```
PS C:\Users\Attacker> C:\Payloads\smb_x64.ps1
At C:\Payloads\smb_x64.ps1:1 char:1
+ Set-StrictMode -Version 2
+ ~~~~~~~~~~~~~~~~~~~~~~~~~
This script contains malicious content and has been blocked by your antivirus software.
```

The alert that Defender produces is tagged with `amsi:` rather than `file:`, indicating that something malicious was detected in memory.

![[detection.png]]




### Properties
---
📆 created   {{28-02-2024}} 15:22
🏷️ tags: #redteam #crto 

---

