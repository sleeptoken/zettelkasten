
2024-06-30 14:57

Tags: [[Binary Exploit]] 

essentially we're going to try to overflow the buffer and redirect the execution to a function of our choice or to an address of our choice and normally that function is called win or hacked or flag.
[1] - code referred later in the document
```python
#include <stdio.h>
void hacked()
{
    printf("This function is TOP SECRET! How did you get in here?! :O\n");
}
void register_name()
{
    char buffer[16];
    printf("Name:\n");
    scanf("%s", buffer);
    printf("Hi there, %s\n", buffer);    
}
int main()
{
    register_name();
    return 0;
}
```
whenever a function is called, you have the function, you have the parameters which are placed on the stack and then you have the return address and then the base pointer and then the buffer which is going to have all of our local variables and things that we're going to use in that function 
so when we get to the end of the function it's returning to the calling function so for example here we have this `main` function it calls `register_name` it's going to set up this buffer, it's going to go through all of this code and then it's going to return to main but what if we overwrote the return address of `main` which is on the stack where it was supposed to return to with another address

- if we had compiled this with pi enabled we wouldn't actually see these addresses. Notice with pie we don't have those same memory addresses what we have is offsets
- the offset will be the same each time the program loads the only difference is going to be this base address
- the base address is going to change each time which means if pi was enabled we wouldn't just be able to provide the function address, we need to find out first of all what the base of the binary is and then go from there

rest of the process is to find where the breakpoint is and then fill the stack up to the breakpoint with bogus data (payload) and then append the payload with the address of the function we want to call 

[[pwntools]] method for ret2win- 

 we're launching the program and we're going to wait for the program to crash and then we're going to read the core dump that it creates and we're going to look in the core dump and see what was in the instruction pointer at the time of crashing and then we'll have a look and see what the offset of that is in this cyclic pattern
 further code is in the references section. 
# Return 2 win with function parameters 

### 32 bit
```c
#include <stdio.h>
void hacked(int first, int second)
{
    if (first == 0xdeadbeef && second == 0xc0debabe){
        printf("This function is TOP SECRET! How did you get in here?! :O\n");
    }else{
        printf("Unauthorised access to secret function detected, authorities have been alerted!!\n");
    }

    return;
}
void register_name().......same as above w/o params...
```
above code is the only notable change in the code from [1]
1. here we use cyclic find out how many of these bytes do we need to overwrite the return address and then we'll know the offset. 
2. use python to craft a payload
3. set up a breakpoint at the return address of the function running inside `main` and send the crafted payload 
4. use `n` to cycle through operations one by one after the breakpoint 
``` 
//view of the disassembled code in gdb
cmp      dword ptr [ebp+8], 0xdeadbeef
jne        hacked+57
```
`cmp` is like an if statement that compares the content at the address of `ebp+8` with `0xdeadbeef` - print out `ebp+8` as hex using `x $ebp + 8`
`jne` (jump not equal) is like an else statement that will jump to `hacked+57` if comparison fails

if you're ever having a lot of problems with the payload try and do it with some automatic functionality in Pwntools and then have a look through what's actually in the payload. It's a good way to debug your manual payloads. 
### 64 bit
```c
#include <stdio.h>

void hacked(long first, long second)
{
    if (first == 0xdeadbeefdeadbeef && second == 0xc0debabec0debabe){
        printf("This function is TOP SECRET! How did you get in here?! :O\n");
    }else{
        printf("Unauthorised access to secret function detected, authorities have been alerted!!\n");
    }
}

void register_name()...... same as [1]
```
in 32 bit notice that the `0xdeadbeef` was on the stack remember it was `ebp+8` and then `0xc0debabe` was on the stack `ebp+12`. if we have a look at the 64-bit edition it's actually using the registers namely `rdi` `rsi` `rdx` in that order

Essentially it means we're going to need to populate these registers we can't just send in the return address of the function followed by the parameters we need to send the return address to the function and then we need to put those parameters into these registers and in order to do that we need to find some `rop` gadgets

Notice that we would expect these bytes to have ended up in the `rip` (`eip` in 32bit) but they didn't and it's just the way 64-bit works if you try to give it an invalid memory address it won't even populate the `rip` so what we need to do in this case is take the values that we have from the `rsp`

 

### References
https://www.youtube.com/watch?v=E4ZWJsGySoY&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=4

ret2win using pwntools - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/03-return_to_win/ropstar.py

ret2win with params using pwntools:
32 bit - w/o ROP - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/04-ret2win_with_params/32-bit/exploit.py
With ROP - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/04-ret2win_with_params/32-bit/ropstar.py

64 bit - w/o ROP - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/04-ret2win_with_params/64-bit/exploit.py
with ROP - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/04-ret2win_with_params/64-bit/ropstar.py