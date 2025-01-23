
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

[[TCP]] is connection oriented, i.e. it prioritizes connection establishment before data transfer between applications 
### Host Discovery Techniques

![[Pasted image 20250121152932.png]]
#### ARP Ping scan

The ARP packets are sent for discovering all active devices in IPv4 range even though the presence of such devices is hidden by restrictive firewalls. 

In most networks, many IP networks are unused, specifically in the private address ranges of the LAN. Hence, when the attackers try to send IP packets such as ICMP echo request to the target host, the OS must determine the hardware destination address (ARP) corresponding to the target IP for addressing the ethernet frame correctly.
For this purpose, **a series of ARP requests are issued.** 

ARP scan is **used** to show the MAC address of the network interface on the device, and it can also show the MAC addresses of all devices sharing the same IPv4 address on the LAN.
when attackers send ARP request probes to the target host, if they receive any ARP response, then the host is active. In case the destination host is found to be unresponsive, the source host adds an incomplete entry to the destination IP in its **kernel ARP table.** 

Note: 
`-sn` is the Nmap command to disable the port scan.
Since Nmap uses ARP ping scan as the default ping scan, to disable it and perform other desired ping scan, you can use `--disable-arp-ping`
#### UDP Ping scan

In UDP ping scan, Nmap sends UDP packets to the target host. The default port number used by [[nmap]] for UDP ping scan is `40,125 `
UDP ping scans have a advantage of detecting systems behind firewalls with strict TCP filtering, leaving the UDP traffic forgotten. 

Note:
in nmap use `-sU` for UDP scan
#### ICMP ECHO ping scan

Attackers use the ICMP ping scan to send ICMP packets to the destination system to gather all necessary information about it. This is because ICMP does not include port abstraction, and it is different from port scanning.

ICMP ECHO ping scan involves sending ICMP ECHO requests to a host. If the host is alive, it will return an ICMP ECHO reply. This scan is useful for locating active devices or determining if ICMP is passing through a firewall

- UNIX/Linux and BSD-based machines use ICMP echo scanning; the TCP/IP stack implementations in these OSs respond to the ICMP echo requests to the broadcast addresses. 
- This technique does not work on Windows-based networks, as their TCP/IP stack implementation does not reply to ICMP probes directed at the broadcast address

Note:
- Nmap uses the `-P` option to ICMP scan the target.
- The user can also increase the number of pings in parallel using the `-L` option.
#### ICMP ECHO ping sweep 

is a basic network scanning technique that is adopted to determine the range of IP address that map to live hosts. 

To understand pings better, one should be able to understand the [[TCP]] IP packet.
When a system pings, it sends a single packet across the network to a specific IP address. 
This packet contains 64 bytes (56 data bytes and 8 bytes of protocol header information). 
The sender then waits or listens for a return packet from the target system. If the connections are good and the target computer is "alive," a good return packet is expected. However, this will not be the case if there is a disruption in communication. 

Pings also detail the time taken for a packet to make a complete trip, called the "round-trip time." They also help in resolving hostnames.
Attackers calculate subnet masks using subnet mask calculators to identify the number of hosts that are present in the subnet. They subsequently use ping sweep to create an inventory of live systems in the subnet.

Note:
in nmap ->  `-sn -PE`
in zenmap -> `-PE` option with a list of IP address is used to perform ICMP ECHO ping sweep.
#### ICMP Timestamp ping scan

Attackers query a timestamp message to acquire the information related to the current time from the target host machine. The target machine responds with a timestamp reply to each timestamp query that is received. 
However, the response from the destination host is conditional, and it may or may not respond with the time value depending on its configuration by the administrator at the target's end. 
This ICMP timestamp pinging is generally used for time synchronization.
> Such a ping method is effective in identifying whether the destination host machine is active, specifically in the condition where the administrator blocks the traditional ICMP ECHO ping requests.

Note:
In Zenmap, the `-PP` option is used to perform an ICMP timestamp ping scan
#### ICMP address mask ping scan 

Attackers send an ICMP address mask query to the target host to acquire information related to the subnet mask. 
However, the address mask response from the destination host is conditional, and it may or may not respond with the appropriate subnet value depending on its configuration by the administrator at the target's end. This type of ping method is also effective in identifying the active hosts similarly to the ICMP timestamp ping, specifically when the administrator blocks the traditional ICMP Echo ping. 

Note:
In Zenmap, the `-PM` option is used to perform an ICMP address mask ping scan.
#### TCP SYN ping scan 

TCP SYN ping is a host discovery technique for probing different ports to determine if the port is online and to check if it encounters any firewall rule sets. 
In this type of host discovery technique, an attacker uses the Nmap tool to initiate the three-way handshake by sending the empty TCP SYN flag to the target host. After receiving SYN, the target host acknowledges the receipt with an ACK flag. After reception of the ACK flag, the attacker confirms that the target host is active and terminates the connection by sending an RST flag to the target host machine.
- Port 80 is used as the default destination port

> TCP SYN ping can be used to determine if a host is active without creating any connection. Hence logs are not recorded on the system level

Note:
A range of ports can also be specified in this type of pinging format without inserting a space between `-PS` and the port number (e.g., PS22-25,80,113,1050,35000), where the probe will be performed against each port parallelly. In Zenmap, the PS option is used to perform a TCP SYN ping scan
#### TCP ACK Ping scan 

Attacker sends empty TCP ACK packet to the target host directly. Since there is no prior connection between the attacker and the target host, after receiving the ACK packet, the target host responds with an RST flag to terminate the request.

firewalls are mostly configured to block the SYN ping packets, as they are the most common pinging technique. In such cases, the ACK probe can be effectively used to bypass these firewall rule sets easily. 

