
2024-10-23 11:13

Source: #htb 

Tags: [[Stream Cipher]] [[Cryptography]]

ChaCha20 is a performant and lightweight, yet highly secure, 256-bit [stream cipher](https://en.wikipedia.org/wiki/Stream_cipher) used to encrypt and decrypt data.
- this encryption algorithm is _symmetric_.
- This [website](https://loup-vaillant.fr/tutorials/chacha20-design) explains that the decryption is the same as encryption (we can infer this since other than the `init` all that is being used is an `XOR`, which undoes itself)
- There is no randomness in the `init` function but as this website explains, it is instead scrambling data we give it    
- Therefore, the solution is to rename the encrypted flag (to bypass the check), and run the program on it which will decrypt it
### HTB

This challenge serves as an introduction to reversing encryption programs, and shows us how we do not need a complete & in-detail understanding to reverse it and get the flag.

This challenge contains two files, a program for encryption and an encrypted version of the flag
> we can create a file and run it through the encryptor to observe the behavior 

now, on decompiling the program we see the program 
- Checks if the file we input ends with `.undead` (the custom extension for files it encrypts) and if so, refuses to encrypt it 
further details about the program are listed [here](https://arushs-notes.gitbook.io/blog/hack-the-boo-24/practice/reversing/crypt-of-the-undead)

### References
https://ctf.hackthebox.com/event/1811

walkthroughs - 
[*The design of Chacha20](https://loup-vaillant.fr/tutorials/chacha20-design)
[What is ChaCha20? | Proton](https://protonvpn.com/blog/chacha20)