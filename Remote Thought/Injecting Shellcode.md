
2024-07-02 11:05

Source: #youtube 

Tags: [[Binary Exploit]]

to change owner of a file to root use -
```
sudo chown root:root flag.txt
```

to set up the sticky bit [[Permissions]] for a file - 
```
sudo chmod 4655 file 
```

our goal is going to be to overflow this buffer and overwrite the return address so instead of returning back to main we want to say jump to `esp` and that's going to jump to the stack

identify `eip` offset using cyclic command to overflow the buffer 

if the instruction in the `eip` was saying jump `esp`, it would jump here and it would start executing whatever's on the stack providing that we don't have that `nx` set (the no execute protection)

we can find the jump `esp` gadget using `ropper`  - 
```
ropper --file <file_ name> --search "jmp esp"
```

we are using `shellcraft` to generate a script than will help us get a shell and using that script in our `pwntools` script

you'll see this time it's` bin/sh` (from `shellcraft`) that's pushing to the stack instead again if it was 64-bit this would be getting popped to the `rdi` register in order for the function to be called 
### References
https://www.youtube.com/watch?v=4zut2Mjgh5M&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=6