Note: 
in Zenmap, the `-PA` option is used to perform a TCP ACK ping scan.
#### IP protocol ping scan

IP protocol ping is the latest host discovery option that sends IP ping packets with the IP header of any specified protocol number
It has the same format as the TCP and UDP ping. 
This technique tries to send different packets using different IP protocols, hoping to get a response indicating that a host is online. 

Multiple IP packets for ICMP (protocol 1), IGMP (protocol 2), and IP-in-IP (protocol 4) are sent by default when no protocols are specified. 

For configuring the default protocols, change `DEFAULT_PROTO_PROBE_PORT_SPEC` in `nmap` during compile time. For specific protocols such as ICMP, IGMP, TCP (protocol 6), and UDP (protocol 17), the packets are to be sent with proper protocol headers, and for the remaining protocols, only the IP header data is to be sent with the packets.

Note:
in Zenmap the -PO option is used to perform an IP protocol ping scan.
### Port scanning techniques 

![[Pasted image 20250123103744.png]]
#### TCP Connect / Full-open Scan

TCP Connect / Full Open Scan is one of the most reliable forms of TCP scanning.
In TCP connect scanning, the OS's TCP `connect()` system call tries to open a connection to every port of interest on the target system. 

TCP connect scan completes a three way handshake with the target machine

Making a separate `connect()` call for every targeted port in a linear manner would take a long time over a slow connection. The attacker can accelerate the scan using many sockets in parallel. Using non-blocking, I/O allows the attacker to set a short time out period and watch all the sockets simultaneously. 

Note:
in zenmap, the `-sT` option is used to perform TCP connect / full open scan.
#### Stealth TCP scanning methods

##### Half open scan 

- The stealth scan involves resetting the TCP connection between the client and the server abruptly before completion of the three-way handshake signals, hence making the connection half-open. A stealth scan sends a single frame to a TCP port without any TCP handshaking or additional packet transfers.
- This type of scan sends a single frame with the expectation of a single response. The half-open scan partially opens a connection but stops halfway through. 
- The stealth scan is also called a "SYN scan," because it only sends the SYN packet. This prevents the service from notifying the incoming connection.
- Attackers use stealth scanning techniques to bypass firewall rules and logging mechanisms

Note:
in Zenmap, the `-sS` option is used to perform the stealth scan / TCP half open scan. 
##### Inverse TCP flag scan

Attackers send TCP probe packets with a TCP flag (FIN, URG, PSH) set or with no flags. 
- When the port is open, the attacker does not get any response from the host, 
- when the port is closed, he or she receives the RST from the target host.

Security mechanisms such as firewalls and IDS detect the SYN packets sent to the sensitive ports of the targeted hosts. Programs such as `Syslog` are available to log half-open SYN flag scan attempts. At times, the probe packets enabled with TCP flags can pass through filters undetected, depending on the security mechanisms installed.

All closed ports on the targeted host will send an RST/ACK response. Since OSs such as Windows completely ignore the RFC 793 standard, you cannot see the RST/ACK response when connected to a closed port on the target host. However, this technique is effective when used with UNIX- based OSs.

These types of scan are most effective against hosts using a BSD - derived TCP/IP stack 

Note: Inverse TCP flag scanning is known as FIN, URG, and PSH scanning based on the flag set in the probe packet. If there is no flag set, it is known as NULL scanning. If only the FIN flag is set, it is known as FIN scanning, and if all of FIN, URG, and PSH are set, it is known as Xmas scanning.
##### Xmas Scan 

Xmas scan is a type of inverse TCP scanning technique with the FIN, URG, and PUSH flags set to send a TCP frame to a remote device. 
- If the target has opened the port, then you will receive no response from the remote system. 
- If the target has closed the port, then you will receive a remote system reply with an RST. 
This scan only works when systems are compliant with RFC 793- based TCP/IP implementation. It will not work against any current version of Microsoft Windows.
###### BSD Networking code

This method relies on the BSD networking code. Thus, you can use this only for UNIX hosts; it does not support Windows NT. If the user scan for Microsoft system, it will show that all the ports on the host are open. 

Note:
In zenmap, the `-sX` option is used to perform Xmas scan whereas the `-sF` and `-sN` options are used to perform `FIN` and `NULL` scans.
##### TCP Maimon scan

This scan technique is very similar to NULL, FIN, and Xmas scan, but the probe used here is FIN/ACK. In most cases, to determine if the port is open or closed, the RST packet should be generated as a response to a probe request. However, in many BSD systems, the port is open if the packet gets dropped in response to a probe. 

Nmap interprets a port as open | filtered when there is no response from the Maimon scan probe even after many retransmissions. 
- The port is closed if the probe gets a response as an RST packet. 
- The port is filtered when the ICMP unreachable error (type 3, code 1, 2, 3, 9, 10, or 13) is returned from the target host.

Note: In Zenmap, the `-sM` option is used to perform the TCP Maimon scan.
##### ACK flag Probe scan 

Attackers send TCP probe packets with the ACK flag set to a remote device and then analyze the header information (TTL and WINDOW field) of the received RST packets to find out if the port is open or closed. 
The ACK flag probe scan exploits the vulnerabilities within the BSD-derived TCP/IP stack. Thus, such scanning is effective only on those OSs and platforms on which the BSD derives TCP/IP stacks.
Categories of ACK flag probe scanning include:
TTL-Based ACK Flag Probe scanning
In this scanning technique, you will first need to send ACK probe packets (several thousands) to different TCP ports and then analyze the TTL field value of the RST packets received. In Zenmap, the syntaxnmap -ttl [time] [target] is used to perform TTL- based scan.


### References
M3 - CEHv13