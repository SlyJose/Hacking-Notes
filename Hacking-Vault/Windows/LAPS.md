
**Local Administrator Password Solution

Organisations often have a build process for physical and virtual machines within their environment.  It's common that everything is built from the same "gold image" to ensure consistency and compliance.  However, these processes can result in every machine having the same password on accounts such as the local administrator.  If one machine and therefore the local administrator password hash is compromised, an attacker may be able to move laterally to every machine in the domain using the same set of credentials.

#### 🚀 - What is it

---
LAPS is a Microsoft solution for managing the credentials of a local administrator account on every machine, either the default RID 500 or a custom account.  It ensures that the password for each account is different, random, and automatically changed on a defined schedule.

#### 📦 - How it works

- [[LAPS Process]]

---

#### 🖊️ - Attacking LAPS

- [[Enumerating LAPS]]
- [[Persistence LAPS via Password Expiration]]
- [[LAPS Backdoors]]

--- 

❗C


### Properties
---
📆 created   {{27-02-2024}} 21:11
🏷️ tags: #redteam #crto 

---
