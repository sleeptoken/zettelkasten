
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
#### Frida Handlers

Before proceeding, we will run [[Tracing with Frida]] `frida-trace` for the first time so that it creates **handlers** for each library function used by the game. 
By editing the handler files, we can tell Frida what to do with the intercepted values of a function call. 
To have Frida create the handler files, you would run the following command

```sh
frida-trace ./main -i '*'
```

You will now see the `__handlers__` directory, containing JavaScript files for each function your application calls from a library.

Each handler will have two functions known as `hooks` since they are hooked into the function respectively before and after the function call : `onEnter`, `onLeave`

We have pointers and not just variables because if we change any value, it has to be permanent; otherwise, we will modify a copy of the value

we want to set the parameter with 1337. To do so, we must replace the first arguments of the args array: `args[0]` with a pointer to a variable containing 1337.

Frida has a function called `ptr()` that does exactly what we need: allocate some space for a variable and return its pointer. We also want to log the value of the original argument, and we have to use the function `toInt32()`, which reads the value of that pointer.

```javascript
// say_hello.js
// Hook the say_hello function from libhello.so

// Attach to the running process of "main"
Interceptor.attach(Module.findExportByName(null, "say_hello"), {
    onEnter: function (args) {
        // Intercept the original argument (args[0] is the first argument)
        var originalArgument = args[0].toInt32();
        console.log("Original argument: " + originalArgument);
        // Replace the original value with 1337
        args[0] = ptr(1337);
        log('say_hello()');
    }
});
```

```sh
frida-trace ./main -i 'say*'
```
The parameter `-i` tells Frida which library to hook, and it can filter using the wildcard, tracing all the functions in all the libraries loaded.
### Lab

#### Bypass OTP 

a function was being called that generated the OTP

execute the following Frida command to intercept all the functions in the `libaocgame.so` library where some of the game logic is present
```sh
frida-trace ./TryUnlockMe -i 'libaocgame.so!*'
```

access the file with `.so` extension - `/home/ubuntu/Desktop/TryUnlockMe/__handlers__/libaocgame.so/`

To get the parameter value, you can use the `log` function, specifying the first elements of the array `args` on the `onEnter` function:
```javascript
log("Parameter:" + args[0].toInt32());
```
#### changing value of an arg

find out what argument corresponds to what value by using the following 
`log("Parameter1:" + args[0].toInt32())`  
`log("Parameter2:" + args[1].toInt32())`  
`log("Parameter3:" + args[2].toInt32())`

By simple inspection, we can determine that the first parameter is the Item ID, the second is the price, and the third is the player's coins. If you manipulate the price and set it as zero, you can buy any item that you want:

`args[1] = ptr(0)`
#### change Boolean values 

You can log the return value of the function by adding the following log instruction in the `onLeave` function:
```javascript
 log("The return value is: " + retval);
 
 //output
 69399931 ms The return value is: 0x0
```
the value returned is 0, which may indicate that it is a Boolean flag set to False.
in order to switch the flag 
`retval.replace(ptr(1))`
### References
