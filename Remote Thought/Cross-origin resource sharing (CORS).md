
2024-07-25 22:48

Source: #web #portswigger 
#### The response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS. 

Cross-origin resource sharing (CORS) is a browser mechanism which enables controlled access to resources located outside of a given domain. It extends and adds flexibility to the same-origin policy (SOP). 
### Same-origin policy 

The same-origin policy ([[SOP]]) is a restrictive cross-origin specification that limits the ability for a website to interact with resources outside of the source domain. It generally allows a domain to issue requests to other domains, but not to access the responses. 
### Relaxation of the same-origin policy

The same-origin policy is very restrictive and consequently various approaches have been devised to circumvent the constraints. Many websites interact with subdomains or third-party sites in a way that requires full cross-origin access. A controlled relaxation of the same-origin policy is possible using cross-origin resource sharing (CORS).

The cross-origin resource sharing protocol uses a suite of HTTP headers that define trusted web origins and associated properties such as whether authenticated access is permitted. \
### Server-generated ACAO header from client-specified Origin header

 - Some applications need to provide access to a number of other domains. Maintaining a list of allowed domains requires ongoing effort, and any mistakes risk breaking functionality. So some applications take the easy route of effectively allowing access from any other domain.
- One way to do this is by reading the Origin header from requests and including a response header stating that the requesting origin is allowed.

CORS Header (shown in the response) -
- `Access-Control-Allow-Origin: http://random-website.com`
- `Access-Control-Allow-Credentials: true`
Adding the origin header in the request - `origin : http://random-website.com`
- if the application accepts it that means the application accepts any random origin on the internet to access its data 
- if the application does not accept it or throws an error that means the application only accepts certain domains to access its data and it's usually just the domain of the site or the subdomains of the site

- `Access-Control-Allow-Credentials` header which allows credentials to be passed in the request that could be either cookies or authorization headers or certificates but in this case the application makes use of a session cookie so what's going to get passed in the request is session cookies
- having both headers is very dangerous because what that means is not only can you access public resources from the site but you could also access private resources  

Some applications that support access from multiple origins do so by using a whitelist of allowed origins. When a CORS request is received, the supplied origin is compared to the whitelist. If the origin appears on the whitelist then it is reflected in the `Access-Control-Allow-Origin` header so that access is granted
### Null origin 

Browsers might send the value null in the Origin header in various unusual situations:
    - Cross-origin redirects.
    - Requests from serialized data.
    - Request using the file: protocol.
    - Sandboxed cross-origin requests.

1. Try changing the origin to a arbitrary value `https://example.com `and see if it accepts it notice that we don't have the `access-control-allow-origin` header with our example url being added to it that means it doesn't accept arbitrary origins.
2. Try changing the origin to `null` , it gives us the `access-control-allow-origin` header and it tells us that the null value or the null origin is allowed to access resources in their application and not only can it access public resources, it can also access private resources because the `access-control-allow-credentials` header is set to true
### Exploiting XSS via CORS trust relationships

Even "correctly" configured CORS establishes a trust relationship between two origins. If a website trusts an origin that is vulnerable to cross-site scripting ([[XSS]]), then an attacker could exploit the XSS to inject some JavaScript that uses CORS to retrieve sensitive information from the site that trusts the vulnerable application.

Given the following request:
```
GET /api/requestApiKey HTTP/1.1
Host: vulnerable-website.com
Origin: https://subdomain.vulnerable-website.com
Cookie: sessionid=...
```
If the server responds with:
```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://subdomain.vulnerable-website.com
Access-Control-Allow-Credentials: true
```

Then an attacker who finds an XSS vulnerability on subdomain.vulnerable-website.com could use that to retrieve the API key, using a URL like:
```
https://subdomain.vulnerable-website.com/?xss=<script>cors-stuff-here</script>
```
### Breaking TLS with poorly configured CORS

Suppose an application that rigorously employs HTTPS also whitelists a trusted subdomain that is using plain HTTP. 

find a functionality on the website that redirects to a subdomain and try finding a xss vulnerability on the subdomain 

---

`Access-Control-Allow-Credentials: true`

Without that header, the victim user's browser will refuse to send their cookies, meaning the attacker will only gain access to unauthenticated content, which they could just as easily access by browsing directly to the target website.

However, there is one common situation where an attacker can't access a website directly: when it's part of an organization's intranet, and located within private IP address space. Internal websites are often held to a lower security standard than external sites, enabling attackers to find vulnerabilities and gain further access. 
### Prevent CORS-based attacks

- If a web resource contains sensitive information, the origin should be properly specified in the `Access-Control-Allow-Origin` header. 
- Avoid using the header `Access-Control-Allow-Origin: null`. Cross-origin resource calls from internal documents and sandboxed requests can specify the null origin. 
- Avoid using wildcards in internal networks. Trusting network configuration alone to protect internal resources is not sufficient when internal browsers can access untrusted external domains. 
- CORS defines browser behaviors and is never a replacement for server-side protection of sensitive data. web servers should continue to apply protections over sensitive data, such as authentication and session management, in addition to properly configured CORS. 
# Testing for CORS Miscofig

- change the origin of an arbitrary value/ True
- change the origin to a null value
- change the origin header to one that begins with the origin of the site 
- change the origin header to one that ends with the origin of the site (checks subdomains)
### References

https://portswigger.net/web-security/learning-paths/cors

Arbitrary value allowed in Origin JS exploit code  - [Web-Security-Academy-Series/cors/lab-01/cors-lab-01.html at main · rkhal101/Web-Security-Academy-Series · GitHub](https://github.com/rkhal101/Web-Security-Academy-Series/blob/main/cors/lab-01/cors-lab-01.html)

Null Origin JS exploit code- https://github.com/rkhal101/Web-Security-Academy-Series/blob/main/cors/lab-02/cors-lab-02.html

XSS on subdomain JS exploit - https://github.com/rkhal101/Web-Security-Academy-Series/blob/main/cors/lab-03/cors-lab-03.html - what the script is doing is it's exploiting an xss vulnerability in the product id parameter in the stock subdomain in order to invoke a request from this subdomain that extracts the api key of the user