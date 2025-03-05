
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

Features of [[TCP]]
- Offers quality-of-service transmission 
- Offers congestion management and flow control 
- Offers automatic retransmission of lost or acknowledged data 
- Allows addressing and multiplexing of data

Features of [[UDP]]
- Audio streaming 
- Video conferencing and teleconferencing 

#### TCP/UDP 53: DNS Zone Transfer 

- The DNS resolution process establishes communication between DNS clients and DNS servers. 
DNS clients send DNS messages to DNS servers listening on UDP port 53. If the DNS message size exceeds the default size of UDP (512 octets), the response contains only the data that UDP can accommodate, and the DNS server sets a flag to indicate the truncated response. The DNS client can now resend the request via TCP over port 53 to the DNS server. In this approach, the DNS server uses UDP as a default protocol.

> In the case of lengthy queries for which UDP fails, TCP is used as a failover solution. 

- Malware such as ADM worm and Bonk Trojan uses port 53 to exploit vulnerabilities within DNS servers, helping intruders launch attacks. 

#### TCP/UDP 135: Microsoft RPC Endpoint Mapper 

RPC is a protocol used by a client system to request a service from a server. An endpoint is the protocol port on which the server listens for the client's RPCS. The RPC Endpoint Mapper enables RPC clients to determine the port number currently assigned to a specific RPC service. There is a flaw in the part of RPC that exchanges messages over TCP/IP. The incorrect handling of malformed messages causes failure. This affects the RPC Endpoint Mapper, which listens on TCP/IP port 135. This vulnerability could allow an attacker to send RPC messages to the RPC Endpoint Mapper process on a server to launch a denial-of-service (DoS) attack. 

#### UDP 137: NetBIOS Name Service (NBNS) 

NBNS, also known as the Windows Internet Name Service (WINS), provides a name-resolution service for computers running NetBIOS. NetBIOS name servers maintain a database of the NetBIOS names for hosts and the corresponding IP address the host is using. NBNS aims to match IP addresses with NetBIOS names and queries. Attackers usually attack the name service first. Typically, NBNS uses UDP 137 as its transport protocol. It can also use TCP 137 as its transport protocol for a few operations, though this might never occur in practice. 

#### TCP 139: NetBIOS Session Service (SMB over NetBIOS) 

TCP 139 is perhaps the most well-known Windows port. It is used to transfer files over a network. Systems use this port for both null-session establishment as well as file and printer sharing. A system administrator considering the restriction of access to ports on a Windows system should make the restriction of TCP 139 a top priority. An improperly configured TCP 139 port can allow an intruder to gain unauthorized access to critical system files or the complete file system, resulting in data theft or other malicious activities. 

#### TCP/UDP 445: SMB over TCP (Direct Host) 

Windows supports file- and printer-sharing traffic using the SMB protocol directly hosted on TCP. In earlier OSs, SMB traffic required the NetBIOS over TCP (NBT) protocol to work on TCP/IP transport. Directly hosted SMB traffic uses port 445 (TCP and UDP) instead of NetBIOS. 

#### UDP 161: Simple Network Management Protocol (SNMP) 

SNMP is widely used in network management systems to monitor network-attached devices such as routers, switches, firewalls, printers, and servers. It consists of a manager and agents. The agent receives requests on port 161 from the managers and responds to the managers on port 162. 

#### TCP/UDP 389: Lightweight Directory Access Protocol (LDAP) 

LDAP is a protocol for accessing and maintaining distributed directory information services over an IP network. By default, LDAP uses TCP or UDP as its transport protocol over port 389. 

#### TCP 2049: Network File System (NFS) 

NFS protocol is used to mount file systems on a remote host over a network, and users can interact with the file systems as if they are mounted locally. NFS servers listen to its client systems on TCP port 2049. If NFS services are not properly configured, then
### References
CEH v13 M4