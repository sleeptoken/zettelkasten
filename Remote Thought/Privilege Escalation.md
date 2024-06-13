
2024-06-13 15:33

Source: #privesc 

Tags: 

All Privilege Escalation are effectively examples of access control violations.
#### Permissions in Linux
##### User 
- Users are identified by an integer user ID (UID). The “root” user account is a special type of account in Linux. It has an UID of 0, and the system grants this user access to every file
##### Group
- By default, a user’s primary group has the same name as their user account
##### File & Directory  
- All files & directories have a single owner and a group.
- 3 types of file and directory permissions (file perms and dir perms look the same but have different functionalities) - 
		- execute
		- read
		- write 
##### Special Permissions
- **setuid (SUID)** bit When set, files will get executed with the privileges of the file owner.
- **setgid (SGID)** bit When set on a file, the file will get executed with the privileges of the file group. 
- When set on a directory, files created within that directory will inherit the group of the directory itself
##### Viewing Permission
- The first 10 characters indicate the permissions set on the file or directory.
- The first character simply indicates the type (e.g. '-' for file, 'd' for directory)
- The remaining 9 characters represent the 3 sets of permissions (owner, group, others).
- Each set contains 3 characters, indicating the read (r), write (w), and execute (x) permissions. 
- SUID/SGID permissions are represented by an 's' in the execute position

each user has 3 user IDs in Linux (real, effective, and saved)
A user’s real ID is who they actually are (the ID defined in /etc/passwd).


### References

[Linux Privilege Escalation Tutorial: Become an Ethical Hacker | Udemy](https://www.udemy.com/course/linux-privilege-escalation/)