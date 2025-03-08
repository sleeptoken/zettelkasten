
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
DNS clients send DNS messages to DNS servers listening on UDP port 53. If the DNS message size exceeds the default size of UDP (512 octets), the response contains only the data that UDP can accommodate, and the DNS server sets a flag to indicate the truncated response. The [[DNS (53 port)]] client can now resend the request via TCP over port 53 to the DNS server. In this approach, the DNS server uses UDP as a default protocol.

> In the case of lengthy queries for which UDP fails, TCP is used as a failover solution. 

- Malware such as ADM worm and Bonk Trojan uses port 53 to exploit vulnerabilities within DNS servers, helping intruders launch attacks. 

#### TCP/UDP 135: Microsoft RPC Endpoint Mapper 

RPC is a protocol used by a client system to request a service from a server. An endpoint is the protocol port on which the server listens for the client's RPCS. 
The RPC Endpoint Mapper enables RPC clients to determine the port number currently assigned to a specific RPC service. There is a flaw in the part of RPC that exchanges messages over TCP/IP. The incorrect handling of malformed messages causes failure. This affects the [[RPC Endpoint Mapper (port 135)]], which listens on TCP/IP port 135. 
- This vulnerability could allow an attacker to send RPC messages to the RPC Endpoint Mapper process on a server to launch a denial-of-service (DoS) attack. 

#### UDP 137: NetBIOS Name Service (NBNS) 

NBNS, also known as the Windows Internet Name Service (WINS), provides a name-resolution service for computers running NetBIOS. NetBIOS name servers maintain a database of the NetBIOS names for hosts and the corresponding IP address the host is using.
- NBNS aims to match IP addresses with NetBIOS names and queries. 
Attackers usually attack the name service first. Typically, [[NetBIOS Name Service (port 137)]] uses UDP 137 as its transport protocol. It can also use TCP 137 as its transport protocol for a few operations, though this might never occur in practice. 

#### TCP 139: NetBIOS Session Service (SMB over NetBIOS) 

TCP 139 is perhaps the most well-known Windows port. It is used to transfer files over a network. Systems use this port for both null-session establishment as well as file and printer sharing. A system administrator considering the restriction of access to ports on a Windows system should make the restriction of TCP [[NetBIOS Session Service (port 139)]] a top priority. 
- An improperly configured TCP 139 port can allow an intruder to gain unauthorized access to critical system files or the complete file system, resulting in data theft or other malicious activities. 

#### TCP/UDP 445: SMB over TCP (Direct Host) 

Windows supports file- and printer-sharing traffic using the SMB protocol directly hosted on TCP. In earlier OSs, SMB traffic required the NetBIOS over TCP (NBT) protocol to work on TCP/IP transport. Directly hosted SMB traffic uses port 445 (TCP and UDP) instead of NetBIOS. 

#### UDP 161: Simple Network Management Protocol (SNMP) 

[[SNMP (161 UDP port)]] is widely used in network management systems to monitor network-attached devices such as routers, switches, firewalls, printers, and servers. 
- It consists of a manager and agents.
- The agent receives requests on port 161 from the managers and responds to the managers on port 162. 

#### TCP/UDP 389: Lightweight Directory Access Protocol (LDAP) 

[[Tags/LDAP (port 389)]] is a protocol for accessing and maintaining distributed directory information services over an IP network. By default, LDAP uses TCP or UDP as its transport protocol over port 389. 

#### TCP 2049: Network File System (NFS) 

NFS protocol is used to mount file systems on a remote host over a network, and users can interact with the file systems as if they are mounted locally. NFS servers listen to its client systems on TCP port 2049. If NFS services should be properly configured to prevent privesc and backdoor injection, etc.

#### TCP 25: Simple Mail Transfer Protocol (SMTP) 

[[SMTP (25 port)]] is a TCP/IP mail delivery protocol. It transfers email across the Internet and across local networks. It runs on the connection-oriented service provided by TCP and uses the well-known port number 25. 
#### TCP/UDP 162: SNMP Trap 

An SNMP trap uses TCP/UDP port 162 to send notifications such as optional variable bindings and the sysUpTime value from an agent to a manager. 

