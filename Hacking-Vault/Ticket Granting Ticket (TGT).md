
Value used to "proof" a successful authentication in an [[Active Directory]] environment. They are create by the Key Distribution Center in the [[Kerberos]] authentication process.


# 🖊️ Function

The [[Ticket Granting Ticket (TGT)]] allow the user to request additional tickets to access specific services. The need for a ticket to get more tickets may sound a bit weird, but it allows users to request service tickets without passing their credentials every time they want to connect to a service.

[[Ticket Granting Ticket (TGT)]] are also used to request [[Ticket Granting Service (TGS)]] tickets.


# 📔 Components

The [[Ticket Granting Ticket (TGT)]] is encrypted using the krbtgt account's password hash, and therefore the user can't access its contents.

It is essential to know that the encrypted TGT includes a copy of the Session Key as part of its contents, and the KDC has no need to store the Session Key as it can recover a copy by decrypting the TGT if needed.

Elements in request for a [[Ticket Granting Ticket (TGT)]]:

![[TGT-1.png]]


### Properties
---
📆 created   {{22-10-2023}} 20:23
🏷️ tags: #activedirectory 
---

