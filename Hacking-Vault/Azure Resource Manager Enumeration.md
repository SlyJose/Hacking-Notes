
##### **Manual Enumeration**

1) Log in with obtained credentials:
`az login`

2) Get some details about your current session
`az account show`

==Subscription==
3) Get all the subscriptions:
`az account list --all`

4) Check an specific subscription:
`az account show -s Subscription-ID/Name`

==Resource Groups==
5) Remember resource groups are containers for resources:
`az group list -s Subscription-ID/Name`

6) Get the list of resources in a subscription:
`az group list -s Subscription-ID/Name`

7) Check all the resources in the tenant:
`az resource list`

8) Check the resources in a resource group:
`az resource list --resource-group ResourceGroupName`

==Role Assignment==

We now want to check how the roles are assigned to objects. 

9) List roles assigned in specified subscription: 
`az role assignment list --subscription Subscription-ID/Name`
You will see first the security principal or objects the permission is assigned to, then the scope (object) that has the role assigned to it, and finally the type of role assigned.

So for example you may see a Security Principal with a Owner role assigned to an specific subscription.

10) Lists of roles assigned in current subscription and inherited
`az role assignment list -all`

11) List of roles assigned to an identity (user, service principal, identity)
`az role assignment list --assignee ObjectID/Sign-InEmail/ServicePrincipal --all`

Now that you've narrowed down the interesting roles, you can check their details:

11) Check all the assigned roles
`az role definition list`

12) Get info on a specific role:
`az role definition list -n RoleName`

13) Also check for custom roles with assigned permissions:
`az role definition list --custom-role-only`



### Properties
---
📆 created   {{30-12-2024}} 17:13
🏷️ tags: #cloud #azure 

---


