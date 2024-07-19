
2024-07-19 16:47

Source: #tryhackme #forensics 

Tags: 

check `/var/log/apache2` for `access.log` file for analyzing tool operations performed in the past 

### Program Execution History

1. `bash_history` - Contains a history of commands run in bash; this file is well known, easy to edit and sometimes disabled by default.
2. `auth.log` - Contains a history of all commands run using `sudo`.
3. `history.log (apt)` - Contains a history of all tasks performed using apt - is useful for tracking programme installation and removal.

systemd services also keep logs in the journald system; these logs are kept in a binary format and have to be read by a utility like journalctl




### References
