

#### 📔 - memory scans

Malicious processes can also trigger alerts during memory scans, Defender will also carry process scans to determine weird patterns in form and actions. 

###### 🚀 - Cobalt Strike
---
1. Beacon is also susceptible to being caught whilst it's running in memory or on a process scan. 
2. Your artifact payload may be fine (no signature detections) but your raw shellcode could be the one causing a detection. The problem is beacon source code is not public. 
3. To handle this problem we use [[Malleable C2 Profile]], which can mold the actions of the beacon in memory. 

--- 

#### 📗 AMSI Detections 

Windows Defender utilizes [[AMSI]] to identify fileless malware and malicious input content. 


###### 🚀 - Cobalt Strike
---

**Resource detection**
1. Cobalt Strike can generate powershell, python, VBA, and other payloads that run in memory, these can be detected by AMSI. Cobalt Strike's [[Resource Kit]] helps Red Teamer's evade Behavioral detections. 
2. If resources are still detected by Defender check [[Manual AMSI Bypass]]

**Post-Ex Detection**
1. Also in various post-exploitation commands AMSI will detect you.  To name a few are `powershell`, `powerpick` and `execute-assembly`.  This occurs because Beacon will spawn new processes to execute these commands, and each process gets its own copy of AMSI.
2. AMSI also detects .NET assemblies known to be malicious, even though they don't touch disk:

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe
[-] Failed to load the assembly w/hr 0x8007000b
[-] lost link to parent beacon: 10.10.123.102
```

It would be very time demanding to scan and obfuscate everything, so Cobalt Strike introduced a configuration that we can apply in Malleable C2 called `amsi_disable`.

- [[AMSI Disable]]

--- 



### Properties
---
📆 created   {{28-02-2024}} 15:16
🏷️ tags: #redteam #crto 

---

