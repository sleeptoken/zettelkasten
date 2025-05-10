
2025-05-10 14:39

Source: #reversing 

Tags: [[Binary Exploit]]

Anything that needs to be run on a Windows Operating System is executed using an executable file, also called a Portable Executable file
A PE file is a Common Object File Format (COFF) data structure. The COFF consists of Windows PE files, DLLs, shared objects in Linux, and ELF files.

If we open a `PE` file in a Hex editor, we will see a random bunch of Hex characters. This bunch of Hex characters are the instructions a Windows OS needs to execute this binary file.
All of these headers are of the data type [STRUCT](https://docs.microsoft.com/en-us/cpp/cpp/struct-cpp?view=msvc-170). A struct is a user-defined data type that combines several different types of data elements in a single variable.

The documentation for each header can be found on [MSDN](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_nt_headers32), where you can find the data types of the different fields inside these headers.

use `pe-tree` to open these exe files.
### The IMAGE_DOS_HEADER

the header consists of the first 64 bytes of the PE file. 

first two bytes that say `4D 5A`. They translate to the `MZ` characters in ASCII. 
The `MZ` characters denote the initials of [Mark Zbikowski](https://en.wikipedia.org/wiki/Mark_Zbikowski), one of the Microsoft architects who created the MS-DOS file format. The `MZ` characters are an identifier of the Portable Executable format. When these two bytes are present at the start of a file, the Windows OS considers it a Portable Executable format file.
#### e_magic

the first entry in the IMAGE_DOS_HEADER dropdown menu. It says `e_magic` and has a value of `0x5a4d MZ`. This is the same as what we saw in the Hex Editor, but the byte order is reversed due to [endianness](https://en.wikipedia.org/wiki/Endianness). 
- The Intel x86 architecture uses a little-endian format
- while ARM uses a big-endian format.
#### e_lfanew

The last value in the IMAGE_DOS_HEADER is called `e_lfanew` it has a value of `0x000000d8`. This denotes the address from where the IMAGE_NT_HEADERS start. Therefore, in this PE file, the IMAGE_NT_HEADERS start from the address `0x000000d8`

The IMAGE_DOS_HEADER is generally not of much use apart from these fields, especially during malware reverse engineering. **The only reason it's there is backward compatibility between MS-DOS and Windows.**
### The DOS_STUB:

In the pe-tree utility, , we see that the following dropdown menu after IMAGE_DOS_HEADER is the DOS STUB.
Please note that the size, hashes, and Entropy shown here by pe-tree are not related to the PE file; instead, it is for the particular section we are analyzing. These values are calculated based on the data in a specific header and are not included.



### References
[TryHackMe | Dissecting PE Headers](https://tryhackme.com/room/dissectingpeheaders)