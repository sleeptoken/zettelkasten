
2025-01-10 18:34

Source: #CEHv13 

| Passive                                                                            | Active                                                                                               |
| ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Involves gathering information about the target without direct interaction         | Involves gathering information about the target with direct interaction                              |
| Involes - OSINT, Sharing intelligence and partner organizations or industry groups | Involes - DNS Interrogation, social engineering, network/port scanning, user and service enumeration |
### Footprinting Techniques

![[Screenshot 2025-01-10 190743.png]]

### Google Dorking 

[Google Hacking Database](https://www.exploit-db.com/google-hacking-database)
once a vulnerable site is identified, attackers attempt to launch various possible attacks, such as buffer overflow and SQL injection, which compromise information security
#### Dorking with AI

Example prompt for ShellGPT
```
use filetype search operator to obtain pdf files on the target website example.org and store the result in the recon.txt file
```

The following shell command is designed to conduct advanced Google hacking using the "filetype" operator to specifically target PDF files within the eccouncil.org domain. The command then saves the obtained results to a file named "recon1.txt":
```sh
lynx --dump "http://www.google.com/search?q=site:eccouncil.org+filetype:pdf" | grep "http" | cut -d "=" -f2 | grep -o "http[^&]*" > reconl.txt 
```
lynx is a text based web browser 
#### Advanced Google Image Search 

[Advanced image search](https://www.google.com/advanced_image_search)
[Advanced search](https://www.google.com/advanced_search)
[reverse image search](https://www.google.com/imghp)

### VPN footprinting through Google Hacking Database 

Google dorks can be used to footprint virtual private networks. 

| Google Dork                                                                                                                                                                                                                                                                            | Description                                                    |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| inurl:"/sslvpn_logon.shtml" intitle:"User Authentication" "WatchGuard Technologies"                                                                                                                                                                                                    | Finds pages containing login portals                           |
| inurl:/sslvpn/Login/Login                                                                                                                                                                                                                                                              | Finds VPN login portals                                        |
| site:vpn.*.*/ intitle:"login"                                                                                                                                                                                                                                                          | Finds VPN login portals                                        |
| inurl:weblogin intitle:("USG20-VPN" \| "USG20W- VPN" \| USG40\|USG40W\|USG60\|USG60W \| USG110\|USG210\|U SG310\|USG1100\|USG1900 \| USG2200\|"ZyWALL110" \| "ZyWALL 310" \| "ZyWALL1100" \| ATP100 \| ATP100W\|ATP200\|ATP500\|AT P700 ATP800\|VPN50\|VPN100\|VPN300\|VPN000\|"FLEX") | FiFinds hosts with the Zyxel hardcoded password vulnerability  |
| intext:Please Login SSL VPN inurl:remote/login intext:FortiClient                                                                                                                                                                                                                      | Finds Fortinet VPN login pages                                 |
| site:vpn.*.*/ intext:"login" intitle:"login"                                                                                                                                                                                                                                           | Retrieves various VPN login pages                              |
| intitle:"index of" /etc/openvpn/                                                                                                                                                                                                                                                       | Retrieves juicy information and sensitive directories          |
| "-----BEGIN OpenVPN Static key V1-----" ext:key                                                                                                                                                                                                                                        | Finds OpenVPN static keys                                      |
| intitle:"index of" "vpn-config.*"                                                                                                                                                                                                                                                      | Retrieves juicy information about the vpn-config file          |
| Index of /*.ovpn                                                                                                                                                                                                                                                                       | Finds OpenVPN configuration files, some certificates, and keys |
| inurl:"/vpn/tmindex.html" vpn                                                                                                                                                                                                                                                          | Finds Netscaler and Citrix Gateway VPN login portals           |
| intitle:"SSL VPN Service" + intext:"Your system administrator provided the following information to help understand and remedy the security conditions:"                                                                                                                               | Finds Cisco Adaptive Security Appliance (ASA) login web pages  |

#### VPN footprinting with AI

Example prompt with ShellGPT
"`Use inurl serach operator to obtain the Fortinet VPN login pages`"
### Footprinting Through Shodan 

A search in Shodan for VoIP and VPN will help gather VPN and VoIP related information 

### References
CEH v13 M2