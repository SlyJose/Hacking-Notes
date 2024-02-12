
Is very time consuming to attempt aaa,aab,aac. You can use masks to reduce the search. 

A mask attack is an evolution over the brute-force and allows us to be more selective over the keyspace in certain positions.

## 🖊️ via Hashcat

This is [[Hashcat]] charset:

```
? | Charset
===+=========
l | abcdefghijklmnopqrstuvwxyz
u | ABCDEFGHIJKLMNOPQRSTUVWXYZ
d | 0123456789
h | 0123456789abcdef
H | 0123456789ABCDEF
s | !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
a | ?l?u?d?s
b | 0x00 - 0xff
```

- Having a hash like `64f12cddaa88057e06a81b54e73b949b` , you can start by testing permutations:

1) Lets say it could start with an upper case (u), have lowercase letters later (l), and finish with a number (d), something like:

	``?u?l?l?l?l?d``

2) But what if the real password has more lowercase characters? You can go trying (incrementing the lower case letters for example) until you hit it:

```
hashcat.exe -a 3 -m 1000 C:\Temp\ntlm.txt ?u?l?l?d
hashcat.exe -a 3 -m 1000 C:\Temp\ntlm.txt ?u?l?l?l?l?l?d
hashcat.exe -a 3 -m 1000 C:\Temp\ntlm.txt ?u?l?l?l?l?l?l?l?d

Success!
64f12cddaa88057e06a81b54e73b949b:Password1
```

You see it incrementing the characters. In this example the hash is store in ntlm.txt, and `-a 3` is for mask mode.
What if you want to avoid manually incrementing these? But your mask is the same. Try Mask Files.

#### Mask Files

Hashcat mask files make the incrementing process a lot easier for custom masks that you use often.

```
PS C:\> cat example.hcmask
?d?s,?u?l?l?l?l?1
?d?s,?u?l?l?l?l?l?1
?d?s,?u?l?l?l?l?l?l?1
?d?s,?u?l?l?l?l?l?l?l?1
?d?s,?u?l?l?l?l?l?l?l?l?1
```

Check "Combined charsets" for the ?d?s explanation.

Usage:

`hashcat.exe -a 3 -m 1000 ntlm.txt example.hcmask`

Mask Files can also have defined words in them, for example:

```
ZeroPointSecurity?d
ZeroPointSecurity?d?d
ZeroPointSecurity?d?d?d
ZeroPointSecurity?d?d?d?d
```

#### Combined charsets

You can combine these charsets within your mask for even more flexibility. It's also common for password to end with a special (such as `!`) rather than a number, but we can specify both in a mask.

`hashcat.exe -a 3 -m 1000 ntlm.txt -1 ?d?s ?u?l?l?l?l?l?l?l?1`
`fbdcd5041c96ddbd82224270b57f11fc:Password!`

Where:

- `-1 ?d?s` defines a custom charset (digits and specials).
- `?u?l?l?l?l?l?l?l?1` is the mask, where `?1` is the custom charset.



### Properties
---
📆 created   {{11-02-2024}} 13:58
🏷️ tags: #redteam #crto 

---

