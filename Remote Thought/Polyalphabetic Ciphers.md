
2024-06-10 20:48

Source: #overthewire #krypton 

Tags: [[Cryptography]] [[linux]]

#### Level 4 - 5

Vigenère cipher is a polyalphabetic cipher
we know the key length, we can map every nth letter from the cipher text corresponding to the key length i.e.
```
P P R O C E E D M E E T I N G A S A G R E E D\
K G O L D G O L D G O L D G O L D G O L D G O\
```

Here the key is "GOLD", and all the letters corresponding to G are collected thus making this a monoalphabetic cipher further freq analysis is carried out to find the 1st letter of the key.
Then increase the shift by one and then repeat the process

#### level 5 - 6 

Find repeated sequences that are certain chars apart (eg. 10), factors of that separated length (eg. 2,5,10) will be possible key length 





### References

Krypton Scripts - 
https://github.com/m-rosinsky/Krypton_Scripts