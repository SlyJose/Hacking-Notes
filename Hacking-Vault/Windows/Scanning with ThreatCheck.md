
#### 🖊️ Search for static signatures

1) Run ThreatCheck on the artifact you want to scan:

```
PS C:\Users\Attacker> C:\Tools\ThreatCheck\ThreatCheck\bin\Debug\ThreatCheck.exe -f C:\Tools\cobaltstrike\artifacts\pipe\artifact64svcbig.exe

[+] Target file size: 329728 bytes
[+] Analyzing...
[!] Identified end of bad bytes at offset 0xBEC
00000000   B9 06 00 00 00 4C 89 E7  4C 8D 05 05 E9 04 00 F3   1····L?çL?··é··ó
00000010   AB 4C 89 E9 C7 84 24 88  00 00 00 68 00 00 00 FF   «L?éÇ?$?···h···ÿ
00000020   15 57 2D 05 00 45 31 C9  45 31 C0 31 C9 4C 89 64   ·W-··E1ÉE1A1ÉL?d
00000030   24 48 4C 89 EA 48 89 6C  24 40 48 C7 44 24 38 00   $HL?êH?l$@HÇD$8·
00000040   00 00 00 48 C7 44 24 30  00 00 00 00 C7 44 24 28   ···HÇD$0····ÇD$(
00000050   04 00 00 00 C7 44 24 20  01 00 00 00 FF 15 8A 2B   ····ÇD$ ····ÿ·?+
00000060   05 00 85 C0 74 32 48 8B  4C 24 70 48 85 C9 74 28   ··?At2H?L$pH?Ét(
00000070   0F 10 44 24 70 48 8D 54  24 50 4C 63 CE 49 89 D8   ··D$pH?T$PLcII?O
00000080   48 8B 84 24 80 00 00 00  0F 11 44 24 50 48 89 44   H??$?·····D$PH?D
00000090   24 60 E8 6E FE FF FF 90  48 81 C4 F8 04 00 00 5B   $`èn_ÿÿ?H?Äo···[
000000A0   5E 5F 5D 41 5C 41 5D C3  57 56 48 83 EC 68 48 8D   ^_]A\A]AWVH?ìhH?
000000B0   35 62 E8 04 00 31 C0 49  89 C9 48 8D 7C 24 20 B9   5bè··1AI?ÉH?|$ 1
000000C0   10 00 00 00 41 89 D2 F3  A5 4C 89 C2 4C 8D 44 24   ····A?Oó¥L?AL?D$
000000D0   20 48 89 C1 83 E1 07 8A  0C 0A 41 30 0C 00 48 FF    H?A?á·?··A0··Hÿ
000000E0   C0 48 83 F8 40 75 EA 31  C0 41 39 C2 7E 12 48 89   AH?o@uê1AA9A~·H?
000000F0   C1 83 E1 07 8A 0C 0A 41  30 0C 01 48 FF C0 EB E9   A?á·?··A0··HÿAëé
```

  ⚠ Ensure real-time protection is **disabled** in Defender before running ThreatCheck against binary artifacts.

Here, we can see that the stageless service binary artifact has something that Defender doesn't like.  

However, there's not much context around what this is or where it is in the binary.  Reversing tools such as [IDA](https://hex-rays.com/) and [Ghidra](https://github.com/NationalSecurityAgency/ghidra) can help here because it allows us to dissect the file.

Go to section "Dissecting Binary Static Signature" in "Build Artifact Kit"



### Properties
---
📆 created   {{28-02-2024}} 12:25
🏷️ tags: #redteam #crto 

---

