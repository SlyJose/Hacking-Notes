
The DNS listener allows Beacon to send and receive C2 messages over several lookup/response types including A, AAAA and TXT.  

TXT are used by default because they can hold the most amount of data.  This requires we create one or more DNS records for a domain that the team server will be authoritative for.

# 🖊️ Opsec

We can test the records by performing an arbitrary lookup. The team server's default response is 0.0.0.0.

Since 0.0.0.0 is the default response (and also rather nonsensical), Cobalt Strike team servers can be fingerprinted in this way.  This can be changed in the Malleable C2 profile.


# 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{05-02-2024}} 22:27
🏷️ tags: #redteam   #crto 
---

