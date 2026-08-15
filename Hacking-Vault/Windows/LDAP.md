Lightweight Directory Access Protocol (LDAP)

[[Active Directory]] authentication method that ==applications== can use. This is NOT USED TO AUTHENTICATE IN ACTIVE DIRECTORY.

LDAP authentication is similar to [[NetNTLM]] authentication, however, with LDAP authentication, the application directly verifies the user's credentials. The application has a pair of AD credentials that it can use first to query LDAP and then verify the AD user's credentials.

# 🖊️ LDAP Authentication

LDAP authentication is a popular mechanism with third-party (non-Microsoft) applications that integrate with AD. These include applications and systems such as:

- [[Gitlab]]
- [[Jenkins]]
- Custom-developed web applications
- Printers
- VPNs

The process of authentication through LDAP is shown below:
![[LDAP.png]]

# 📔 Risks

If any application or service is using [[LDAP]]  for authentication and exposed on the internet, the same type of attacks as those leveraged against [[NetNTLM]] authenticated systems can be used. 

However, since a service using [[LDAP]] authentication requires a set of [[Active Directory]] credentials, it opens up additional attack avenues. In essence, we can attempt to recover the AD credentials used by the service to gain authenticated access to AD.  

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{26-10-2023}} 19:32
🏷️ tags: #activedirectory #protocol  
---

