
2024-10-11 11:21

Source: #web 

proxy that sits between u and the server acts as middleman

client        ->        proxy          ->         sever
PC                  localhost:8080          google.com

- PCs browser need to be configured like in [[burpsuite]]
- if we want to apply this to Apache we can use `mod_proxy`
- if we want to apply to use load balancer we can use `mod_proxy_balancer`
- here we will see the response coming from localhost:8080 as google.com forwards the message to localhost:8080 and then localhost:8080 forwards it to us
### Exploitation 

if we find multiple virtual hosts then chances are the Apache server is running proxy servers weather it is Forward proxy or [[Reverse Proxy]] we have to inspect it
### References
[[Apache Proxy Server CVE]]