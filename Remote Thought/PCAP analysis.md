
2024-07-20 19:49

Source: 

Tags: [[Wireshark]]

head over to statistics -> endpoints to see the people in conversation - In Wireshark, a Conversation is between two Endpoints

#### Following TCP stream

right click on the stream/connection you are interested in -> follow -> TCP stream 
Following a protocol stream applies a display filter which selects all the packets in the current stream.
##### *Lab*

Your TCP stream in Wireshark shows **only repeated retransmissions** of the **same 100-byte payload**, which includes the **beginning of a PNG file**

if you have all TCP connections and sabmei follow stream karne pe same dikh raha hai toh most likely the message is fragmented and u got to reassemble manually. But **that's it** — you're only getting the first part of the [[PNG]] (header and some metadata). There's **no image data** (`IDAT` chunk) or PNG end marker (`IEND`). the sender kept on sending due to lack of `ACK` message. 

- Run this to dump each TCP segment's payload
```bash
tshark -r capture.pcap -Y "tcp.stream eq 0" -T fields -e tcp.payload > payloads.txt
```

once you get payloads.txt covert the hex in that file to binary using py 
```python
with open('payloads.txt', 'r') as f:
    hex_data = f.read().replace('\n', '').strip()

# Convert hex to binary
binary_data = bytes.fromhex(hex_data)

# Save to a PNG file
with open('recovered_image.png', 'wb') as f:
    f.write(binary_data)

print("Image saved as recovered_image.png")
```
### Following the HTTP2 Traffic 

Sometimes we have to decrypt the ssl traffic in wireshark: 
- open the .pcap file and then load the sslkey.log file in **Wireshark**. The sslkey.log file is used for **decrypting the ssl traffic** in wireshark.
- Then, we had to **filter and follow the HTTP2 traffic stream** to find the flag-

Since we saw that there was TCP traffic there, let us see how many TCP streams are in there.
```
andrew@kali:~$ $ tshark -nr packet3.pcap -T fields -e tcp.stream | sort -u
```
### References
[TryHackMe | h4cked](https://tryhackme.com/r/room/h4cked)
https://andrewroderos.com/how-to-solve-my-pcap-ctf-challenges/