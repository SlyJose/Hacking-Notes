
It is necessary to acquire an initial set of valid [[Active Directory]] credentials before attempting other attacks like privilege escalation or lateral movement. A low level privilege user will work since there are many avenues to move from there.

Being an outsider (not contact to the AD environment), you can try the following methods to obtain [[Active Directory]] credentials:

## 🚀 - Methods
---
1. [[Brute Forcing Authentication Services]]
2. [[LDAP Pass-back Attack]]
3. [[Authentication Relays]]
4. [[Microsoft Deployment Toolkit]]
5. Configuration Files

---

#### 🖊️ Domain Dominance

Once an attacker achieves a very high level of privilege in the domain, such as Domain or Enterprise Admin, it's very difficult for a defending organisation to recover or even consider the forest as "clean" again.  Attackers with this access can extract credential material from the domain and use it to re-obtain DA-level access at any time.  The majority of these credentials are never changed, which grants practically unlimited access.

- [[Silver Tickets]]
- [[Golden Tickets]]
- [[Diamond Tickets]]


### Properties
---
📆 created   {{26-10-2023}} 18:42
🏷️ tags: #activedirectory   #crto 

---
