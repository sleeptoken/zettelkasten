
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

goal instead is going to be to overflow this buffer and overwrite the return address so instead of returning back to main we want to say jump to esp and that's going to jump to the stack

### References
https://www.youtube.com/watch?v=4zut2Mjgh5M&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=6