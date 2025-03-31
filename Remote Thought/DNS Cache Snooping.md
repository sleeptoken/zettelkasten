
2025-03-31 14:48

Tags: [[DNS (53 port)]]

DNS cache snooping is a type of DNS [[Enumeration]] technique in which an attacker queries the DNS server for a specific cached DNS record. By using this cached record, the attacker can determine the sites recently visited by the user. This information can further reveal important information such as the name of the owner of the DNS server, its service provider, the name of its vendor, and bank details. By using this information, the attacker can perform a social engineering attack on the target user. 

### Non-recursive Method 

In this method, to snoop on a DNS server, attackers send a non-recursive query by setting the Recursion Desired (RD) bit in the query header to zero. Attackers query the DNS cache for a specific DNS record such as A, CNAME, PTR, CERT, SRV, and MX. If the queried record is present in the DNS cache, the DNS server responds with the information indicating that some user on the system has visited a specific domain. Otherwise, the DNS server responds with the information about another DNS server that can return an answer to the query, or it replies with the root. hints file containing information about all root DNS servers. 

Attackers use the dig command followed by the name/IP address of the DNS server, domain name, and type of DNS record file. The `+norecurse` option is used to set the query to non-recursive. 
```sh
dig @<IP of DNS server> <Target domain> A +norecurse
```
The status `NOERROR` in the output implies that the query was accepted but no answer was returned, thereby indicating that no user from the system had visited the queried site.

### Recursive Method 

In this method, to snoop on the DNS server, attackers send a recursive query by setting the `+recurse` option instead of the `norecurse` option. Similar to the non-recursive method, the attackers query the DNS cache for a specific DNS record such as A, CNAME, PTR, CERT, SRV, and MX. 
In this method, the time-to-live (TTL) field is examined to determine the duration for which the DNS record remains in the cache. Here, the TTL value obtained from the result is compared with the TTL that was initially set in the TTL field. If the TTL value in the result is less than the initial TTL value, the record is cached, indicating that someone on the system has visited that site. However, if the queried record were not present in the cache, it will be added to the cache after the first query is sent. 

```sh
dig @<IP of DNS server> <Target domain> A +recurse 
```

If the TTL value for a domain is considerably high, which strongly suggests that the domain record was not in the cache when the query was issued.

### References
