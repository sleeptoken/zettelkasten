How to read and open zlib file
```sh
$ binwalk -e steg_flag.png
---------------------------------------
HEXADECIMAL   DESCRIPTION 
0x0           PNG image
0x29          Zlib compressed data
```

extract kaise kiya? `binwalk -e` ya `binwalk -dd=‘.*’` ?
`zlib-flate -uncompress < IN_FILE > OUT_FILE`
 `printf "\x1f\x8b\x08\x00\x00\x00\x00\x00\x00\x00" |cat - 29.zlib | gzip -dc >/tmp/out`
uh
that's just going to be the PNG data
PNG stores it's data in zlib format, so binwalk recognizes it has "compressed data"
you are following a rabbit hole
change your path

I will consider this to be a bug in binwalk then
I mean, it should be able to figure out how many bytes to skip for that given PNG image