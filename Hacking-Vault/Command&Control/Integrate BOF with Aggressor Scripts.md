
BOFs can be integrated with Aggressor by registering custom aliases and commands.  For example:

```
alias hello-world {
    local('$path $handle $bof $args');
    
    # read the bof file (assuming x64 only)
    $handle = openf(getFileProper("C:\\Tools\\bofs\\x64\\Release", "demo.x64.o"));
    $bof = readb($handle, -1);
    closef($handle);
    
    # print task to console
    btask($1, "Running Hello World BOF");
    
    # execute bof
    beacon_inline_execute($1, $bof, "go");
}

# register a custom command
beacon_command_register("hello-world", "Execute Hello World BOF", "Loads demo.x64.o and calls the \"go\" entry point.");
```

In this example, we are using the BOF created in [[BOF Hello World]], the function's name is "go", that's why in `beacon_inline_execute` the third parameter is "go"

Execution will look like this:

![[demo-bof-aggressor.png]]



#### ⚠ Opsec




### Properties
---
📆 created   {{06-03-2024}} 16:07
🏷️ tags: #redteam #crto 

---

