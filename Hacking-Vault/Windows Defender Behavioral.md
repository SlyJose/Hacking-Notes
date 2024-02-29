
Defender identifies threats based on process behaviors on endpoints, even when attacks are already in progress.

####  📗 Behavior 

The behavior alerts look something like this:

```
PSComputerName                 : fs
ProcessName                    : C:\Windows\System32\rundll32.exe
RemediationTime                : 9/14/2022 5:40:03 PM
Resources                      : {behavior:_pid:4964:111820579542652, 
                                 process:_pid:4040,ProcessStart:133076508002529669, 
                                 process:_pid:4964,ProcessStart:133076507626927382}
```


###### 🚀 - Cobalt Strike
---
When Cobalt Strike runs a post-ex command that uses the [fork & run](https://hstechdocs.helpsystems.com/manuals/cobaltstrike/current/userguide/content/topics/appendix-a_beacon-opsec-considerations.htm) pattern, it will spawn a sacrificial process, inject the post-ex capability into it, retrieve the output over a named pipe, and then kill the process.  The primary reason to do this is to ensure that unstable post-ex tools don't crash the Beacon.

rundll32 being the default "spawnto" for Cobalt Strike has been [a thing](https://www.cobaltstrike.com/blog/why-is-rundll32-exe-connecting-to-the-internet/) for a long time and is now a common point of detection.  The service binary payload used by psexec also uses this by default, which is why you see those Beacons running as rundll32.exe.

- [[Behavioral Detection Evasion]]

--- 
###### 📦 - Parent/Child Detections

A subset of behavioural detections come from the parent/child relationships of running processes.  As a rule of thumb, the parent of a process is the process that started it.  As such, a process only has one parent but can have many children.

There are many parent/child relationships that are considered ==highly suspicious== like Word (parent) -> powershell (child)

- [[Parent Child Evasion]]

---
###### 🖊️ - Command line detections

Defender also has the ability to inspect the command line arguments of a process and can prevent it from starting if it has malicious content, for example using the normal `pth` command in Cobalt Strike.

This occurs because Defender has a driver in the kernel. This driver receives [notifications](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/ntddk/nc-ntddk-pcreate_process_notify_routine_ex) when new processes are being created, which contain a  [PS_CREATE_NOTIFY_INFO](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/ntddk/ns-ntddk-_ps_create_notify_info) structure.  The eagle-eyed will spot the _CreationStatus_ member, which is the NTSTATUS value to return.  The driver can simply set this to STATUS_ACCESS_DENIED to prevent the process from starting, and that error code propagates down to the caller.

There are manual situations to avoid this detection, for example: [[Manual PTH]]

### Properties
---
📆 created   {{28-02-2024}} 15:17
🏷️ tags: #redteam #crto 

---

