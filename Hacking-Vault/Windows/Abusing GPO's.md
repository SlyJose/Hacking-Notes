
By default, only Domain Admins can create [[Group Policy Objects]] and link them to OUs, but it's common practice to delegate those rights to other teams.  This delegation is typically assigned to domain groups - for example, a "Workstation Admins" group may have rights to manage GPOs that apply to a "Workstation" OU.  

These can create privilege escalation opportunities by allowing user to apply malicious GPOs to domain admin users or their computers.

#### 🚀 - [[Modify Existing GPO]]

#### 📦 - [[Create and Link GPO]]
--- 


### Properties
---
📆 created   {{20-02-2024}} 20:16
🏷️ tags: #redteam #crto 

---
