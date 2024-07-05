
2024-07-05 14:15

Source: #youtube 

Tags: [[Binary Exploit]]

**PIE ENABLED**
**NX ENABLED**

if we were running this against a remote server we're not going to be able to run local commands on their system until we get a shell so we need to find a way to leak a `libc` function so we can then find our way back to the base and then find our way to the functions that we're interested in bearing in mind that every system will have a different version of `libc` and they'll have different offsets




### References
https://www.youtube.com/watch?v=NAUA1EB-TZg&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=9