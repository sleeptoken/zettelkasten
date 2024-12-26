
2024-12-25 10:18

Tags: 
## IP Addressing and Subnetting

-  IP address -- in its simplest form -- is just a series of `1` and `0` bits (binary or base 2 numbering)  
    - **IPv4** is 32 bits -- thirty-two 1s and 0s
    - **IPv6** is 128 bits -- one hundred and twenty eight 1s and 0s
- We can take **base 2** and convert to **base 8, base 10,** and **base 16**
    - Converting the IP address to **decimal (base 10)** makes it easier to store that information in databases
More information - [The many faces of an IP address](https://web.archive.org/web/20230924135354/https://www.hacksparrow.com/networking/many-faces-of-ip-address.html)
## Network Masking

An IP address is _**meaningless without a network mask**_. Given only an IP address, it is impossible to be certain which network this IP address is a member of. You may be able to infer, but you cannot be certain.

```
192 . 168 . 1 . 1 <-- IP Address
11000000 . 10101000 . 00000001 . 00000001

255 . 255 . 255 . 0 <-- Subnet Mask
11111111 . 11111111 . 11111111 . 0
```
 
- Think of it like a zip code (or postal code)
    - A postal code confines residences to a specific area
    - A network mask confines hosts to a specific route
### No More Classful Networking

- The world now operates on **Classless Inter-Domain Routing (CIDR)**
- Instead, you should think of networks in terms of **internet-routable** and **non-internet-routable**

**More Information:  
**[https://docs.netgate.com/pfsense/en/latest/network/addresses.html](https://docs.netgate.com/pfsense/en/latest/network/addresses.html)  
[https://en.wikipedia.org/wiki/Reserved_IP_addresses](https://en.wikipedia.org/wiki/Reserved_IP_addresses)
### Exploring Private IPv4 Network Masks

- A network mask can be broken up into two parts
    - The **network** segment
    - The **host** segment
- **Private IPv4 Address Spaces**
    - `10.0.0.0/8`
        - Any host address between `10.0.0.1` and `10.255.255.254` is valid
    - `172.16.0.0/12`
        - Any host address between `172.16.0.1` and `172.31.255.254` is valid
    - `192.168.0.0/16`
        - Any host address between `192.168.0.1` and `192.168.255.254` is valid

> The **/8, /12, /16** are called **CIDR notation** or **CIDR blocks**. The number appearing after the slash ( `**/**` ) directly correlates to the sum of `1`  bits in the binary notation of subnet mask. So **/8** indicates that eight bits have been switched on.

Given a subnet mask, you can calculate the _**total number of possible networks**_ the following ways:

1. Find the _**partial network mask**_
    - A fully masked network ID is _**8 bits**_ -- `11111111`
    - `/8` , `/16` , `/24` , `/32` represent CIDR masks where network ID octets are fully masked
        - `11111111.00000000.00000000.00000000` = `/8`
        - `11111111.11111111.00000000.00000000` = `/16`
        - `11111111.11111111.11111111.00000000` = `/24`
    - `/12` is _**between**_ `/8` and `/16`
        - `11111111.11110000.00000000.0000000`  
                 full          partial         host         host
2. Then...
    - Use the _**decimal value**_ of the network mask
    - Or, use the **_number of host bits_** as an exponent
further listed here - https://notes.benheater.com/link/376#bkmrk-exploring-private-ip
## **Subnetting**

### Why Do We Subnet? 

**A)** To shrink the size of a network  
**B)** To make multiple networks from a single network block

to learn to do these go to - https://notes.benheater.com/link/376#bkmrk-why-do-we-subnet%3F
## MAC Addressing 

Media Access Control Address
- Identifies the physical network interface of a host
    - Hexadecimal addressing: `0-F`
    - 48 bits (6 bytes)
In early, primitive networks, computers were often on a shared "media" and needed a way to address each other  
- On a shared medium, all computers will receive the data
- Network interfaces _**are supposed to**_ (see promiscuous mode) ignore anything not addressed to them
### Broadcast Domain vs Collision Domain

- the **broadcast domain** is designated as the **entire** Local Area Network (LAN)
    - When a host sends a layer 2 broadcast -- `FF:FF:FF:FF:FF:FF` -- every host on the LAN receives it
- the **collision domain** is designated as the **shared media** between hosts, the cable
    - Traffic from both switches can be on the cable in duplex, and therefore has the possibility to collide
## Address Resolution Protocol (ARP)

- IP addressing is the primary way we identify hosts belonging to a network
    - It's easier to remember than a MAC address
- When preparing to transmit data, to another host on the network, the sender needs to know two things:
    - The recipient IP address
    - The recipient MAC address
- ARP is the protocol that is used to look up a MAC address for any given IP address
	- Send a layer 2 broadcast -- `FF:FF:FF:FF:FF:FF`
## Virtual Local Area Networks

- VLANs are a _**layer 2**_ concept
    - `802.1q` is the protocol that defines VLAN
    - The `802.1q` header is injected into the Ethernet frame of a packet
    - A `802.1q` ID can range from `1` to `4094`, due to limits on the maximum size of an Ethernet frame
- Not going to do a deep dive on VLANs, but hope to clear some things up
    - [How Do VLANs Work? Exploring the 802.1q Protocol (benheater.com)](https://benheater.com/how-do-vlans-work/)
    
## Encapsulation Visualization

**[WildPackets Protocol Family Encapsulations (lmi.net)](http://users.lmi.net/canepa/subdir/encasulation_chart.pdf?ref=benheater.com)**
## OSI Model Visualization

- **[OSI Model Visualization (benheater.com)](https://benheater.com/osi-model-visualization/)**



## References
[Computer Networking | 0xBEN | Notes](https://notes.benheater.com/books/computer-networking)

[IP Address Converter: Dotted-decimal, Binary, Octal, Hexadecimal, Dword/Decimal](https://web.archive.org/web/20230924142818/https://www.hacksparrow.com/tools/converters/ip-address.html)