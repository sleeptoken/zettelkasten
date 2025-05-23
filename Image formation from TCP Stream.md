
2025-05-23 12:22

Source: #tryhackme #CTF 

Tags: [[PCAP analysis]]

# TL;DR

we are given a PCAP file which captures a conversation where a bunch of images are parsed, follow the TCP stream, save the HEX stream, run a py script to find where the headers of a new image starts and separate out the files 
# Identify 

Pick any one of the requests, right click on TCP segment data and copy as a HEX Stream
go to cyber chef and paste the copied values 

Now you got to remove the part of the header - Boundary String , we use `drop bytes` for that 

identify the number of bytes the header is taking up from the start, here it is talking up 158 bytes (written where input and output is divided in the RHS of the page, shows up after selecting the part you want to delete)

In the Recipe section 
- Drop bytes - length - 158 
- From Hex 
- Render Image 
# Binwalk 

```
binwalk --dd='.*' -e security-footage-1648933966395.pcap
```
this dumps all the images in a new folder, but this also produces some corrupt images 

# Py Script - Traditional 

Click on anyone of the request then follow


### References
https://www.youtube.com/watch?v=t3uQYJiBqzo&t=192s