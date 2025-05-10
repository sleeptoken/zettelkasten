
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

> The IMAGE_DOS_HEADER is generally not of much use apart from these fields, especially during malware reverse engineering. **The only reason it's there is backward compatibility between MS-DOS and Windows.**
### The DOS_STUB:

In the pe-tree utility, , we see that the following dropdown menu after IMAGE_DOS_HEADER is the DOS STUB.
Please note that the size, hashes, and Entropy shown here by pe-tree are not related to the PE file; instead, it is for the particular section we are analyzing. These values are calculated based on the data in a specific header and are not included
- The size value denotes the size of the section in bytes. 
- Entropy is the amount of randomness found in data. The higher the value of Entropy, the more random the data is.

> The DOS STUB is a small piece of code that only runs if the PE file is incompatible with the system it is being run on. For example, since this PE file we are examining is a Windows executable, if it is run in MS-DOS, the PE file will exit after showing the message in the DOS STUB.

### NT_HEADERS:

The starting address of IMAGE_NT_HEADERS is found in `e_lfanew` from the IMAGE_DOS_HEADER

So let's start by going to this offset and see what we find there. We can do that by pressing `Ctrl+G` in the Hex Editor Window or going to `Edit > Go to offset` from the GUI.
#### Signature

The first 4 bytes of the NT_HEADERS consist of the Signature. We can see this as the bytes `50 45 00 00` in Hex, or the characters `PE` in ASCII as shown in the Hex editor.
#### FILE_HEADER

the FILE_HEADER has the following fields:

- **_Machine**:_ This field mentions the type of architecture for which the PE file is written. In the above example, we can see that the architecture is i386 which means that this PE file is compatible with 32-bit Intel architecture.

- **_NumberOfSections**:_ A PE file contains different sections where code, variables, and other resources are stored. This field of the IMAGE_FILE_HEADER mentions the number of sections the PE file has. In our case, the PE file has five sections. We will learn about sections later in the room.

- **_TimeDateStamp**:_ This field contains the time and date of the binary compilation. 

- **_PointerToSymbolTable** and **NumberOfSymbols**:_ These fields are not generally related to PE files. Instead, they are here due to the COFF file headers.

- **_SizeOfOptionalHeader**:_ This field contains the size of the optional header, which we will learn about in the next task. In our case, the size is 224 bytes. 

- **_Characteristics**:_ This is another critical field. This field mentions the different characteristics of a PE file. In our case, this field tells us that the PE file has stripped relocation information, line numbers, and local symbol information. It is an executable image and compatible with a 32-bit machine.

#### OPTIONAL_HEADER

some of the critical fields in the OPTIONAL_HEADER.

- _Magic:_ The Magic number tells whether the PE file is a 32-bit or 64-bit application. If the value is 0x010B, it denotes a 32-bit application; if the value is 0x020B, it represents a 64-bit application. The above screenshot of the Hex Editor shows the highlighted bytes, which show the magic of the loaded PE file. Since the value is 0x010B, it shows that it is a 32-bit application.

- _AddressOfEntryPoint:_ This field is significant from a malware analysis/reverse-engineering point of view. This is the address from where Windows will begin execution. In other words, the first instruction to be executed is present at this address. This is a Relative Virtual Address (RVA), meaning it is at an offset relative to the base address of the image (ImageBase) once loaded into memory.

- BaseOfCode and BaseOfData: These are the addresses of the code and data sections, respectively, relative to ImageBase.

- _ImageBase:_ The ImageBase is the preferred loading address of the PE file in memory. Generally, the ImageBase for .exe files is 0x00400000, which is also the case for our PE file. Since Windows can't load all PE files at this preferred address, some relocations are in order when the file is loaded in memory. These relocations are then performed relative to the ImageBase.

- _Subsystem:_ This represents the Subsystem required to run the image. The Subsystem can be Windows Native, GUI (Graphical User Interface), CUI (Commandline User Interface), or some other Subsystem. The screenshot above from the pe-tree utility shows that the Subsystem is 0x0002, representing Windows GUI Subsystem. We can find the complete list in [Microsoft Documentation](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_optional_header32).

- _DataDirectory:_ The DataDirectory is a structure that contains import and export information of the PE file (called Import Address Table and Export Address Table). This information is handy as it gives a glimpse of what the PE file might be trying to do. We will expand on the import information later in this room.

### IMAGE_SECTION_HEADER

The data that a PE file needs to perform its functions, like code, icons, images, User Interface elements, etc., are stored in different Sections. We can find information about these Sections in the IMAGE_SECTION_HEADER.

`the IMAGE_SECTION_HEADER` has different sections, namely `.text`, `.rdata`, `.data`, `.ndata` and `.rsrc`

- _.text:_ The .text section is generally the section that contains executable code for the application. We can see above that the Characteristics for this section include CODE, EXECUTE and READ, meaning that this section contains executable code, which can be read but can't be written to.

- _.data:_ This section contains initialized data of the application. It has READ/WRITE permissions but doesn't have EXECUTE permissions.

- ._rdata/.idata:_ These sections often contain the import information of the PE file. Import information helps a PE file import functions from other files or Windows API.

- .ndata: The .ndata section contains uninitialized data.

- _.reloc:_ This section contains relocation information of the PE file.

- _.rsrc:_ The resource section contains icons, images, or other resources required for the application UI.

### IMAGE_IMPORT_DESCRIPTOR

PE files don't contain all the code they need to perform their functions. In a Windows Operating System, PE files leverage code from the Windows API to perform many functions. The IMAGE_IMPORT_DESCRIPTOR structure contains information about the different Windows APIs that the PE file loads when executed. This information is handy in identifying the potential activity that a PE file might perform.
For example, if a PE file imports CreateFile API, it indicates that it might create a file when executed.

These files are dynamically linked libraries that export Windows functions or APIs for other PE files.
To find more information about what the function does, we can check out Microsoft Documentation. For example, [this link](https://docs.microsoft.com/en-us/windows/win32/api/winreg/nf-winreg-regcreatekeyexw) has details about the `RegCreateKeyExW` function.

we can see the values OriginalFirstThunk and FirstThunk. The Operating System uses these values to build the Import Address Table (IAT) of the PE file

example:
 Take the redline binary from the attached VM as an example. Its IMAGE_IMPORT_DESCRIPTOR imports notable functions such as `CreateProcessW`, `CreateDirectoryW`, and `WriteFile` from `kernel32.dll`. This implies that this PE file intends to create a process, create a directory, and write some data to a file.



### References
[TryHackMe | Dissecting PE Headers](https://tryhackme.com/room/dissectingpeheaders)