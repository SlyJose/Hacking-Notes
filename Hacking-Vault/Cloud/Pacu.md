
Installation: https://github.com/RhinoSecurityLabs/pacu

#### 📦 - Set PACU
--- 
Pacu allows to enumerate, escalate privileges, and many other exploitation uses. 

Once the tool is installed, set a pair of credentials:

> set keys

==If you can across a new set of credentials you can use the set keys command again to add the new ones, just change the key alias value every time.==

You can check all the commands with "help" command. 


#### 🚀 - Enumeration
---

-Enumerate permissions:
> exec iam__enum_permissions

-Also, check all your user permissions:
> whoami

You may not get an output with whoami due to your lack of permissions to GET information about the user, but later on you can use privesc checkers to try to escalate and obtain this privilege.

-Check for ec2 instances (this will enumerate every region if you want, or you can select one specifically):
> exec ec2__enum

This check will give you an overview of all the products and resources that the user may have access to. It will take some time to run it all.

-Check all the data about EC2 instances:
> data EC2

This will prompt you with info about ALL the resources, you have to scroll and find the one about EC2s in case you are searching for it.

-Check for policy's and roles that could be used for privilege escalation:
> exec iam__privesc_scan

Pacu will provide you with a follow up in the attack, giving you possible scenarios to continue the privesc.


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
📆 created   {{23-12-2024}} 12:50
🏷️ tags: #cloud #aws 

---
