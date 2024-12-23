
2024-12-23 16:25

Source: #AOC24 

Tags: [[Wi-Fi Hacking]]

access points (often the routers) that are broadcasting Wi-Fi signals with a unique **SSID** (network name). You can connect to any of these if you know the correct password, also known as a pre-shared key (**PSK**)
### Attacks on Wi-Fi

#### Evil twin attack

- In this attack, the attacker creates a fake access point that has a similar name to one of your trusted Wi-Fi access points.
- The attack starts with the attacker sending de-authentication packets to all the users connected to their legitimate Wi-Fi access points. The users would face repeated disconnections from the network after this. With frustration, the users are likely to open the Wi-Fi access points list for troubleshooting and will find the attacker's Wi-Fi with almost similar name and with greater signal strength. They would go to connect it, and once connected, the attacker could see all their traffic to or from the Internet.
#### Rogue access point

- objective is similar to that of the evil twin attack. 
- In this attack, the attacker sets up an open Wi-Fi access point near or inside the organisation's physical premises to make it available to users with good signal strength. 
#### WPS attack

Wi-Fi Protected Setup (WPS) was created to allow users to connect to their Wi-Fi using an 8-digit PIN without remembering complex passwords. However, this 8-digit PIN is vulnerable in some networks due to its insecure configuration. The attack is made by initiating a WPS handshake with the router and capturing the router's response, which contains some data related to the PIN and is vulnerable to brute-force attacks. Some of the captured data is brute-forced, and the PIN is successfully extracted along with the Pre-Shared Key (PSK).
#### WPA/WPA2 cracking

Wi-Fi Protected Access (WPA) was created to secure wireless communication. It uses a strong encryption algorithm. However, the security of this protocol is heavily influenced by the length and complexity of the Pre-Shared Key (PSK). While cracking WPA, attackers start by sending de-authentication packets to a legitimate user of the Wi-Fi network. Once the user disconnects, they try to reconnect to the network, and a 4-way handshake with the router takes place during this time. Meanwhile, the attacker turns its adaptor into monitor mode and captures the handshake. After the handshake is captured, the attacker can crack the password by using brute-force or dictionary attacks on the captured handshake file.




### References
Day 11