
2024-08-09 15:51

Source: #htb #CTF 

Tags: 

### recon

1. we got a machine with port 22 - ssh and 80 - http open
2. the website at the ip has a `upload` page - we find this by running gobuster 
3. the upload page for a book that allows us to upload a file of our choice (intended to be an image) and then preview that file. the page also allows us to upload a cover url for our book.
### enum

1. on uploading a php revshell, click on the image and open in new tab, the file name renamed and the file extension is removed, further the file downloads directly so we can't execute a shell directly.
2. Next, let’s upload a valid image file and download the previewed file. We can see if the file using any software like `ImageMagick` by using `exiftool`.

























### References
https://app.hackthebox.com/machines/Editorial

walkthrough 
https://www.youtube.com/watch?v=jUJNxdOHHzQ