
Used to receive a beacon's callback. 
There are two main types of listeners: **egress** and **peer-to-peer**.

# 🖊️ Egress

An egress listener is one that allows Beacon to communicate outside of the target network to our team server.  The default egress listener types in [[Cobalt Strike ]]are HTTP/S and DNS, where Beacon will encapsulate C2 traffic over these respective protocols.

To manage your listeners, go to _Cobalt Strike > Listeners_ or click on the headphone icon.  This will open the Listeners tab, from which you can add, edit, remove and restart listeners.

Types of Egress listeners: [[HTTP-HTTPS Listener]], [[DNS Listener]]


# 📔 Peer-to-peer

They don't communicate with the team server directly.  
P2P listeners are designed to chain multiple Beacons together in parent/child relationships.  It helps to:

1. To reduce the number of hosts talking out to your team server, as the higher the traffic volume, the more likely it is to get spotted.
2. To run Beacon on machines that can't even talk out of the network, e.g. in cases of firewall rules and other network segregation.

P2P listener types: [[SMB Listener]] and [[Raw TCP Listener]].  It's important to understand that these protocols do not leave the target network (i.e. the team server is not listening on port 445 for SMB).  Instead, a child SMB/TCP Beacon will be linked to an egress [[HTTP-HTTPS Listener]] or [[DNS Listener]] Beacon, and the traffic from the child is sent to the parent, which in turn sends it to the team server.

There's another type of P2P listener but works similarly to [[Raw TCP Listener]] but in reverse called [[Pivot Listener]]

##  📗 Other types 


1.  [[Foreign Listener]]
2. 


### Properties
---
📆 created   {{05-02-2024}} 21:00
🏷️ tags: #redteam   #crto 

---

