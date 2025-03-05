
2025-03-05 11:44

Source: #CEHv13 

Enumeration is the process of extracting usernames, machine names, network resources, shares, and services from a system or network. Attacker creates active connections w the system and sends directed queries to gain more information about the target. 
### Techniques of Enumeration 

#### Extract usernames using email IDs 

Every email address contains two parts, a username and a domain name, in the format "username@domainname." 
#### Extract information using default passwords 

Many online resources provide a list of default passwords assigned by manufacturers to their products.
#### Brute force Active Directory 

Microsoft [[Active Directory]] is susceptible to username enumeration at the time of user-supplied input verification. This is a design error in the Microsoft Active Directory implementation. If a user enables the "logon hours" feature, then all the attempts at service authentication result in different error messages. 
- Attackers take advantage of this to enumerate valid usernames. An attacker who succeeds in extracting valid usernames can conduct a brute-force attack to crack the respective passwords. 
#### Extract information using DNS Zone Transfer 

A network administrator can use DNS zone transfer to replicate DNS data across several DNS servers or back up DNS files. For this purpose, the administrator needs to execute a specific zone-transfer request to the name server. If the name server permits zone transfer, it will convert all the DNS names and IP addresses hosted by that server to ASCII text. 
 
If the network administrators did not configure the DNS server properly, the DNS zone transfer can be an effective method to obtain information about the organization's network. This information may include lists of all named hosts, sub-zones, and related IP addresses. A user can perform DNS zone transfer using `nslookup` and `dig` commands. 
#### Extract user groups from Windows 

To extract user groups from Windows, the attacker should have a registered ID as a user in the Active Directory. The attacker can then extract information from groups in which the user is a member by using the Windows interface or command-line method. 
#### Extract usernames using SNMP 

Attackers can easily guess read-only or read-write community strings by using the SNMP application programming interface (API) to extract usernames. Attackers can also methodically query the SNMP tree to gather detailed information about network resources and topology 

### Services and ports to Enumerate 

### References
CEH v13 M4