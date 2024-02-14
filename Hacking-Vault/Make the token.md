
The `make_token` command allows you to impersonate a user if you know their plaintext password. 

This works under the hood by calling the [LogonUserA](https://learn.microsoft.com/en-gb/windows/win32/api/winbase/nf-winbase-logonusera) API, which takes several parameters including a username, password, domain name and logon type.  In this instance, the `LOGON32_LOGON_NEW_CREDENTIALS` logon type is used, which allows the caller to clone its current [[Access Token]] and specify new credentials for outbound network connections.

The API outputs a handle to a token which can then be passed to the [ImpersonateLoggedOnUser](https://learn.microsoft.com/en-us/windows/win32/api/securitybaseapi/nf-securitybaseapi-impersonateloggedonuser) API.  This allows the calling thread to impersonate the context of token (i.e. the impersonated user's context).

## 🖊️ Usage

1) Run the command like this:

```
beacon> make_token DEV\jking Qwerty123
[+] Impersonated DEV\jking (netonly)
```


### ⚠ Opsec




### Properties
---
📆 created   {{14-02-2024}} 09:52
🏷️ tags: #redteam #crto 

---

