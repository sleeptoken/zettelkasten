
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

One approach is to exploit the presence of the “@” symbol within an URL for authentication. By placing the “@” symbol in the URL, the server may interpret everything preceding the “@” symbol as authentication credentials and disregard it, thereby gaining control over the entire URL.

Attack URL: `http://206.189.28.151:30600/?url=@0.0.0.0:1337/debug/environment`

Upon successful exploitation, you’ll gain access to the environment variables
##### Strategy 2: DNS Rebinding Attack

Another strategy involves performing a DNS rebinding attack configuring a domain you own to redirect to 127.0.0.1. The domain should reflect the URL that is being built in the `targetURL` variable. Knowing that “reddit.com” gets prepended to the URL we can setup a subdomain that is `"reddit.com" + subdomain_value`.

In this example, I’m using a domain I own “http://onlyexploits.com” and create a subdomain “reddit.comlocal”

Attack URL: `http://206.189.28.151:30600/?url=local.onlyexploits.com:1337/debug/environment`
### Mitigations

There are a bunch of measures that could been taken to prevent SSRF. A few specific ones could be:

1. Use an Allowlist over a Denylist, as it’s less prone to faults by explicitly specifying what is permitted.
2. Avoid dynamic target URLs
3. Use proper authentication and authorization mechanisms to control access to sensitive endpoints, such as the `/debug/environment` route. Restrict access to trusted users only.
### References
https://app.hackthebox.com/challenges/proxyasaservice

walkthrough
https://vulnerable.sh/posts/htb_proxyasaservice/

Hacktricks
https://book.hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/url-format-bypass.html