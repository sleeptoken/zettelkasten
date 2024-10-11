
2024-10-11 11:21

Source: #web 

Tags: 

proxy that sits betwn u and the server acts as middleman

client        ->        proxy          ->         sever
PC                 localhost:8080       google.com

- PCs browser need to be configured like in [[burpsuite]]
- if we want to apply this to apache we can use `mod_proxy`
- if we want to apply to use load balancer we can use `mod_proxy_balancer`


### References
