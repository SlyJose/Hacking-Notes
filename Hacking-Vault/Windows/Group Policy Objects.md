
#### 📔 Description

GPO's are used to deploy different policies for each [[Organizational Unit]], meaning having different configurations and security baselines to [[Users]] depending on their department.

They are managed with the tool "Group Policy Management". Any policy applied to a [[Organizational Unit]] will be applied to sub-[[Organizational Unit]]s.

####  📗 GPO Distribution

GPO's are distributed to the network via a network share called [[SYSVOL]], which is stored in the [[Domain Controller]]. 

Any change to the GPO's will take some time to distribute or it can be forced by using:

```
PS C:\> gpupdate /force
```





### Properties
---
📆 created   {{22-10-2023}} 19:46
🏷️ tags: #activedirectory 

---
