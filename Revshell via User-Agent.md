
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

```sh
curl -s http://mafialive.thm/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/test.php
```

above URL returns a base64 encoded string, decoding the string reveals code of `test.php`









### References
[TryHackMe | Cyber Security Training](https://tryhackme.com/room/archangel)

walkthrough

[Writeups/TryHackMe/Easy/archangel/archangel.md at master · 0xNirvana/Writeups · GitHub](https://github.com/0xNirvana/Writeups/blob/master/TryHackMe/Easy/archangel/archangel.md)
[TryHackMe-Archangel - aldeid](https://www.aldeid.com/wiki/TryHackMe-Archangel)
[Archangel - Write-up - TryHackMe | Rawsec](https://blog.raw.pm/en/TryHackMe-Archangel-write-up/)
[TryHackMe: Archangel Writeup. https://tryhackme.com/room/archangel | by ALIEN0X | Medium](https://medium.com/@infohellohumans/tryhackme-archangel-writeup-2807313edfb7)
[TryHackMe – Archangel walkthrough - narancs's blog](https://narancsblog.com/thm/easy/archangel/)