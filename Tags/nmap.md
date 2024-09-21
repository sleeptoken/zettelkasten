```
nmap --script vuln 192.168.1.1
```
Use scripts to check for vulnerabilities.

```
nmap --script smb-enum-shares -p 445 192.168.1.1
```
Enumerates [[SMB shares]] using Nmap script.

```
nmap --script http-enum -p 80 192.168.1.1
```
Enumerates web server directories using Nmap script.

```
nmap --script smb-vuln-ms17-010 192.168.1.1
```
Checks for MS17-010 (EternalBlue) vulnerability.

```
nmap --script smb-vuln-regsvc-dos 192.168.1.1
```
Checks for registry service DoS vulnerability.

```
nmap --script http-sql-injection --script args='http-sql-injection.args' -p 80 192.168.1.1
```
Checks for [[SQL injection]] vulnerabilities using Nmap script.

```
nmap -sL 192.168.1.0/24
```
List all IPs in the subnet without scanning them.

```
nmap -p80 --script http-methods 192.168.1.1
```
Discover allowed HTTP methods on a webserver.

```
nmap -p80 --script http-headers 192.168.1.1
```
Retrieve HTTP headers from the server.

```
nmap -p80 --script http-auth 192.168.1.1
```
Test for HTTP authentication methods.