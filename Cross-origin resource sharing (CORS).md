
2024-07-25 22:48

Source: #web #portswigger 

Tags: 
####  The response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS. 

Cross-origin resource sharing (CORS) is a browser mechanism which enables controlled access to resources located outside of a given domain. It extends and adds flexibility to the same-origin policy (SOP). 
## Same-origin policy

The same-origin policy is a restrictive cross-origin specification that limits the ability for a website to interact with resources outside of the source domain. It generally allows a domain to issue requests to other domains, but not to access the responses. 
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

Even "correctly" configured CORS establishes a trust relationship between two origins. If a website trusts an origin that is vulnerable to cross-site scripting (XSS), then an attacker could exploit the XSS to inject some JavaScript that uses CORS to retrieve sensitive information from the site that trusts the vulnerable application.









## Testing for CORS Miscofig
- change the origin of an arbitrary value/ True
- change the origin to a null value

### References

https://portswigger.net/web-security/learning-paths/cors

Arbitrary value allowed in Origin JS exploit code  - [Web-Security-Academy-Series/cors/lab-01/cors-lab-01.html at main · rkhal101/Web-Security-Academy-Series · GitHub](https://github.com/rkhal101/Web-Security-Academy-Series/blob/main/cors/lab-01/cors-lab-01.html)

Null Origin JS exploit code- https://github.com/rkhal101/Web-Security-Academy-Series/blob/main/cors/lab-02/cors-lab-02.html