
2025-03-20 20:31

Source: #htb #DFIR 

Tags: [[Forensics]]

# [Brutus](https://app.hackthebox.com/sherlocks/Brutus) 

we’ll track how an attacker conducted an SSH brute force attack

On Debian/Ubuntu systems, this log is stored at `/var/log/auth.log`
while on RedHat/CentOS systems, it is located at `/var/log/secure`

The wtmp file is one of three files used to log login and logout events on a Linux system:

1. `/var/run/utmp`: Tracks currently logged-in users.
2. `/var/log/wtmp`: Maintains a historical record of login and logout events.
3. `/var/log/btmp`: Logs unsuccessful login attempts.

These files store their data in a binary format, so their contents cannot be easily understood when accessed directly. Linux utilities are used to parse and display their information.

- For `btmp`, the `who` (or `w`) command will display its contents.
- The `last` command can be used to view `wtmp`
- The `lastb` command is for examining `btmp`

To examine a `wtmp` file independently, the `utmpdump` utility can be used. It is included in the `util-linux` package, which can be installed via `sudo apt install util-linux`



### References
https://motasemhamdan.medium.com/hackthebox-sherlock-brutus-writeup-2fae098f4b64