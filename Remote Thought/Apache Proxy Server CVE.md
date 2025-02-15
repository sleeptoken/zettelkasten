
2024-10-09 21:06

Source: #web #htb #CTF 

basically we want to access the content behind [[Reverse Proxy]] 
### Reconnaissance

Upon submitting the URL, I encountered a specific requirement stating that the page will exclusively open under the host `“dev.apacheblaze.local.”`
To address this, I initiated the use of Burp Suite to modify the request header and gain access to the desired response.
### Foothold

Our initial strategy involves requesting the API while strategically setting the `X-Forwarded-Host` header to `dev.apacheblaze.local`. it’s not working But why ? Let’s Examine the website locally - go to the config file 
The configuration entails Apache’s administration to intercept requests arriving at port `1337`, where it selectively routes certain requests to an alternate server operating on port `8080`, while managing others internally. Within this setup, the virtual host established on port `8080` acts as a proxy, seamlessly forwarding incoming requests to a designated backend server cluster named `mycluster`, thereby ensuring balanced distribution of workload across the specified backend servers.

> When acting in a reverse-proxy mode (using the ProxyPass directive, for example),  
> mod_proxy_http adds several request headers in order to pass information to the origin server. These headers are:  
>   
> X-Forwarded-For      > The IP address of the client.  
> X-Forwarded-Host    > The original host requested by the client in the Host HTTP request header.  
> X-Forwarded-Server > The hostname of the proxy server.

because we need all 3 headers and we only have one of them we will need to find an exploit which is the CVE 

Upon thorough examination, it becomes apparent that there exist two virtual hosts operating at ports `1337` and `8080`, serving as reverse proxies and load balancers. These hosts intercept incoming requests and augment them with pertinent information contained within the `X-Forwarded-Host` request header, including host and port details.

However, a vulnerability known as [[HTTP Request Smuggling]] Attack (CVE-2023–25690) has been identified, posing a potential threat to the system’s security. A detailed GitHub repository elucidates the exploitation of CVE-2023–25690, outlining the method to leverage the vulnerability. This exploit facilitates the concealment of a secondary request within the initial one, utilizing the `\r\n\r` splitting technique. Consequently, it becomes feasible to dispatch the request directly from the reverse proxy, bypassing any additional appended data to the X-Forwarded-Host header.

```
URL encoded values  
\r\n     ->  %0d%0a  
\r\n\r\n   ->  %0d%0a%0d%0a
```

**Before Encoding request is :**

```
GET /api/games/click_topia HTTP/1.1\r\nHost: dev.apacheblaze.local\r\n\r\nGET /HTTP/1.1
```

**After Encoding**

```
GET /api/games/click_topia%20HTTP/1.1%0d%0aHost:%20dev.apacheblaze.local%0d%0a%0d%0aGET%20/ HTTP/1.1
```


### References
https://app.hackthebox.com/challenges/ApacheBlaze/activity

Walkthrough
https://motasem-notes.net/apache-proxy-servers-explained-http-request-smuggling-hackthebox-apacheblaze/
https://medium.com/@syed_dawood/fuel-your-innovation-ignite-possibilities-with-apacheblaze-579778982b4d
https://github.com/dhmosfunk/CVE-2023-25690-POC/tree/main#internal-http-request-smuggling-via-header-injection
[HTB ApacheBlaze.mp4 - Google Drive](https://drive.google.com/file/d/1hKrlqEkamuUjj0UQHGRJTUlZbFk1hRlz/view?t=25)