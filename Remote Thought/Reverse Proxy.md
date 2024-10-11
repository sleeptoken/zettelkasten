
2024-10-11 11:28

Source: #web 

Tags: 

  .- - - - - - - - - - - - - - - - - - - - - -.
  |                                                     |
client                proxy                   sever
PC                localhost:8080         google.com

no config needed in PC unlike in [[Forward Proxy]]

reverse proxy is transparent 
here we will see the response coming from google.com not from localhost:8080
the reverse proxy is used in a namespace i.e. we need to specify that what type of content if requested goes through the reverse proxy that is why no special config of client is necessary 

basically reverse proxy is used to hide content  

when we use apache server with reverse proxy mode we have 3 headers set

> X-Forwarded-For      > The IP address of the client.  
> X-Forwarded-Host    > The original host requested by the client in the Host HTTP request header.  
> X-Forwarded-Server > The hostname of the proxy server.(reverse proxy)

### Exploitation 


### References
