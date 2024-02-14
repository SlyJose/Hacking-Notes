
The `spawn` command will spawn an x86 or x64 process and inject shellcode for the specified listener into it.

```
beacon> spawn x64 http
```

![[spawn.png]]

In this example, I have a DNS Beacon checking in from bfarmer every 1 minute.  Instead of operating through this Beacon, I want to leave it open as a lifeline on a slow check-in.  In which case, I can spawn a new HTTP session and work from there instead.

### ⚠ Opsec




### Properties
---
📆 created   {{14-02-2024}} 14:53
🏷️ tags: #redteam #crto 

---

