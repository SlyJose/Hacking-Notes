
To tunnel `nmap`, it would be:

`attacker@ubuntu ~> proxychains nmap -n -Pn -sT -p445,3389,4444,5985 10.10.122.10`

 ⚠  ICMP and SYN scans cannot be tunnelled, so we must disable ping discovery (`-Pn`) and specify TCP scans (`-sT`) for this to work.

### ⚠ Opsec


### Properties
---
📆 created   {{17-02-2024}} 18:18
🏷️ tags: #redteam #crto 

---

