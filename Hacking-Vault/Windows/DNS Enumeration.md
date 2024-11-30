
Domain Name System (DNS) records can provide a wealth of information regarding services that may be exposed to the Internet.



# 🖊️ Records Lookup

Using example domain "cyberbotic.io"
A Records lookup:

```
$ dig cyberbotic.io

;; QUESTION SECTION:
;cyberbotic.io.                 IN      A

;; ANSWER SECTION:
cyberbotic.io.          0       IN      A       172.67.205.143
cyberbotic.io.          0       IN      A       104.21.90.222
```

Performing a `whois` on each public IP address can show who it belongs to. We can see that it resolves to a 3rd party provider, Cloudflare.

```
$ whois 172.67.205.143

OrgName:        Cloudflare, Inc.
OrgId:          CLOUD14
Address:        101 Townsend Street
RegDate:        2010-07-09
Updated:        2021-01-11
Ref:            https://rdap.arin.net/registry/entity/CLOUD14
```

If the target is located in the cloud, you must obtain permissions to enumerate and attack these machines from both owner and the cloud provider.

Perform subdomain discoveries:

```
~/dnscan$ ./dnscan.py -d cyberbotic.io -w subdomains-100.txt
[*] Processing domain cyberbotic.io
[*] Using system resolvers: 172.19.80.1
[+] Getting nameservers
172.64.32.56 - adi.ns.cloudflare.com
173.245.58.56 - adi.ns.cloudflare.com
108.162.192.56 - adi.ns.cloudflare.com
```

Weak email security (SPF, DMARC and DKIM) may allow us to spoof emails to appear as though they’re coming from their own domain.  [Spoofy](https://github.com/MattKeeley/Spoofy) is a Python tool that can verify the email security of a given domain.

```
~/Spoofy$ pip3 install -r requirements.txt
~/Spoofy$ python3 spoofy.py -d cyberbotic.io -o stdout
```


## 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{06-02-2024}} 16:27
🏷️ tags: #redteam #crto   
---

