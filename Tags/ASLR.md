stands for address space layout randomization. This randomizes where in memory the executable is loaded each time it is run. Like PIE, it makes attacks that rely on memory layout more difficult.

If aslr is enabled then it Randomizes address 0000xxxxxxxx where x is written P64

In p32 randomizes 000xx000 here it's easy to bruteforce x in here