#### UDP 500: Internet Security Association and Key Management Protocol (ISAKMP)/Internet Key Exchange (IKE) 

Internet Security Association and Key Management Protocol (ISAKMP)/Internet Key Exchange (IKE) is a protocol used to set up a security association (SA) in the IPsec protocol suite. It uses UDP port 500 to establish, negotiate, modify, and delete SAs and cryptographic keys in a virtual private network (VPN) environment. 

#### TCP 22: Secure Shell (SSH) / Secure File Transfer Protocol (SFTP) 

Secure Shell ([[ssh (22 port)]]) is a command-level protocol mainly used for managing various networked devices securely. It is generally used as an alternative protocol to the unsecure Telnet protocol. 
SSH uses the client/server communication model, and the SSH server, by default, listens to its client on TCP port 22. 
- Attackers may exploit the SSH protocol by brute-forcing SSH login credentials. 

SFTP, by default, uses port 22, facilitating the secure exchange of data over a single Internet connection. This designated port for SFTP enhances its security and simplicity compared to protocols such as FTP/S that require multiple ports for operation. SFTP's reliance on just one port makes its use more straightforward while it continues to secure data transfers through SSH encryption. 
- Attackers enumerate SFTP to gather information about user accounts, file and directory permissions, and the server's configuration.
 
#### TCP/UDP 3268: Global Catalog Service 

- Microsoft's Global Catalog server, a domain controller that stores extra information, uses port 3268. 
- Its database contains rows for every object in the entire organization, instead of rows for only the objects in one domain. 
- Global Catalog allows one to locate objects from any domain without having to know the domain name. 

LDAP in the Global Catalog server uses port 3268. This service listens to port 3268 through a TCP connection. Administrators use port 3268 for troubleshooting issues in the Global Catalog by connecting to it using LDP. 

#### TCP/UDP 5060, 5061: Session Initiation Protocol (SIP) 
 
The Session Initiation Protocol (SIP) is a protocol used in Internet telephony for voice and video calls. It typically uses TCP/UDP port 5060 (non-encrypted signaling traffic) or 5061 (encrypted traffic with TLS) for SIP to servers and other endpoints. 

#### TCP 20/21: File Transfer Protocol 

FTP is a connection-oriented protocol used for transferring files over the Internet and private networks. [[FTP (21,20 port)]] is controlled on TCP port 21, and for data transmission, FTP uses TCP port 20 or some dynamic port numbers depending on the server configuration. 
If attackers identify that FTP server ports are open, then they perform enumeration on FTP to find information such as the software version and state of existing vulnerabilities to perform further exploitations such as the sniffing of FTP traffic and FTP brute-force attacks. 

#### TCP 23: Telnet 

The Telnet protocol is used for managing various networked devices remotely. It is an unsecure protocol because it transmits login credentials in the cleartext format. Therefore, it is mostly used in private networks. The Telnet server listens to its clients on [[Telnet (port 23)]].
Attackers can take advantage of the Telnet protocol to perform banner grabbing on other protocols such as SSH and SMTP, brute-forcing attacks on login credentials, port-forwarding attacks, etc. 

#### UDP 69: Trivial File Transfer Protocol (TFTP) 

[[TFTP (port 69)]] is a connectionless protocol used for transferring files over the Internet. TFTP depends on connectionless UDP; therefore, it does not guarantee the proper transmission of the file to the destination. TFTP is mainly used to update or upgrade software and firmware on remote networked devices. It uses UDP port 69 for transferring files to a remote host
- Attackers may exploit TFTP to install malicious software or firmware on remote devices. 

#### TCP 179: Border Gateway Protocol (BGP) 

[[Border Gateway Protocol (BGP) (port 179)]] is widely used by Internet service providers (ISPs) to maintain huge routing tables and for efficiently processing Internet traffic. BGP routers establish sessions on TCP port 179. 
- The misconfiguration of BGP may lead to various attacks such as dictionary attacks, resource-exhaustion attacks, flooding attacks, and hijacking attacks.

### References
CEH v13 M4