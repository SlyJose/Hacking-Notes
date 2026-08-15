
A typical console application may have an entry point which looks like `main(int argc, char* argv[])`, but a BOF uses `go(char* args, int len)`.  These arguments are "packed" into a special binary format using the [bof_pack](https://hstechdocs.helpsystems.com/manuals/cobaltstrike/current/userguide/content/topics_aggressor-scripts/as-resources_functions.htm#bof_pack) aggressor function, and can be "unpacked" using Beacon APIs.  Don't try to unpack these yourself if you value your sanity.

#### 🖊️ Pass a string to a function

1) First, call `BeaconDataParse` to initialize the parser, then `BeaconDataExtract` to extract the packed data.

![[unpack-data.png]]


#### 📔 Multiple arguments

If providing multiple arguments, they should be unpacked in the same order that they were packed.  For instance, if we were sending two strings, we would do:

![[multiple-args.png]]


####  📗 Arguments from Aggressor Script

Check [[Integrate BOF with Aggressor Scripts]] section first.
To pack and send arguments from an Aggressor script, we need to call `bof_pack`, specifying both the data types and values.  Let's start off with hardcoding some arguments for simplicity.

```
# pack arguments
$args = bof_pack($1, "zz", "rasta", "hello");
    
# execute bof
beacon_inline_execute($1, $bof, "go", $args);
```

You could change line 2 with:
`$args = bof_pack($1, "zz", $2, $3);` to avoid hardcoding values in the code, and pass them to the aggressor script via the command line.

Here, we're packing "rasta" and "hello", where "zz" tells Cobalt Strike that these are two zero-terminated strings.  The CS documentation provides the following table of valid data formats and how to unpack them:

![[data-formats.png]]



#### ⚠ Opsec




### Properties
---
📆 created   {{06-03-2024}} 16:17
🏷️ tags: #redteam #crto 

---

