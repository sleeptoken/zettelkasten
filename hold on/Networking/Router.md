
Works At layer 3 

Central competent of both our network and our security 
>The router is usually the default gateway for all traffic on the internet 

##### Find IP of router

List of default ip addresses https://www.techspot.com/guides/287-default-router-ip-addresses/
In windows
	- `route print`  
	- `ipconfig`
In linux 
	- `sudo route -n`
In Mac 
	- `route -n get default

Whenever You access a device via an IP address, you access it via a port 

Most Routers will have a web interface at Port 80 or 443
Login to the web interface using user pass 
https://www.routerpasswords.com/

Router will shield your devices from the internet 
It has an external and an internal IP address we communicate w internal IP adress (default gateway) to determine your external IP use whereismyip

No one will be able to make a direct connection to my internal devices via the internet because of NAT(network address translation)
Cause of NAT you would have to specifically configure within the router to forward traffic to a specific device on a specific port in order for internet devices to connect to your internal devices it is often call port forwarding or DMZ 

When you make connections out of the router to the internet. The router will remember who you have connected to and allows them to send traffic back to you in response this is called maintaining state and will be configured by default 

What we call a router these days are a collection of switch, firewall, router, modem, wireless access point 

## Switch

Switches keep a table of Ethernet mac addresses called a Mac table. It uses these unique mac addresses for your devices to send traffic to its destination on the land and works at the data link too.
Once data is traveling on your local network max addresses are used for traffic to find its destination to the local network. IP addresses are only used on the internet 

Switches are more secure than hub 
Cause switch have isolated collision domain (u can't sniff the traffic on the network )

So When traffic goes into the switch it knows what what the mac address is. So instead of sending the data of all the devices, it just sends it to that one physical port 
We can defeat ICD with OPP spoofing 

### modems 

They modulate and de modulate the signal so it can be passed onto the local loop of your network carrier 

### bridge 

Which forwards packets and filters based on MAC addresses. It forwards broadcast traffic but not collision 

### DHCP 

Cat /var/log/syslog | grep DHCP
If ipaddresses are not dynamically assigned by DHCP then they are statically assigned
### Os of router

- OpenWrt is used in a lot of devices
- Busybox is used in a lot of routers 
- LibreCMC 
- dd-wrt.com based on OpenWrt and is easier to use than openwrt 

An extension of `RADIUS`, called `TACACS` -
Access control for routers, network access servers and other networked computing devices 


| TCP/IP | OSI Model | Data Unit | Hardware | Protocols |
|---|---|---|---|---|
| Application | 7. Application | Data | Gateway | S/MIME, SMTP, SNMP, HTTP, LPD, FTP, TFTP, Telnet, POP, SMB, NNTP, CDP, GOPHER, NDS, AFP, SAP, NCP, SET |
| Application | 6. Presentation | Data | Gateway Redirector | No protocols. Service: TIFF, GIF, JPEG, ASCII, MPEG, MIDI, EBCDIC, POSTSCRIPT - Ensures confidentiality, Compression/Encryption |
| Application | 5. Session | Data | Gateway | SOCKS, RPC, NFS, SQL, NetBios, RADIUS, DNS, ASP - Doesn't ensure security, CORBA, DCOM, SOAP, .NET |
| Host-to-host | 4. Transport | TCP-Segments, UDP-Datagram | Gateway | TCP, UDP, SSL/TLS, SPX, SSH-2, ATP |
| Internet | 3. Network | Packets | Router, Brouter | IP, IPsec, ICMP, IGMP, RIP, OSPF, BGP, IPX, SKIP, SWIPE, NAT, IGRP, EIGRP, BOOTP, DHCP, ISIS, ZIP, DDP |
| Network access | 2. Data Link | Frames | Switch, Bridge, NIC | Ethernet 802.3, Token Ring 802.5, ATM, FDDI, 802.4, Wi-Fi 802.11, PPP, L2TP, SLIP, ARP, RARP, 802.1AE MACsec, HDLC |
| Network access | 1. Physical | Bits | Repeater, Hub, NIC, Cables, MAU, Multiplexer | ISDN, DSL, SONET, 10BASE-T, 10BASE2, 10BASE5, 100BASE-TX, 100BASE-FX, 1000BASE-T, 1000BASE-SX, EIA-X, RS-X |
