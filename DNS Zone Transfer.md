
2025-03-29 21:23

Source: 

Tags: 

### Enumeration
 
DNS zone transfer is the process of transferring a copy of the DNS zone file from the primary DNS server to a secondary DNS server. In most cases, the primary DNS server maintains a backup or secondary server for redundancy, which holds all the information stored in the primary server. The DNS server uses zone transfer to distribute changes made to the main server to the secondary server(s). 

In DNS enumeration using zone transfer, an attacker attempts to retrieve a copy of the entire zone file for a domain from the DNS server. Attackers can perform DNS zone transfer using tools such as nslookup, dig command, and DNSRecon. If the DNS transfer setting is enabled on the target name server, it will provide the DNS information; else, it will return an error stating it has failed or refused the zone transfer. 

To perform a DNS zone transfer, the attacker sends a zone-transfer request to the DNS server pretending to be a client; the DNS server then sends a portion of its database as a zone to the attacker. This zone may contain a large amount of information about the DNS zone network.
#### Dig command 

Attackers use the dig command on Linux-based systems to query the DNS name servers and retrieve information about the target host addresses, name servers, mail exchanges, etc

```
dig ns <target domain> 
```
The above command retrieves all the DNS name servers of the target domain.

Next, attackers use one of the name servers from the output of the above command to test whether the target DNS allows zone transfers. They use the following command for this purpose
```
dig @<domain of name server> <target domain> axfr 
```

#### nslookup Command 

Source: https://docs.microsoft.com 
Attackers use the nslookup command on Windows-based systems to query the DNS name servers and retrieve information about the target host addresses, name servers, mail exchanges, etc.
Attackers use the following command to perform DNS zone transfer:

```
nslookup 
set querytype=soa 
<target domain> 
```

The above command sets the query type to the Start of Authority (SOA) record to retrieve administrative information about the DNS zone of the target domain `certifiedhacker.com.` The following command is used to attempt to transfer the zone of the specified name server: 

```
/ls -d <domain of name server> 
```

#### DNSRecon 
Source: https://github.com 

Attackers use DNSRecon to check all NS records of the target domain for zone transfers. As shown in the screenshot, attackers use the following command for DNS zone transfer: 
dnsrecon -t axfr -d <target domain> 
In the above command, the -t option specifies the type of enumeration to be performed, axfr is the type of enumeration in which all NS servers are tested for a zone transfer, and the -d option specifies the target domain. 
X
### References
[[Enumeration]]