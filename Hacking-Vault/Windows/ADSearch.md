
[ADSearch](https://github.com/tomcarver16/ADSearch) has fewer built-in searches compared to PowerView and SharpView, but it does allow you to specify custom Lightweight Directory Access Protocol [[LDAP]] searches.  These can be used to identify entries in the directory that match a given criteria.

## 🖊️ Searches

- Get Users

`beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Release\ADSearch.exe --search "objectCategory=user"`

- Get domain groups (with word filter)

`beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Release\ADSearch.exe --search "(&(objectCategory=group)(cn=*Admins))"`

- Filter by attributes and parameters:
These can be made more complex with further AND, OR and NOT conditions.  All attributes can be returned using the `--full` parameter, or specific attributes with the `--attributes` parameter.

`beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Release\ADSearch.exe --search "(&(objectCategory=group)(cn=MS SQL Admins))" --attributes cn,member`


## 📔 Description

- 

##  📗 Action to perform 


### ⚠ Opsec




### Properties
---
📆 created   {{11-02-2024}} 20:10
🏷️ tags: #redteam #crto 

---

