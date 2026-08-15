
[[DLL enforcement]] is not commonly enabled which allows us to call exported functions from DLLs on disk via rundll32.


#### 📦 - using Cobalt Strike

Beacon's DLL payload exposes several exports including DllMain and StartW.  These can be changed in the Artifact Kit under src-main, dllmain.def.

`C:\Windows\System32\rundll32.exe http_x64.dll,StartW`




#### ⚠ Opsec




### Properties
---
📆 created   {{29-02-2024}} 15:55
🏷️ tags: #redteam #crto 

---

