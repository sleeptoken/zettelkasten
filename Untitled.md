
2024-08-09 15:51

Source: #htb #CTF 

Tags: 

### Reconnaissance 

1. we got a machine with port 22 - ssh and 80 - http open
2. the website at the ip has a `upload` page - we find this by running gobuster 
3. the upload page for a book that allows us to upload a file of our choice (intended to be an image) and then preview that file. the page also allows us to upload a cover url for our book.
### Enumeration 

1. on uploading a php revshell, click on the image and open in new tab, the file name renamed and the file extension is removed, further the file downloads directly so we can't execute a shell directly.
2. Next, let’s upload a valid image file and download the previewed file. We can see if the file using any software like `ImageMagick` by using `exiftool`.
### SSRF VULNERABILITY

1. The interesting part is the url preview, this feature might be vulnerable to SSRF.
2. Insert local address(127.0.0.1) in the url field. intercept the request on [[burpsuite]] - The response is showing a image directory location.
3. This machine might have another port running locally, we can bruteforce the port by using burp intruder, and add a port number from 1–65535.
4. we find that port 5000 has a different result 




















### References
https://app.hackthebox.com/machines/Editorial

walkthrough 
https://www.youtube.com/watch?v=jUJNxdOHHzQ

[Hack The Box | Season 5-Editorial Writeup | by Luc1f3r | Jun, 2024 | Medium](https://medium.com/@nkrohitkumar2002/hack-the-box-season-5-editorial-writeup-552635f389d6)
[Editorial | editorial writeup htb | hackthebox | htb editorial writeup | editorial htb | Medium](https://medium.com/@kryntol6/editorial-htb-writeup-9610307c54ef)