
2024-06-24 16:51

Source: #tryhackme  

Tags: [[web]]

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

DNS records all come with a TTL (Time To Live) value. This value is a number represented in seconds that the response should be saved for locally until you have to look it up again. (i.e. this field specifies how long a DNS record should be cached for) Caching saves on having to make a DNS request every time you communicate with a server.


### References
[TryHackMe | DNS in detail](https://tryhackme.com/r/room/dnsindetail)

a full list of over 2000 TLDs [click here](https://data.iana.org/TLD/tlds-alpha-by-domain.txt).