
2024-09-21 14:12

Tags: [[Forensics]]

```bash 
# basic syntax for volatality 
sudo vol -f <file-name> <plugin name>
```

**Getting list of processes using `windows.pslist`**
- This plugin gives running processes on the machine at the time of the memory dump. Just like running `ps` on linux system.
```bash
vol -f memory_dump.elf windows.pslist
```

**Getting live connections using `windows.netscan`**
- This plugin gives us the connections at the time of the memory dump. Just like running `netstat` command.
```bash
vol -f memory_dump.elf windows.netscan
```

**Getting commands run by using `windows.cmdline`**
- We can get all the commands that were used to run the process using the command:
```bash
vol -f memory_dump.elf windows.cmdline
```

- But to only get the command of a particular process, we can use PID with `--pid` flag (eg. of powershell)
```bash
vol -f flounder-pc-memdump.elf windows.cmdline --pid 2752 
```






### References
https://app.hackthebox.com/challenges/Reminiscent