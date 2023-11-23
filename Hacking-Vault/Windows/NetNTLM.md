
[[NetNTLM]] works using a challenge-response mechanism. 


##  📗 Authentication Process 

1. The client sends an authentication request to the server they want to access.
2. The server generates a random number and sends it as a challenge to the client.
3. The client combines their NTLM password hash with the challenge (and other known data) to generate a response to the challenge and sends it back to the server for verification.
4. The server forwards the challenge and the response to the [[Domain Controller]] for verification.
5. The [[Domain Controller]] uses the challenge to recalculate the response and compares it to the original response sent by the client. If they both match, the client is authenticated; otherwise, access is denied. The authentication result is sent back to the server.
6. The server forwards the authentication result to the client.

![[NetNTLM.png]]

Note that the user's password (or hash) is never transmitted through the network for security.

Note: The described process applies when using a domain account. If a local account is used, the server can verify the response to the challenge itself without requiring interaction with the [[Domain Controller]] since it has the password hash stored locally on its SAM.

While [[NetNTLM]] should be considered obsolete, most networks will have both protocols enabled.

Services that use [[NetNTLM]] can also be exposed to the internet. Check [[NetNTLM Authenticated Services]]


### Properties
---
📆 created   {{22-10-2023}} 21:14
🏷️ tags: #activedirectory   
---

