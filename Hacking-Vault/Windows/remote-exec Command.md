
[[Cobalt Strike]] strategy to jump into another machine.

The `remote-exec` commands simply provide a means of executing commands on a remote target.  They are therefore not exclusive to lateral movement, but they can be used as such.  

They require more manual work to manage the payload, but do offer a wider degree of control over what gets executed on the target.  You also need to connect to P2P Beacons manually using `connect` or `link`.

## 🖊️ Usage

The syntax is `remote-exec [method] [target] [command]`

The available methods are:

```
beacon> remote-exec

Beacon Remote Execute Methods
=============================

    Methods                         Description
    -------                         -----------
    psexec                          Remote execute via Service Control Manager
    winrm                           Remote execute via WinRM (PowerShell)
    wmi                             Remote execute via WMI
```

- [[Windows Remote Management WinRM]]
- [[PSExec]]
- [[Windows Management Instrumentation WMI]]

### ⚠ Opsec




### Properties
---
📆 created   {{14-02-2024}} 10:52
🏷️ tags: #redteam #crto 

---

