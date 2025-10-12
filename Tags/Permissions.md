
`-rwx, drwx`

File types listed in below table 

| Character | Type              |
| --------- | ----------------- |
| `-`       | Regular file      |
| `d`       | Directory         |
| `l`       | Symbolic link     |
| `c`       | Character device  |
| `b`       | Block device      |
| `s`       | Socket            |
| `p`       | Named pipe (FIFO) |

`rwx`   ->  read write execute 
r    -> 4
w   -> 2
x    -> 1

eg. we wanna set ->  `rw-rw-rw-` 
we write  ->  `chmod 666`  ->  `rw-`  -> 4+2+0 = 6