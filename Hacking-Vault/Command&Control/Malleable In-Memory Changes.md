
---
The easiest way to modify reflective loader and Beacon DLL (check "Beacon Payload Generation" page) is through Malleable C2 profile, avoiding memory scan and process scan detections.

Recommended settings:

```
stage {
        set userwx "false";
        set cleanup "true";
        set obfuscate "true";
        set module_x64 "xpsservices.dll";
}
```


1. Setting ==`userwx`== to _false_ tells the reflective loader to allocate memory for the Beacon DLL as RW/RX rather than RWX.  Although this does not remove any indications from the Beacon itself, having RX memory is certainly less suspicious than RWX.

2. Setting ==`cleanup`== to _true_ tells Beacon to free the memory associated with the reflective loader after it has been loaded.  Once the Beacon is loaded, the reflective loader is no longer required and leaving it in memory just results in indicators sitting there waiting to be spotted.  This allows the reflective loader to be freed, thus removing those indicators for the remainder of Beacon's lifetime.

3. Setting ==`obfuscate`== to _true_ does a number of things but the most interesting is that it instructs the reflective loader to load Beacon into memory without its DLL headers.  The omission of these headers reduces the number of indicators in memory, which will circumvent signatures target them specifically.

4. We need to point the beacon to an specific DLL since we are not loading the DLL headers with the `obfuscate` parameter. If we dont do this, the file looks "weird" on memory. Setting the ==_module_x64_== (and _module_x86_ for 32-bit payloads) tells the reflective loader to load the specified DLL from disk first and then overwrite the memory allocated for it with Beacon.  This has the effect of making it appear as though Beacon's memory region is backed by a legitimate DLL.  There are two caveats to consider when choosing a DLL to use.

- The DLL size must be equal to or greater than Beacon.
- The DLL must not be needed by the application hosting Beacon.  This isn't a concern when executing the artifacts, but does become relevant when injecting Beacon shellcode into existing processes.


#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 15:04
🏷️ tags: #redteam #crto 

---

