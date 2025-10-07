> Supporting non English characters necessitates a letter character set, leading to creation of unicode

Unicode encompasses characters from all writing systems worldwide, including accents and many others, assigning each character a unique identifier called Unicode code point. As of July 2023, Unicode 15 defines nearly 150,000 characters. To represent all these we require 17.2 bits, which unfortunately is slightly larger than the size of 2 bytes. Consequently, the natural data type to hold a single code point is int32, and that is precisely what UTF-32 does.
### UTF8 — Efficient Unicode

Indeed, using a fixed length of 32 bits to represent every character, as done in UTF-32, ensures functional adequacy for character encoding. However, it comes at the cost of increased memory and storage consumption
### UTF-8 — Variable-length encoding

UTF-8 employs a variable-length encoding scheme, utilizing 1 to 4 bytes to represent each Unicode character. Notably, commonly used characters benefit from shorter encodings. ASCII characters, in particular, require only 1 byte, ensuring that UTF-8 and ASCII encodings are identical

For most characters in modern languages such as `Tiếng Việt, 中文, ภาษาไทย, and عربي`, UTF-8 typically uses 2 or 3 bytes. It judiciously reserves the use of 4 bytes for less frequent characters, optimizing memory usage without compromising on comprehensive character representation

### Decoding UTF-8

RULES
- If the higher-order bit is 0, the character is represented by 7 bits, corresponding to standard ASCII characters.

- If the higher-order bits are 1, the number of leading `1s` indicates the number of bytes occupied by the character.
	-For example, 110xxxxx, 1110xxxx, and 11110xxx signify characters taking 2, 3, and 4 bytes, respectively. Subsequent bytes of a multi-byte character always start with 10xxxxxx.

Let’s illustrate this with an example: Encoded string: 
```
01000001_11000011_10000010_11100100_10111000_10101101_11110000_10011111_10001111_10000000
```

- `01000001` → Starts with 0 → ASCII → “A”
- `11000011_10000010` → Starts with 110 → 2 bytes → `“Â”`
- `11100100_10111000_10101101` → Starts with 1110 → 3 bytes →` “中”`
- `11110000_10011111_10001111_10000000` → Starts with 11110 → 4 bytes → `“🏀”`
### Drawback

one drawback of UTF-8 is the lack of direct indexing, making it challenging to access the Nth character in a UTF-8 encoded string in O(1) time complexity. When scenarios demand frequent direct indexing, it may be beneficial to consider re-encoding the string using fixed-length encodings like UTF-32.

### Reference 

https://medium.com/@briannqc/utf-8-explained-its-not-8-bits-encoding-nor-32-bits-unicode-205362df9c80
[[Encoding clusterfuck]]