
2024-07-19 16:47

Source: #tryhackme #forensics 

check `/var/log/apache2` for `access.log` file for analyzing tool operations performed in the past 
the Apache access log keeps a history of all of the requests sent to the webserver
### Program Execution History

1. `bash_history` - Contains a history of commands run in bash; this file is well known, easy to edit and sometimes disabled by default.
2. `auth.log` - Contains a history of all commands run using `sudo`.
3. `history.log (apt)` - Contains a history of all tasks performed using apt - is useful for tracking programme installation and removal.

`systemd` services also keep logs in the `journald` system; these logs are kept in a binary format and have to be read by a utility like `journalctl`
### Script running in background 

Malware can also maintain persistence using [[systemd]] as scripts run under systemd can run in the background and restart whenever the system is booted or whenever the script crashes.

- Running `systemctl` will list all of the services loaded into the system. And much like Windows, there's usually a lot of them. It might be worth adding 
- `--type=service --state=active` to the command as it will reduce the list to services that are running. 
- Once the name of a suspicious service is found, more information can then be extracted by running `systemctl status <service name>`


### References
