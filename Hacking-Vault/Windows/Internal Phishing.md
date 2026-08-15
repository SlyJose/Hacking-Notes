
Sending a payload to the phished user(s) is a direct way to gain a foothold, since it will be executed on their system.  There are broadly two options for delivering a payload.

1. Send a URL where a payload can be downloaded.
2. Attach the payload to the phishing email.

## 🖊️ OPSEC

Any file downloaded via a browser (outside of a trusted zone) will be tainted with the "Mark of the Web" (MOTW).
```
PS C:\Users\bfarmer\Downloads> gc .\test.txt -Stream Zone.Identifier
[ZoneTransfer]
ZoneId=3
HostUrl=http://nickelviper.com/test.txt
```

In a nutshell, this is a data stream that gets embedded into the file which says it was downloaded from an untrusted location.

```
The possible zones are:

- 0 => Local computer
- 1 => Local intranet
- 2 => Trusted sites
- 3 => Internet
- 4 => Restricted sites
```

Files that are emailed "internally" via a compromised Exchange mailbox are not tagged with a Zone Identifier.
If the file is attached to the email then the MOTW won't be in the file. 

## 📔 Attack Methods

- [[VBA Macros]] 
- [[Remote Template Injection]]
- [[HTML Smuggling]]
- 

##  📗 Phishing Templates 

1. [Office 365 HTML templates](https://github.com/ZeroPointSecurity/PhishingTemplates/tree/master/Office365)
2. [HTML online editor for phishing templates](https://html5-editor.net/)
3. 


### Properties
---
📆 created   {{07-02-2024}} 09:10
🏷️ tags: #redteam #crto   
---

