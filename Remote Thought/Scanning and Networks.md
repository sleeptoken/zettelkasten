
2025-01-21 10:42

Source: #CEHv13 
## Scanning

purpose is to discover exploitable communication channels, probe as many listeners as possible, and track the ones that are responsive.

Types of Scanning :

Port scanning - involves connecting to or probing TCP and UDP ports of the target system
Network Scanning - Lists active hosts and IP address
Vulnerability Scanning - Shows the presence of a weakness
### TCP communication Flags

![[Pasted image 20250121113334.png]]

6 TCP control flags manage the connection between hosts and give instructions to the system.
- 4 of these (SYN, ACK, FIN, RST) govern the establishment, maintenance, and termination of a connection,
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
###### TTL-Based ACK Flag Probe scanning

In this scanning technique, you will first need to send ACK probe packets (several thousands) to different TCP ports and then analyze the TTL field value of the RST packets received.

Note: In Zenmap, the syntax `nmap -ttl [time] [target]` is used to perform TTL based scan.

If the TTL value of the RST packet on a particular port is less than the boundary value of 64, then that port is open 
###### Window-based ACK Flag probe scanning 

you will first need to send ACK probe packets (several thousands) to different TCP ports and then analyze the window field value of the RST packets received. The user can use this scan when all the ports return the same TTL value. 

Note: In zenmap, the `-sW` option is used to perform a window scan.
###### Checking the filtering systems of target networks

Sending an ACK probe packet with a random sequence number and 
- getting no response from the target - the port is filtered (Stateful firewall is present);
- an RST response from the target - the port is not filtered (no firewall is present.)

Note:
in Zenmap, the `-sA` option is used to perform an ACK flag probe scan
##### IDLE/ IPID Header scan 

This method can be used to send a spoofed source address to a computer to determine what services are available. 

A machine that receives an unsolicited `SYN | ACK` packet responds with an `RST`. An unsolicited RST is ignored.
Every IP packet on the Internet has an "IP identifier" (IPID) that uniquely identifies fragments of an original IP datagram. The OS increases the IPID for each packet sent. Thus, probing an IPID reveals to an attacker the number of packets sent since the last probe. 

The attacker performs this scan by impersonating another computer via spoofing. The attacker does not send a packet from their IP address; instead, they use another host, often called a "zombie," to scan the remote host and identify open ports. In this attack, the attacker expects the sequence numbers of the zombie host, and if the remote host checks the IP of the scanning party, the IP of the zombie machine is displayed

Note: 
In Zenmap, the `-sI` option is used to perform an IDLE scan.
#### UDP Scan

You can use one ICMP that checks for open or closed ports. If you send a UDP packet to a port without an application bound to it, the IP stack will return an ICMP port unreachable packet.
If any port returns an ICMP error, it will be closed, leaving the ports that did not answer if they are open or filtered through the firewall.
##### UDP packets

When you send a packet to a closed UDP port, most of the hosts send an `ICMP_PORT_UNREACH` error. Thus, you can determine whether a port is open if UDP packets or ICMP errors are not guaranteed to arrive. 
UDP scanners interpret lost traffic as open ports.

Note:
In zenmap, the `-sU` option is used to perform a `UDP` scan
##### UDP RECVFORM() & WRITE() Scanning

Although non-root users cannot read unreachable port errors directly, Linux informs u indirectly when it receives messages.

UDP scan provides port information only. If additional information of the version is needed, the scan must be supplemented with a version detection scan `-sV` or the OS fingerprinting option `-O`.

#### SCTP Scanning

##### SCTP INIT Scanning

Stream Control Transport Protocol is a reliable message oriented transport layer protocol. 
[[SCTP]] is specifically used to perform multi-homing and multi streaming activities.

SCTP INIT is similar to TCP SYN scan, comparatively it is also stealthy and unobtrusive, as it cannot complete SCTP associations, hence making the connection half-open.

- Attackers send INIT chunk to the target host. if the port is listening or open, it sends an acknowledgement as an INIT+ACK chunk
- If the target is inactive and it is not listening, then it send an acknowledgement as an ABORT chunk
After several retransmissions, if there is no response, then the port is indicated as a filtered port. 
The port is also indicated as a filtered port if the target sever responds with an ICMP unreachable exception (type 3, code 0, 1, 2, 3, 9, 10 or 13)

