
2025-01-06 17:41

Source: #htb 
### Reconnaissance

looking at the dockerfile we know The flag is being placed in an environment variable.
##### routes.py

- The `URL` parameter get validated if it’s not empty and the static `SITE_NAME` gets added before doing a request to this URL. 

> There is a `/debug/environment` route that returns the environment variables, only when the parameter `URL` is a local URL.
##### utils.py

There is a denylist that prevents the proxy from serving local URLs to prevent Server-Side Request Forgery attacks.
```python
RESTRICTED_URLS = ['localhost', '127.', '192.168.', '10.', '172.']
```
### Exploit

we want to overcome 2 challenges 
#### Challenge 1: `is_from_localhost` Validation Method

The first challenge is the validation method known as `is_from_localhost`. This check is designed to verify if the provided URL is a local one. Bypassing it is not overly complex since denylists are often incomplete as it’s challenging to cover every possible payload. This holds true for the `RESTRICTED_URLS` denylist as well. Numerous bypass techniques exist, and you can find a comprehensive list on [hacktricks](https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery/url-format-bypass). For instance, one example would be [`http://0.0.0.0/`](https://en.wikipedia.org/wiki/0.0.0.0).
#### Challenge 2: Manipulating `SITE_NAME` in `target_url`

The second obstacle requires a bit more creativity. There are at least two strategies to approach it:
##### Strategy 1: Utilizing the “@” Symbol





### References
https://app.hackthebox.com/challenges/proxyasaservice

walkthrough
https://vulnerable.sh/posts/htb_proxyasaservice/
