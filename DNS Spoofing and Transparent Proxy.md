
2024-09-12 11:15

Source: #android 

Continuation of [[Advanced Interception Tricks]]

> When an application wants to do an HTTP request or connect with any other protocol for that matter, **it first has to resolve the domain to an IP address.**
> 
> So what happens if we spoof the DNS response and return our own IP? Well, this is a **great way to intercept any kind of traffic.**

In order to make this work, we first have to create a setup that allows us to manipulate the DNS response.

```powershell
docker ps
docker pull andyshinn/dnsmasq
```

There are lots of ways how this could be done, but I will be using DNSmasq, which is a simple DNS server. I'm going to run this in a Docker container because then it can be set up on Linux, Mac, or Windows as long as Docker has installed.
### dnsmasq config

So first, let's create the `dnsmasq.conf`, configure mappings between domain and IP.
```dnsmasq.config
address=/hextree.io/192.168.178.37 
address=/ht-api-mocks-lcfc4kr5oa-uc.a.run.app/192.168.178.37 
log-queries
```

Then we can execute docker, run mount the config into the container,
```powershell
docker run --name my-dnsmasq --rm -it -p 0.0.0.0:53:53/udp \ -v D:\tmp\proxy\dnsmasq.conf:/etc/dnsmasq.conf andyshinn/dnsmasq.conf andyshinn/dnsmasq
```

and bind DNS mask to port 53 on our host and expose it to the outside. Now we have a local DNS server running, which we can query with, for example on linux terminal run 
- `dig hextree.io` -> So this is the real IP, 
but if we use our local DNS server,
- `dig @127.0.0.1 hextree.io`   -> It'll return the configured IP.

## Configure this DNS server on the phone

and there are two options, which both don't really work.
### Private DNS

We could set up a private DNS (search in settings) but that only works with host names and not IPs. 

### Change Wi-Fi connection

Or we can change the Wi-Fi connection from DHCP to static. Make sure we enter exactly the same values as we got from the DHCP and then specify our own DNS.
- IP address - 10.0.2.16
- Gateway  - 10.0.2.2
- Network prefix length - 24
- DNS 1 - 192.168.178.37

The problem is though, that applications such as Google Chrome might use DNS over HTTPS and ignore our DNS server. And that's where Android VPN tricks come into play.
### Using Rethink VPN

Again, using the Rethink DNS app, we can create a VPN connection where we can control the DNS server. So in the DNS setting ->Other DNS -> DNS 53 (DNS proxy), and we want to proxy it to our DNS mask instance. (IP address - 192.168.178.37, Port number - 53)

After that, we can launch the VPN, which doesn't really do much except it forces the apps to use our DNS server.

You know, it would be really bad for privacy if apps could bypass the VPN DNS because that could lead to IP leaks. So this method ensures everything uses our DNS. 

## OkHttp - NO-PROXY

Remember, this request ignores any proxy setting, and in fact, we have not configured any kind of proxy right now anyway (we have not configured a proxy, we just use our own DNS server to return our IP).
- So the app tries to connect to our computer on HTTPS Port 443, expecting there to be a web server, which means we could now run a web server on our computer to respond to this request. Also, if the app used any other kind of protocol, a different port, it would still try to connect to our computer and we could create a custom server on that Port to respond.

> [!NOTE]
> **DNS spoofing** 
> redirecting the traffic via DNS also works great with other protocols. as long as the app uses domains to establish communication with a server, we can make it connect to a different computer. 

but this is an excellent way to Intercept traffic for even custom protocols. 
## Transparent Proxy Config  

For HTTP and HTTPS Interception, luckily we can use Burp Suite again, because in the Burp Suite proxy settings, we can configure transparent proxies. So let's create a proxy listener for HTTP requests on port 80 on all Interfaces and enable transparent proxy. 

- Bind to port: 80
- Bind to address: All interfaces 

- go to `Request handling` tab and check `support invisible proxying`

And then we do the same for HTTPS on port 443. What this basically does is that Burp will act as a web server, but forward the request to the real destination.
## Transparent Proxy Working

When the app wants to send an HTTP request, it first has to resolve the domain to an IP, which now points at our computer.

It then establishes a TCP connection on port 443. In the case of HTTPS, with that computer where Burp is listening, the app thinks it is now connected to the real web server. So it will send the HTTP request. 

Burp receives this HTTP request, and from the host header, it knows where the request was intended to go. So it will now send this request to the real server, gets a response back, and sends that response back to the app. 

> And that's why it's called Transparent Proxying, because from the point of view of the app. there was no proxy Involved. It simply sent an HTTP request to what it thinks is the real server. The server just happened to be burp suite intercepting and forwarding the request.

### References
[DNS Spoofing and Transparent Proxy (hextree.io)](https://app.hextree.io/courses/network-interception/advanced-interception-tricks/dns-spoofing-and-transparent-proxy)