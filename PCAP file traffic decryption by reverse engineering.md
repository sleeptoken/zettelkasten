
2025-06-18 18:50

Source: #reversing 

Tags: [[PCAP analysis]]

The challenge is designed to teach you about PCAP file parsing and traffic decryption by reverse engineering an executable used to generate it. This is a typical scenario in our malware analysis practice where we need to figure out precisely what the malware was doing on the network.

As part of the challenge, you were provided two files: an executable binary and a PCAP network capture file. 
In Wireshark filter for `http`. You should be able to notice a series of POST requests 
The user-agent string is a hint that the payload of these requests likely contains the key that you need to extract where each request appears to be a part of the larger message.

after combining the key values we get a long key that couldn't be decoded easily, luckily we have the executable that produced the traffic captured in the PCAP file, so we can figure out whether or not there are additional steps involved in encrypting the transmitted key. 





### References
[Flare-On-Challenges/Write-ups/2015/2015solution5.pdf at master · fareedfauzi/Flare-On-Challenges · GitHub](https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution5.pdf)