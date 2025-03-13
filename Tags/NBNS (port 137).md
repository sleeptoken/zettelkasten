
The first step in enumerating a Windows system is to take advantage of the NetBIOS API. #CTF 

NetBIOS was originally developed as an API for client software to access local area network (LAN) resources. 
- Windows uses NetBIOS for file and printer sharing.

The NetBIOS name is a unique 16-character ASCII string assigned to Windows systems to identify network devices over TCP/IP; 15 characters are used for the device name, and the 16th is reserved for the service or record type. 

> NetBIOS uses UDP port 137 (name services), UDP port 138 (datagram services), and [[NetBIOS Session Service (port 139)]] TCP port 139 (session services). 

Attackers usually target the NetBIOS service because it is easy to exploit and run on Windows systems even when not in use. 
Attackers use NetBIOS enumeration to obtain the following: 
- The list of computers that belong to a domain 
- The list of shares on the individual hosts in a network 
- Policies and passwords 

An attacker who finds a Windows system with port 139 open can check to see which resources can be accessed or viewed on a remote system. However, to enumerate the NetBIOS names, the remote system must have enabled file and printer sharing. NetBIOS enumeration may allow an attacker to read or write to a remote computer system, depending on the availability of shares, or launch a DoS attack.

##### Nbtstat Utility 

used to troubleshoot NETBIOS name resolution problems



Note: 
NetBIOS name resolution is not supported by Microsoft for Internet Protocol Version 6 (IPv6)