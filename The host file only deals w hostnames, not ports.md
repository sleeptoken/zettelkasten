
2025-03-26 15:54

Tags: [[linux]]

In Linux, `/etc/hosts` is a file used by the operating system to translate hostnames to IP-addresses. we can map arbitrary hostnames to arbitrary IP-addresses, which then we can use for testing websites locally.

## The _hosts_ File

First, let’s take a look at an example. Assuming we need to map _baeldung.com_ to _127.0.0.1_, we add the following line to _/etc/hosts_:

```bash
127.0.0.1    baeldung.com
```

Now, every time we go visit the site, our OS tells the browser to visit our localhost.
In order to map a port to this IP address we use [[Reverse Proxy]]

## Using a Reverse Proxy

To make it work, we can use a reverse proxy. A reverse proxy is typically a web server, like Nginx, that takes client requests and directs them to the appropriate backend server. These backend servers can run on a different host and, more interesting to us, a different port.

Let’s take a look at how to configure this with Nginx. We can easily install _nginx_ from our package manager like [_yum_ or _apt-get_](https://www.baeldung.com/linux/yum-and-apt). Its default installation folder is _/etc/nginx_.

To configure a reverse proxy for _baeldung.com_, we add the following in a file called _/etc/nginx/conf.d/baeldung.conf_:

```markdown
server {
    listen 80;

    server_name baeldung.com;

    location / {
        proxy_pass http://127.0.0.1:8080/;
    }
}
```

When we use this config together with
`127.0.0.1 baeldung.com`

in _/etc/hosts_, _nginx_ will receive our requests for _baeldung.com_ and direct those to the webserver running on _127.0.0.1:8080_.
### References
https://www.baeldung.com/linux/mapping-hostnames-ports