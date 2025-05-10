
2025-05-09 11:52

Source: #reversing 

In Ida we have the breakpoints accessible whenever we have a debugger selected

Right click on a line and click edit breakpoint 
in breakpoint settings we have options separated in the following manner

| Settings        | Actions                 |
| --------------- | ----------------------- |
| Enabled         | Break                   |
| Hardware        | Trace                   |
| Module Relative | Refresh Debugger Memory |
| Symbolic        | Enable Tracing          |
| Source Code     | Disable Tracing         |
## Settings

### Absolute breakpoint 

you can see it doesn't have any of those attributes so it's a direct address (0x7FF66ABA3BC0), now since this is the main executable IDA knows how to rebase it if this target has [[ASLR]] 
now if we are adding break points to external modules and so on absolute break points might not be the way to go since those addresses will mean something else if rebasing happens

### Module Relative 

instead of having an absolute address it's going to be in function of the module name. This is nice when we have targets that might come in later and we have their name and the relative offset from the image base so we can put a symbolic name as I'll show you based on the image base

to illustrate in that case if we have a moving target we can for example make it module relative and the location is the name of the module + the offset from the base (`C:\Win\System32\notepad.exe+0x23BC0`) and this is good enough we don't have necessarily to put a break point just on our own executable we could have put it on any other DLL it could be a temporary DLL that gets dropped we know the name we know the RVA(Relative Virtual Address) IDA will follow it nicely once that DLL matches when it loads matches our breakpoint relative description here then it will become active.

### Symbolic name 

is a symbol name just we put something that Ida resolves, any name known to IDA and then when that name is available the breakpoint will become available 

We are putting a name as for example if we have a name anywhere in the database that's resolvable we can use the symbolic break point and we just put the name here
### Source code

is source code as it says it could be pseudo code from the decompiler or actually Source level debugging and 
source so suppose here Control+Alt+B to open the breakpoint window and so here we have a source breakpoint 
`$7FF66ABA3BC0;4` 
so here that's the function start; line four that's the source level breakpoint so these
## Actions 

- Break what action to take 
- Trace to add a trace log 
- Refresh debugger memory when the breakpoint hits for example we want to issue refresh debugger memory this will recompute all the map pages and the memory information 
- Simply a breakpoint to enable or disable tracing so let's say we put a breakpoint at Point A to enable tracing and point B to disable tracing

Conditional breakpoint - use custom script 

### Hardware breakpoints 

When enabled we get the following options - read, write, execute 
Hardware break points are subject to limitations from the underlying Hardware, on x86 we have up to four different architectures which might have no Hardware break points or have more or less etc and we can control the size of the hardware breakpoint as well based on the architecture 
hardware breakpoint is good enough execute read write break points, very handy if you want to catch when a certain memory is being read from or written and so on. for example if we want to use Hardware breakpoint on execute it's pretty handy when we're dealing with packed code, we can put the breakpoint from the start even though the code has not been unpacked, so what does that mean it means we let the Packer write the actual code and then when it executes we'll hit the breakpoint if we didn't use hardware breakpoint in that case then we're putting a breakpoint on the packed code and that can influence the unpacking or perhaps trigger self check sum for example.

### References
https://www.youtube.com/watch?v=EVG1GkTZu5w