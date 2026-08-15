
Also known as "Systems Center Configuration Manager" (SCCM).  Fundamentally, SCCM's role is to help with system management tasks such as application & software deployments, updates and compliance configuration & reporting.

#### 🚀 - Functionality
---
The complexity of a deployment can vary from a single site, to a sprawling hierarchy of multiple primary and secondary sites.  

The ability to connect multiple sites helps with scalability, particularly when dealing with different geographic locations.  The examples below are taken directly from the Microsoft documentation.

![[MCM.png]]

---

#### 🖊️ - Attack Vector

SCCM is an attractive target for attackers because given enough privilege, it can be used to push malicious scripts and applications to devices that it manages.  The deployment in the RTO lab is only setup as a single site in order to demonstrate basic abuse primitives against Configuration Manager.  There are further resources out there, including [this](https://medium.com/specter-ops-posts/sccm-hierarchy-takeover-41929c61e087) post by [Chris Thompson](https://twitter.com/_Mayyhem) that describes how a compromised primary site also compromises the entire hierarchy.

❗ [[MCM Enumeration]]
❗ [[Network Account Credentials]]
❗[[MCM Lateral Movement]]


⚠ Alert 1
⚠ Alert 2
⚠ Alert 3


--- 

 1️⃣ A
 2️⃣ B
 
--- 

❗C


### Properties
---
📆 created   {{24-02-2024}} 20:50
🏷️ tags: #redteam #crto 

---
