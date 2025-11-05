
Source: 

Tags: [[Tags/Forensics|Forensics]] [[linux]] 

Disk partition is a contiguous area of a hard disk that has several logical disks.
Every file system is organized into a sequence of blocks (e.g. 1024 bytes each) With the following components 
- Boot block - contains the initial bootstrap program used to load the O.S(kernal). Located in the 1st few sections of the file system 
- Super block - contains global file system information and is considered as the balance sheet of UNIX file system
- inode block - an inode is a data structure used to represent a file object, a file or a directory. An inode is identified by a unique number called the "inode number". Users identify files by file names, but Unix identifies files by the inode number
- data block - contains the actual data
