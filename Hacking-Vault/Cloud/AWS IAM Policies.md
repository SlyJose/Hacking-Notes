Define the permissions for an action to perform the operation.

They are attached to IAM identities.

Example:

>If a policy allows the GetUser action, then a user with that policy can get user
information from the AWS Management Console, the AWS CLI, or the AWS API.

#### 📦 - Management
--- 

- AWS Managed: which are handled by AWS
- Customer Managed: which are handled by the customer

#### 🚀 - Policy Types
---
- **Inline Policy**: embedded in a IAM Identity. Can be attached to one identity only. These are very attractive to hackers since they may allow extra permissions.
- **Managed Policy**: can be attached to multiple identities at a time.

---
#### 🖊️ - Policy Data


⚠ Effect: Allow or Deny Access
⚠ Action: (Get, Put, Delete) actions allowed or denied
⚠ Resource: list of resources to which the actions apply


--- 

 1️⃣ A
 2️⃣ B
 
--- 

❗C


### Properties
---
📆 created   {{15-12-2024}} 11:25
🏷️ tags: #cloud #aws 

---
