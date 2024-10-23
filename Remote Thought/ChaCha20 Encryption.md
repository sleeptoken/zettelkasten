
2024-10-23 11:13

Source: #htb 

Tags: [[Stream Cipher]] [[Cryptography]]

This challenge serves as an introduction to reversing encryption programs, and shows us how we do not need a complete & in-detail understanding to reverse it and get the flag.

This challenge contains two files, a program for encryption and an encrypted version of the flag
> we can create a file and run it through the encryptor to observe the behavior 

now, on decompiling the program we see





### References
https://ctf.hackthebox.com/event/1811

walkthroughs - 
[*The design of Chacha20](https://loup-vaillant.fr/tutorials/chacha20-design)

[Crypt of the Undead | Blog / Notes / Write-ups](https://arushs-notes.gitbook.io/blog/hack-the-boo-24/practice/reversing/crypt-of-the-undead)
[What is ChaCha20? | Proton](https://protonvpn.com/blog/chacha20)