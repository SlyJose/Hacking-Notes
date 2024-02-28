

 1) The Active Directory schema is extended and adds two new properties to computer objects, called _ms-Mcs-AdmPwd_ and _ms-Mcs-AdmPwdExpirationTime_.
2. By default, the DACL on ms-Mcs-AdmPwd only grants read access to Domain Admins.  Each computer object is given permission to update these properties on itself.
3. Rights to read AdmPwd can be delegated to other principals (users, groups etc), which is typically done at the OU level.
4. A new GPO template is installed, which is used to deploy the LAPS configuration to machines (different policies can be applied to different OUs).
5. The LAPS client is also installed on every machine (commonly distributed via GPO or a third-party software management solution).
6. When a machine performs a gpupdate, it will check the AdmPwdExpirationTime property on its own computer object in AD. If the time has elapsed, it will generate a new password (based on the LAPS policy) and sets it on the ms-Mcs-AdmPwd property.

#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 08:58
🏷️ tags: #redteam #crto 

---

