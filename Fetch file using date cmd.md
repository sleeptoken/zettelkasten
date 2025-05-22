
2025-05-22 11:06

Source: [[linux]]

Tags: [[PHP]]

The first thing was to understand the following exec command in php source code.

```
$result = shell_exec("date '+" . escapeshellcmd($format) . "' 2>&1");
```

The only parameter we could change was format and fortunately it was not being sanitized. So I was sure that I have to exploit something from here.  
I understood that the following command runs on the server
```
date +'<format here>'
```

For a long time I kept thinking ways to execute `cat` command somehow but the `escapeshellcmd` function wouldn’t allow me to do so. So I went ahead and opened the man page of date using `man page` and see if I could print a file using this and I did. The `-f` flag can open DATEFILE and read date from it, however if the file format was not DATEFILE, it would print the contents of the file as part of the error. I got it!  
So I carefully crafted a request to read the flag file (which btw had to be guessed where is) - turned out it was at /flag.  
`format=' -f /flag'`  
I used this to first close the opening single quote, then read the file and then open the closing the single quote. I URL encoded it and sent the request to get the flag! Final request looked like this

```
GET /?format=%27%20-f%20%2Fflag%27 HTTP/1.1
Host: strange.kp7b0h3vueu5g.ap-south-1.cs.amazonlightsail.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:126.0) Gecko/20100101 Firefox/126.0
Content-Length: 4
```
### References
date man page - https://man7.org/linux/man-pages/man1/date.1.html

https://0xkn1gh7.com/2024/06/20/Bitguard-Cybersecurity-Hackathon-2024-Writeups/