
2024-06-24 16:51

Source: #tryhackme  #web 

**TLD (Top-Level Domain)**
A TLD is the most righthand part of a domain name. There are two types of TLD
1. gTLD (Generic Top Level) - tell the user the domain name's purpose eg. `.com, .org, .edu, .gov`
2. ccTLD (Country Code Top Level Domain) - for geographical purposes eg. `.ca, .in, .us`

**Second-Level Domain**

When registering a domain name, the second-level domain is limited to 63 characters + the TLD and can only use a-z 0-9 and hyphens (cannot start or end with hyphens or have consecutive hyphens).

**Subdomain**

A subdomain sits on the left-hand side of the Second-Level Domain using a period to separate it. A subdomain name has the same creation restrictions as a Second-Level Domain. The max length of a domain name must be kept to 253 characters or less. 
##### DNS Record Types
###### A Record 
These records resolve to IPv4 addresses,
###### AAAA Record 
These records resolve to IPv6 addresses
###### CNAME Record
These records resolve to another domain name
###### MX Record
- These records resolve to the address of the servers that handle the email for the domain you are querying.
- These records also come with a priority flag.
- This tells the client in which order to try the servers, this is perfect for if the main server goes down and email needs to be sent to a backup server.
###### TXT Record
- TXT records are free text fields where any text-based data can be stored. 
- TXT records have multiple uses, but some common ones can be to list servers that have the authority to send an email on behalf of the domain (this can help in the battle against spam and spoofed email). 
- They can also be used to verify ownership of the domain name when signing up for third party services.
- write a little about SPF records

DNS records all come with a [[TTL]] (Time To Live) value. This value is a number represented in seconds that the response should be saved for locally until you have to look it up again. (i.e. this field specifies how long a DNS record should be cached for) Caching saves on having to make a DNS request every time you communicate with a server.

### Enumeration using Amass

OWASP Amass is a DNS enumeration tool that allows attackers to map the target network and discover potential attack surfaces. Attackers use a combination of both active and passive reconnaissance techniques to gather information from DNS. This tool enables attackers to enumerate critical information without triggering any security alerts within the DNS environment of the targeted network. 

```
amass enum -d <Target Domain> 
```
The above command allows attackers to gather all the details of DNS, including its subdomains. Using this command, attackers can also enumerate details of IP addresses, SSL/TLS, HTTP, APIs, certificates, web archives, and data scraping incorporated with the target domain.
### Enumeration using [[nmap]]

list all the available services on the target host
```
nmap --script=broadcast-dns-service-discovery <target domain>
```

to retrieve all the subdomains associated with the target host along w the IP address
```
nmap -T4 -p 53 --script dns-brute <target domain>
```
If any wildcard entries are recorded they are represented as `*A*` for IPv4 addresses and `*AAAA*` for IPv6 address

to check weather DNS recursion is enabled on the target server
```
nmap -Pn -sU -p 53 --script=dns-recursion <IPaddress>
```
#### DNSSEC Enumeration 

Enumerate using `dns-nsec-enum.nse` or `dns-nsec3-enum.nse`
to retrieve the list of subdomains associated with the target domain 
```
nmap -sU -p 53 --script dns-nsec-enum --script-args dns-nsec-enum.domains=eccouncil.org <target IP>
```
#### Enumeration Countermeasure

- Ensure that the resolver can be accessed only by the hosts inside the network 
- Ensure that the request packets exiting the network use random ports
- Audit DNS zones to identify vulnerabilities in domains and subdomains
- Update and patch nameservers with the most recent versions of software
### References
[TryHackMe | DNS in detail](https://tryhackme.com/r/room/dnsindetail)

a full list of over 2000 TLDs [click here](https://data.iana.org/TLD/tlds-alpha-by-domain.txt).