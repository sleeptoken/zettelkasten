
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




### References
[Computer Networking | 0xBEN | Notes](https://notes.benheater.com/books/computer-networking)

[IP Address Converter: Dotted-decimal, Binary, Octal, Hexadecimal, Dword/Decimal](https://web.archive.org/web/20230924142818/https://www.hacksparrow.com/tools/converters/ip-address.html)