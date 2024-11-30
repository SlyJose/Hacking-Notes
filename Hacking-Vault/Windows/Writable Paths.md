
Always check for writable paths based on what the AppLocker rule says, and use those paths to write your payload.

You can also run this command to check write permissions:

`beacon> powershell Get-Acl C:\Windows\Tasks | fl`

Checking to see if we have write permissions on `Tasks`.


#### ⚠ Opsec




### Properties
---
📆 created   {{29-02-2024}} 15:06
🏷️ tags: #redteam #crto 

---

