
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









### References
