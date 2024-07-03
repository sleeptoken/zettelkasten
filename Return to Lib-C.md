
2024-07-03 18:42

Source: #youtube 

Tags: [[Binary Exploit]]

**This time we have NX enabled** 
**File should be dynamically linked** 

dynamically linked set which means that the functions that are called from `libc` for example gets and puts rather than including all of the code in the binary that code is stored in your `libsy` library on your system and whenever this whenever the program wants to access one of those functions that's going to have a look at the global offset table here basically what's going to happen is whenever it calls for example gets it's going to go to this global offset table section and it's going to look to see do we know what address the gets function is in the lib c library on this computer because every version of `libc` is going to have different functions with different amounts of code so the offsets will always be different and sometimes you'll have protections like [[aslr]] on there which will actually randomize the address so it'll go and find out what the address is and it'll return and it'll populate the global offset table with that address so that the next time it's called it won't need to find out what that address is it'll just use what it's populated in the global offset table



### References
https://www.youtube.com/watch?v=0CFWHjc4B-I&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=7