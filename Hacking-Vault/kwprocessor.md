
There are a number of external utilities that are separate from the main hashcat application. Here we'll review one called [kwprocessor](https://github.com/hashcat/kwprocessor).  This is a utility for generating key-walk passwords, which are based on adjacent keys such as `qwerty`, `1q2w3e4r`, `6yHnMjU7` and so on. To humans, these can look rather random and secure (uppers, lowers, numbers & specials), but in reality they're easy to generate programmatically.

kwprocessor has three main components:

1. Base characters - the alphabet of the target language.
2. Keymaps - the keyboard layout.
3. Routes - the directions to walk in.

## 🖊️ Usage

There are several examples provided in the **basechars**, **keymaps** and **routes** directory in the kwprocessor download.

```
kwp64.exe basechars\custom.base keymaps\uk.keymap routes\2-to-10-max-3-direction-changes.route -o keywalk.txt

PS C:\> Select-String -Pattern "^qwerty$" -Path keywalk.txt -CaseSensitive

D:\Tools\keywalk.txt:759:qwerty
D:\Tools\keywalk.txt:926:qwerty
D:\Tools\keywalk.txt:931:qwerty
D:\Tools\keywalk.txt:943:qwerty
D:\Tools\keywalk.txt:946:qwerty
```

Here, based on the custombase, the keymap and routes, a password list was generated which includes the same word several times, is important to clean these out.



### Properties
---
📆 created   {{11-02-2024}} 14:36
🏷️ tags: #redteam #crto 

---

