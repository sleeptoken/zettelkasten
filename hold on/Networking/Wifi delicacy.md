## wifi weakness
### WEP (IEEE 802.11)

Weakness of wep
- Use of static encryption keys
	- no session keys (encrypts with the same symmetric key for every packet) 
	- weak RC4 stream cipher used,
- lack of packet integrity assurance - bits can be changed 
- the ineffective use of initialisation vectors 
	- IVs are random values that are used with algorithms to ensure patters are not created during the encryption process. If IVs are not used then 2 identical plain text that are encrypted with the same key will create the same cipher text
- IVs are not random enough, only 24bits, leading to reused IVs with the same key
### WPA (IEEE 802.11i)
Draft standard 

Algorithm used here is TKIP (Temporal key integrity protocol)
Here RC4 stream cipher is used with a 128 bit per packet key . Meaning that it dynamically generates a new key for each packet. 
Web key + IV value + MAC address = new encryption key 
IV length is increased over web 
TKIP is backwards compatible with 802.11 standard 
TKIP provides a key mixing function, which allows the RC4 algorithm to provide a higher degree of protection then web, also has a sequence counter and a integrity check mechanism 

TKIP is susceptible to packet injection attacks.
Avoid using TKIP, use only for backwards compatibility if you have an old access point 

### WPA2 (IEEE 802.11i-2004)

Supports CCMP - counter cipher modem block chaining message authentication code protocol (*usually in WPA algorithms this is written as AES because CCMP is an AES based encryption counter mode mechanism CTR*)
- CCMP is a replacement for TKIP encryption protocol 
CCMP uses CCM that combines CTR for data confidentiality and CBC- MAC for authentication and integrity 
AES is slower than TKIPa
#### Access control method 

WPA2 Personal
Is what we use at our homes w a pre shared key 

WPA2 Enterprise 
- Requires a radius server which is an authentication server 
- Uses an Extensible authentication protocol EAP, which means you can use stronger authentication mechanism like dual certificate authentication (both client and server)

- Both WPA and WPA2s handshake are susceptible to dictionary and brute force password guessing of the pre shared key 
	This attack can be demonstrated by `Cowpatty` in kali
- Weakness with the salt
	- WPA and wpa2 use SSID as salt value 
	- Common SSID are more vulnerable (rainbow tables)  [here](https://www.renderlab.net/projects/WPA-tables/) is the link to download pre computed data for common SSID for brute force 
### WPS (WiFi protected setup)

One of the methods is to enter a short pin number Instead of a password to gain access to wifi network 

Many routers have WPS pin enabled by default. The flame allows a remote attacker to recover the WPS pin w a brute force attack, after getting the WPS you can get access to WPA, WPA2, pre shared key. Tool in kali to do this is named `Reaver`.
## Wireless security 
Security config ( in decreasing order ) from best to worse.
1. WP2 - enterprise (WPA-802.1X mode), RADIUS authentication server, with EAP authentication (dual certificate authentication)
2. WPA2-Personal (WPA- PSK pre-shared key) using CCMP/AES encryption

RF isolation 

### who's on ur wifi

- On your routers / access point under the section of wireless nodes
- https://whoisonmywifi.en.softonic.com/?ex=RAMP-3507.2&rex=true
- W glasswire firewall 
- On Android/iOS use fing - network tools

```sh
arp-scan eth0 192.168.1.0/24

arp -a # to see existing arp cache
```
Scan IP range and return Mac address 

```sh
airodump-ng wlan1
```
Will detect access points and also list the connected clients 

