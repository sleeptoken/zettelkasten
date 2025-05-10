
2025-05-10 14:39

Source: #reversing 

Tags: [[Binary Exploit]]

Anything that needs to be run on a Windows Operating System is executed using an executable file, also called a Portable Executable file
A PE file is a Common Object File Format (COFF) data structure. The COFF consists of Windows PE files, DLLs, shared objects in Linux, and ELF files.

If we open a `PE` file in a Hex editor, we will see a random bunch of Hex characters. This bunch of Hex characters are the instructions a Windows OS needs to execute this binary file.
All of these headers are of the data type [STRUCT](https://docs.microsoft.com/en-us/cpp/cpp/struct-cpp?view=msvc-170). A struct is a user-defined data type that combines several different types of data elements in a single variable.

The documentation for each header can be found on [MSDN](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_nt_headers32), where you can find the data types of the different fields inside these headers.
### The IMAGE_DOS_HEADER

the header consists of the first 64 bytes of the PE file. 
first two bytes that say `4D 5A`. They translate to the `MZ` characters in ASCII. 
The MZ characters denote the initials of [Mark Zbikowski](https://en.wikipedia.org/wiki/Mark_Zbikowski), one of the Microsoft architects who created the MS-DOS file format. The MZ characters are an identifier of the Portable Executable format. When these two bytes are present at the start of a file, the Windows OS considers it a Portable Executable format file.

### References
[TryHackMe | Dissecting PE Headers](https://tryhackme.com/room/dissectingpeheaders)