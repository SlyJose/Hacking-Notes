
[[Kerberos]] tickets can help an attacker perform malicious actions.

#### Methods where you are extracting existing tickets

- [[Kerberoasting]] (extracts TGS)
- [[extract TGTs with Rubeus]] (extracts TGT)
- [[Local Delegation]] (extracts local TGT)
- [[Unconstrained Delegation]] (extracts TGT)
- [[Constrained Delegation]] (uses TGT to ask for TGS)

#### Methods where you are asking for a new ticket

- [[AS-REP Roasting]] (asks for TGT)


---

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

