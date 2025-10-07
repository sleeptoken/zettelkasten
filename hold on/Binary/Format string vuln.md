6 THM

This vulnerability happens in `printf` and `scanf` family 

Different types of format pointer 
- %c char
- %d int 
- %s string 
- %x for 32bit int -> output is in little endian and prints only 4 bytes 
- %lx for 64bit int -> output is in little endian and prints only 8 bytes 
- %n pointer 
- %hn half pointer 

### how to identify 

File 32 bit use %x
File 64 bit use %lx

`%x.%x.%x` used to overflow hexadecimal string in the heap, here `.` is a field seperator 

Add` %lx.%lx` (small x to have the output in lower case) around 6 times to bypass the registers that are printed so that we can get the juicy data 

`ABCD.%lx.%lx.%lx.%lx.%lx.%lx.%lx.%lx`
Add lx till you see the content overflowing (if you start seeing overflowing bytes in the shell). That point is where the program stops taking input and just before that point is where stack exists

Rsi, rdx, rcx usually never empty



