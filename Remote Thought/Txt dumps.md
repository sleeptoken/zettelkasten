
2025-02-04 11:28

Source: #CTF  

Tags: [[stego]]
### Web request dump

Its displays — GET Requests, Path, HTTP version, Status code (404 & 200) & Apache - HTTPCilent Server.

> HTTP 404 means “Not Found”.
> 
> HTTP 200 means “OK” .

all the 200 status codes were having user and pass in the URL and the user was changing with every request 
Joining all the users from the URL we get the flag
### Repeated words

Replace with the repeated word with empty space you are most likely to get the flag 
### Hexdump from pdf

when running strings on the given pdf we find interesting hexadecimal value, So I pasted it into Cyberchef and decoded it with hex; the output gave out a PNG file, save the output file (top right of the output tab)
and that gives the flag.
### Analyzing multiple JSON files.

directly search for what you want
```sh
cat * | grep "Flag"
```
### Mirror a website 

```sh
wget -m https://planet.com
```

### References
 
TCS Hackquest S8 - https://kishoreramk.medium.com/tcs-hackquest-season-8-round-1-and-round-2writeup-d5a99f61d645
TCS Hackquest S5 - https://0xdb9.in/2021/03/19/tcs-hackquest-round2.html
TCS Hackquest S7 - https://medium.com/@iborrareddy/tcs-hackquest-season-7-round-1-2-walkthrough-194c321d9062