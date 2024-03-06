
In this first example, we'll send a simple output and error message back to the Cobalt Strike console.

![[hello-world.png]]


#### 🖊️ Code analysis

`BeaconPrintf` is an internal Beacon API defined in `beacon.h` and is the simplest way to send output back to the operator.  The type argument determines how CS will process the output and how it will present it.  They are:

- `CALLBACK_OUTPUT` is generic output.  CS will convert it to UTF-16 using the target's default character set.
- `CALLBACK_OUTPUT_OEM` is generic output. CS will convert it to UTF-16 using the target's OEM character set.  You probably won't need this unless you're dealing with output from cmd.exe.
- `CALLBACK_ERROR` is a generic error message.
- `CALLBACK_OUTPUT_UTF8` is generic output.  CS will convert it from UTF-8 from UTF-16.


#### 📔 Testing 

To test the BOF in Visual Studio, ensure that the Debug build option is selected and run it with the local debugger.

###### in VS console 

In debug mode, some of the Beacon APIs have mock implementations (since we're obviously not running this BOF inside a real Beacon yet) that attempt to replicate its functionality.  For example, BeaconPrintf will print debug-style statements to the console:

![[debug-output.png]]

###### in real beacon

Other Beacon APIs simply display error message if called but it's worth noting that you can modify `mock.h` and `mock.cpp` files to implement your own mockups for these unimplemented functions if it makes sense for your use case.  
To test the BOF on a real Beacon, switch the build to Release mode and build the project (in my case, this will produce `C:\Tools\bofs\x64\Release\demo.x64.o` and execute it using the `inline-execute` command.

![[demo-bof-beacon.png]]




#### ⚠ Opsec




### Properties
---
📆 created   {{06-03-2024}} 15:57
🏷️ tags: #redteam #crto 

---

