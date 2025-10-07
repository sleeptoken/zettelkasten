Advanced firewalls run at the application layer or on a dedicated hardware firewalls
Advanced firewalls will also look at the content entering through the port and check if it fits the rules, rather than just looking at the port and allowing everything that enters that port 
### ingress traffic filtering

Inbound traffic filtering
On all firewalls, whether they're host based or network based, there should be an implicit deny all to external traffic connection inbound unless it's specially required. In home routers that isn't required because of NAT. But on a host based firewall that may very well need to be configured 
### egress / outbound filtering 

Rev shell ...
Outbound connections from a home network to an internet IP is how malware communicates not via inbound communications
#### network isolation 

This will be to prevent the propagation of an attack from internal device to another internal device. 

### on a home network 

#### network based firewall 

They filter traffic between the home network to the internet and vice versa 
The firewall could be on your router commonly using IP tables and that possibly is the easiest way to get yourself a network based firewalls is to install custom firmware with IP tables 

Major disadvantage of network based firewalls is an inability to block outbound connections because we can't distinguish between Good and bad traffic because Malware use common ports that are open like 443 to communicate out 
### virtual firewalls

Have a virtual box with a pfsense VM to protect guest virtual machines

Go to network settings of your pfSense VM > adapter 1 should be attached to NAT > adapter 2 should be a internal pfsense network 

If you want some other VM to use this firewall then go to the network setting of that vm and select internal pfsense network in adapter 1

#### Dynamic packet filtering and stateful packet inspection 

You only need to allow outbound connections you don't need a inbound rule for traffic to return to you

When a connection is made a source port is sent, the file remembers this port and automatically allows through dynamic access control lists the inbound connections while the session is running 

When the connection is closed with a FIN or RST packet, the firewall removed the dynamic access control list. And for UDP, which is connection less, the access control list just times out

# windows host based 

### windows firewall (free and pre-installed )

Default policy is inbound deny and outbound allow all (not ideal config)
#### profiles (public, private)

When connecting to a network the systems asks what type of profile it wants to use for that network i.e. what firewall profile to use

Disable Upnp (recommend) while setting rules 

#### limitation of windows firewall 

- If you use a program that executes from a temporary folder, you won't be able to create a rule. So even if you create a rule for a executable file, if that file is executed from a different folder. That rule will not work
	A new rule is required for each location 

##### Windows Firewall Control 

You can fix the GUI problem by installing `Windows Firewall Control`, it's a client app for original firewall app 

##### Comodo

Remove geekbuddy and don't download the whole suite, only and only use Comodo firewall

The thing about third party firewalls like Comodo is that it tells you what applications are trying to connect outbound whereas the windows firewall doesn't.

Other alternatives are glasswire, tinywall, Kaspersky etc
# Linux based firewall 

### IP tables 

All modern Linux kernels use the net filter system as a firewall solution for packet filtering. The kernel packets filtering system net filter needs a user interface. IP tabels is one such interface.
#### how it works

When a packet reaches a device, it is handed off to the net filter subsystem for acceptance, manipulation or rejection based on the rules supplied to it via IP tables. 
> IP tables is the database of firewall rules 

```sh
$ iptables -L
Chain INPUT
# Controls inbound connections 
# eg you want to ping this laptop 
Chain FORWARD 
# Control inbound connections but for those destined to be forwarded on to another device and not this device 
# Think of it as a chain for like a router that forwards traffic. 
Chain OUTPUT
# Controls outbound connections
# eg you wanna surf the web enable port 80
```
IP tabels Will start from the top and searches if any rules matches if nothing does then it executes the default behaviour 

### Uncomplicated Firewall (UFW)

- Is considered as a base for `GUFW`
- `Shorewall` has more power than `UFW`
### NFtables 

NFtables is a project that aims to replace the existing IPtables also `ebtables` and arptables 
- IP 6 tables are the IPv6 version of IP tables 
- `ebtables` is a filtering tool for Linux based bridging firewalls. It enables transparent filtering of network traffic passing through a Linux bridge 
Arptable is for maintaining the address resolution protocol arp packet filtering rules in a Linux kernel firewall modules 
# Mac

#### Application firewall
If you wanna configure using cmd, the default config is stored in the `/usr/libexec/ApplicationFirewall/com.apple.alf.plist`
Read more about it in documentation 

#### PF(firewall)
This is comparable to netfilter and IPtables in linux. It provides full functionality of an IP firewall such as iptables at the network and transport layer 
`pfctl` is the main command 
Configs are stored in anchor files 

GUI for pflists - icefloor, murus (has more power), little snitch 

# Network based firewalls 

##### Router - DDWRT
Custom firmware installed on routers will give access to more firewall features

Firewall builder (fwbuilder.org)
If you are looking for a third party firewall GUI that you would run on a seperate machine, which will manage the iptable rules on the router and does it remotely via SSH 
##### Hardware 
Use this if you want more functionality, power and ease of configuration.
##### pfsense 
Based on FreeBSD, based on pffirewall More powerful than DDWRT 
Opnsense - open source version of pfsense 
Vyos - is more In line with Cisco stuff, is quite fast

## arp spoofing and switches 

Switches have mactable 
On a Ethernet network connected via switch it is relatively easy to perform man in the middle attack, an attacker fools other devices on the network into believing that the attacker is the default gateway or the router. By abusing the ARP
If you have a hub you can observe traffic anyway, but if you have a switch then you would need to do arp spoofing 

Arp is IP(network layer) to Mac(data link layer) resolution 

Arp spoofing is possible cause absolutely no authentication is required 

> Block outbound connections for DMZ only enable temporarily for software updates 

Can be implemented using firewall rules like iptables, NAT, having this network not routable to other networks that you do not wish to communicate with

### vlans
Works at layer2 
Vlans are logical seperation of the networks and they use tags included in the packets that are sent between the devices on the networks in order to determine that they are separate VLANS.

Tags are included in the data packets based on the physical port a computer is plugged into on a switch, tagging mechanism are ISL or 802.1Q

Vlans will have no idea that other networks exists if configured properly 
##### vlan hopping 
This is used to hop between different vlans 
Kali tool - yersinia 

#### Arp protection 
Sniffdat, xarp, arpwatch, tuxcut

#### DHCP snooping 
Is a feature in some Cisco switches that watches DHCP requests and builds a table of IP addresses to Mac addresses similar to an arp table that relies on the DHCP sever to be the trusted authority 