Note:
in Zenmap, the `-sY` option is used to perform the SCTP INIT scan 
##### SCTP Cookie / ECHO Scanning

In this type of scan, attackers send the COOKIE ECHO chunk to the target, and 
- if the target port is open, it will silently drop the packets onto the port and you will not receive any response from the target.
- If the target sends back the ABORT chunk response, then the port is considered as a closed port.

The COOKIE ECHO chunk is not blocked by non-stateful firewall rule sets as in the INIT scan. Only an advanced IDS can detect the SCTP COOKIE ECHO scan

Note:
In Zenmap, the `-sZ` option is used to perform the SCTP COOKIE ECHO scan.
#### SSDP scanning 

Simple Service Discovery Protocol ([[SSDP]]) is a network protocol that generally communicates with machines when querying them with routable IPv4 or IPv6 multicast addresses. 
The SSDP service controls communication for the Universal Plug and Play (UPnP) feature.

It generally works when the machine is not firewalled; however, it can sometimes work through a firewall. 
The SSDP service will respond to a query sent over IPv4 or IPv6 broadcast addresses. This response includes information about the UPnP feature associated with it. 

The attacker uses SSDP scanning to detect UPnP vulnerabilities that may allow him/her to launch buffer overflow or DoS attacks

The attacker may use the UPnP SSDP M-SEARCH information discovery tool to check whether the machine is vulnerable to UPnP exploits. 
#### List Scan 

In a list scan, the discovery of the active network host is indirect. A list scan simply generates and prints a list of IPs/Names without actually pinging or scanning the hosts. As a result, the list scan shows all IP addresses as "not scanned" (0 hosts up). By default, a reverse DNS resolution is still carried out on each host by Nmap to learn their names

Note:
in Zenmap, the `-sL` option is used to perform a list scan
#### IPv6 Scanning

IPv6 increases the size of the IP address space from 32 bits to 128 bits to support higher levels of the addressing hierarchy. Traditional network scanning techniques are computationally less feasible because of the larger search space (64 bits of host address space, or 264 addresses) provided by IPv6 in a subnet. Scanning the IPv6 network is more difficult and complex compared to IPv4. Additionally, a number of scanning tools do not support ping sweeps on IPv6 networks. [[Ping IPv6 address]]

Note:
In Zenmap, the `-6` option is used to perform the IPv6 scan.
### OS Discovery / Banner Grabbing

Banner grabbing or OS fingerprinting is the method used to determine the OS running on a remote target system.

There are 2 methods for banner grabbing
1. Spotting the banner while trying to connect to a service, such as an `FTP` site
2. Downloading the binary file/bin/ls to check the system architecture. 

A more advanced fingerprinting technique depends on stack querying, which transfers the packets to the network host and evaluates them by the reply. The first stack-querying method designed with regard to the TCP mode of communication evaluates the response to connection requests. 

The next method, known as initial sequence number (ISN) analysis, identifies the differences in random number generators found in the TCP stack. 
ICMP response analysis is another method used to fingerprint an OS. It consists of sending ICMP messages to a remote host and evaluating the reply.
#### Active Banner Grabbing

Active banner grabbing applies the principle that an OS's IP stack has a unique way of responding to specially crafted TCP packets. This happens because of different interpretations that vendors apply while implementing the TCP/IP stack on a particular OS.

