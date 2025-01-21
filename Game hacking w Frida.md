
2025-01-21 20:54

Source: #tryhackme #AOC24 

Tags: [[Frida]]
### Example

originally the code behaves like this 
```sh
ubuntu@thm:~$ ./main
Hello, 1
Hello, 1
```
  
What we want to achieve is replacing that value with an arbitrary one, let's say 1337.

Before proceeding, we will run `frida-trace` for the first time so that it creates **handlers** for each library function used by the game. 
By editing the handler files, we can tell Frida what to do with the intercepted values of a function call. 
To have Frida create the handler files, you would run the following command

```
frida-trace ./main -i '*'
```

You will now see the `__handlers__` directory, containing JavaScript files for each function your application calls from a library.

Each handler will have two functions known as `hooks` since they are hooked into the function respectively before and after the function call






### References
