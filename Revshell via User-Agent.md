
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
### Finding LFI 

dirsearch finds a `robots.txt`, the `robots.txt` tells us that `test.php` 

### References
[TryHackMe | Cyber Security Training](https://tryhackme.com/room/archangel)

walkthrough

[Writeups/TryHackMe/Easy/archangel/archangel.md at master · 0xNirvana/Writeups · GitHub](https://github.com/0xNirvana/Writeups/blob/master/TryHackMe/Easy/archangel/archangel.md)