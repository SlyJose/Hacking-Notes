AppLocker is an application whitelisting technology that is built into the Windows Operating System.  Its purpose is to restrict applications and scripts that are allowed to run on a machine, defined through a set of policies which are pushed via GPO.

#### 🚀 - How it works
---
Rules can be based on file attributes such as publisher, name, version, hash or path; they can be to "allow" or deny"; and can be assigned on an individual user or group basis.

[**AppLocker**](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview) will also change the PowerShell [Language Mode](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_language_modes) from FullLanguage to ConstrainedLanguage.  This restricts the .NET types that can be used, preventing `Add-Type` with any arbitrary C# as well as `New-Object` on types that are not specifically permitted.


---
#### 📦 - Security

As a defense, [**AppLocker**](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview) is only as good as the defined rule set.  Microsoft ships default rules, which are very broad and allow all executables and scripts located in the Program Files and Windows directories.

- [[AppLocker Enumeration]] (check if AppLocker is present)
- [[Writable Paths]] (check where you can write)
- [[LOLBAS]] (use local bins to bypass AppLocker)
- [[Break out Powershell CLM]] (escape Powershell CLM)
- [[Bypass DLL Enforcement]] (call DLL functions)

--- 

#### 🖊️ - C


⚠ Alert 1
⚠ Alert 2
⚠ Alert 3


--- 

 1️⃣ A
 2️⃣ B
 
--- 

❗C


### Properties
---
📆 created   {{29-02-2024}} 14:38
🏷️ tags: #redteam #crto 

---
