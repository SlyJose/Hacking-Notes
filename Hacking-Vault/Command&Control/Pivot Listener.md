
A pivot listener can only be created on an existing Beacon, and not via the normal Listeners menu. 
The pivot listener tells the existing Beacon to bind and listen on a port, and the new Beacon TCP payload initiates a connection to it instead.

# 🖊️ Setup

To create a pivot listener, right-click on a Beacon and select _Pivoting > Listener_.  This will open a "New Listener" window.

You will notice the payload type is _beacon_reverse_tcp_, rather than _beacon_bind_tcp_.  Even though there's a drop-down menu, this is currently the only payload type that you can use with the pivot listener.  

The _listen host_ and _listen port_ options are the connection details that will be baked into the payloads generated from this listener.  

![[pivot_listener.png]]

# 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{06-02-2024}} 14:54
🏷️ tags: #redteam #crto   
---

