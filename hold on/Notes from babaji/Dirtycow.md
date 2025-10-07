### what is copy on write 

COW basically optimization trick hai. Same memory page is mapped into multiple processes. For example, agar kisi process ne `fork()` kiya, to child process ke liye pura memory copy nahi karte. 

Parent ke pages child se shared rahte hain.

Child ne agar kahi pe read kiya, to nothing changes.

Child ne agar kahi pe write attempt kiya, to write gets intercepted. Shared page copy hoga. Child process ki copy alag ho gayi. Aur ab wo write wo copied page me hoga.

COW filesystem me bhi implement kar sakte hain vaise.

> Multiple child, agar same page write krenge, to sabki ek alag copy bnegi fir? or manlo do process ki same writes they tab?

Yes, har process ke liye alag copy banegi
do process hain, dono same content write krenge to bhi copy banegi
that is the safest bet

Okay, kya idhar waise optimization ho skta tha
is case me?
in theory yes. but implementation ki performance penalty rahegi
dono write alag time pe huye, but content same raha fir?
content pahle alag alag tha. Fir bad me same ho gya. Fir?
### what is DirtyCOW

A race condition was found in the way the Linux kernel's memory subsystem handled the copy on write (COW) breakage of private read only memory mappings. An unprivileged local user could use this flaw to gain write access to otherwise read-only memory mappings and thus increase their privileges on the system.
