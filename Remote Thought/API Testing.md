
2024-06-27 20:15

Source: #portswigger #web

Tags:  [[API]]
## Recon
#### Tools: 

- You can use [[burpsuite]] **Burp Scanner** to crawl the application, then manually investigate interesting attack surface.
- Burp Scanner automatically extracts some endpoints during crawls, but for a more heavyweight extraction, use the **JS Link Finder BApp**.
- You can use the built-in **HTTP verbs list** in Burp Intruder to automatically cycle through a range of HTTP methods. 
- You can use the **Content type converter BApp** to automatically convert data submitted within requests between XML and JSON.
- The **Param miner BApp** enables you to automatically guess up to 65,536 param names per request. Param miner automatically guesses names that are relevant to the application, based on information taken from the scope. 
- the **Backslash Powered Scanner BApp** identifies server-side injection vulnerabilities. The scanner classifies inputs as boring, interesting, or vulnerable. You'll need to investigate interesting inputs using the manual techniques
#### API Documentation 

Even if API documentation isn't openly available, you may still be able to access it by browsing applications that use the API. To do this, you can use Burp Scanner to [[crawling a website]] 

While browsing the application, look for patterns that suggest API endpoints in the URL structure, such as `/api/`. Also look out for JavaScript files. These can contain references to API endpoints that you haven't triggered directly via the web browser. 
##### Interacting with API endpoints

- Observe the API's behavior and discover additional attack surface eg. you could investigate how the API responds to changing the HTTP method and media type. 
- **review error messages and other responses closely.** Sometimes these include information that you can use to construct a valid HTTP request. 
- **Identifying supported HTTP methods** - GET,POST,OPTIONS etc.
- **Identifying supported content types** -
	- To change the content type, modify the `Content-Type` header, then reformat the request body accordingly. 
	- Changing the content type may enable you to:
	    - Trigger errors that disclose useful information.
	    - Bypass flawed defenses.
	    - Take advantage of differences in processing logic. For example, an API may be secure when handling JSON data but susceptible to injection attacks when dealing with XML.
##### Mass assignment vulnerabilities

Mass assignment (also known as auto-binding) can inadvertently create hidden parameters. It occurs when software frameworks automatically bind request parameters to fields on an internal object. Mass assignment may therefore result in the application supporting parameters that were never intended to be processed by the developer.

For example, consider a `PATCH /api/users/` request, which enables users to update their username and email, A concurrent `GET /api/users/123` can be created 
#### Sever side parameter pollution 

Server-side parameter pollution occurs when a website embeds user input in a server-side request to an internal API without adequate encoding. Attacker can - 
    - Override existing parameters.
    - Modify the application behavior.
    - Access unauthorized data.
##### Testing for server-side parameter pollution in the query string

To test for [[server-side parameter pollution]] in the query string, place query syntax characters like `#,` `&,` and `=` in your input 

- You can use a URL-encoded `#` character to attempt to truncate the server-side request
	- *It's essential that you URL-encode the `#` character. Otherwise the front-end application will interpret it as a fragment identifier and it won't be passed to the internal API.* 
	- if you see no change in the response after adding `#` then this possibly removes the significance of anything trailing `#` .

- You can use an URL-encoded `&` character to attempt to add a second parameter to the server-side request. 
###### Overriding existing parameters

To confirm whether the application is vulnerable to server-side parameter pollution, you could try to override the original parameter. Do this by injecting a second parameter with the same name. This varies across different web technologies. For example: 
- `PHP` parses the last parameter only. 
- `ASP.NET` combines both parameters.
- `Node.js / express` parses the first parameter only.
If you're able to override the original parameter, you may be able to conduct an exploit.
##### Testing for server-side parameter pollution in REST paths

A RESTful API may place parameter names and values in the URL path, rather than the query string.
To test for this vulnerability, add path traversal sequences to modify parameters and observe how the application responds. 
##### Testing for server-side parameter pollution in structured data formats

- An attacker may be able to manipulate parameters to exploit vulnerabilities in the server's processing of other structured data formats, such as a `JSON` or `XML`.
- To test for this, inject unexpected structured data into user inputs and see how the server responds. 
 Structured format injection can also occur in responses. For example, this can occur if user input is stored securely in a database, then embedded into a JSON response from a back-end API without adequate encoding. 
## HTTP security headers 

Below headers will either strength on weaken the security if they are added 

| Strengthen Security     | Weaken Security                          |
| ----------------------- | ---------------------------------------- |
| X-Frame-Options         | [[Cross-origin resource sharing (CORS)]] |
| Content-Security-Policy |                                          |
| HSTS                    |                                          |
| HTTPOnly Cookie         |                                          |
X-Frame-Options header is only useful when the HTTP response where it is included has something to interact with (e.g. links, buttons). If the HTTP response is a redirect or an API returning JSON data, X-Frame-Options does not provide any security.

CSP can protect websites from XSS attacks or other kind of attacks. But because CSP “just” protects, it doesn’t make it the fix for an issue. If there is an XSS, CSP doesn’t fix it, it could just block exploitation. it’s a defense in depth strategy. So setting `script-src CSP` only matters in the context of an XSS issue existing in the first place. And that means, missing CSP in itself is not really a vulnerability.

The Strict-Transport-Security header is ignored by the browser when your site is accessed using HTTP; this is because an attacker may intercept HTTP connections and inject the header or remove it. "ONLY" When your site is accessed over HTTPS with no certificate errors, the browser knows your site is HTTPS capable and will honor the Strict-Transport-Security header.

Cookie Security Flags (HttpOnly)
And the setting causes cookies to only be transmitted in HTTP request, but cannot be read from javascript. It often is considered a defense against XSS attacks stealing cookies, but it’s rather ineffective. Yes the cookie cannot be stolen, so you as the attacker cannot perform requests as the user, but when you have a XSS, the XSS can just send the requests you want for you. And those requests will be authenticated.
#### Internship Notes - 

- X-frame-options : SAMEORIGIN - for clickjacking  
- X-XSS- protection:1;mode=block- secures from pop ups  
- Strict-transport-security- for enforcing http to https- HSTS  
- Content-security-policy:default-src 'none'; script-src 'self'; connect-src 'self'; img-src 'self'; style-src 'self';  
- Access-Control-Allow-Origin - shouldn't be set to * this means that it allows anybody to send request to that api

Try using successful login cookies to relogin with wrong creds  
Line 1 ka get req ko slowly strip down karna by directory and then check kispe kya response aa raha hai
### References

cheat sheet for http headers - [HTTP Headers - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)

test cases - 
https://www.evernote.com/shard/s433/client/snv?isnewsnv=true&noteGuid=588ed6bf-f893-4953-aeb2-d4f2260fd72f&noteKey=6fe301e3f61af998&sn=https%3A%2F%2Fwww.evernote.com%2Fshard%2Fs433%2Fsh%2F588ed6bf-f893-4953-aeb2-d4f2260fd72f%2F6fe301e3f61af998&title=API%2BTest%2BCases%2BCreation%2B%2528API%2BTesting%2B-%2BPart%2B33%2529

you can access a given policy - https://csp-evaluator.withgoogle.com/
https://hstspreload.org/