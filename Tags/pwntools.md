
2024-06-29 18:04

Tags: 

`p32(0xdeadbeef)` - specifying that we want to pack it as a 32-bit address
or just simply set the `context.binary` use `pack()`
and this means we don't have to worry about doing all that `endianness` stuff (`\xef\xde....`)

anytime you're having problems with a pwntool script set - 
`context.log_level = 'debug'` see what's being sent and what's being received



### References
