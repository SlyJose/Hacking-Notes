The Artifact Kit contains the source code for the artifacts used by Cobalt Strike (ex. psexec) and is designed to facilitate the development of sandbox-safe injectors. 

The idea is to develop artifacts that inject Beacon shellcode in a way that cannot be emulated by AV engines.  There are several bypass techniques provided with the kit which you can modify, or you can implement entirely new ones yourself.  Where the Artifact Kit does not help is with making Beacon resilient to detection once it's running in memory (e.g. from memory scanners).

#### 🚀 - Why using it?
---
For example, we may have access to certain machine:

```
 beacon> ls \\fs.dev.cyberbotic.io\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     09/14/2022 15:44:51   $Recycle.Bin
          dir     08/10/2022 04:55:17   $WinREAgent
          dir     08/10/2022 05:05:53   Boot
```

But can't jump to it, because [[Windows Defender]] blocks us:

```
beacon> jump psexec64 fs.dev.cyberbotic.io smb
[-] Could not start service 633af16 on fs.dev.cyberbotic.io: 225
```

The file is detected as malicious. These Cobalt Strike "artifacts" are nothing more than shellcode runners that inject Beacon shellcode when executed.  As a rule of thumb, they inject the shellcode into themselves (e.g. using the VirtualAlloc & CreateThread pattern).  The service binary is the one exception, as it spawns a new process and injects the shellcode into that process instead (check [[Beacon Payload Generation]] .  This is done so that when moving laterally with PsExec, the artifact can be deleted from disk immediately.


---
#### 📦 - Structure

- [[Artifact Kit Structure]]

--- 

#### 🖊️ - Adding and Running to Cobalt Strike

- [[Build the Artifact Kit]]


⚠ Alert 1
⚠ Alert 2
⚠ Alert 3


--- 

 1️⃣ A
 2️⃣ B
 
--- 

❗C


### Properties
---
📆 created   {{28-02-2024}} 10:40
🏷️ tags: #redteam #crto 

---
