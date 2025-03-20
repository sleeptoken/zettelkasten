
2025-03-19 22:57

Source: #htb #Boot2root 

rustscan shows port 80 and 22

add to `etc/hosts` 
```sh
echo "10.10.11.55 titanic.htb" | sudo tee -a /etc/hosts
```
### Enumeration

The website only has one button that is interactable i.e. validate trip. When we validate a trip, we download the ticket

The request when captured on burp looks like 
```
GET /download?ticket=random-int.json HTTP/1.1
```
since the ticket reading functionality is not implemented securely, we can replace the name of the ticket file with the one we want to read. For example `/etc/passwd`

- we see a user named developer
- we can directly read the `user.txt` flag by replacing ticket parameters value to `/home/developer/user.txt`

since ssh key files aren't readable we read the `etc/hosts` and see a subdomain  - `dev.titanic.htb` - 
after adding this to our `etc/hosts` file and visiting the website we see `gitea`


### References
https://app.hackthebox.com/machines/648
https://www.pentestnotes.ru/en/writeups/hackthebox/titanic-htb-writeup/