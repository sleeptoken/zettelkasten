
2024-07-30 15:14

Source: #htb 
 
Tags: [[Binary Exploit]] [[Format String Vulnerabilities (printf)]]

We are given a binary that takes a bunch of Boolean inputs and if it matches a randomly generated value (referred to as winning) then it gives you an option to input some data that gets stored in the stack .

now if you win then the code checks if a `flag.txt` exists in the working directory and if it doesn't then it throws an error.
create a `flag.txt` in the working directory 


### References
https://app.hackthebox.com/challenges/racecar

walkthroughs - 
[Hack the Box rev HTB_RACECAR (pwnjournal.github.io)](https://pwnjournal.github.io/HTB_RACECAR/)
[HTB pwn →‘racecar’. | by Apothiphis_z | Medium](https://medium.com/@fracchetto1995/htb-pwn-racecar-f9206cfd943f)
[HTB: Racecar - remoteshell.zip](https://remoteshell.zip/racecar/)
[Racecar | WriteUps (gitbook.io)](https://sayonara.gitbook.io/writeups/hackthebox/challenges/pwn/racecar)