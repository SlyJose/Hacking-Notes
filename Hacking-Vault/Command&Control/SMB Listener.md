
[[SMB]] listeners are very simple as they only have a single option - the named pipe name.  

The default is `msagent_##` (where `##` is random hex), but you can specify anything you want.  A Beacon [[SMB]] payload will start a new named pipe server with this name and listen for an incoming connection.  This named pipe is available both locally and remotely.

# 🖊️ Opsec

This default pipe name is quite well signatured.  A good strategy is to emulate names known to be used by common applications or Windows itself.  Use `PS C:\> ls \\.\pipe\` to list all currently listening pipes for inspiration.


# 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{06-02-2024}} 08:55
🏷️ tags: #redteam   #crto
---

