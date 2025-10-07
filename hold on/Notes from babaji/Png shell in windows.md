matlab ki `CreateProcess(“C:\Foo\Bar.png”,..)` kiya to wo PNG file ko process kar sake?

Driver -> executes loader -> jo PNG process karke uske andar ka shellcode run karega
But iske liye humko `Createprocess` ke process ko hook karna hoga ki agar windows bola ki bad format then driver execute Karo..
Driver -> executes loader

Only if this chain works seamlessly by `CreateProcess`
I am not sure but try karna padega
PNG se shellcode load hota hai woh pata hai 😬
Yes agar hook karke usko custom process karwaya toh possible hai