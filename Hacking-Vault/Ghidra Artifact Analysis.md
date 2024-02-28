
#### 🖊️ Analyzing a service stageless exec (from Cobalt Strike)

1) Launch Ghidra by running the start script at `C:\ghidra-10.3.1\ghidraRun.bat`.
2) Create a new non-shared project from _File > New Project,_ then import your artifact by going to _File > Import File_.
3) Double-click on the imported file to open it in the _CodeBrowser_.  When prompted, select _Yes_ to analyze the binary (the default selected analyzers are fine).
4) The next task is to find the portion of code reported by your static signature scanner, for which there are two easy ways(Depends if you got a byte sequence or a byte offset by the scanner).
	
	4.1) The first method is to search for a specific byte sequence output by scanner, for example `C1 83 E1 07 8A 0C 0A 41 30 0C 01 48 FF C0 EB E9`.  Go to _Search > Memory_, paste the string into the search box and click _Search All_.

![[ghidra-search-memory.png]]

. 
		4.1.1) Click on it, it will take you to the code browser

.
	4.2) Instead if you got byte offset from your scanner, to go to the code section do: _Select Navigation > Go To_ and enter `file(n)` where `n` is the offset.  In this case it would be `file(0xBEC)`.

Unfortunately, we don't have debug symbols for the compiled payloads so function and variables names will be quite generic, like `FUN_xxx` and `lVarx`.  However, we can still quite easily see that the portion of highlighted code is a `for` loop.  We can go back to the Artifact Kit source code and search for any such loops.

![[bad-code.png]]

5) Search in artifact kit files for similar loops:

![[vscode-search.png]]

6) Dismiss the files that use a technique that you didnt apply when you compiled the artifacts. For example, in this artifacts creation we did not use readfile bypass or syscalls. Therefore, the candidates in `patch.c` seem the most promising.
7) Compare the code against what Ghidra showed

![[comparison.png]]

Because this is a service binary payload, we know that it will perform a "migration" (i.e. it spawns a new process and injects Beacon shellcode into it before exiting).  This `spawn` function under an `#ifdef _MIGRATE_` directive is a dead ringer for the decompiled version in Ghidra.

8) To break the detection, we just have to modify the routine so that it compiles to a different byte sequence.  For example:

```
for (x = 0; x < length; x++) {
    char* ptr = (char *)buffer + x;

    /* do something random */
    GetTickCount();

    *ptr = *ptr ^ key[x % 8];
}
```

9) Rebuild the kit and scan the new version of the artifact.  This time we have a different signature - this is an iterative process, so we must repeat these steps until all the detections have been removed.

In most cases, it doesn't really matter what you change things to, as long as it's different (and still functional).  With that change, we finally have a clean artifact.
```
PS C:\Users\Attacker> C:\Tools\ThreatCheck\ThreatCheck\bin\Debug\ThreatCheck.exe -f C:\Tools\cobaltstrike\artifacts\pipe\artifact64svcbig.exe
[+] No threat found!
[*] Run time: 0.72s
```


#### 📔 Load newly modified artifacts

To tell Cobalt Strike to use these new artifacts, we must load the aggressor script.

- Go to _Cobalt Strike > Script Manager > Load_ and select the `artifact.cna` file in your output directory.
- Any DLL and EXE payloads that you generate from hereon will use those new artifacts, so use _Payloads > Windows Stageless Generate All Payloads_ to replace all of your payloads in `C:\Payloads`.



#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 12:36
🏷️ tags: #redteam #crto 

---

