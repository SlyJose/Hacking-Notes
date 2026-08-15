

#### 🚀 - CLI Based Enumeration
---

#### User enumeration

-Check the user identity
`aws sts get-caller-identity --profile [profile file name]`

We will see the ID, and type of user.

-List all users
`aws iam list-users`

-Always check to what groups a user belongs to.
`aws iam list-groups-for-user --user-name [user-name]`
OR
`aws iam list-groups-for-user --user-name [user-name] --profile [profile name]`

Group's permissions are inhered to all its users, so it's important to know who belongs where.

-List any policies attached to a user

Remember there are 2 types of policies ([[AWS IAM Policies]]). Inline ones are very attractive for hackers.

Managed:
`aws iam list-attached-user-policies --user-name [user-name]`
OR
`aws iam list-attached-user-policies --user-name [user-name] --profile [profile name]`

Inline:
`aws iam list-user-policies --user-name [user-name]`
OR
`aws iam list-user-policies --user-name [user-name] --profile [profile name]`

#### Group Enumeration

-List groups
`aws iam list-groups`

-List users in a group:
`aws iam get-group --group-name [group-name]`

-List managed policies of a group:
`aws iam list-attached-group-policies --group-name [group-name]`

-List inline policies of a group:
`aws iam list-group-policies --group-name [group-name]`

#### Roles Enumeration

-List roles in the AWS account
`aws iam list-roles`
OR
`aws iam list-roles --profile [profile name]`

-List managed policies of a role:
`aws iam list-attached-role-policies --role-name [ role-name]`
OR
`aws iam list-attached-role-policies --role-name [ role-name] --profile [profile name]`

-List inline policies of a role:
`aws iam list-role-policies --role-name [ role-name]`
OR
`aws iam list-role-policies --role-name [ role-name] --profile [profile name]`

#### Policy Enumeration

Once you have checked which policies are assigned to users, groups, and roles, you will want to see what are the actual permissions of those policies.


**Managed Policies**

**Important:** These commands will show the permissions of managed policies, not inline policies.

-List policies of a profile
`aws iam list-policies --profile [name of policy]`

There will be a lot of polices, since AWS has their own. You can distinguish AWS from custom in their "Arn" value. If it has the account ID number in that parameter, then it is custom. If not, then it is managed by AWS.

-List information about one specific policy
`aws iam get-policy --policy-arn [policy-arn]`
OR
`aws iam get-policy --policy-arn [policy-arn] --profile [profile name`

-Policies can have multiple versions, so you will want to check that too:

`aws iam list-policy-versions --policy-arn [policy-arn]`
OR
`aws iam list-policy-versions --policy-arn [policy-arn] --profile [profile name]`

A version may have higher permissions than the other, so you will want to use the correct one. If you as an attacker have the permission to change the version in use of a policy, you could try that and use a higher privilege policy.

-List the permissions of a policy in an specific version:
`aws iam get-policy-version --policy-arn policy-arn --version-id [version-id]`
OR
`aws iam get-policy-version --policy-arn policy-arn --version-id [version-id] --profile [profile name]`

**Inline Policies**

**Important:** These commands will show the permissions of inline policies, not managed policies.

-List permissions of a user's inline policy
`aws iam get-user-policy --user-name user-name --policy-name [policy-name] --profile [profile name]`

-List permissions of a group's inline policy:
`aws iam get-group-policy --group-name group-name --policy-name [policy-name] --profile [profile name]`

-List permissions of a role's inline policy:
`aws iam get-role-policy --role-name role-name --policy-name [policy-name] --profile [profile name]`



---
#### 📦 - B
--- 

#### 🖊️ - C


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
📆 created   {{15-12-2024}} 12:31
🏷️ tags: #cloud #aws 

---
