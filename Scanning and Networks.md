
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

The ARP packets are sent for discovering all active devices in IPv4 range even though the presence of such devices is hidden by restrictive firewalls. 

In most networks, many IP networks are unused, specifically in the private address ranges of the LAN. Hence, when the attackers try to send IP packets such as ICMP echo request to the target host, the OS must determine the hardware destination address (ARP) corresponding to the target IP for addressing the ethernet frame correctly.
For this purpose, **a series of ARP requests are issued.** 

ARP scan is **used** to show the MAC address of the network interface on the device, and it can also show the MAC addresses of all devices sharing the same IPv4 address on the LAN.
when attackers send ARP request probes to the target host, if they receive any ARP response, then the host is active. In case the destination host is found to be unresponsive, the source host adds an incomplete entry to the destination IP in its **kernel ARP table.** 

Note: 
`-sn` is the Nmap command to disable the port scan 
#### UDP Ping scan

In UDP ping scan, Nmap sends UDP packets to the target host. The default port number used by [[nmap]] for UDP ping scan is `40,125 `
UDP ping scans have a advantage of detecting systems behind firewalls with strict TCP filtering, leaving the UDP traffic forgotten. 
#### ICMP ECHO ping scan

Attackers use the ICMP ping scan to send ICMP packets to the destination system to gather all necessary information about it. This is because ICMP does not include port abstraction, and it is different from port scanning.

ICMP ECHO ping scan involves sending ICMP ECHO requests to a host. If the host is alive, it will return an ICMP ECHO reply. This scan is useful for locating active devices or determining if ICMP is passing through a firewall

- UNIX/Linux and BSD-based machines use ICMP echo scanning; the TCP/IP stack implementations in these OSs respond to the ICMP echo requests to the broadcast addresses. 
- This technique does not work on Windows-based networks, as their TCP/IP stack implementation does not reply to ICMP probes directed at the broadcast address
#### ICMP ECHO ping sweep 

is a basic network scanning technique that is adopted to determine the range of IP address that map to live hosts. 
To understand pings better, one should be able to understand the TCP/IP packet. When a system pings, it sends a single packet across the network to a specific IP address. This packet contains 64 bytes (56 data bytes and 8 bytes of protocol header information). The sender then waits or listens for a return packet from the target system. If the connections are good and the target computer is "alive," a good return packet is expected. However, this will not be the case if there is a disruption in communication. Pings also detail the time taken for a packet to make a complete trip, called the "round-trip time." They also help in resolving hostnames. In this case, if the packet bounces back when sent to the IP address, but not when sent to the name, then the system is unable to reconcile the name with the specific IP address. Attackers calculate subnet masks using subnet mask calculators to identify the number of hosts that are present in the subnet. They subsequently use ping sweep to create an inventory of live systems in the subnet.


### References
M3 - CEHv13