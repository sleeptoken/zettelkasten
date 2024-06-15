
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

- the system will check the directory locations in the path variable sequentially for cronjobs
- view the contents of the system-wide crontab - `cat /etc/crontab`
- Note that the `/home/user` directory (which we can write to) is at the start of the PATH variable, and the first cron job does not use an absolute path



### References
