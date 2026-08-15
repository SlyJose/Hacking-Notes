
[PowerView](https://github.com/PowerShellMafia/PowerSploit) has long been the de-facto tool for domain enumeration.  One of its biggest strengths is that the queries return proper PowerShell objects, which can be piped to other cmdlets.  

This allows you to chain multiple commands together to form complex and powerful queries.

## 🖊️ Get-Domain

Returns a domain object for the current domain or the domain specified with `-Domain`. Useful information includes the domain name, the forest name and the domain controllers.

`beacon> powershell Get-Domain`

## 🖊️ Get-DomainController

Returns the [[Domain Controller]] for the current or specified domain.

`beacon> powershell Get-DomainController | select Forest, Name, OSVersion | fl`

## 🖊️ Get-ForestDomain

Returns all domains for the current [[Forest]] or the [[Forest]] specified by `-Forest`.

`beacon> powershell Get-ForestDomain`

## 🖊️ Get-DomainPolicyData

Returns the default domain policy or the domain controller policy for the current domain or a specified domain/domain controller. Useful for finding information such as the domain password policy.

`beacon> powershell Get-DomainPolicyData | select -expand SystemAccess`

## 🖊️ Get-DomainUser

Return all (or specific) user(s). To only return specific properties, use `-Properties`. By default, all user objects for the current domain are returned, use `-Identity` to return a specific user.

`beacon> powershell Get-DomainUser -Identity jking -Properties DisplayName, MemberOf | fl

## 🖊️ Get-DomainComputer

Return all computers or specific computer objects.

`beacon> powershell Get-DomainComputer -Properties DnsHostName | sort -Property DnsHostName

## 🖊️ Get-DomainOU

Search for all [[Organizational Unit]] (OUs) or specific OU objects.

`beacon> powershell Get-DomainOU -Properties Name | sort -Property Name

## 🖊️ Get-DomainGroup

Return all domain groups or specific domain group objects.

`beacon> powershell Get-DomainGroup | where Name -like "*Admins*" | select SamAccountName

## 🖊️ Get-DomainGroupMember

Return the members of a specific domain group.

`beacon> powershell Get-DomainGroupMember -Identity "Domain Admins" | select MemberDistinguishedName

## 🖊️ Get-DomainGPO

Return all [[Group Policy Objects]] or specific GPO objects. To enumerate all GPOs that are applied to a particular machine, use `-ComputerIdentity`.

`beacon> powershell Get-DomainGPO -Properties DisplayName | sort -Property DisplayName

## 🖊️ Get-DomainGPOLocalGroup

Returns all GPOs that modify local group membership through Restricted Groups or Group Policy Preferences.

You can then manually find which OUs, and by extension which computers, these GPOs apply to.

`beacon> powershell Get-DomainGPOLocalGroup | select GPODisplayName, GroupName

⚠  This shows that the Support Engineers group is being assigned some sort of local access to the machines to which these GPOs apply.  Although the GPO naming convention suggests this is local admin access, it may also be a different localgroup such as Remote Desktop Users.

## 🖊️ Get-DomainGPOUserLocalGroupMapping

Enumerates the machines where a specific domain user/group is a member of a specific local group.  

This is useful for finding where domain groups have local admin access, which is a more automated way to perform the manual cross-referencing described above.

`beacon> powershell Get-DomainGPOUserLocalGroupMapping -LocalGroup Administrators | select ObjectName, GPODisplayName, ContainerName, ComputerName | fl

Basically this will tell you into which computers does X domain group or user has access to based on Y GPO.

## 🖊️ Get-DomainTrust

Return all domain trusts for the current or specified domain.

`beacon> powershell Get-DomainTrust


### Properties
---
📆 created   {{11-02-2024}} 19:37
🏷️ tags: #redteam #crto 

---