In active banner grabbing, the attacker sends a variety of malformed packets to the remote host, and the responses are compared with a database. Responses from different OSs vary because of differences in TCP/IP stack implementation. For example, NMAP uses 9 tests to determine an OS fingerprint or banner grabbing. [OS Detection | Nmap Network Scanning](https://nmap.org/book/man-os-detection.html)
- Windows boxes use a "time-dependent" model in which the ISN is incremented by a fixed amount for each occurrence.
#### Passive Banner Grabbing

Like active banner grabbing, passive banner grabbing also depends on the differential implementation of the stack and the various ways in which an OS responds to packets
However, instead of relying on scanning the target host, passive fingerprinting captures packets from the target host via sniffing to study telltale signs that can reveal an OS.

passive banner grabbing includes
1. **Banner grabbing from error messages**: Error messages provide information, such as type of server, type of OS, and SSL. tools used by the target remote system. 
2. **Sniffing the network traffic**: Capturing and analyzing packets from the target enables an attacker to determine the OS used by the remote system. 
3. **Banner grabbing from page extensions**: Looking for an extension in the URL may help in determining the application version. For example, `.aspx` => IIS server and Windows platform

The four areas that typically determine the OS are given below:
- TTL (time to live) of the packets: What does the OS sets as the Time To Live on the outbound packet? 
- Window Size: What is the Window size set by the OS? 
- Whether the DF (Don't Fragment) bit is set: Does the OS set the DF bit?
- TOS (Type of Service): Does the OS set the TOS, and if so, what setting is it?

Take a look at how to [[Analysis of a sniffed packet for passive OS disc]]

##### How to identify target system OS

Attackers can identify the OS running on the target machine by looking at the Time To Live (TTL) and TCP window size in the IP header of the first packet in a TCP session. 

Sniff/capture the response generated from the target machine using packet-sniffing tools like Wireshark and observe the TTL and TCP window size fields

The TTL field determines the maximum time that a packet can remain in a network, and the TCP window size determines the length of the packet reported

These values vary among OSs, as described in the following table:

| OS            | TTL | TCP window size            |
| ------------- | --- | -------------------------- |
| Linux         | 64  | 5840                       |
| FreeBSD       | 64  | 65,535                     |
| OpenBSD       | 255 | 16,384                     |
| Windows       | 128 | 65,535 bytes to 1 Gigabyte |
| Cisco Routers | 255 | 4128                       |
| Solaris       | 255 | 8760                       |
| AIX           | 255 | 16,384                     |
### OS disc using nmap 

Note:
In Zenmap, the `-O` option is used to perform OS discovery.

`smb-os-discovery` is an inbuilt script that can be used for collecting OS information on the target machine through SMB protocol.

##### IPV6 Fingerprinting 

The difference between IPV4 and IPV6 fingerprinting is that, IPv6 uses several additional advanced IPv6 specific probes along with a separate IPv6 specific OS detection engine. Nmap send nearly 18 probes  to identify the target OS using the IPv6 fingerprinting method

In Zenmap, the `-6` along with the `-O` option is used to perform OS disc using IPv6 Fingerprinting method.
##### Automation

The following bash script is designed to automate network scanning efforts on the target IP range 10.10.1.0/24:
```
#!/bin/bash 
nmap -sP 10.10.1.0/24 -OG | awk '/Up$/{print $2}' > live_hosts.txt && 
nmap -il live_hosts.txt -SV -OA scan_results && 
cat scan_results.nmap
```

### Scanning beyond IDS & Firewall

Though Firewalls and IDSs can prevent malicious traffic (packets) from entering a networking, attackers can manage to send intended packets to the target by evading an IDS or firewall through the following techniques:

#### Packet Fragmentation 

Packet fragmentation refers to the splitting of a probe packet into several smaller packets (fragments) while sending it to a network.

**Why**
When these packets reach a host, the IDS and firewalls behind the host generally queue all of them and process them one by one, the configuration of most IDS cause them to skip fragmented packets during port scans, as this involves greater CPU and network resource consumption.

**How**
Therefore, attackers use packet fragmentation tools such as Nmap to split the probe packet into smaller packets that circumvent the port-scanning techniques employed by IDS. Once these fragments reach the destined host, they are reassembled to form a single packet. 
##### SYN/FIN Scanning Using IP Fragments 

SYN/FIN scanning using IP fragments is not a new scanning method but a modification of previous techniques. 
- The TCP header splits into several packets to evade the packet filter. For any transmission, every TCP header must have the source and destination port for the initial packet (8-octet, 64-bit).
- The initialized flags in the next packet allow the remote host to reassemble the packets upon receipt via an Internet protocol module that detects the fragmented data packets using field-equivalent values of the source, destination, protocol, and identification.

However, IP reassembly on the server side may result in unpredictable and abnormal results, such as fragmentation of the IP header data. Some hosts may fail to parse and reassemble the fragmented packets, which may lead to crashes, reboots, or even network device monitoring dumps. 

Some firewalls might have rule sets that block IP fragmentation queues in the kernel (e.g., CONFIG_IP_ALWAYS_DEFRAG option in the Linux kernel), although this is not widely implemented because of its adverse effects on performance.

> Since many IDS use signature-based methods to indicate scanning attempts on IP and/or TCP headers, the use of fragmentation will often evade this type of packet filtering and detection, resulting in a high probability of causing problems on the target network. 

Note:
SYN/FIN scan using the Zenmap tool.  ->  `nmap -sS -T4 -A -f -v <ip>`
#### Source Routing 

An IP datagram contains various fields, including the IP options field, which stores source routing information and includes a list of IP addresses through which the packet travels to its destination. 

The routers in the path of a packet hop might include configured firewalls and IDS that block such packets. To avoid them, attackers enforce a loose or strict source routing mechanism, in which they manipulate the IP address path in the IP options field so that the packet takes the attacker-defined path (without firewall-/IDS-configured routers) to reach the destination, thereby evading firewalls and IDS. 
#### Source Port manipulation

The actual port numbers are manipulated with common port numbers for evading certain IDS and firewall rules. 

The administrator mostly configures the firewall by allowing the incoming traffic from well-known ports such as HTTP, DNS, FTP, etc. The firewall can simply allow the incoming traffic from the packets sent by the attackers using such common ports.

Although the firewalls can be made secure using application-level proxies or protocol-parsing firewall elements, this technique helps the attacker to bypass the firewall rules easily. The attacker tries to manipulate the original port number with the common port numbers, which can easily bypass the IDS/firewall. 

Note:
In Zenmap, the `-g` or `--source-port` option is used to perform source port manipulation. ->`nmap -g 80 <ip>`
#### IP address Decoy

The IP address decoy technique refers to generating or manually specifying IP addresses of the decoys to evade IDS/firewalls.
It appears to the target that the decoys as well as the host(s) are scanning the network. This technique makes it difficult for the IDS/firewall to determine which IP address is actually scanning the network and which IP addresses are decoys. 
The Nmap scanning tool comes with a built-in scan function called a decoy scan, which cloaks a scan with decoys. 
##### Decoy scan using nmap

`nmap -D RND:10 10.10.10.01`
`nmap -D decoy1,decoy2,decoy3,.. <target>`  -  manually specify IP address ,separate each decoy w a comma 

This isn't successful if the target employs active mechanism such as router path tracing, response dropping etc.
#### IP address spoofing 

Most firewalls filter packets based on the source IP address. These firewalls examine the source IP address and determine whether the packet is coming from a legitimate source or an illegitimate source. 

IP address spoofing is a hijacking technique in which an attacker obtains a computer's IP address, alters the packet headers, and sends request packets to a target machine, pretending to be a legitimate host.
- The packets appear to be sent from a legitimate machine but are actually sent from the attacker's machine, while his/her machine's IP address is concealed. 
- When the victim replies to the address, it goes back to the spoofed address and not to the attacker's real address. 

Attackers mostly use IP address spoofing to perform DoS attacks. 

When spoofing a nonexistent address, the target replies to a nonexistent system and then hangs until the session times out, thus consuming a significant amount of its own resources.

Note:
`Hping3 www.certified.com -a 7.7.7.7` -> you can use Hping3 to perform IP spoofing.
You will not be able to complete the 3 way handshake and open a successful TCP connection with spoofed IP address.
#### MAC address spoofing

Network firewalls filter packets based on the source media access control (MAC) address. They examine the MAC address in the packet header using MAC filtering rules and determine whether the packets originate from a legitimate source.
The MAC address spoofing technique allows attackers to send request packets to the target machine/network, pretending to be a legitimate host. Attackers use the Nmap tool to evade firewalls via MAC address spoofing. 

Attackers use the-spoof-mac Nmap option to choose or set a specific MAC address for packets and send them to the target system/network
- `nmap -ST -Pn --spoof-mac 0 [Target IP]`
The above command automatically generates a random MAC address and attaches it to the packets in place of the original MAC address while performing host scanning. Here, `-spoof-mac 0` represents the randomization of the MAC address.
#### Creating custom Packet

The attacker creates and sends custom TCP packets to scan the intended target beyond the IDS/Firewall 
- various packet crafting tools can be used namely, Colasoft packet builder, NetScanTools Pro etc. 
- Packet crafting tools craft and send packet streams using different protocols at different transfer rates.
#### Randomizing Host Order 

The attacker scans the number of hosts in the target network in a random order to scan the intended target that is lying beyond the firewall. The option used by Nmap to scan with a random host order is `--randomize-hosts`. 

This technique instructs Nmap to shuffle each group of 16384 hosts before scanning with slow timing options, thus making the scan less notable to network monitoring systems and firewalls. If larger group sizes are randomized, the `PING_GROUP_SZ` should be increased in `nmap.h` and it should be compiled again. 

Another method can be followed by generating the target IP list with the list scan command `-sL -n -oN <filename>` and then randomizing it with a Perl script and providing the whole list to Nmap using the `-iL` command.
### Sending Bad checksums

The attacker sends packets with bad or bogus TCP/UDP checksums to the intended target to avoid certain firewall rule sets.
- TCP/UDP checksums are used to ensure data integrity.
Sending packets with incorrect checksums can help attackers to acquire information from improperly configured systems by checking for any response. 
- If there is a response, then it is from the IDS or firewall, which did not verify the obtained checksum.
- If there is no response or the packets are dropped, then it can be inferred that the system is configured. 

Note: 
This technique instructs Nmap to send packets with invalid TCP, UDP, or SCTP checksums to the target host. The option used by Nmap is `--badsum`
### Proxy servers

**How does a proxy server work?** 

Initially, when you use a proxy to request a particular web page on an actual server, the proxy server receives it. The proxy server then sends your request to the actual server on your behalf.

In this process, the proxy receives the communication between the client and the destination application. To take advantage of a proxy server, an attacker must configure client programs so that they can send their requests to the proxy server instead of the final destination. 
### Proxy chaining 

Internet anonymity depends on the number of proxies used for fetching the target application; the larger the number of proxy servers used, the greater is the attacker's anonymity  

**Proxy tools** 

Proxy tools are intended to allow users to surf the internet anonymously by keeping the IP hidden through a chain of SOCKS or HTTP proxies. Below are the tools - 

[Anonymous Browsing via Proxy Servers with Proxy Switcher](https://www.proxyswitcher.com/)
### Anonymizers

An anonymizer is an intermediate server placed between an end user and a website that accesses the website on their behalf and makes web surfing activities untraceable. Anonymizers allow users to bypass Internet censorship. An anonymizer eliminates all identifying inform ation (IP address) from the system while surfing the Internet, thereby ensuring privacy. It encrypts the data transferred from a computer to the Internet service provider (ISP). Most anonymizers can anonymize web (HTTP), File Transfer Protocol (FTP), and gopher (gopher) Internet services. 
#### Networked anonymizers

A networked anonymizer first transfers your information through a network of Internet-connected computers before passing it on to the website. Because the information passes through several Internet computers, it becomes cumbersome for anyone trying to track your information to establish the connection between you and the anonymizer. 
- Disadvantage: Any multi-node network communication incurs some degree of risk of compromising confidentiality at each node.
#### Single-Point Anonymizers
 
Single-point anonymizers first transfer your information through a website before sending it to the target website and then pass back the information gathered from the target website to you via the website to protect your identity. 
- Advantage: Arms-length communication hides the IP address and related identifying information. 
- Disadvantage: It offers less resistance to sophisticated traffic analysis. 

Anonymizer tools use various techniques such as SSH, VPN, and HTTP proxies, which allow access to blocked or censored content on the Internet with advertisements omitted. 
##### Whonix 

Source: https://www.whonix.org 
Whonix is a desktop OS designed for advanced security and privacy. It mitigates the threat of common attack vectors while maintaining usability. Online anonymity is realized via fail-safe, automatic, and desktop-wide use of the Tor network. It consists of a heavily reconfigured Debian base that is run inside multiple virtual machines, providing a substantial layer of protection from malware and IP address leaks.

### Censorship Circumventing tools 

- [Fast, Secure & Anonymous VPN | Astrill VPN](https://www.astrill.com/)
- [Tails](https://tails.net/)
### Ping Sweep Countermeasures

1. Configure firewalls to block incoming ICMP echo requests from unknown or untrusted sources 
2. Use intrusion detection systems (IDSes) and intrusion prevention systems (IPSes), such as Snort to detect and prevent ping sweep attempts 
3. Carefully evaluate the type of ICMP traffic flowing through enterprise networks 
4. Terminate the connection with any host sending more than 10 ICMP ECHO requests 
5. Use a  demilitarized zone (DMZ) and allow only commands such as ICMP ECHO_REPLY, HOST UNREACHABLE, and TIME EXCEEDED in the DMZ 
6. Limit ICMP traffic with access-control lists (ACLs) to the ISP's specific IP addresses
### Port Scanning Countermeasures 

1. Configure firewall and IDS rules to detect and block probes and are updated to their latest releases/versions .
2. Run port scanning tools against hosts on the network to determine whether the firewall properly detects port scanning activity 
3. Ensure that the mechanisms used for routing and filtering at the routers and firewalls, respectively, cannot be bypassed using a particular source port or source routing methods 
4. Use a custom rule set to lock down the network and block unwanted ports at the firewall
5. Filter all ICMP messages (i.e. inbound ICMP message types and outbound ICMP type 3 unreachable messages) at the firewalls and routers 
6. Perform TCP and UDP scanning along with ICMP probes against your organization's IP address space to check the network configuration and its available ports 
### Banner Grabbing Countermeasures

#### Disabling or changing banner

1. Display false banners to mislead or deceive attackers 
2. Turn off unnecessary services on the network host to limit the disclosure of information 
3. Use server masking tools to disable or change banner information 
4. For Apache 2.x with the `mod_headers` module, use a directive in the `httpd.conf` file to change the banner information header and set the server as `New Server Name `
5. Alternatively, change the `ServerSignature` line to `ServerSignatureoff` in the `httpd.conf` file 
#### Hiding file extensions from web pages 

1. File extensions reveal information about the underlying server technology that an attacker can utilize to launch attacks. It is preferable to not use file extensions at all. 
2. Replace application mappings such as .asp with.htm or .foo, etc. to disguise the identities of servers 
3. Apache users can use mod_negotiation directives
### IP Spoofing detection techniques 

#### Direct TTL Probes

- In this technique, you initially send a packet (ping request) to the legitimate host and wait for a reply. 
Check whether the TTL value in the reply matches with that of the packet you are checking. Both will have the same TTL if they are using the same protocol. Although the initial TTL values vary according to the protocol used. For TCP/UDP, the values are 64 and 128; for ICMP, they are 128 and 255.

- If the reply is from a different protocol, then you should check the actual hop count to detect the spoofed packets. 
The packet is a spoofed packet if the reply TTL does not match the TTL of the packet.
This technique is successful when the attacker is in a different subnet from that of the victim.
#### IP Identification number

Users can identify spoofed packets by monitoring the IP identification (IPID) number in the IP packet headers
The IPID increases incrementally each time a system sends a packet. Every IP packet on the network has a unique "IP identification" number, which is increased by one for every packet transmission.

- To identify whether a packet is spoofed, send a probe packet to the source IP address of the packet and observe the IPID number in the reply. 
- The IPID value in the response packet must be close to but slightly greater than the IPID value of the probe packet. 
- The source address of the IP packet is spoofed if the IPID of the response packet is not close to that of the probe packet. 

This method is effective even when both the attacker and the target are on the same subnet.
#### TCP Flow Control

- The TCP can optimize the flow control on both the sender's and the receiver's end with its algorithm.
- The algorithm accomplishes flow control using the sliding window principle.
- The user can control the flow of IP packets by the window size field in the TCP header.
- This field represents the maximum amount of data that the recipient can receive and the maximum amount of data that the sender can transmit without acknowledgement.
- Thus, this field helps to control data flow. The sender should stop sending data whenever the window size is set to zero. 

In general flow control, the sender should stop sending data once the initial window size is exhausted. The attacker, who is unaware of the ACK packet containing window size information, might continue to send data to the victim. 
If the victim receives data packets beyond the window size, they are spoofed packets. 

> For effective flow control and early detection of spoofing, the initial window size must be very small. 

Most spoofing attacks occur during the handshake, as it is challenging to build multiple spoofing replies with the correct sequence number.
### IP Spoofing Countermeasures

- Avoid Trust Relationships - Do not rely on IP-based auth. Test all packets even when they originate from trusted hosts
- Use Firewalls and filtering mechanisms 
	- Use Random Initial Sequence number - Initial sequence numbers should not be based on timed counters, this makes it predictable for the attackers 
- Ingress Filtering
- Egress Filtering
- Use Encryption
- SYN Flooding Countermeasures



### References
M3 - CEHv13