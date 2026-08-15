Cracking tool.

## 🚀 - Identify a hash
---
- Ctrl + F loo for similar start characters of the hash in:

	`[https://hashcat.net/wiki/doku.php?id=example_hashes]`

	`https://hashes.com/decrypt/basic`

- Using hashid command:
	`hashid pastedHash`

- Using built in hashcat examples:

`$./hashcat --example-hashes | grep -i [identifier or words in the hash you are comparing]`

- Get the ID of hash:

`$hashcat -h | grep [name of most possible hash, example SHA-256]`

---

## 📜 Running hashcat

- Convert to hashcat format

`$[command to obtain the hash you want] -format hashcat`

- Brute force mode

`$hashcat -m [ID of hash] [location of hash value] [password list] --force`

- Rules
The [hashcat wiki](https://hashcat.net/wiki/doku.php?id=rule_based_attack) contains all the information we need to write a custom rule. To run rules:

`$hashcat -m [ID of hash] [location of hash value] [password list] -r rules\InsidePro-PasswordsPro.rule`

- Dictionary attack:

`hashcat -m [number of hash identifier, check page or previous method to discover it] -a [attack mode, on dictionary use 0] [file with hash] [wordlist]`

Example:

`hashcat -m 9900 -a 0 hash.txt ~/Documents/Tools/rockyou.txt`

### Attack mode

```
0 = Straight/Dictionary
1 = Combination
3 = Mask mode
6 = Hybrid Wordlist + Mask
7 = Hybrid Mask + Wordlist
```

Check advanced wordlists to use the other modes.

### Advanced Wordlists

- [[Combinator]] - combines several wordlists
- [[Masks]] - elaborate brute forcing
- [[Hybrid]] - uses both combinator and Masks
--- 



### Properties
---
📆 created   {{11-02-2024}} 13:42
🏷️ tags: #redteam #crto 

---
