
2024-06-17 17:15

Source: #tryhackme 

```
find / -type f -name user.txt 2> /dev/null
```
- **_-type f_** — you are telling find to look exclusively for files
- **_-name user.txt_** — instructing the find command to search for a file with the name “user.txt”
- **_2> /dev/null_** — so error messages do not show up as part of the search result

```
find / -user root -perm /4000
```
Search for files with SUID permission. It is looking for a file with SUID permission that can be run as root.


### References
[RootMe — TryHackMe CTF Walkthrough | by WiktorDerda | Medium](https://medium.com/@wiktorderda/rootme-tryhackme-ctf-walkthrough-656232b4c901)