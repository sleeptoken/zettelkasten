
2024-06-29 10:52

Source: #youtube 

Tags: [[Binary Exploit]] 

**Debugger as well as a disassembler** 

to start - `gdb file`

to see functions used - `info functions`

to have a look at the assembly code use `disassemble func_name `
eg. `disassemble main`

example - continuation of the manual exploit mentioned in [[Overwriting variables on the stack]]

- we saw that the binary is comparing the user string to the predefined key to permit further entry. 
- get the address that corresponds to the comparison part from [[ghidra]] .
- after opening the file in gdb set up a breakpoint in the comparison part
	eg. - `break *0x80491e0`
	we can also use `disassemble 0x80491e0` to see the code at that address 
- do a standard entry (something within the size of the stack). we hit the breakpoint & we've got this comparison and again it's checking `[ebp - 0xc], 0xdeadbeef`
- to print what is at that location use `x $ebp - 0xc`
- we can cheat by manipulating what's in the stack
```
pwndbg> x $ebp - 0xc
0xffffd03c: 0x12345678

pwndbg> set *0xffffd03c = 0xdeadbeef

pwndbg> x $ebp - 0xc
0xffffd03c: 0xdeadbeef
```
### References
