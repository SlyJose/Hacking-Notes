
Let's admins authorize who can take action on specific resources. 
It follows a Resource based policy instead of identity based policy. 
IAM policies are attached to resources not identities.

#### 🖊️ Organization of IAM

![[gcp-IAMstructure.png]]

Google allows to apply policies to resources, inside that policy you can see which accounts, roles, and members have access to it and what kind of permissions they have in that resource.

All permissions are inherited, so if you assign high permissions at the organization level, these will be passed down to all the other resources. Check [[Google Cloud Platform]] architecture to understand hierarchy of resources.
#### 📔 Identity

Known as members. Can be: google account (for end users), [[Google Service Accounts]] (for apps and virtual machines), a group, a google workspace, or a cloud identity domain. 

The identity of a member is an email address associated with the member.

Types of members:

- Google Account
- Service Account
- Google group
- Google workspace domain
- Cloud identity domain
- All authenticated users
- All users

####  📗 Roles

Permissions. Determines what operations are allowed on resources. When you grant a role to a member, all permissions in that role are given to that member.

Types:
- Owner
- Editor
- Viewer
- Predefined roles: already configured
- Custom

Each role has a list of permissions.

#### 🖊️ Authentication

- Long Term (GUI, CLI/SDK): you use username and password or json files to get into gmail or gsuite
- Short Term (CLI, SDK,API): you use an access token to use the services provided

The CLI access uses the [[gcloud]] binary to contact the cloud platform. Gcloud can use json files to authenticate, they work as a certificate.



### Properties
---
📆 created   {{30-11-2024}} 15:10
🏷️ tags: #cloud #google 

---

