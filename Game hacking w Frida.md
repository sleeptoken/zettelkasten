
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

Before proceeding, we will run [[Tracing with Frida]] `frida-trace` for the first time so that it creates **handlers** for each library function used by the game. 
By editing the handler files, we can tell Frida what to do with the intercepted values of a function call. 
To have Frida create the handler files, you would run the following command

```sh
frida-trace ./main -i '*'
```

You will now see the `__handlers__` directory, containing JavaScript files for each function your application calls from a library.

Each handler will have two functions known as `hooks` since they are hooked into the function respectively before and after the function call : `onEnter`, `onLeave`

We have pointers and not just variables because if we change any value, it has to be permanent; otherwise, we will modify a copy of the value

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

### References
