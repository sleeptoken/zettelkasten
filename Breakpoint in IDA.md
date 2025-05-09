
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
### Module Relative 

instead of having an absolute address it's going to be in function of the module name. This is nice when we have targets that might come in later and we have their name and the relative offset from the image base so we can put a symbolic name as I'll show you based on the image base
### Symbolic name 

is a symbol name just we put something that Ida resolves, any name known to IDA and then when that name is available the breakpoint will become available 
### Source code

is source code as it says it could be pseudo code from the decompiler or actually Source level debugging and 

## Actions 

- Break what action to take 
- Trace to add a trace log 
- Refresh debugger memory when the breakpoint hits for example we want to issue refresh debugger memory this will recompute all the map pages and the memory information 
- Simply a breakpoint to enable or disable tracing so let's say we put a breakpoint at Point A to enable tracing and point B to disable tracing

### References
https://www.youtube.com/watch?v=EVG1GkTZu5w