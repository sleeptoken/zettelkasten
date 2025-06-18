
2025-05-03 14:30

Source: #reversing 

Tags: 

when running the `strings` command. The `UPX!` strings indicate that the file was packed using UPX. UPX — or Ultimate Packer for Executables. Packers like UPX are used to obfuscate executables in an effort to make it more difficult for analysts to figure out what is going on during static analysis.

- For the next step you'll want to unpack the challenge so it can be easily loaded into static analysis tools.
- A common method of unpacking UPX packed binaries is the command line option `-d`. This option extracts the original packed binary. 

how "`upx -d`" operates. When unpacking a compressed file using the command line option, UPX references a structure that is located at the end of the PE header. This header includes checksums, packed and unpacked sizes, and compression details. This process is done entirely within the UPX utility and no code is executed from the packed sample. 

- In contrast, when a UPX packed program is executed a small unpacking stub embedded within the program is run first. 
- This stub unpacks the executable to memory, loads the required imports and executes the program. This means the stub can be patched or produce an entirely different executable than one created by "`upx -d`".

### References
https://monliclican.medium.com/rootcon-2017-ctf-binforcry-350-write-up-walkthrough-part-2-of-x-5731c91c2266

[Flare-On-Challenges/Write-ups/2015/2015solution4.pdf at master · fareedfauzi/Flare-On-Challenges · GitHub](https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution4.pdf)