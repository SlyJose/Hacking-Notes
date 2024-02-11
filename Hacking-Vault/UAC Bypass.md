User Account Control (UAC) is a technology that exists in Windows which forces applications to prompt for consent when requesting an administrative access token.

A UAC "bypass" is a technique that allows a medium integrity process to elevate itself or spawn a new process in high integrity, without prompting the user for consent.  Being in high integrity is important for attackers because it's required for various post-exploitation actions such as dumping credentials.

## 🚀 - [[Cobalt Strike]] [Elevate Kit](https://github.com/cobalt-strike/ElevateKit)
---
1. The ElevateKit comes with several techniques including UAC bypasses:

- uac-eventvwr: Bypass UAC with eventvwr.exe
- uac-schtasks: Bypass UAC with schtasks.exe (via SilentCleanup)
- uac-wscript: Bypass UAC with wscript.exe

For example:

```
beacon> elevate uac-schtasks tcp-local
[*] Tasked Beacon to run windows/beacon_bind_tcp (127.0.0.1:4444) in a high integrity context
[+] established link to child beacon: 10.10.123.102
```

![[uac-bypass.png]]

---

### Properties
---
📆 created   {{10-02-2024}} 15:15
🏷️ tags: #redteam #crto   

---
