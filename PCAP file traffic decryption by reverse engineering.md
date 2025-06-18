
2025-06-18 18:50

Source: #reversing 

Tags: [[PCAP analysis]]

The challenge is designed to teach you about PCAP file parsing and traffic decryption by reverse engineering an executable used to generate it. This is a typical scenario in our malware analysis practice where we need to figure out precisely what the malware was doing on the network.

As part of the challenge, you were provided two files: an executable binary and a PCAP network capture file. 
In Wireshark filter for `http`. You should be able to notice a series of POST requests 
The user-agent string is a hint that the payload of these requests likely contains the key that you need to extract where each request appears to be a part of the larger message.

after combining the key values we get a long key that couldn't be decoded easily, luckily we have the executable that produced the traffic captured in the PCAP file, so we can figure out whether or not there are additional steps involved in encrypting the transmitted key. 

On running strings on the executable 

> The User-Agent string is the same one that we saw in the PCAP file: `Mozilla/5.0 (Windows NT 6.1; WOW64) KEY.` If this was a real malware this unique string could really be useful for writing a detection signature. 

- The rest of the strings appear to be debugging log entries except the last two. The purpose of the string `flarebearstare` is not immediately clear. 
- The last string appears almost like the base64 alphabet `abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567891/` with the upper-case and lower-case blocks flipped. It is a common practice for malware authors to use custom Base64 alphabets, so take a mental note of this string for later analysis.




### References
[Flare-On-Challenges/Write-ups/2015/2015solution5.pdf at master · fareedfauzi/Flare-On-Challenges · GitHub](https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution5.pdf)