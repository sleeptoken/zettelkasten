
2024-12-25 10:28

Tags: [[linux]] [[Networking]]

The command for pinging IPv6 address is `ping6`.
Let's ping `localhost` using its IPv6 address:

```sh
$ ping6 ::1
```
In Windows just specify the `-6` flag of the `ping` command. Eg: `ping -6 ::1`
### TROUBLESHOOTING

Now let's try pinging an external IPv6 address.
```sh
$ ping6 2a03:2880:f12f:83:face:b00c::25de
ping6: UDP connect: No route to host
```

"ping6: UDP connect: No route to host?"

We will need to specify the network interface. On Macs, it will either be `en1` or `en0`. On Linux, it is very likely `eth0`.

```sh
$ ping6 -I en1 2a03:2880:f12f:83:face:b00c::25de
```

In case, you get "ping6: sendmsg: No route to host", something in your system or router needs to be configured for IPv6. Once configured, you will be able to ping IPv6 addresses using `ping6`.
### References
[How to ping IPv6 addresses](https://web.archive.org/web/20231011052555/https://www.hacksparrow.com/networking/ping-ipv6-address.html)