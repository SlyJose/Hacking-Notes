
The kit includes a build script which uses mingw to compile the artifacts.

You can also review the _README.md_ file inside the Resource Kit directory for more information.

#### 🖊️ Basic Build

  Running it without any arguments will show the usage:

```
ubuntu@DESKTOP-3BSK7NO /m/c/T/c/a/k/resource> ./build.sh /mnt/c/Tools/cobaltstrike/resources
[Resource Kit] [+] Copy the resource files
[Resource Kit] [+] Generate the resources.cna from the template file.
[Resource Kit] [+] The resource kit files are saved in '/mnt/c/Tools/cobaltstrike/resources'
```

####  📗 Analyzing the Resources

There will be signatures for your newly generated resources since AV vendors check these, and tools like AMSI are constantly scanning memory and processes, we need to add some analysis and changes to avoid detections:

1) [[AMSI Evasion Scanning]]
2)  Once the malicious byte offset is given by threat check, you can use the code analysis method detailed in [[Ghidra Artifact Analysis]] to identify the malicious piece of code in your resource. Instead of being an artifact is a resource but is the same method. 
3) Rebuilt the resources with the build script:

`ubuntu@DESKTOP-3BSK7NO /m/c/T/c/a/k/resource> ./build.sh /mnt/c/Tools/cobaltstrike/resources`

Repeat this process until the detections are clean.

4) Once you got a clean resource, load `resources.cna` into Cobalt Strike.
5) Host your payloads in a safe way and avoid Scripted Web Delivery.

❗If ThreatCheck finds no detections but you still can't run the resource, check:

- [[Manual AMSI Bypass]]

### Properties
---
📆 created   {{28-02-2024}} 20:00
🏷️ tags: #redteam #crto 

---

