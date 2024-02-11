Default authentication protocol for Windows. Logged [[Users]] into [[Kerberos]] service receive ==tickets==. These are "proofs" of a successful authentication. 

[[Users]] present the ==tickets== to a service to demonstrate they have already authenticated into the network and therefore enabled to use the service.

# 📜 Kerberos Authentication Process

1. The user sends their username and a timestamp encrypted using a key derived from their password to the Key Distribution Center (KDC), a service usually installed on the [[Domain Controller]] in charge of creating Kerberos tickets on the network. 

	The KDC will create and send back a [[Ticket Granting Ticket (TGT)]] and also a Session Key is given to the user, which they will need to generate the following requests.
	

![[TGT-1.png]]

2. When a user wants to connect to a service on the network like a share, website or database, they will use their TGT to ask the KDC for a [[Ticket Granting Service (TGS)]]. 
	

![[TGT-2.png]]

  As a result, the KDC will send us a TGS along with a Service Session Key, which we will need to authenticate to the service we want to access. The TGS is encrypted using a key derived from the Service Owner Hash. The Service Owner is the user or machine account that the service runs under. The TGS contains a copy of the Service Session Key on its encrypted contents so that the Service Owner can access it by decrypting the TGS.

3. The TGS can then be sent to the desired service to authenticate and establish a connection. The service will use its configured account's password hash to decrypt the TGS and validate the Service Session Key.

![[TGT-3.png]]



### Properties
---
📆 created   {{22-10-2023}} 20:23
🏷️ tags: #activedirectory 

---

