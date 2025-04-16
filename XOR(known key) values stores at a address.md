
2025-04-16 09:01

Source: #reversing 
### TLDR;

exe file asks us to enter password; user entered password is XOR'd and compared with values stored in a buffer 
if pass is correct we get success message. 
### REAL DEAL 

Opening the program in IDA we see we only have one Function 


```python
#!/usr/bin/env python
s = [0x1F,0x8,0x13,0x13,0x4,0x22,0x0E,0x11,0x4D,0x0D,0x18,0x3D,0x1B,0x11,0x1C,0x0F,0x18,0x50,0x12,0x13,0x53,0x1E,0x12,0x10]
print ''.join([chr(i ^ 0x7d) for i in s])
```

### References
https://adamhlt.com/flare-on-2015-0x1-first-challenge-writeup/#password-recovery
https://github.com/fareedfauzi/Flare-On-Challenges?tab=readme-ov-file
https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution1.pdf
https://www.aldeid.com/wiki/The-FLARE-On-Challenge-2015/Challenge-1
https://secwriteups.blogspot.com/2016/08/flare-2015-challenge-1.html


https://www.reddit.com/r/Assembly_language/comments/1855gmj/what_does_edxecxebx_or_eax_mean_in_assembly/