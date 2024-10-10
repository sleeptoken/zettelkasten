
2024-09-10 13:54

Source: #tryhackme #web 

HTTP Request Smuggling is a vulnerability that arises when there are mismatches in different web infrastructure components. This includes proxies, load balancers, and servers that interpret the boundaries of HTTP requests.

Request splitting or HTTP desync attacks are possible because of the nature of keep-alive connections and HTTP pipelining, which allow multiple requests to be sent over the same TCP connection. Without these mechanisms, request smuggling wouldn't be feasible. When calculating the sizes for Content-Length (CL) and Transfer-Encoding (TE), it's crucial to consider the presence of carriage return `\r` and newline `\n` characters. These characters are not only part of the HTTP protocol's formatting but also impact the calculation of content sizes.

> Note that testing for request smuggling can potentially break a website in many ways (cache poisoning, other user requests may start failing, or even the back-end pipeline might get fully desynced), so extreme care should be taken when testing this on a production website.

### Modern Infrastructure

[[Load balancing]] for web servers is often done by reverse proxies\

**[[Reverse Proxies]]**: A reverse proxy sits before one or more web servers and forwards client requests to the appropriate web server. While they can also perform load balancing, their primary purpose is to provide a single access point and control for back-end servers. Examples include NGINX, Apache with mod_proxy, and Varnish.

![[adaa5ac3ca48557bdf12e4b7758fd429.svg]]
#### Role of Caching Mechanisms

Caching is a technique used to store and reuse previously fetched data or computed results to speed up subsequent requests and computations. In the context of web infrastructure:

1. **Content Caching**: By storing web content that doesn't change frequently (like images, CSS, and JS files), caching mechanisms can reduce the load on web servers and speed up content delivery to users.
2. **Database Query Caching**: Databases can cache the results of frequent queries
3. **Full-page Caching**: Entire web pages can be cached, so they don't need to be regenerated for each user. This is especially useful for websites with high traffic.
4. **Edge Caching/CDNs**: Content Delivery Networks (CDNs) cache content closer to the users (at the "edge" of the network), reducing latency and speeding up access for users around the world.
5. **API Caching**: Caching the responses can significantly reduce back-end processing for APIs that serve similar requests repeatedly.

### Headers
#### Content-Length Header

The Content-Length header indicates the request or response body size in bytes. It informs the receiving server how much data to expect, ensuring the entire content is received.
#### Transfer-Encoding Header

The Transfer-Encoding header is used to specify the form of encoding applied to the message body of an HTTP request or response. A commonly used value for this header is "chunked", indicating that the message body is divided into a series of chunks, each preceded by its size in hexadecimal format. Other possible values for the Transfer-Encoding header include "compress", "deflate", and "gzip", each indicating a different type of encoding.

```shell-session
POST /submit HTTP/1.1
Host: good.com
Content-Type: application/x-www-form-urlencoded 
Transfer-Encoding: chunked

b
q=smuggledData 
0
```
In this example, "b" (in hexadecimal, equivalent to 11 in decimal) specifies the size of the following chunk. The chunk `q=smuggledData` is the actual data, followed by a new line. The request is terminated with a "0" line, indicating the end of the message body. Each chunk size is given in hexadecimal format, and the end of the chunked body is signified by a chunk of size 0.
### HTTP Request Smuggling Origin

HTTP Request Smuggling primarily occurs due to discrepancies in how different servers (like a front-end server and a back-end server) interpret HTTP request boundaries. For example:

1. If both Content-Length and Transfer-Encoding headers are present, ambiguities can arise.
2. Some components prioritize Content-Length, while others prioritize Transfer-Encoding.
3. This discrepancy can lead to one component believing the request has ended while another thinks it's still ongoing, leading to smuggling.

 Suppose a front-end server uses the Content-Length header to determine the end of a request while a back-end server uses the Transfer-Encoding header. An attacker can craft a request that appears to have one boundary to the front-end server but a different boundary to the back-end server. This can lead to one request being "smuggled" inside another, causing unexpected behavior and potential vulnerabilities.
### Request Smuggling CL.TE

**CL.TE** stands for **Content-Length/Transfer-Encoding**. 
- The proxy uses the Content-Length header to determine the end of a request.
- The back-end server uses the Transfer-Encoding header.

 Imagine sending a request with both `Content-Length` and `Transfer-Encoding` headers. The front-end server might use the Content-Length header and think the request ends at a certain point due to the provided number of bytes. In contrast, the back-end server, relying on the Transfer-Encoding header, might interpret the request differently, leading to unexpected behavior.
#### Exploiting CL.TE for Request Smuggling

