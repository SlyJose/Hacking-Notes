
Windows Defender utilizes [[AMSI]] to identify fileless malware and malicious input content. 


#### 📔 - bypass behavior detections

Check the [[AMSI]] execution attempt. In there we are trying to execute `smb_x64.ps1` which helps out in lateral movement. The script is being detected by AMSI. Same outcome would happen if we try winrm64:

```
beacon> jump winrm64 fs.dev.cyberbotic.io smb
[-] Could not connect to pipe: 2 - ERROR_FILE_NOT_FOUND
```

Even though this is in-memory, the detections are still based on "known bad" signatures.

Evasion:

1) [[AMSI Evasion Scanning]]

###### 🚀 - Cobalt Strike
---
1. B 

--- 

#### 🖊️ A


#### 📔 B


####  📗 C


#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 15:17
🏷️ tags: #redteam #crto 

---

