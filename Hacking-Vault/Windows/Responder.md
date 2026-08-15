
Location: https://github.com/SpiderLabs/Responder

Responder allows us to perform Man-in-the-Middle attacks by poisoning the responses during [[NetNTLM]] authentication, tricking the client into talking to you instead of the actual server they wanted to connect to.

Responder is an [[LLMNR]], [[NetBios Name Service (NBNS)]] and MDNS poisoner. It will answer to _specific_ NBT-NS (NetBIOS Name Service) queries based on their name suffix (see: [http://support.microsoft.com/kb/163409](http://support.microsoft.com/kb/163409)). 

By default, the tool will only answer to File Server Service request, which is for [[SMB]].

The concept behind this is to target our answers, and be stealthier on the network. This also helps to ensure that we don't break legitimate [[NetBios Name Service (NBNS)]] behavior.

# 🖊️ Usage

- For [[Authentication Relays]] attacks check here. 
	- 
# 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{23-11-2023}} 17:44
🏷️ tags: #tools 
---