craft a request that includes both headers, ensuring that the front-end and back-end servers interpret the request boundaries differently. For example, an attacker sends a request like:
```shell-session
POST /search HTTP/1.1
Host: example.com
Content-Length: 130
Transfer-Encoding: chunked

0

POST /update HTTP/1.1
Host: example.com
Content-Length: 13
Content-Type: application/x-www-form-urlencoded

isadmin=true
```
Here, the front-end server sees the `Content-Length` of 130 bytes and believes the request ends after  `isadmin=true`. However, the back-end server sees the `Transfer-Encoding: chunked` and interprets the `0` as the end of a chunk, making the second request the start of a new chunk. This can lead to the back-end server treating the `POST /update HTTP/1.1` as a separate, new request, potentially giving the attacker unauthorized access.
#### Incorrect Content-Length

When creating a request smuggling payload, if the `Content-Length` is not equal to the actual length of the content, several problems might arise. First, the server might process only the portion of the request body that matches the `Content-Length`. For example - when the Content-Length is set to less than the actual size of the body `username=test&query=test` (which is 24 bytes), the back-end server will only read part of the request body based on the specified Content-Length. For instance, setting Content-Length to 10 bytes means the server will only consider the first 10 bytes of the body, leading to incomplete data being processed.
### Request Smuggling TE.CL

The TE.CL technique arises when 
- the proxy prioritizes the `Transfer-Encoding` header 
- the back-end server prioritizes the `Content-Length` header.

**TE.CL** stands for **Transfer-Encoding/Content-Length**, the discrepancy in header interpretation is flipped because the front-end server uses the Transfer-Encoding header to determine the end of a request, and the back-end server uses the Content-Length header.
#### Exploiting TE.CL for Request Smuggling

For example, an attacker sends a request like:
```shell-session
POST / HTTP/1.1
Host: example.com
Content-Length: 4
Transfer-Encoding: chunked

78
POST /update HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

isadmin=true
0
```
- In the above payload, the front-end server sees the `Transfer-Encoding: chunked` header and processes the request as chunked. The `78` (hexadecimal for 120) indicates that the next 120 bytes are part of the current request's body. The front-end server considers everything up to the `0` (indicating the end of the chunked message) as part of the body of the first request.
- The back-end server, however, uses the Content-Length header, which is set to 4. It processes only the first 4 bytes of the request, not including the entire smuggled request `POST /update`. The remaining part of the request, starting from **POST /update**, is then interpreted by the back-end server as a separate, new request.
### Transfer Encoding Obfuscation (TE.TE)

**Transfer Encoding Obfuscation,** also known as **TE.TE** stands for **Transfer-Encoding/Transfer-Encoding**. Unlike the CL.TE or TE.CL methods, the TE.TE technique arises when both the front-end and the back-end servers use the Transfer-Encoding header. 
#### Exploiting TE.TE for Request Smuggling

To exploit the TE.TE technique, an attacker may craft a request that includes Transfer-Encoding headers that use different encodings. 

```shell-session
POST / HTTP/1.1
Host: example.com
Content-length: 4
Transfer-Encoding: chunked
Transfer-Encoding: chunked1

4e
POST /update HTTP/1.1
Host: example.com
Content-length: 15

isadmin=true
0
```

In the above payload, the front-end server encounters two `Transfer-Encoding` headers. The first one is a standard chunked encoding, but the second one, `chunked1`, is non-standard. Depending on its configuration, the front-end server might process the request based on the first `Transfer-Encoding: chunked` header and ignore the malformed `chunked1`, interpreting the entire request up to the `0` as a single chunked message.

The back-end server, however, might handle the malformed `Transfer-Encoding: chunked1` differently. It could either reject the malformed part and process the request similarly to the front-end server or interpret the request differently due to the presence of the non-standard header. If it processes only the first 4 bytes as indicated by the `Content-length: 4`, the remaining part of the request starting from `POST /update` is then treated as a separate, new request.
#### Lab

The vulnerable environment has ATS (Apache Traffic Server) as the front-end proxy, Nginx as the web server back-end, and PHP processing the dynamic content. Due to differences in how ATS and Nginx prioritizes `Content-Length` and `Transfer-Encoding` headers,

better read this from tryhackme directly linked in references
### References
[TryHackMe | HTTP Request Smuggling](https://tryhackme.com/r/room/httprequestsmuggling)

[mod_proxy vulnerable configuration on Apache HTTP Server versions 2.4.0 - 2.4.55 leads to HTTP Request Smuggling vulnerability.](https://github.com/dhmosfunk/CVE-2023-25690-POC)