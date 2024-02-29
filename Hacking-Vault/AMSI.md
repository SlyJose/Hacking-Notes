
**Antimalware Scan Interface AMSI**

#### 🖊️ what is it

AMSI is a component of Windows which allows applications to integrate themselves with an antivirus engine by providing a consumable, language agnostic interface.  

It was designed to tackle "fileless" malware that was so heavily popularized by tools like the [EmpireProject](https://github.com/EmpireProject/Empire), which leveraged PowerShell for complete in-memory C2.

#### 📔 environment

Any 3rd party application can use AMSI to scan user input for malicious content.  Many Windows components now use AMSI including PowerShell, the Windows Script Host, JavaScript, VBScript and VBA.

####  📗 alerts

- [[AMSI Detections and Blocks]]

### Properties
---
📆 created   {{28-02-2024}} 15:22
🏷️ tags: #redteam #crto 

---

