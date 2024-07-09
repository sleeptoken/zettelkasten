
2024-07-09 10:40

given a **multi-byte piece of data** like a number how should we represent it in memory should we put the most significant byte first which would be big endian 
```
0xa01183d1
| a0 | 11 | 83 | d1 |
	  BIG ENDIAN
```

or should we put the least significant bite first which would be little endian
```
0xa01183d1
| d1 | 83 | 11 | a0 |
	LITTLE ENDIAN 
```

1. Endianness is about byte ordering and not bit ordering
2. Endianness only applies to multibyte values meaning that if we only have one byte there's no such thing as endianness (e.g. `0xAA`)
### References

https://www.youtube.com/watch?v=LxvFb63OOs8