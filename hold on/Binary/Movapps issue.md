[[Binary exploitation]]
3-THM

We have a function which has a empty return value 
You also have a function that is `admin_only` but it isn't called at all.

Essentially you fill the buffer with A's till you reach return and then add admin's address 
So the program will return the admin function 

Here instead of putting the 0x... version of the address we are using `binary.sym` to get the address of the admin function. Because the function is not called anywhere so it's not easy to get the address of the function 

Put address of any functions return value before adding `p64(admin)` to the payload

```
admin = binary.sym['admin_only']
b'A'*0x28+p64(0x004010677)+p64(admin)
```


