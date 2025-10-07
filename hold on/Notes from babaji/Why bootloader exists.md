### What is the use

ye dual boot wala concept hamesha to tha nahi tha 
- you purchased a computer. 
- It came with its software suite.
- And you used that. 

Then “installing OS” became a thing. hardware wahi rahega. OS ke naye version aate jate rahenge. Ya dusra OS bhi ban jayega.

Now we have a problem: har OS ka apna “boot protocol” hai. Matlab kernel kaha - kidhar - kaise load karna hai etc. arguments / configurations kaise apply karne hain etc.
And har hardware ka bhi apna alag “startup” wala sequence hai
ki wo storage se “OS” kaise load karega.

You have M operating systems, and N hardware. Sab ko sab pe run karna hai to you need MxN variants.
Utna code karo. Fir utne variants test karo.
To bich me ek middle layer ghusa diya: **bootloader**.

> jiska kaam sirf ek hai: to load some OS
> then you do some more fuckery. And variants ka count MxN se reduce ho ke M+N pe aa gaya

most of the time, M+N < MxN
less variants, less headache

This is true today as well. You want to dual boot windows and Linux both. What do you do? You setup grub.
And grub simply chains another bootloader: `bootmgr`
- bootmgr can deal with windows specific things.
- Grub can handle Linux first hand. But not Windows. Because it does not have to: it simply chains another bootloader.

### Unified Extensible Firmware Interface

Then UEFI happened.
Traditional BIOS was simple: read MBR, take a jump, load bootloader etc.

UEFI changed things:

1. Hardware chain of trust
2. ⁠Hardware announces itself (instead of being discovered in BIOS)
3. ⁠It can load kernels directly.
4. ⁠It can do multi-boots.

### Bootloader better than UEFI

to technically, for dead simple cases, bootloader ki jarurat nahi hai

But then, bootloader abhi bhi use hote hain
Because, bootloader extra “fancy” features dete hain
-for example. 
	- grub allows you to change kernel boot config temporarily
	- also lets you deal with encrypted partitions.

root partition encrypted hai? Server DC (data controller) me hai?
You can SSH into it, even if it is still waiting for password
punch the decryption key
and let it boot the actual OS
Can’t have the same thing via UEFI