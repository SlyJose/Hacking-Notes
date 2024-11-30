
#### 🚀 - Local CLI Enumeration
---
If you land in a victim machine and wish to enumerate accounts, credentials, projects, follow a hierarchical order:

-Check the organization:
`gcloud organizations list`    - it may not show anything because you have no permissions

-Check org policy
`gcloud organizations get-iam-policy [organization id]`

-Enumerate the projects:
`gcloud config list`
`gcloud projects list`

-Check project policy
`gcloud organizations get-iam-policy [project id]`  - you will see the members that have access to certain roles in that resource (the project id). Also check the type of access they have (owner, edit, read)

-List projects:
`gcloud projects list`     - it will show only the projects you have access to

-Check configured service accounts in the box:
`$ gcloud auth list`

-Check all the service accounts:
`gcloud iam service-accounts list`   - check out names and email, identify default service accounts and user managed service accounts (check [[Google Service Accounts]] for guidance)

-Now having service accounts, you can check policies associated to them:
`gcloud iam service-accounts get-iam-policy [email of service account]`

This command will show you (not all the time) the members associated to that service account, and the permissions (roles) they have over that service account.

-Check keys of a service account:
`gcloud iam service-accounts keys list --iam-account [service account email]`

-Check all the roles
`gcloud iam roles list`

-Check the description of a role
`gcloud iam roles describe roles\[name of role]`
This will show you all the permissions in that role

-Check custom roles:
`gcloud iam roles list --project [project id]`
You will see in the 'title' if there are any custom roles

-Always check the description of custom roles:
`gcloud iam roles describe [name of role] --project [name of project]`
You will see the permissions on the project.



1. A
2. B
3. C

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
📆 created   {{30-11-2024}} 17:10
🏷️ tags: #cloud #google 

---
