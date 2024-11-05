
2024-09-12 11:15

Source: #android 

Continuation of [[Advanced Interception Tricks]]

When an application wants to do an HDP request or connect with any other protocol for that matter, it first has to resolve the domain to an IP address.

So what happens if we spoof the DNS response and retum our own ip? Well, this is a great way to intercept any kind of traffic.

In order to make this work, we first have to create a setup that allows us to manipulate the DNS response.

There are lots of ways how this could be done, but I will be using DNS Mask, which is a simple DNS server.

I'm going to run this in a Docker container because then it can be set up on Linux, Mac, or Windows as long as Dock has installed.

So first, let's create the DNS mask config, where we can override the IP for some domains. For example, HX three 10

or the API mox URL used by the okay HDP proxy bypass test. Then we can execute docker, run mount the config into the container,

and bind DNS mask to port five three on our host and expose it to the outside. Now we have a local DS server running, which we can query

with, for example, Dick. So this is the real lp, but if we use our local DS server, it'll return the configured ip.

Next, we have to configure this DS server on the phone,
Next, we have to configure this DS server on the phone, and there are two options, which both don't really work.

We could set up a private D Ns, but that only works with host names and not ips. Or we can change the wifi connection from DHCP to static.

Make sure we enter exactly the same values as we got from the DHCP and then specify our own DNS.

The problem is though, that applications such as Google Chrome might use DNS over HT PS and ignore our DNS server.

So when we try to resolve the IP for HX three IO, it still shows the real one, and that's where Android VPN tricks come into play.

Again, using the Rethink DNS app, we can create a VPN connection where we can control the DNS server. There are many other apps that could do that as well,

but this one as open source. So in the DNS setting, we can now select to proxy DNS, and we want to proxy it to our DNS mask instance.

After that, we can launch the VPN, which doesn't really do much except it forces the apps to use our DNS server.

You know, it would be really bad for privacy if apps could bypass the V-P-N-D-N-S because that could lead to IP leaks.
So this method ensures everything uses our DNS, and so if we now resolve H three lo, our IP is returned.

Now let's come back to our okay, HDP test. Remember, this request ignores any proxy setting, and in fact, we have not configured any kind

of proxy right now anyway. But when we issue the request, the domain IPE mock has to be resolved to an ip, which will now point at our ip.

So the app tries to connect to our computer on HT PS Port 4, 4 3, expecting there to be a web server, which means we could now run a web

server on our computer to respond to this request. Also, if the app used any other kind of protocol, a different port, it would still try to connect

to our computer and we could create a custom server on that Port to respond. This of course, requires some engineering and finessing,

but this is an excellent way to Intercept traffic for even custom protocols. But for HDP and HT PS Interception, luckily we can use Burp Suite again,

because in the Bur Sulte proxy settings, we can configure transparent proxies. So let's create a proxy listener for HTP requests on port 80 on all Interfaces

and enable transparent proxy. And then we do the same for HTT PS on port 4, 4 3. What this basically does is that Burp will act as a web server, but forward the request to the real destination. Here's how it works. When the app wants

to send an HDP request, it first has to resolve the domain to an ip, which now points at our computer.

It then establishes a TCP connection on port 4, 4 3. In the case of H-G-D-P-S, with that computer where Burb is listening, the app thinks it is now connected

to the real web server. So it will send the HDP request. Bur receives this HDP request, and from the host header, it knows

where the request was intended to go. So it will now send this request to the real server, gets a response back,

and sends that response back to the app. And that's why it's called Transparent Proxying, because from the point of view of the app.

there was no proxy Involved. It simply sent an HGP request to what it thinks is the real server. The server just happened to be birth suite intercepting

and forwarding the request.

### References
[DNS Spoofing and Transparent Proxy (hextree.io)](https://app.hextree.io/courses/network-interception/advanced-interception-tricks/dns-spoofing-and-transparent-proxy)