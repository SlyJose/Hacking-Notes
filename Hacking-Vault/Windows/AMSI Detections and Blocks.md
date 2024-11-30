
#### 🖊️ detections on Cobalt Strike

AMSI is very effective against out of the box Cobalt Strike resources.

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

---
Same outcome would happen if we try winrm64:

```
beacon> jump winrm64 fs.dev.cyberbotic.io smb
[-] Could not connect to pipe: 2 - ERROR_FILE_NOT_FOUND
```

Even though this is in-memory, the detections are still based on "known bad" signatures.




### Properties
---
📆 created   {{28-02-2024}} 19:50
🏷️ tags: #redteam #crto 

---

