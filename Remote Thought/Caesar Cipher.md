
2024-06-04 18:17

Source: #overthewire #krypton

Tags: [[Cryptography]] [[linux]]

## Little to no help was taken from internet crypto tools for solving krypton 

#### level 1-2

Problem - It is 'encrypted' using a simple rotation called ROT13. ROT13 is a variation of a Caesar Cipher [Modular conversion, encoding and encryption online - cryptii](https://cryptii.com/)
It is also in non-standard ciphertext format.  When using alpha characters for cipher text it is normal to group the letters into 5 letter clusters, regardless of word boundaries.  This helps obfuscate any patterns. This file has kept the plain text word boundaries and carried them to the cipher text.

`cat "krypton2"  | tr "[A-Z]" "[N-ZA-M]"` - ROT13
tr "[]"  "[]" (translate) command takes 2 arguments . 
1. set of chars it's gonna be translating from 
2. translate to 

#### level 2-3

run "[A-Z]" through the given encrypt function see how it affects the original alphabetical order, further use translate command to 

### References

[OverTheWire: Level Info: Krypton Level 1 → Level 2](https://overthewire.org/wargames/krypton/krypton1.html)

Solution - 
https://www.youtube.com/playlist?list=PLNQtvINcOqXoWJutFBW0JGn0m7Nw2FrlK