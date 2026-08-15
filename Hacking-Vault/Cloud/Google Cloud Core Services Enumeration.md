
==Method==

1) Enumerate all the orgs and projects
2) Enumerate all the accounts in the box
3) Enumerate your account roles in the projects


#### 🚀 - Local CLI Enumeration - Manual
---
If you land in a victim machine and wish to enumerate accounts, credentials, projects, follow a hierarchical order of search.

`*Start creating a diagram of your access in order to understand the user's access.*`

If you find multiple service accounts either in the system or you stole (an access token for example), you can either try to auth with that account:

`gcloud auth activate-service-account --key-file authfile.json`
==Make sure== you configure the project ID as well, otherwise you will get errors:
`gcloud config set project [project ID]`

Or, enumerate using a token:

`[gcloud enum command]--access-token-file tokenfile.txt`

###### Organization & Projects

-Check the organization:
`gcloud organizations list`    - it may not show anything because you have no permissions

-Check org policy
`gcloud organizations get-iam-policy [organization id]`

-Enumerate the projects:
`gcloud config list`
`gcloud projects list`
As an attacker,  you want to enumerate each project completely and individually.

-Check project policy
`gcloud projects get-iam-policy [project id]`  - you will see the members that have access to certain roles in that resource (the project id). Also check the type of access they have (owner, edit, read)

###### Accounts

-Check configured service accounts in the box:
`$ gcloud auth list`

-Check all the service accounts:
`gcloud iam service-accounts list`   - check out names and email, identify default service accounts and user managed service accounts (check [[Google Service Accounts]] for guidance)

-Now having service accounts, you can check policies associated to them:
`gcloud iam service-accounts get-iam-policy [email of service account]`

This command will show you (not all the time) the members associated to that service account, and the permissions (roles) they have over that service account.

-Check keys of a service account:
`gcloud iam service-accounts keys list --iam-account [service account email]`

###### Roles

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

-Check the roles a service account has on a project:
```
gcloud projects get-iam-policy [project ID] --flatten="bindings[].members" --filter="bindings.members=serviceaccount:[service account ID]" --format="value(bindings.role)"
```

-Check the description of all the service accounts and find their associated roles:
`gcloud projects get-iam-policy alert-nimbus-335411 --format="json" --access-token-file token.txt > roles.txt`

###### Assets
Even if you only have viewer access in a project, always start checking of resources in the project like machines, DBs, containers, etc.

-Check for **compute instances**:
`gcloud compute instances list`

Try to reach any instances either over the browser or scan it. Attempt any exploitation in them.

-Check for **storage instances**:
`gcloud storage ls`
Or
`gcloud storage ls --access-token-file [token auth file.txt]`  - if you have a token file for another account

This will give you all the storage resources you may have access to.

-Check for files inside storage resources:
`gcloud storage ls [storage resource url] --access-token-file token.txt`
Example:
`gcloud storage ls gs://devops-storage-metatech --access-token-file token.txt`

-Obtain a file from storage:
`gcloud storage cp [storage resource url] . --access-token-file token.txt `

==Always check the service accounts associated with new instances or resources you discover:==
- For compute instances:
`gcloud compute instances describe INSTANCE_NAME --zone=ZONE --format="get(serviceAccounts)"
`
-> Repeat enumeration if any new accounts are discovered

-Check for **local files**:
gcloud files location: `/home/user/.config/gcloud`

#### 🚀 - Local CLI Enumeration - Automated

You can use this tool to perform an initial enumeration of the environment, but based on the output proceed to continue your analysis of it:

[gcp_enum](https://gitlab.com/gitlab-com/gl-security/threatmanagement/redteam/redteam-public/gcp_enum)



### Properties
---
📆 created   {{30-11-2024}} 17:10
🏷️ tags: #cloud #google 

---
