
2024-07-06 13:03

Source: #youtube 

Tags: [[Binary Exploit]]

**PARTIAL RELRO**  -->  if this was set to full RELRO we wouldn't be able to overwrite the global offset table address this basically defines the readability and writability of the global offset table

**STACK CANARY FOUND** 
**NX ENABLED**
**NO PIE**

 we're going to use a format string vulnerability but there's going to be no buffer overflow like list time [[Leak PIE & Ret2Lib-C]] . we're going to use the format string write operation and we're going to use that to overwrite an element of the global offset table



### References
https://www.youtube.com/watch?v=KgDeMJNK5BU&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=10