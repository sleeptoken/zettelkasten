
2024-06-13 15:33

Source: #privesc 

Tags: 

All privesc are effectively examples of access control violations.

#### Permissions in Linux
##### User 
- User accounts are configured in the /etc/passwd file. 
- User password hashes are stored in the /etc/shadow file.
- Users are identified by an integer user ID (UID). The “root” user account is a special type of account in Linux. It has an UID of 0, and the system grants this user access to every file
##### Group
- Groups are configured in the /etc/group file. 
- By default, a user’s primary group has the same name as their user account
#### File & Directory  
- All files & directories have a single owner and a group.
- Only the owner can change permissions.
- 3 types of file and directory permissions - 
		- execute
		- read
		- write 


### References

[Linux Privilege Escalation Tutorial: Become an Ethical Hacker | Udemy](https://www.udemy.com/course/linux-privilege-escalation/)