
Windows Authentication protocol. This protocol is used when a user wants to authenticate in a Windows network. It has been replaced partially by [[Kerberos]].


# 🚀 - Authentication
---
This protocol uses another method for authentication: [[NetNTLM Authentication Process]]

While [[NetNTLM]] should be considered obsolete, most networks will have both ([[NetNTLM]] and [[Kerberos]]) protocols enabled.

Authentication protocols like [[NetNTLM]] use [[SMB]] for transport help over the network and reach their workstation/server destinations. 

---

# 📜 Services using NetNTLM

Services that use [[NetNTLM]] can also be exposed to the internet. Check [[NetNTLM Authenticated Services]]


# ❗❗ Authentication Risks

- Since the [[NetNTLM]] Challenges can be intercepted, we can use offline cracking techniques to recover the password associated with the [[NetNTLM]] Challenge. However, this cracking process is significantly slower than cracking [[NetNTLM]] hashes directly.
- We can use a rogue device to stage a man in the middle attack, relaying the [[SMB]] authentication between the client and server, which will provide us with an active authenticated session and access to the target server.

### Properties
---
📆 created   {{22-10-2023}} 21:14
🏷️ tags: #activedirectory   
---

