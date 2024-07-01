
2024-06-30 14:57

Source: #youtube 

Tags: [[Binary Exploit]] 

essentially we're going to try to overflow the buffer and redirect the execution to a function of our choice or to an address of our choice and normally that function is called win or hacked or flag.
[1]
```
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
# Return 2 win function parameters 

32 bit
```
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










### References
https://www.youtube.com/watch?v=E4ZWJsGySoY&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=4

ret2win using pwntools - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/03-return_to_win/ropstar.py