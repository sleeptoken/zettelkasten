
2025-01-21 10:42

Source: #CEHv13 
## Scanning

purpose if to discover exploitable communication channels, probe as many listeners as possible, and track the ones that are responsive.

Types of Scanning :

Port scanning - involves connecting to or probing TCP and UDP ports of the target system
Network Scanning - Lists active hosts and IP address
Vulnerability Scanning - Shows the presence of a weakness
### TCP communication Flags

![[Pasted image 20250121113334.png]]

6 TCP control flags manage the connection between hosts and give instructions to the system.
- 4 of these (SYN, ACK, FIN, RST) govern the establishment, maintanace, and termination of a connection,
- the other 2 flags (PSH, URG) provide instructions to the system 
The size of each flag is 1 bit
when the flag value is 1 it is turned on 
### TCP/IP connection 

![[Pasted image 20250121150849.png]]

TCP is connection oriented, i.e. it prioritizes connection establishment before data transfer between applications 
### Host Discovery Techniques

![[Pasted image 20250121152932.png]]
#### ARP Ping scan










### References
M3 - CEHv13