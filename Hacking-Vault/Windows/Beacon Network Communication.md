

![[Cobalt_Stream.png]]

The red text is the request made by Beacon.  It's asking the team server if there are any jobs it needs to execute.  The random-looking characters in the URI is the Beacon's encoded metadata.  The blue text is the team server's response.  Since there were no jobs, it simply sends a "NOP" (no-operation) frame.  Everything about how this traffic appears can be customised in the Malleable C2 profile.

![[cobalt_stream2.png]]

If the teamserver sends a command to beacon, we can see how the team server response has changed when it sends a task like "pwd" to Beacon.

![[cobalt_stream3.png]]

We can also see the output being sent by Beacon in the POST body.

We should try to have high check-in times since interactive mode can be very noisy.

#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 10:17
🏷️ tags: #redteam #crto 

---

