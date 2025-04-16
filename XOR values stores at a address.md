
2025-04-16 09:01

Source: #reversing 
### TLDR;

exe file asks us to enter password; user entered password is XOR'd and compared with values stored in a buffer 
if pass is correct we get success message. 
### REAL DEAL 

Opening the program in IDA we see we only have one Function 



### References
https://adamhlt.com/flare-on-2015-0x1-first-challenge-writeup/#password-recovery
https://github.com/fareedfauzi/Flare-On-Challenges?tab=readme-ov-file
https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution1.pdf
https://www.aldeid.com/wiki/The-FLARE-On-Challenge-2015/Challenge-1
https://secwriteups.blogspot.com/2016/08/flare-2015-challenge-1.html