Define a set of permissions for making AWS service requests. It means, a group of permissions attached to a service.

These roles **can only** be attached to compute instances (EC2, lambdas, etc).

#### 🚀 - Structure
---
![[aws-roles-structure.png]]

It would be a bad security practice if resources like compute instances had the same permissions as the users that use them, this would allow the resource control over other resources that have nothing to do with them.

To control that issue, roles are given to resources to avoid them having access to other resources that they shouldn't. Just like permissions can be attached to users and groups, roles are attached to resources to control their access. 

In the image example, a role is given to the EC2 instance to access an S3 bucket.

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
📆 created   {{15-12-2024}} 11:12
🏷️ tags: #cloud #aws 

---
