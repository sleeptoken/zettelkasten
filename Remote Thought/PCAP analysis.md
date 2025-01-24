
2024-07-20 19:49

Source: 

Tags: [[Wireshark]]

head over to statistics -> endpoints to see the people in conversation - In Wireshark, a Conversation is between two Endpoints

#### Following TCP stream

right click on the stream/connection you are interested in -> follow -> TCP stream 
Following a protocol stream applies a display filter which selects all the packets in the current stream.
### Following the HTTP2 Traffic 

Sometimes we have to decrypt the ssl traffic in wireshark: 
- open the .pcap file and then load the sslkey.log file in **Wireshark**. The sslkey.log file is used for **decrypting the ssl traffic** in wireshark.
- Then, we had to **filter and follow the HTTP2 traffic stream** to find the flag-


### References
[TryHackMe | h4cked](https://tryhackme.com/r/room/h4cked)