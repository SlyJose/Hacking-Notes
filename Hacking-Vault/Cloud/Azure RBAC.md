Authorization system built on Azure Resource Manager
(ARM).

Azure ARM roles control access to Azure cloud resources like virtual machines, storage accounts, and networks within your subscription. These roles are not the same as Entra ID roles, check the [[Difference between Roles ARM vs Entra ID]]

#### 🚀 - Role Assignment
---
Process of attaching a user, group, [[Service Principal]], or managed identity ([[Security Principal]]) to a role definition at a particular scope for the purpose of granting access.

![[azure-roleassignment.png]]

1. [[Security Principal]]
2. [[Scope]]  
3. [[Roles Definition]]

All assignments are inhered.
All assignments can be done through the API:

```
{HTTP method} https://management.azure.com/{version}/{resource}?{query-parameters}
```

####  📗 Role Difference

Check [[Difference between Roles ARM vs Entra ID]]


### Properties
---
📆 created   {{30-12-2024}} 09:48
🏷️ tags: #cloud #azure 

---
