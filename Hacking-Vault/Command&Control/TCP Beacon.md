
The [[TCP Beacon]] uses a TCP socket to communicate through a parent Beacon. This peer-to-peer communication works with Beacons on the same host and across the network.

In order to do this connection, generate a raw tcp payload and execute it on the host you want to start listening on, this will create a [[Raw TCP Listener]]. Then, from the machine you wish to connect to it run the connect command. 

![[tcp_beacon.png]]

 


### Properties
---
📆 created   {{06-02-2024}} 11:40
🏷️ tags: #redteam #crto   

---

