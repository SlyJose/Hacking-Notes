
Using the Hello World template, argument calls, integration with aggressor scripts, and calling windows APIs, we can have an example of a Hello World.


#### 🖊️ BOF code

![[msgbox-code.png]]

#### 📔 Aggressor Script that calls BOF

```
alias hello-world {
    local('$handle $bof $args');
    
    # read the bof file (assuming x64 only)
    $handle = openf(getFileProper("C:\\Tools\\bofs\\x64\\Release", "demo.x64.o"));
    $bof = readb($handle, -1);
    closef($handle);
    
    # print task to console
    btask($1, "Running Hello World BOF");

    # pack arguments
    $args = bof_pack($1, "z", $2);
    
    # execute bof
    beacon_inline_execute($1, $bof, "go", $args);
}

# register a custom command
beacon_command_register("hello-world", "Execute Hello World BOF", "Loads demo.x64.o and calls the \"go\" entry point.");
```


####  📗 Command Output

![[msgbox.png]]


#### ⚠ Opsec




### Properties
---
📆 created   {{06-03-2024}} 16:45
🏷️ tags: #redteam #crto 

---

