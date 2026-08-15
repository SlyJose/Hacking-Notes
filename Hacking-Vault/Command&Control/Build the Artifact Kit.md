The kit includes a build script which uses mingw to compile the artifacts.

You can also review the _README.md_ file inside the Artifact Kit directory for more information.

#### 🖊️ Basic Build

  Running it without any arguments will show the usage:

```
ubuntu@DESKTOP-3BSK7NO /m/c/T/c/a/k/artifact> ./build.sh
[Artifact kit] [-] Usage:
[Artifact kit] [-] ./build <techniques> <allocator> <stage size> <rdll size> <include resource file> <stack spoof> <syscalls> <output directory>
```


#### 📔 Build with a bypass technique

Let's build a new set of artifact templates using the bypass-pipe technique:

```
ubuntu@DESKTOP-3BSK7NO /m/c/T/c/a/k/artifact> ./build.sh pipe VirtualAlloc 310272 5 false false none /mnt/c/Tools/cobaltstrike/artifacts
[Artifact kit] [+] You have a x86_64 mingw--I will recompile the artifacts
[Artifact kit] [*] Using allocator: VirtualAlloc
[Artifact kit] [*] Using STAGE size: 310272
[Artifact kit] [*] Using RDLL size: 5K
[Artifact kit] [*] Using system call method: none
[Artifact kit] [+] Artifact Kit: Building artifacts for technique: pipe
[Artifact kit] [*] Recompile artifact32.dll with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact32.exe with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact32svc.exe with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact32big.dll with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact32big.exe with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact32svcbig.exe with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact64.x64.dll with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact64.exe with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact64svc.exe with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact64big.x64.dll with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact64big.exe with src-common/bypass-pipe.c
[Artifact kit] [*] Recompile artifact64svcbig.exe with src-common/bypass-pipe.c
[Artifact kit] [+] The artifacts for the bypass technique 'pipe' are saved in '/mnt/c/Tools/cobaltstrike/artifacts/pipe'
```

In this example (check your path) all artifacts and the aggressor script `artifact.cna` will be stored in the */artifacts/pipe*

The naming convention of these files tell you what they are used for.
```
- '32/64' denotes 32 and 64bit architectures.
- 'big' denotes that it's stageless.
- 'svc' denotes that it's a service executable.
```


####  📗 Analyzing the Artifacts

There will be signatures for your newly generated artifacts since AV vendors check these, we need to add some analysis and changes to avoid static signatures:

- [[Scan Artifacts]]
- [[Dissecting Binary Static Signature]]


#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 12:10
🏷️ tags: #redteam #crto 

---

