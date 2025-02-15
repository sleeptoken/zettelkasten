
2024-06-13 12:40

Tags: [[linux]]

Often during pen tests you may obtain a shell without having `tty`, yet wish to interact further with the system. Here are some commands which will allow you to spawn a `tty` shell. Obviously some of this will depend on the system environment and installed packages.

Shell Spawning

```
python3 -c 'import pty;pty.spawn("/bin/bash")'

python -c 'import pty; pty.spawn("/bin/sh")'

echo os.system('/bin/bash') 

/bin/sh -i

perl -e 'exec "/bin/sh";'

perl: exec "/bin/sh";

ruby: exec "/bin/sh"

lua: os.execute('/bin/sh')

exec "/bin/sh"  ..... (From within IRB)

:!bash  ..... (From within vi) 

:set shell=/bin/bash:shell ..... (From within vi)

!sh  ..... (From within nmap)
```

Another method for spawning a TTY shell is documented in [[Stabilizing Shells and RootBash]] without using python

Many of these will also allow you to escape jail shells. The top 3 would be my most successful in general for spawning from the command line

### References

[Full TTYs | HackTricks | HackTricks](https://book.hacktricks.xyz/generic-methodologies-and-resources/shells/full-ttys)

[What is the exact difference between a 'terminal', a 'shell', a 'tty' and a 'console'? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/4126/what-is-the-exact-difference-between-a-terminal-a-shell-a-tty-and-a-con)