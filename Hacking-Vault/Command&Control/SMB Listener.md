
[[SMB]] listeners are very simple as they only have a single option - the named pipe name.  

The default is `msagent_##` (where `##` is random hex), but you can specify anything you want.  A Beacon [[SMB]] payload will start a new named pipe server with this name and listen for an incoming connection.  This named pipe is available both locally and remotely.

To connect to an SMB beacon listener, run (from a machine that can talk to the target machine running the SMB beacon):

`beacon> link sql-2.dev.cyberbotic.io TSVCPIPE-ae2b7dc0-4ebe-4975-b8a0-06e990a41337
`[+] established link to child beacon: 10.10.122.25`

In this example you will connect to the beacon running on sql-2 server.

# 🖊️ Opsec

This default pipe name is quite well signatured.  A good strategy is to emulate names known to be used by common applications or Windows itself.  Use `PS C:\> ls \\.\pipe\` to list all currently listening pipes for inspiration.





### Properties
---
📆 created   {{06-02-2024}} 08:55
🏷️ tags: #redteam   #crto

---

