
2024-06-13 12:40

Source: #youtube #google

Tags: [[linux]]

Often during pen tests you may obtain a shell without having tty, yet wish to interact further with the system. Here are some commands which will allow you to spawn a tty shell. Obviously some of this will depend on the system environment and installed packages.

Shell Spawning\

```
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
reference deleted from the web