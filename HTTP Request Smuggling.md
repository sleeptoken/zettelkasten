
2024-09-10 13:54

Source: #tryhackme #web 

Tags: 

HTTP Request Smuggling is a vulnerability that arises when there are mismatches in different web infrastructure components. This includes proxies, load balancers, and servers that interpret the boundaries of HTTP requests.

Request splitting or HTTP desync attacks are possible because of the nature of keep-alive connections and HTTP pipelining, which allow multiple requests to be sent over the same TCP connection. Without these mechanisms, request smuggling wouldn't be feasible. When calculating the sizes for Content-Length (CL) and Transfer-Encoding (TE), it's crucial to consider the presence of carriage return `\r` and newline `\n` characters. These characters are not only part of the HTTP protocol's formatting but also impact the calculation of content sizes.

> Note that testing for request smuggling can potentially break a website in many ways (cache poisoning, other user requests may start failing, or even the back-end pipeline might get fully desynced), so extreme care should be taken when testing this on a production website.
### References
[TryHackMe | HTTP Request Smuggling](https://tryhackme.com/r/room/httprequestsmuggling)