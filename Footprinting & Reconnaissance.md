
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
IoT search engines such as `Shodan`, `Censys` and `ZoomEye` attackers can obtain information such as the manufacturer details, geographic location, IP address & open ports on the IoT device. 
### Meta search engines

These search engines do not have their own search indexes; instead, they take the inputs from the users and simultaneously send out the queries to the third-party search engines to obtain the results. Once sufficient results are gathered, they are ranked according to their relevance and presented to the user through the web interface.
> Using meta search engines, such as `Startpage`, `MetaGer`, and `eTools.ch`, attackers can send multiple search queries to several search engines simultaneously and gather substantially detailed information 
### FTP search engines

FTP search engines are used to search for files located on FTP servers that contain valuable information regarding the target organization. A special client such as [FileZilla](https://filezilla-project.org) can be used to access FTP accounts; it also supports functionalities such as uploading, downloading, and renaming files. 
> Using FTP search engines such as `NAPALM FTP Indexer`, `FreewareWeb FTP File Search`, `Mamont`, and `Globalfilesearch.com`, attackers can search for critical files and directories containing valuable information 

| Google Dork                                                                                                   | Description                                                                                                                                |
| ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| site:.in .com \| .net intitle:"index of" ftp                                                                  | Finds files containing juicy information                                                                                                   |
| intitle:"index of" "*/ftp.txt"                                                                                | Finds files containing juicy information                                                                                                   |
| intext:"index of" "ftp"                                                                                       | Finds files containing juicy information                                                                                                   |
| inurl:WS_FTP.log                                                                                              | Finds files containing juicy information                                                                                                   |
| intitle:index.of /cftp/robots.txt                                                                             | Finds files containing juicy information                                                                                                   |
| intitle: "Index of ftp passwords"                                                                             | Finds files containing passwords                                                                                                           |
| inurl: /ftp intitle:"office"                                                                                  | Detects the web server                                                                                                                     |
| inurl:/web-ftp.cgi                                                                                            | Finds pages containing login portals                                                                                                       |
| site:sftp.*.*/ intext:"login" intitle:"server login"                                                          | Finds pages containing login portals                                                                                                       |
| intitle:"Index of" ws_ftp.ini                                                                                 | Finds the "ws_ftp.ini" file, which contain usernames and passwords of FTP users                                                            |
| inurl:ftp-inurl:(http\|https) intext:"@gmail.com" intext:subject fwd confidential \| important \| CARD \| cvv | Finds archived email conversations, at times revealing full credit-card numbers and customer information as well as private company emails |
| allintitle:"CrushFTP WebInterface"                                                                            | Detects various pages of CrushFTP WebInterface, which includes login portals as well password reset/recovery page                          |
| "ws_ftp.log" ext:log                                                                                          | Finds sensitive directories                                                                                                                |
| intitle:"Monsta ftp" intext:"Lock session to IP"                                                              | Shows websites that use the FTP service of Monsta FTP                                                                                      |
| "index of" /ftp/logs                                                                                          | Finds potential log files                                                                                                                  |
| intitle:"index of" inurl:ftp intext:admin                                                                     | Lists admin folders on FTP servers                                                                                                         |
### Find subdomains 

#### Google Dork

Google dork to identify all the sub-domains of the target
```
site:microsoft.com -inurl:www
```
#### Online Tools 

[Netcraft](https://www.netcraft.com) - provides internet security services and analysis the various parameters of the internet / can also do OS foot printing.  
[DNSDumpster - Find & lookup dns records for recon & research](https://dnsdumpster.com/) - Domain research tool
[Penetration testing toolkit](https://pentest-tools.com/) - Freemium

### Web archieve

[archive](https://archive.org)
CLI - Photon to retrieve achieved URLs of the target website from `archive.org`
```
python3 photon.py -u <URL of the target website> -l 3 -t 200 --wayback
```
### Public search services 

online people search servies such as `Spokeo`, `intelius`, `pipl`, `BeenVerified`, `Whitepages`, `Instant Checkmate`, & `PeekYou` provide details about people

Also search Job sites such as Linkedin, Glassdoor, Dice
### Dark Web

Queries to find sensitive info on the dark web.

![[Pasted image 20250112133453.png]]
### Internet research services 

![[Pasted image 20250113104721.png]]
### Public Source-code repo 

Recon-ng - it assists attackers in gathering information from public source code repositories.
### Social networking sites

Attackers use social media as a medium to perform social engineering attacks
`sherlock` - search websites with a target username - available via cli
[Social Searcher](https://www.social-searcher.com/)search for content on social networks in real time 
#### LinkedIn 

use `theHarvester` tool to perform enumeration on LinkedIn
example cmd to obtain employee details via linkedin - 
```sh
theHarvester -d eccouncil -l 200 -b linkedin
# -d = domain or company name
# -l = number of results to be retrieved
# -b = data source
```
#### Email lists

use automated tools such as `theHarvester` and `Email Spider` to collect publicly available email addresses.  
```sh
theharvester -d microsoft.com -l 200 -b baidu -f output.xml
# -f specifies the output file
```
#### Social media presence 

use tools like [BuzzSumo ](https://buzzsumo.com/) , `Google Trends` to locate information on social media sites
### Whois

Whois is a query and response protocol used for querying databases that store the registered users or assignees of an internet resource, such as domain name 
This protocol listens on port 43(TCP)  
Regional Internet Registries maintain `whois` databases

Three types of data models exist to store and lookup `whois` information 
- **Thick Whois (Distributed Model)** - Stores the complete Whois information from all the registrars for a particular set of data
- **Thin Whois (Centralized Model)** - Stores only the name of the Whois server of the registrar of a domain, which in turn holds complete details on the data being looked up.
- **Decentralized Whois** - stores complete Whois information and has multiple independent entities to manage the Whois database 

`whois` services such as [TamoSoft](https://www.tamos.com/), [Whois Lookup, Domain Availability & IP Search - DomainTools](https://whois.domaintools.com/) can help perform `whois` lookups

batch IP converter available at [SABSOFT](http://sabsoft.com/) , provides information about the IP and a lot of other juicy information. 
### Geolocation information 

IP geolocation lookup tools, such as [IP Address to IP Location and Proxy Information | IP2Location](https://www.ip2location.com/) and `IP Location Finder`
### DNS

 After collecting the `Whois` records of the target, comes the DNS foot printing part 
- DNS records provide important information about the location and types of servers 

![[Pasted image 20250113151440.png]]
### DNS Interrogation tools

Attackers use DNS interrogation tools such as
[SecurityTrails: Data Security, Threat Hunting, and Attack Surface Management Solutions for Security Teams](https://securitytrails.com/) , DNSChecker

#### Fierce CLI

is a DNS recon tool used to enumerate subdomains related to the target 

Run the following command to start basic scan on the target domain 
```
fierce --domain certifiedhacker.com
```

to scan the target domain to obtain specific subdomains
```
fierce --domain certifiedhacker.com  -subdommains write admin mail
```

to scan domains near the discovered records of the targeted domain 
```
fierce --domain certifiedhacker.com  -subdomains mail -traverse 10
```

to attempt an HTTP connection on the discovered domains of the target 
```
fierce --domain certifiedhacker.com  --subdomains mail --connect
```

to scan all the discovered records of the target domain i.e. a full detailed scan 
```
fierce --domain certifiedhacker.com  --wide
```
### Reverse DNS lookup

DNS Lookup is used to find the IP address for a given domain name, and a reverse DNS operation is performed to obtain the domain name of a given IP address 

Attackers perform a reverse DNS lookup on IP ranges in an attempt to locate a DNS PTR record for those IP addresses.
when we obtain an IP address or a range of IP addresses, we can use these tools to obtain the domain name

**Dnsrecon**
- example cmd
	- `dnsrecon -r 10.10.10.0 - 10.10.10.255`
		`-r` = specifies the IP address for a reverse lookup by brute force

[MX Lookup Tool - Check your DNS MX Records online - MxToolbox](https://mxtoolbox.com/)

### Network and Email Footprinting 

next step after retrieving DNS information is to gather network-related info and track email communications 
#### Locate the network range

- detailed information regarding IP allocation and the nature of allocation is available with the appropriate regional registry database
- To find the network range of the target network, one must enter the server IP address in the ARIN Whois database search tool. A user can also visit the [American Registry for Internet Numbers](https://www.arin.net/)  website and enter the server IP in to the SEARCH site or `Whois` text box.
- If an attacker traces a route to a machine, it could be possible to obtain the internal IP address of the gateway, which can be useful 
#### Traceroute 

> Uses the ICMP protocol and Time to Live (TTL) field of the IP header to find the path of the target host in the network. 

The Traceroute utility can detail the path through which IP packets travel between two systems. The utility can trace the number of routers the packets travel through, the round-trip time (duration in transiting between two routers), and, if the routers have DNS entries, the names of the routers and their network affiliation.

It can also trace geographic locations. It works by exploiting a feature of the Internet Protocol called [[TTL]]. The TTL field indicates the maximum number of routers a packet may traverse. Each router that handles a packet decrements the TTL count field in the ICMP header by one. When the count reaches zero, the router discards the packet and transmits an ICMP error message to the originator of the packet

![[Pasted image 20250114120109.png]]

- The utility records the IP address and DNS name of the router and sends out another packet with a TTL value of two. This packet makes it through the first router and then times-out at the next router in the path.
- This second router also sends an error message back to the originating host.
- Traceroute continues to do this and records the IP address and name of each router until a packet finally reaches the target host or until it decides that the host is unreachable.
- In the process, it records the time taken for each packet to make a round trip to each router. 
- Finally, when it reaches the destination, the normal ICMP ping response will be sent back to the sender. 
- The utility helps to reveal the IP addresses of the intermediate hops in the route to the target host from the source

- ICMP Traceroute 
	windows OS by default uses the ICMP traceroute 
	- `C:\>tracert 216.239.36.10`
	
- TCP Traceroute
	Many devices in any network are generally configured to block ICMP traceroute messages. In this scenario, an attacker uses TCP and UDP traceroute, also known as layer 4 traceroute 
	- in linux - `sudo tcptraceroute www.google.com`

- UDP traceroute
	Like windows, Linux also has a built in traceroute ability, but it uses UDP protocol for tracing the route 
	- in linux - `traceroute www.google.com`

***Traceroute Tools**

[Netscantools pro](https://www.netscantools.com/index.html) - Pro version isn't free
[Pingplotter](https://www.pingplotter.com/) - Best for visualization 
#### Email Tracking Tools

MXToolbox, 
DNS Checker Mail Header Analyzer, 
social catfish 
[eMailTrackerPro](https://emailtrackerpro.en.softonic.com/)
[IP Address to IP Location and Proxy Information | IP2Location](https://www.ip2location.com/)

### Social Engineering 

**Eavesdropping** 
	Unauthorized listening of conversations or reading of messages
	It is the Interception of any form of communication, such as audio, video, or text
	
**Shoulder Surfing** 
	Secretly observing the target to gather critical information, such as passwords, personal identification number, account numbers, and credit card information

**Dumpster Diving**
	Looking for treasure in someone else's trash It involves the collection of phone bills, contact information, financial information, operations-related information, etc. from the target company's trash bins, printer trash bins, user desk for sticky notes, etc.

**Impersonation** 
	Pretending to be a legitimate or authorized person and using the phone or other communication medium to mislead targets and trick them into revealing information

### Advanced footprinting tools

#### [Maltego](https://www.maltego.com/)

is an automated tool that can be used to determine the relationship and real world links between people, group of people, websites etc 
#### [FOCA](https://github.com/ElevenPaths)

Fingerprinting Organizations with Collected Archives (FOCA) is a tool used mainly to find metadata and hidden information in the documents that its scans.

Features involve:
- Web Search - Searches for hosts and domain names through URLs associated with the main domain.
- DNS Search - Checks each domain to ascertain the host names configured in NS, MX, and SPF servers to discover the new host and domain names. 
- IP Resolution -  performs analysis against the organization's internal DNS.
- PTR Scanning - Finds more servers in the same segment of a determined address
- Common Names - Perform dictionary attacks against the DNS.

Attackers search the target domain and obtain the file information stored in it. The extracted files can be viewed on the web browser. 
#### [subfinder](https://github.com/projectdiscovery/subfinder)

Subdomain discovery tool that uses passive online sources for discovery.
#### [OSINT Framework](https://osintframework.com/)

#### [ReconDog](https://github.com/s0md3v/ReconDog)

ALL in one tool for all basic information gathering, uses APIs to collect information about the target 
#### [BillCipher](https://github.com/bahatiphill/BillCipher)

For website and IP address, needs python2, python3 & ruby 

### Ai Powered OSINT tools

#### [Taranis AI](https://taranis.ai/)

uses Ai to enhance information gathering and situational analysis. uses NLP to improve the data received from data sources 
#### [OSS Insight](https://ossinsight.io/)

enables users to query GitHub data using natural languages, generate SQL queries, and present the results visually, basically can compare stats of different repos
#### Develop custom scripts

The following script is designed to perform website foot printing including DNS lookup, WHOIS records retrieval, email  enumeration and more.

change domain in script to desired target domain	
```sh
cat <<< EOF > website_footprinting.py 
import subprocess 

def dns_lookup(domain): 
	return subprocess.getoutput(f"dig {domain} ANY +noall +answer")

def whois_lookup(domain):
	return subprocess.getoutput(f"whois (domain)") 

def email enumeration (domain): 
	return subprocess.getoutput (f"theHarvester -d {domain} -b all -1 100") 

def run_footprinting(domain): 
	print("Performing DNS Lookup...") 
	dns_info = dns_lookup(domain) 
	print(dns_info)
	
	print("\nPerforming Whois Lookup...")
	whois_info = whois_lookup(domain)
	print(whois_info) 
	
	print("\nEnumerating Emails...") 
	emails = email_enumeration(domain)
	print(emails) 
	
	domain = 'www.microsoft.com' 
	run_footprinting(domain)

EOF

python3 website_footprinting.py
```

### Foot printing Countermeasure 

- Develop and enforce security policies to regulate the information that employees can reveal to third parties 
- Set apart internal and external DNS or use split DNS, and restrict zone transfer to authorized servers 
- Disable directory listings in the web servers 
- Opt for privacy services on a Whois Lookup database 
- Avoid domain-level cross-linking for critical assets 
- Place critical documents, such as business plans and proprietary documents offline to prevent exploitation 
- Sanitize the details provided to Internet registrars to hide the direct contact details of the organization 
- Disable the geo-tagging functionality on cameras to prevent geolocation tracking 


### References
CEH v13 M2