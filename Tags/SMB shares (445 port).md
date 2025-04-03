
2024-09-03 14:14

```
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse <ip>
```
Using [[nmap]] we can enumerate a machine for SMB shares.
### Enumeration 

Server Message Block (SMB) is a transport protocol that is generally used by Windows systems for providing shared access to files, printers, and serial ports as well as remote access to Windows services. 

By default, SMB runs directly on TCP port 445 or via the NetBIOS API on UDP ports 137 and 138 and TCP ports 137 and 139. By using the SMB service, users can access files, read, write, and modify the files and other data stored at a remote server. A network running this service is highly vulnerable to SMB enumeration, which provides a good amount of information about the target. 

In SMB enumeration, attackers generally perform banner grabbing to obtain information such as OS details and versions of services running. By using this information, attackers can perform various attacks such as SMB relay attacks and brute-force attacks. 

As shown in the screenshot, attackers use the following Nmap command to enumerate the SMB service running on the target IP address: 
```
nmap -p 445 -A <target IP> 
```
#### Enumeration Countermeasure

- Disable SMB protocol on Web and DNS Servers
- Disable SMB protocol on Internet facing servers 
- Disable ports TCP 139 and TCP 445 used by the SMB protocol 
- Restrict anonymous access through `RestrictNullSessAccess` parameter from the Windows Registry

### References
[[Enumeration]]