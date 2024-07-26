
2024-07-25 22:48

Source: #web #portswigger 

Tags: 

Cross-origin resource sharing (CORS) is a browser mechanism which enables controlled access to resources located outside of a given domain. It extends and adds flexibility to the same-origin policy (SOP). 
## Same-origin policy

The same-origin policy is a restrictive cross-origin specification that limits the ability for a website to interact with resources outside of the source domain. It generally allows a domain to issue requests to other domains, but not to access the responses. 
### Relaxation of the same-origin policy

The same-origin policy is very restrictive and consequently various approaches have been devised to circumvent the constraints. Many websites interact with subdomains or third-party sites in a way that requires full cross-origin access. A controlled relaxation of the same-origin policy is possible using cross-origin resource sharing (CORS).

The cross-origin resource sharing protocol uses a suite of HTTP headers that define trusted web origins and associated properties such as whether authenticated access is permitted. 

CORS Header (shown in the response) -
- `Access-Control-Allow-Origin: http://random-website.com`
- `Access-Control-Allow-Credentials: true`
Adding the origin header in the request - `origin : http://random-website.com`
- if the application accepts it that means the application accepts any random origin on the internet to access its data 
- if the application does not accept it or throws an error that means the application only accepts certain domains to access its data and it's usually just the domain of the site or the subdomains of the site

- `Access-Control-Allow-Credentials` header which allows credentials to be passed in the request that could be either cookies or authorization headers or certificates but in this case the application makes use of a session cookie so what's going to get passed in the request is session cookies
- having both headers is very dangerous because what that means is not only can you access public resources from the site but you could also access private resources  


### References

https://portswigger.net/web-security/learning-paths/cors