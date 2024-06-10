
2024-06-07 13:10

Source: #overthewire #krypton 

Tags: [[linux]] [[Cryptography]]

#### Level 3-4

create a temp dir on the ssh'ed machine
use [[scp]](secure copy) command in Linux system to copy file(s) between servers in a secure way.
`scp -P port source_file user@hostname:destination_file`
here source file is the file you wanna transfer and destination file is the directory path where you wanna transfer the file to 

in frequency analysis along with single letter occurrence also check trigrams (3 letter occurrence - "the" in English)


### References

Freq analysis chart - 
[Frequency Table (cornell.edu)](https://pi.math.cornell.edu/~mec/2003-2004/cryptography/subs/frequencies.html)

freq analysis script - 
https://github.com/m-rosinsky/Krypton_Scripts/blob/master/freq_analysis.py

scp - https://www.geeksforgeeks.org/scp-command-in-linux-with-examples/