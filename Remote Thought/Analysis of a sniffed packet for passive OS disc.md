
2025-01-30 10:29

Source: #CEHv13 

Tags: [[Scanning and Networks]]

The following is an analysis of a sniffed packet for passive fingerprinting: 

```
2024-03-15 11:5.330465 10.10.1.11 -> 10.10.1.22 

Time To Live: 128 
Protocol: ICMP (1) 
Fragment Offset: 0 
Differentiated Service Field: 0x00 (DSC, CSO, ECN, NOT-ECT)
Ack: 0xE3C65D7 Win: 0x7D78 
```

According to the four criteria, the following are identified: 
- TTL: 45 
- Window Size: 0x7D78 (or 32120 in decimal) 
- DF: The DF bit is set 
- TOS: 0x0 

Compare this information with a database of signatures. 
#### TTL

The TLL from the analysis is 45. The original packet went through 19 hops to get to the target, so it sets the original TTL to 64. Based on this TTL, it appears that the user sent the packet from a Linux or FreeBSD box (however, more system signatures need to be added to the database). 
- This TTL confirms it by implementing a traceroute to the remote host. If the trace needs to be performed stealthily, the traceroute TTL (default 30 hops) can be set to one or two hops fewer than the remote host (-m option).
- Setting the traceroute in this manner reveals the path information (including the upstream provider) without actually contacting the remote host. 
#### Window Size

The window size is another effective tool for determining precisely what window size is used and how often it is changed. In the previous signature, the window size is set at 0x7D78, which is the default window size used by Linux. 
- In addition, FreeBSD and Solaris tend to maintain the same window size throughout a session.
- However, Cisco routers and Microsoft Windows NT window sizes constantly change. 
The window size is more accurate when measured after the initial three-way handshake (due to TCP slow start). 
#### DF bit

Most systems use the DF bit set; hence, this is of limited value. However, this makes it easier to identify a few systems that do not use the DF flag (such as SCO or OpenBSD). 
#### TOS

TOS is also of limited value, as it seems to be more session-based than OS-based. In other words, it is not so much the OS as the protocol used that determines the TOS to a large extent.

Passive fingerprinting, like active fingerprinting, has some limitations. 
1. Applications that build their own packets (e.g., Nmap, Hunt, Nemesis, etc.) will not use the same signatures as the OS.
2. It is relatively simple for a remote host to adjust the TTL, window size, DF, or TOS setting on the packets. 

### References

Passive banner grabbing