
####  CLI Access Tools

You can use these tools to authenticate:

- Az [Cross Platform]
- Az Powershell
- MgGraph Powershell

Microsoft CLI graph is used for Office 365 management, unlike AZ which is used for resource management.

Your access will depend on your credentials either [[Azure Long Term Credentials]] or [[Azure Short Term Credentials]]

#### 📦 - AZ
--- 

This cross platform (Linux, Windows, Mac) will route you to the browser to finish up login in. To start the authentication you run:

`az login`

Or, you can ingest your credentials in the command:

`az login --service-principal -u ApplicationID -p Password --tenant TenantID`


#### 🖊️ - AZ Powershell
---
Used in Windows, this tool also allows you to connect:

`> Connect-AzAccount`

Or, use your [[Service Principal]]:

```
> $cred = Get-Credential 
> Connect-AzAccount -ServicePrincipal -Tenant [TentantID] -Credential $cred
```

Or use an access token (short time):

```
> az account get-access-token
--resource=https://management.azure.com
> Connect-AzAccount -AccessToken "[AADAccessToken, keep the quotes]"
```

#### 🚀 Mg Graph Powershell
--- 

Used in Windows, this tool also allows you to connect:

`Connect-MgGraph -Scopes "Directory.Read.All"`

This will prompt you to the browser. 

### Properties
---
📆 created   {{30-12-2024}} 12:28
🏷️ tags: #cloud #azure 

---
