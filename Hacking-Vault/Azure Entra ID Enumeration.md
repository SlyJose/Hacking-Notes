
1) First, we can check if the organization has an Entra ID:
`https://login.microsoftonline.com/getuserrealm.srf?login=Username@DomainName&xml=1`

You can replace the domain with your target.

2) Notice both the username and also if the "NameSpaceType" tag is in there. If it says "Managed", it will mean is probably using Entra ID authentication.

If you have credentials of a pwned user, you can start doing further enumeration:

- Auth with graph powershell
`Connect-MgGraph -Scopes "Directory.Read.All" `

- Log in with the credentials you have
- Check the context of the user
`Get-MgContext`
You will see different user settings

==Directory Roles==

- Check all the directory roles, in here you will see who is the user or owners of all the directories (super admin kind of):
`Get-MgDirectoryRole | ConvertTo-Json`
Check all the different directory roles as well as to identify an administrator one as target.
Objects are assigned to these roles remember, their permissions depend of these.

- Check the members of directory roles now:
`Get-MgDirectoryRoleMember -DirectoryRoleId [Directory RoleID] -All | ConvertTo-Json`
Since you identified the important roles in the previous step, use their ID now to check its members

==Users and Groups==

- Check all the users in the tenant (domain):
`Get-MgUser`

- Check all the roles and groups a user is member of:
`Get-MgUserMemberOf -UserId [UserID]`

- Check all the groups:
`Get-MgGroup`

- Check members of a group:
`Get-MgGroupMember -GroupId [GroupID] | ConvertTo-Json`

==Applications==

- Check all the applications
`Get-MgApplication`

- Details of an specific app:
`Get-MgApplication -ApplicationId [ApplicationObjectID] | ConvertTo-Json`

- Check the owner of an application:
`Get-MgApplicationOwner -ApplicationId [ApplicationObjectID] | ConvertTo-Json`

- Check the permissions of an application:
```
$app= Get-MgApplication -ApplicationId [ApplicationObjectID]
$app.RequiredResourceAccess
$app.RequiredResourceAccess | ConvertTo-Json
```
If the victim sets up an application in the tenant, application permissions and access tokens are created, if the application can be accessed externally then that is turned into a service principal. 

Applications have two types of permissions, the ones assigned to the app (Role) and the delegated ones (where the user agrees access to certain resources). On delegated permissions, applications will access the data the user gave consent to access.

Any "Scope" tags are for delegated permissions, and any "Role" tags are for application permissions.

- Check the details of a delegation permission:
`$app= Get-MgApplication -ApplicationId [ApplicationObjectID] $app.Oauth2RequirePostResponse | ConvertTo-Json`

- Now, check the details of a the role permission (you can also use it for delegation permission):
`$res=Get-MgServicePrincipal -Filter "DisplayName eq 'Microsoft Graph'"`
`$res.AppRoles | Where-Object {$_.ID -eq 'AppRoleID’} | ConvertTo-Json`

You must replace the "AppRoleID" with the permission ID (obtained from the previous check looking for app permissions)