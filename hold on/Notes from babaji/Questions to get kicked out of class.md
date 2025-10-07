> [!NOTE]
> If you don’t shift the belief behind the behavior, then it is hard to stick with long-term changes
> 	- The goal is not to read a book, the goal is to become a reader.

Some questions which got me kicked out of class / lab; or got me suspended in university (in no particular order):

- When I declare a variable, who decides the address for it in RAM? Is it compiler, linker, loader, kernel, or something else?

- When I declare a variable, who keeps track of data types of different addresses in RAM? How do I see that record?

- What is this "activation record" / "stack frame" we keep talking about in recursion? What does it look like?

- How do I recognize critical section? What does it look like?

- If CPU instruction set only contains arithmetic, logical and data movement operations (register to memory, memory to register); how is keypress on keyboard is read as input? What about mouse or touchpad? How does my laptop talk to internet? Which arithmetic / logic makes speakers in my laptop produce sound?

- In programming we had some function similar to 'random()' which was giving random output. In theory of computing, computer is fully predictable. Which one is it? If predictable, where is randomness in `random() coming from? If random, why are computers so reliable?

- If power fails, there can be data losses in computer (e.g. MS Word will give you option to recover your unsaved work to some extent). There is no assembly which can make file writes atomic. There is no function in C, C++, which makes file writes atomic. There is no OS API which makes file writes atomic. So from where is guarantee of atomicity coming from? Why can't we have it everywhere?

- How does firmware know where is boot-loader? How does boot-loader know where is kernel?

- If we say OS is necessary to handle device I/O; so that other softwares can use devices properly; how does boot-loader does it? Boot loaders can have GUI, can talk to network, can take keyboard and even mouse inputs. How?

- Why do we even need boot loader? Why does Windows require 'bootmgr'? Why does Linux require something like 'grub'? Why can 'grub' boot windows, but 'bootmgr cannot boot Linux? If we can make boot loader boot somehow; why can't we make kernel boot directly?

- Why are OS written in C and bits of assembly mostly? Why not other languages like C# or Java, or even Python?

- Why do we need these esoteric algorithms for tokenization, lexical analysis, and preparing parse tables? Why can't we read the source input line by line, split it across space; and process it word by word, line by line?

- In dynamic linking, how do we know that "data types" in different modules (DLL / shared objects) are same? What if module1 is written in language A, module2 is written in language B; and data types in those languages are incompatible (e.g. different sizes)? If we cannot reuse compiled modules, why bother with dynamic linking? If we can, how is data type incompatibility handled?

Beta batao DOM kya hota hai? 
DOM Execution and Parsing ka farak batao?
JS and JSON object ka farak batao?
DOM XSS to CSTI kaise jaate hai yeh path batao?
Burp Proxy kaam kaise karti hai?
### Recon

