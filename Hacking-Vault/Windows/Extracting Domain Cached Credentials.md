
Domain Cached Credentials (DCC) was designed for instances where domain credentials are required to logon to a machine, even whilst it's disconnected from the domain (think of a roaming laptop for example).  The local device caches the domain credentials so authentication can happen locally, but these can be extracted and cracked offline to recover plaintext credentials.

## 🖊️ via [[Mimikatz]]

The `lsadump::cache` Mimikatz module can extract these from `HKLM\SECURITY`.

To crack these with [hashcat](https://hashcat.net/hashcat/), we need to transform them into the expected format. The [example hashes page](https://hashcat.net/wiki/doku.php?id=example_hashes) shows us it should be `$DCC2$<iterations>#<username>#<hash>`.

### ⚠ Opsec

This command generates an alert opening a handle in the security registry hive.


### Properties
---
📆 created   {{11-02-2024}} 11:52
🏷️ tags: #redteam #crto 

---

