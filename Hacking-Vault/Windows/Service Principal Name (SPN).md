
A service principal name (SPN) is a unique identifier of a service instance. 

In other words, an SPN is like an alias for an [[Active Directory]] object, which can be a [[Service Accounts]], [[Users]] account, or [[Computers]] object , that lets other [[Active Directory]] resources know which services are running under which accounts and creates associations between them (objects & accounts).

[[Kerberos]] authentication uses SPN's to associate a service instance with a service sign-in account. 

# 🖊️ SPN Format

An SPN must be unique in the forest in which it is registered. If it is not unique, authentication will fail. The SPN syntax has four elements: two required elements and two additional elements that you can use, if necessary, to produce a unique name as listed in the following table.

```
<service class>/<host>:<port>/<service name>
```


Example of SPN's:

```
MyDBService/host1.example.com/CN=hrdb,OU=mktg,DC=example,DC=com
MyDBService/host2.example.com/CN=hrdb,OU=mktg,DC=example,DC=com
MyDBService/host3.example.com/CN=hrdb,OU=mktg,DC=example,DC=com
```

# 📔 Elements of SPN

- Service Class: A string that identifies the general class of service; for example, "SqlServer". There are well-known service class names, such as "www" for a web service or "ldap" for a directory service. In general, this can be any string that is unique to the service class. Be aware that the SPN syntax uses a forward slash (/) to separate elements, so this character cannot appear in a service class name.

- Host: The name of the computer on which the service is running. This can be a fully qualified DNS name or a NetBIOS name. Be aware that NetBIOS names are not guaranteed to be unique in a forest, so an SPN that contains a NetBIOS name may not be unique.

- Port: An optional port number to differentiate between multiple instances of the same service class on a single host computer. Omit this component if the service uses the default port for its service class.

- Service Name: An optional name used in the SPNs of a replicable service to identify the data or services provided by the service or the domain served by the service. This component can have one of the following formats:

	- The distinguished name or objectGUID of an object in [[Active Directory]] Domain Services, such as a service connection point (SCP).
	- The DNS name of the domain for a service that provides a specified service for a domain as a whole.
	- The DNS name of an SRV or MX record.



### Properties
---
📆 created   {{22-10-2023}} 20:43
🏷️ tags: #activedirectory 

---

