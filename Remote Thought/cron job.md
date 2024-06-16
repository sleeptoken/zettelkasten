
2024-06-15 21:28

Source: #privesc 

Tags: [[linux]]

#### Privilege Escalation

The system-wide crontab is located at `/etc/crontab`
##### File Permissions

1. view the contents of the system-wide crontab - `cat /etc/crontab`
2. Locate the (example file -  overwrite.sh) file on the server - `locate <file-name>`
3. Check the file’s permissions - Note that the file is world writable 
4. Replace the contents of the example file with a reverse shell
5. Run a netcat listener on your local machine and wait for the cron job to run.

##### PATH Environment Variable

- The system will check the directory locations in the path variable sequentially for cronjobs
1. view the contents of the system-wide crontab - `cat /etc/crontab`
2. Note that the `/home/user` directory (which we can write to) is at the start of the PATH variable, and the first cron job does not use an absolute path
3. Create the file overwrite.sh in `/home/user` with the [[rootbash]]:
```
	#!/bin/bash 
	cp /bin/bash /tmp/rootbash 
	chmod +s /tmp/rootbash
```
4. Ensure that overwrite.sh is executable
5. Once the `/tmp/rootbash` file is created, execute it (with -p to preserve the effective UID) to gain a root shell
##### Wildcard

- When a wildcard character `(*)` is provided to a command as part of an argument, the shell will first perform filename expansion (also known as globbing) on the wildcard.
- This process replaces the wildcard with a space-separated list of the file and directory names in the current directory.
- An easy way to see this in action is to run the following command from your home directory:
	$ echo *`

Since filesystems in Linux are generally very permissive with filenames, and filename expansion happens before the command is executed, it is possible to pass command line options (e.g. -h, --help) to commands by creating files with these names. 
The following commands should show how this works
```
$ ls * 
% touch ./-l 
$ ls *
```
- Filenames are not simply restricted to simple options like -h or --help. 
- In fact we can create filenames that match complex options: `--option=key=value` 
- GTFOBins (https://gtfobins.github.io) can help determine whether a command has command line options which will be useful for our purposes

in the example machine we have a cronjob named `/usr/local/bin/compress.sh`
cat the file and we see the contents of the file as
```
#!/bin/sh
cd /home/user
tar czf /tmp/backup.tar.gz *
```
on checking gtfo bins we find out there is a way to spawn a shell using tar using command line options. we can modify to our needs and spawn a reverse root shell back to our local machine 

on your local machine generate a revshell linux binary using msfvenom 



### References