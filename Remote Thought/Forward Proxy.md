
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

**Attackers use proxy servers** 

- To hide the actual source of a scan and evade certain IDS/firewall restrictions. 
- To hide the source IP address so that they can hack without any legal corollary. 
- To mask the actual source of the attack by employing a fake source address of the proxy. 
- To remotely access intranets and other website resources that are normally off limits. 
- To interrupt all the requests sent by a user and transmit them to a third destination; hence, victims will only be able to identify the proxy server address. 
- To chain multiple proxy servers to avoid detection. 
### References
[[Apache Proxy Server CVE]]