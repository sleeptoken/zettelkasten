**Directory Services,** which map and provide access to network resources within an organization. The **Lightweight Directory Access Protocol (LDAP)** forms the core of Directory Services.

LDAP is an Internet protocol for accessing distributed directory services. LDAP accesses directory listings within Active Directory or from other directory services. 
- LDAP is a hierarchical or logical form of a directory, similar to a company's organizational chart.
Directory services may provide any organized set of records, often in a hierarchical and logical structure, such as a corporate email directory. It uses DNS for quick lookups and the fast resolution of queries. A client starts an LDAP session by connecting to a Directory System Agent (DSA), typically on TCP port 389, and sends an operation request to the DSA. The Basic Encoding Rules (BER) format is used to transmit information between the client and server. 
An attacker can anonymously query the LDAP service for sensitive information such as usernames, addresses, departmental details, and server names, which an attacker can use to launch attacks.
### Manual LDAP Enumeration 

Attackers can perform manual LDAP enumeration using py.
### Automated LDAP Enumeration

Attackers use the `ldap-brute NSE` script to brute-force `LDAP` authentication. By default, it uses the built-in username and password lists. The `userdb` and `passdb` script arguments can be employed to use custom lists. 
```sh
nmap -p 389 --script ldap-brute --script-args ldap.base='"cn=users,dc=CEH,dc=com "' <Target IP Address> 
```
###### Other Enumeration tools 

[Softerra LDAP Administrator & Browser: Directory Management Tool for Windows](https://www.ldapadministrator.com/)
[LDAPsearch](https://linux.die.net/man/1/ldapsearch)



#### Enumeration Countermeasure

| By default, LDAP traffic is transmitted unsecured; use SSL or STARTTLS technology to encrypt the traffic<br>   |
| -------------------------------------------------------------------------------------------------------------- |
| Select a username different from your email address and enable account lockout                                 |
| Use NT LAN Manager (NTLM), Kerberos, or any basic authentication mechanism to limit access to legitimate users |