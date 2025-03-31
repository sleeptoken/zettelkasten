
2025-03-31 15:43

Tags: [[DNS (53 port)]]

Domain Name System Security Extensions (DNSSEC) zone walking is a type of DNS [[Enumeration]] technique in which an attacker attempts to obtain internal records if the DNS zone is not properly configured. The enumerated zone assist the attacker in building a host network map. 

Organizations use DNSSEC to add security features to the DNS data and provide protection against known threats to the DNS. This security feature uses digital signatures based on public-key cryptography to strengthen authentication in DNS. These digital signatures are stored in the DNS name servers along with common records such as MX, A, AAAA, and CNAME. 

While DNSSEC provides Internet security, it is also susceptible to a vulnerability called zone enumeration or zone walking. 

To overcome the zone enumeration vulnerability, a new version of DNSSEC that uses Next Secure version 3 (NSEC3) is used. The NSEC3 record provides the same functionality as NSEC records, except that it provides cryptographically hashed record names that are designed to prevent the enumeration of record names present in the zone. 

To perform zone enumeration, attackers can use various DNSSEC zone enumerators such as LDNS, DNSRecon, nsec3map, nsec3walker, and DNSwalk..
#### Tools - LDNS 

[GitHub - NLnetLabs/ldns: LDNS is a DNS library that facilitates DNS tool programming](https://github.com/NLnetLabs/ldns)
LDNS - walk enumerates the DNSSEC Zone and obtains results on the DNS record files.
### DNSRecon

DNSRecon is a zone enumeration tool that assists users in enumerating DNS records such as A, AAAA, and CNAME. It also performs NSEC Zone enumeration to obtain DNS record files of a target domain 
```
dnsrecord -d <target domain> -z
```

### References
