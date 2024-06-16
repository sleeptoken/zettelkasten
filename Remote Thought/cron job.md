
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


### References
