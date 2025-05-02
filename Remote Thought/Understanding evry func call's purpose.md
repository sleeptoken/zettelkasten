
2025-05-01 20:52

Source: #reversing 

Overall structure is similar to [[XOR (known key) values stored at a address]] 

The structure of this code looks very similar to the last challenge, with the two calls to `GetStdHandle()`
 - a `WriteFile()` call to display the prompt
 - a `ReadFile()` call to collect the input to a buffer (at 0x402159 in this case). 
 Where the two programs differ is that this challenge performs a function call (at address 0x40105F) to determine its success or failure condition whereas the last challenge performed a loop directly in this function.

```Dissassembly of the call to check function
text:0040104E      lea    eax. [ebp-4] 
text:00401051      push   eax             :1pNumberOfBytesWritten
text:00401052      push   11h             :nNumberOfBytesToWrite 
text:00401054      push   dword ptr [ebp-4] 
text:00401057      push   offset unk_402159 
text:0040105C      push   dword ptr [ebp-18h] 
text:0040105F      call   sub_401084
``` 

 In this block of code we see the call to the function whose return value (zero or non-zero) will determine if the program displays the success or failure message. This function takes 3 arguments: 
 - The pointer value (0x4010E4) from the first call instructiox2.n (stored at cbp-10h),
 - A pointer to buffer of input (0x402159)
 - The number of bytes entered as input (stored in ebp-4).
 
```
mov      ecx. 25h 
cmp      [ebp+dwInputLength], ecx 
loc_401096: 
jl       short loc_401007
```

This sequence simply checks the length of the entered input and exits if it is less than 37. It just so happens that the length of the correct answer is 37, The next sequence of instructions just loads the function parameters into registers, `esi` for the input and `edi` for the key. 

```Loop Initialization
text:00401098        mov     esi, [ebp + input] 
text:00401098        mov     edi, [ebp + key] 
text:0040109E        lea     edi, [edi + ecx-1] 
```

The last instruction in this sequence overwrites `edi` by adding `ecx` (which is 0x25 at this point in the program) and -1 to it. This effectively sets `edi` to point to the end of the key instead of the beginning. 

Notice that the end of the loop structure is a subtraction against `edi` (which points to the key) instead of a more common increment
```
sub       edi,  2
loop      loc_4010A2
```

This along with the fact that `edi` is set to the end of the key at the beginning of the loop is a sign that the key is stored in reverse order.

The [`BX`](https://www.quora.com/What-is-the-purpose-of-a-general-purpose-register-BX) register maintains a rolling sum throughout the loop, which is factored into the computation of each byte. 
- BX is used as a data register, indicating that it can temporarily store general data.

the [`SAHF`](https://www.felixcloutier.com/x86/sahf) instruction loads the high byte of that value (which is 0x01) into the `EFLAGS` register. The result of this is that the Carry flag is set. 
- Loads the SF, ZF, AF, PF, and CF flags of the [EFLAGS](https://stackoverflow.com/questions/75048577/conditions-under-which-eflags-flags-are-set-in-x86-x64) register with values from the corresponding bits in the AH register (bits 7, 6, 4, 2, and 0, respectively)

The [`LODSB`](https://pdos.csail.mit.edu/6.828/2018/readings/i386/LODS.htm#:~:text=LODS%20loads%20the%20AL%2C%20AX,index%20register%20is%20automatically%20advanced.) instruction loads a byte of the input, pointed to by `ESI` into `AL` and the `PUSHF` instruction stores the `EFLAGS` to the stack. 
- LODS loads the AL, AX, or EAX register with the memory byte, word, or doubleword at the location pointed to by the source-index register. After the transfer is made, the source-index register is automatically advanced

Then the [POPF](https://pdos.csail.mit.edu/6.828/2008/readings/i386/POPF.htm) instruction restores the EFLAGS register with the saved flags on the stack (carry flag set).
- POPF/POPFD pops the word or doubleword on the top of the stack and stores the value in the flags register.

The [ADC](https://www.felixcloutier.com/x86/adc) instruction that follows is just like a normal ADD instruction but it also adds the carry flag, which is always set in this case.

The [SCASB](https://pdos.csail.mit.edu/6.828/2004/readings/i386/SCAS.htm) will then compare this low byte with the key value that is pointed to by EDI, as well as increment EDI.

If the values do not match then the CMOVNZ instruction will move a zero value stored in DX to the loop counter CX (part of ECX), effectively terminating the loop prematurely.

The last JECXZ instruction checks to see if the loop counter is zero, which signifies a premature termination and a key mismatch, and jumps to the failure code path.

If the loop iterates a full 37 times with no mismatches between the input and the key, then the LOOP instruction will stop branching when it decrements ECX to zero and the code will follow the success path.

Deducing the inverse to this function is not straight-forward and it may be easier to simply place a breakpoint at the SCASB instruction and repeatedly try values until one produces the correct value needed to match the key. 

below is the code that will give the answer
```
def rol(byte, count): 
    byte = (byte << count | byte >> (8 - count)) & 0xFF 
    return byte 
    
data = '\xAF\xAA\xAD\xEB\xAE\xAA\xEC\xA4\xBA\xAF\xAE\xAA\x8A\xC0\xA7\xB0\xBC\x9A\xBA\xA5\xA5\xBA\xAF\xB8\x9D\xB8\xF9\xAE\x9D\xAB\xB4\xBC\xB6\xB3\x90\x9A\xA8'[::-1]  

AH = AL = AX = BX = DX = 0 
result = '' 

for c in data: 
    AH = rol(1, DX) 
    AL = (ord(c) - AH - 1) ^ 0xC7 
    BX += ord(c)
    DX = BX & 3 
    result += chr(AL)

print(result)
```
### References
Flare-on 2 2015

[Flare-On-Challenges/Write-ups/2015/2015solution2.pdf at master · fareedfauzi/Flare-On-Challenges · GitHub](https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution2.pdf)