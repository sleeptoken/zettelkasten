
2024-09-10 13:54

Source: #tryhackme #web 

Tags: 

HTTP Request Smuggling is a vulnerability that arises when there are mismatches in different web infrastructure components. This includes proxies, load balancers, and servers that interpret the boundaries of HTTP requests.

Request splitting or HTTP desync attacks are possible because of the nature of keep-alive connections and HTTP pipelining, which allow multiple requests to be sent over the same TCP connection. Without these mechanisms, request smuggling wouldn't be feasible. When calculating the sizes for Content-Length (CL) and Transfer-Encoding (TE), it's crucial to consider the presence of carriage return `\r` and newline `\n` characters. These characters are not only part of the HTTP protocol's formatting but also impact the calculation of content sizes.

> Note that testing for request smuggling can potentially break a website in many ways (cache poisoning, other user requests may start failing, or even the back-end pipeline might get fully desynced), so extreme care should be taken when testing this on a production website.

### Modern Infrastructure

[[Load balancing]] for web servers is often done by reverse proxies\

**[[Reverse Proxies]]**: A reverse proxy sits before one or more web servers and forwards client requests to the appropriate web server. While they can also perform load balancing, their primary purpose is to provide a single access point and control for back-end servers. Examples include NGINX, Apache with mod_proxy, and Varnish.

![[adaa5ac3ca48557bdf12e4b7758fd429.svg]]

Caching is a technique used to store and reuse previously fetched data or computed results to speed up subsequent requests and computations. In the context of web infrastructure:

1. **Content Caching**: By storing web content that doesn't change frequently (like images, CSS, and JS files), caching mechanisms can reduce the load on web servers and speed up content delivery to users.
2. **Database Query Caching**: Databases can cache the results of frequent queries, reducing the time and resources needed to fetch the same data repeatedly.
3. **Full-page Caching**: Entire web pages can be cached, so they don't need to be regenerated for each user. This is especially useful for websites with high traffic.
4. **Edge Caching/CDNs**: Content Delivery Networks (CDNs) cache content closer to the users (at the "edge" of the network), reducing latency and speeding up access for users around the world.
5. **API Caching**: Caching the responses can significantly reduce back-end processing for APIs that serve similar requests repeatedly.



### References
[TryHackMe | HTTP Request Smuggling](https://tryhackme.com/r/room/httprequestsmuggling)