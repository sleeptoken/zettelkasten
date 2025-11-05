### Order of volatility of Evidence 
1. Memory contents
2. Swap files 
3. Routing table, ARP caches, process tables, and kernal statistics 
4. File system information and Temp files 
5. Raw disk blocks 
6. Remote logging and monitoring 
7. Physical configuration and network topology 
8. Offline media, such as backups, cds and dvds 

#### pagefile 

In Unix, OSX it's called swap file on windows it's called a pagefile. 
Is a place on the hard drive used to store data that is used by programs not currently in use

#### AD1 file

File format Used by FTK to copy all of the original storage media, including the deleted files, slack space, as well as any unallocated space to a disk image 

#### Network Evidence 

- See routing table to see if attacker has tried to reroute the information being parsed, by trying to be man in the middle 
- use `pslist` to see the process list 
- `psloggedon` to see if anyone is logged on to the system

### legal - Daubert standard 

Experimental practices in the scientific community can also be admitted as evidence 

Is actually used to evaluate the reliability and validity of the forensic evidence 

---

adding `/home/username/.local/bin` to your the `PATH` -  [How to Install Volatility 2 and Volatility 3 on Debian, Ubuntu, or Kali Linux](https://seanthegeek.net/1172/how-to-install-volatility-2-and-volatility-3-on-debian-ubuntu-or-kali-linux/)

https://www.youtube.com/watch?v=kMBOtaTrfio

https://www.reddit.com/r/CTFlearn/comments/xwy0dw/first_ctf_binary_file_analysis/?rdt=65088